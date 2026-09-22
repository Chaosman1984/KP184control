# KP184control v2.1.0 — Complete User Manual

This manual describes the functions that are actually available in **KP184control v2.1.0**.

> **Supported load modes:** CC, CP/CW and CR  
> **CV:** visible in the interface, but intentionally disabled in v2.1.0  
> **Tested device profile:** KUNKIN KP184, model ID `0x0730`

---

## 1. Purpose of KP184control

KP184control controls a KUNKIN KP184 electronic load and is intended for running, monitoring and recording battery discharge tests.

The software can, among other things:

- automatically recognize the KP184;
- show live voltage, current and power;
- discharge in CC, CP/CW and CR mode;
- calculate Ah and Wh;
- save and graph the discharge curve;
- automatically stop at a configured cutoff voltage;
- calculate a safe default cutoff voltage based on battery chemistry and maximum battery voltage;
- check hardware limits before a test is started;
- automatically save test results as CSV;
- compare measured capacity with factory capacity;
- estimate remaining capacity after a usable automatic cutoff.

---

## 2. Battery data

### Battery chemistry

Available choices:

- Li-ion (NMC/NCA)
- LiPo
- LiFePO4
- NiMH

The selected chemistry is used for:

1. estimating the number of cells in series;
2. automatically calculating the safe default cutoff voltage;
3. later estimating capacity below the configured cutoff.

### Factory capacity

Enter the original nominal battery capacity in Ah, for example:

`30.000 Ah`

This field is not required for the KP184 to operate as a load, but it is used for:

- percentage of factory capacity;
- progress bar;
- Battery Health display;
- estimated total capacity after automatic cutoff.

### Maximum battery voltage

Enter the voltage of the fully charged battery.

Example:

`67.200 V`

KP184control uses this value together with the selected chemistry to estimate the series configuration.

---

## 3. Automatic cell configuration

The software divides the entered maximum battery voltage by the typical fully charged cell voltage and rounds the result to a whole number of cells.

Values used:

| Chemistry | Fully charged per cell |
|---|---:|
| Li-ion (NMC/NCA) | 4.20 V |
| LiPo | 4.20 V |
| LiFePO4 | 3.65 V |
| NiMH | 1.45 V |

Example:

- chemistry: Li-ion
- maximum battery voltage: 67.2 V
- 67.2 / 4.20 = 16

The software then shows approximately:

`Configuration: 16S | 4.200 V/cell`

---

## 4. Automatic safe cutoff voltage

### Important

In v2.1.0 the **default safe cutoff voltage is not calculated from the configured current**.

The automatic cutoff voltage is calculated from:

- battery chemistry;
- maximum battery voltage;
- the derived number of cells in series.

The configured current is checked separately against the current and power limits of the KP184.

### Default cutoff per cell

| Chemistry | Automatic cutoff per cell |
|---|---:|
| Li-ion (NMC/NCA) | 3.10 V |
| LiPo | 3.20 V |
| LiFePO4 | 3.00 V |
| NiMH | 1.00 V |

Example for a 16S Li-ion battery:

`16 × 3.10 V = 49.6 V`

The software then automatically enters approximately **49.600 V** as the cutoff voltage.

The user can adjust the cutoff voltage manually afterwards.

---

## 5. Automatic stop at cutoff

The option **Automatically stop at cutoff** is enabled by default.

During an active test:

1. KP184control waits 2 seconds before cutoff protection becomes active;
2. battery voltage is checked about once per second;
3. voltage must be at or below the configured cutoff for 3 consecutive readings;
4. LOAD OFF is then sent and the test is stopped.

This prevents one very short voltage dip from immediately ending the test.

If automatic cutoff is disabled manually, the software first shows a warning.

---

## 6. Connecting to the KP184

### COM port and address

Select the COM port to which the KP184 is connected. The Modbus address is configurable; address 1 is used by default.

### Automatic communication detection

KP184control automatically tries multiple baud rates:

- 9600
- 115200
- 57600
- 38400
- 19200
- 4800
- 2400

The software also tests both CRC byte orders used by the device.

### Model detection

Model ID `0x0730` is recognized as a KP184.

For this confirmed profile, the software uses these device limits:

- maximum 150 V;
- maximum 40 A;
- maximum 400 W.

If a device can be read but its write profile has not been confirmed, KP184control will not start a load test for safety reasons.

### Supported models and revisions

**KP184control v2.1.0 is currently hardware-validated for the KUNKIN KP184 with model ID `0x0730`.**

Other KUNKIN models or revisions may use related communication, but they have not been hardware-validated in this release and are therefore **not officially supported**.

An unknown device profile may be detected or read where possible, but KP184control will not enable the load until the write profile has been confirmed as safe.

---

## 7. Live measurement

After connection, the software continuously shows:

- voltage in V;
- current in A;
- power in W.

The measurement is updated about once per second.

During a test, the same values are used for:

- capacity in Ah;
- energy in Wh;
- CSV logging;
- graphs;
- cutoff monitoring.

---

## 8. CC — Constant Current

In CC mode, the KP184 attempts to draw a constant discharge current.

You configure:

- discharge current in amperes;
- cutoff voltage;
- soft-start on/off.

### Checks before START

KP184control checks, among other things:

- current must be greater than 0 A;
- configured current must not exceed the model limit;
- `voltage × current` must not exceed the maximum KP184 power.

Example:

At 66 V and 10 A, about 660 W would be requested. Because the confirmed KP184 profile is limited to 400 W, the test will not start.

In such a case, the software also calculates approximately what current would still remain within 400 W at the current voltage.

---

## 9. CC soft-start

Soft-start is enabled by default in v2.1.0.

The soft-start:

1. first sets a maximum of 0.100 A;
2. switches LOAD ON;
3. then increases the CC current by 0.100 A;
4. waits 100 ms between each step;
5. stops increasing when the configured final current has been reached.

Example for a 1.000 A target:

`0.1 → 0.2 → 0.3 → ... → 1.0 A`

This reduces the abrupt load step when starting a test.

Soft-start can be disabled manually. In that case, the selected CC current is set directly before LOAD ON is activated.

---

## 10. CP/CW — Constant Power

In CP/CW mode, the KP184 attempts to draw constant power.

You configure the desired power in watts.

### Checks before START

KP184control checks:

- power must be greater than 0 W;
- power must not exceed the model limit of the KP184;
- the expected current at the configured cutoff must not exceed the maximum current of the KP184.

For constant power:

`I = P / V`

This means current increases when battery voltage falls.

The software therefore specifically checks approximately what the current will become at the selected cutoff voltage.

---

## 11. CR — Constant Resistance

In CR mode, the KP184 behaves like a configured resistance.

You configure the resistance in ohms.

For this mode:

`I = V / R`

and:

`P = V² / R`

### Checks before START

Using the current starting voltage, the software checks:

- expected starting current;
- expected starting power;
- maximum 40 A limit;
- maximum 400 W limit.

If the selected resistance is too low, the test will not start and KP184control indicates approximately the minimum safe resistance for the current voltage.

### Hardware confirmation

For the tested KP184 profile, CR was hardware-confirmed with a scale of:

`0.1 Ω per register step`

The written CR setting is also read back and verified before LOAD ON.

---

## 12. CV — Constant Voltage

The CV tab is visible, but **CV is intentionally disabled in v2.1.0**.

During development, both the native CV setting and software-based current-limited CV were investigated. The experimental control was not considered stable enough for inclusion in the release.

START therefore cannot be used in CV mode.

---

## 13. Safety checks before a test

In addition to the checks for each load mode, KP184control performs general checks.

### Valid live voltage

A valid live battery voltage must first have been measured.

### KP184 maximum voltage

If measured battery voltage is above the maximum voltage of the confirmed device profile, the test will not start.

### Check of entered maximum battery voltage

If live battery voltage is more than **1.5 V higher** than the maximum battery voltage entered by the user, KP184control blocks the start.

This is intended to detect, for example, an incorrectly entered battery voltage.

### Confirmed write profile

An unknown device profile may be read, but the software will not activate LOAD until the write profile has been confirmed as safe.

---

## 14. START TEST

For a valid start:

1. old measurement data is cleared;
2. Ah and Wh are reset to zero;
3. a new CSV file is opened;
4. LOAD is explicitly switched OFF first;
5. the selected load mode is written to the KP184;
6. the settings are written;
7. LOAD is switched on;
8. settings are locked while the test is active.

The STOP button becomes active and START is temporarily disabled.

---

## 15. STOP TEST

When STOP is used:

- LOAD OFF is sent to the KP184;
- the active test stops;
- the CSV file is closed;
- final results are appended to the CSV file;
- input fields become available again;
- Battery Health is updated.

The stop reason is recorded, for example:

- manually stopped;
- automatic cutoff;
- program closed.

---

## 16. Capacity in Ah

During the test, KP184control integrates measured current over time.

In simplified form:

`Ah += current × elapsed time in hours`

This builds the actually measured discharge capacity.

---

## 17. Energy in Wh

At every measurement point, the software first calculates:

`power = voltage × current`

Then:

`Wh += power × elapsed time in hours`

This calculates the energy delivered by the battery during the test.

---

## 18. Comparison with factory capacity

If factory capacity has been entered, the software shows:

- measured Ah;
- percentage of factory capacity;
- graphical progress bar.

Example:

- factory: 30 Ah
- measured: 26 Ah

then directly measured Battery Health is approximately:

`26 / 30 × 100 = 86.7 %`

---

## 19. Estimated remaining capacity

After an **automatic cutoff**, KP184control can, if enough discharge data is available, estimate the capacity that could theoretically remain below the configured cutoff.

The software uses the final part of the actually measured discharge curve for this.

An estimate is only made if the curve is sufficiently usable. In v2.1.0 this requires, among other things:

- at least 30 measurement points;
- at least 0.5 Ah measured capacity;
- sufficient voltage change in the curve.

The software then shows, among other things:

- measured capacity;
- estimated remaining Ah;
- estimated total capacity;
- estimated Battery Health.

This value is an **estimate based on the curve**, not directly measured capacity.

---

## 20. Graphs

Several graph views are available during a test:

- Voltage (V)
- Current (A)
- Capacity (Ah)
- Energy (Wh)
- All graphs

The graphs are updated during the test.

The voltage graph also shows the configured cutoff as a reference.

---

## 21. CSV logging

For each test, a CSV file is automatically created in:

`Documents\KP184-Logs`

The filename contains date and time.

### Device information

The file contains, among other things:

- model profile;
- model ID;
- baud rate;
- CRC profile;
- read profile;
- write profile;
- maximum device voltage;
- maximum device current;
- maximum device power.

### Test settings

Among other things:

- battery chemistry;
- factory capacity;
- maximum battery voltage;
- live starting voltage;
- estimated series configuration;
- load mode;
- CC current, CP power or CR resistance;
- cutoff voltage.

### Measurement rows

Each measurement row contains:

- timestamp;
- elapsed seconds;
- voltage;
- current;
- power;
- capacity;
- energy.

### Final results

After stopping, the following are added, among other things:

- stop reason;
- start time;
- end time;
- final voltage;
- measured capacity;
- measured energy;
- test duration;
- measured Battery Health;
- when available: estimated remaining and total capacity.

---

## 22. Languages

The interface supports:

- Nederlands
- English
- Deutsch

The selected language is stored locally and restored at the next start.

---

## 23. Practical workflow

For a normal battery test:

1. Connect the battery and KP184 correctly.
2. Connect the KP184 to the PC by USB/serial.
3. Select the COM port.
4. Click **Connect**.
5. Choose the correct battery chemistry.
6. Enter the factory capacity.
7. Enter the maximum fully charged battery voltage.
8. Check the calculated series configuration.
9. Check the automatically entered cutoff voltage.
10. Choose CC, CP or CR.
11. Enter a conservative load setting.
12. Preferably leave automatic cutoff enabled.
13. Preferably use soft-start in CC mode.
14. Click START TEST.
15. Monitor the battery, wiring, connectors and KP184 during the test.
16. Use STOP TEST if anything unexpected happens.

---

## 24. Important safety note

KP184control helps validate settings, but it cannot determine the safe discharge current of a specific battery.

For example, the software knows the device limits of the confirmed KP184 profile, but does not automatically know:

- maximum continuous current of every battery;
- maximum current of every BMS;
- wire gauge;
- fuse rating;
- connector limit;
- cell temperature;
- battery damage or wear.

The user therefore remains responsible for safe test settings and supervision.

---

## 25. Limits of v2.1.0

Not active in this release:

- CV control;
- dynamic/pulse load;
- internal-resistance test;
- OCP test;
- native slew-rate configuration;
- programmable load profiles.

These functions may be investigated in future versions.

---

Copyright © 2026 Richard Uilenberg. All rights reserved.
