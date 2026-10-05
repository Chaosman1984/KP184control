# KP184control v2.3.0 — User Manual

This manual describes the functions available in **KP184control v2.3.0**.

> **Supported load modes:** CC, CP/CW, CR and CV  
> **Validated device profile:** KUNKIN KP184, model ID `0x0730`  
> **Windows release:** x64, self-contained

## 1. Purpose

KP184control controls a KUNKIN KP184 electronic load for running, monitoring and recording battery discharge tests. It displays live voltage, current and power, calculates Ah and Wh, records graphs and CSV logs, and can stop tests automatically.

## 2. Battery data and safe cutoff

Select the battery chemistry and enter the correct fully charged maximum pack voltage. KP184control uses this information to estimate the series cell configuration and a default safe cutoff voltage.

> [!WARNING]
> Always enter the correct maximum battery voltage. An incorrect value can produce an incorrect series-cell estimate and therefore an incorrect automatic cutoff.

Supported chemistries include Li-ion (NMC/NCA), LiPo, LiFePO4 and NiMH. Factory capacity in Ah is used for progress, capacity comparison and Battery Health.

## 3. Connecting to the KP184

Select the correct COM port. KP184control automatically detects supported communication settings and verifies the device profile. The hardware-validated profile is the KUNKIN KP184 with model ID `0x0730`.

For this profile the known device limits are applied:

- maximum 150 V;
- maximum 40 A;
- maximum 400 W.

An unknown profile may be read where possible, but LOAD is not enabled until a safe write profile has been confirmed.

## 4. Load modes

### CC — Constant Current

The KP184 draws a set constant current. Software soft-start is enabled by default and ramps the current up gradually.

### CP/CW — Constant Power

The KP184 attempts to draw constant power. Because `I = P / V`, current can rise as battery voltage falls. KP184control therefore also checks the expected current at the selected cutoff voltage.

### CR — Constant Resistance

The KP184 behaves as a set resistance. For the validated profile, CR scaling has been hardware-confirmed at 0.1 Ω per register step.

When the **expected CR current is below approximately 0.15 A**, v2.3.0 shows a warning because practical regulation accuracy can decrease at very low current.

### CV — Constant Voltage

CV is **enabled in v2.3.0** for the validated KP184 profile.

Native CV has no separately configurable hardware current limit. KP184control therefore performs extra checks and monitors the startup phase at a higher rate. If excessive startup current is measured, LOAD is switched off.

> [!WARNING]
> Software monitoring is an additional safety layer and does not replace external or hardware current limiting where the test requires it.

## 5. START and safety checks

Before LOAD ON, KP184control checks among other things:

- that a valid live voltage is available;
- that the detected KP184 profile permits safe writes;
- voltage, current and power limits;
- expected load for the selected mode;
- entered battery and cutoff data.

During an active test, known current and power limits are monitored. Warnings are shown before a confirmed limit violation results in LOAD OFF.

## 6. Automatic stopping

Automatic cutoff is enabled by default. A test can also stop at:

- maximum test duration;
- maximum Ah;
- maximum Wh.

The stop reason is recorded in results and CSV data.

## 7. USB loss and resume

After multiple consecutive communication failures, an active test is paused and KP184control attempts to reconnect automatically. A test is **never resumed automatically**.

After a successful reconnect, the software first confirms a safe LOAD OFF state. The user can then resume the test manually.

## 8. Presets and history

v2.3.0 supports local test presets for reusable test settings. A compact local history of completed tests is also maintained. Full measurement samples remain stored in the CSV files.

## 9. CSV, graphs and reports

New CSV logs include, among other data:

- `APP;Naam;KP184control`
- `APP;Versie;v2.3.0`
- device and communication information;
- safety information;
- test settings;
- samples and events;
- final results and stop reason.

The application displays graphs for measured test data and can generate a PDF test report.

## 10. Mobile/web interface

KP184control v2.3.0 includes a local web interface that can be used from a phone or other device on the same network.

Available access modes:

- **Off** — web access disabled;
- **Read-only** — monitor without control commands;
- **Full control** — supported settings and remote START/STOP.

Control actions use a local action token. The token can be renewed from Windows; existing mobile pages must then be reloaded before control commands work again.

The current release is intended for **local-network access**, not as a public internet service.

## 11. Installing v2.3.0

Download from GitHub Releases:

- `KP184control-v2.3.0-windows-x64.zip`
- `KP184control-v2.3.0-windows-x64-SHA256.txt`

Extract the ZIP and start `KP184control.exe`.

The Windows x64 release is **self-contained**. A separate Microsoft .NET Desktop Runtime installation is not required for this package.

The SHA256 file can be used to verify the integrity of the downloaded ZIP.

## 12. Safety

Battery and electronic-load testing can involve high current, heat, arcing, BMS shutdowns and fire risk. Use appropriately rated wiring, connectors and fuses, verify polarity and limits before START, and do not leave a test unattended.

KP184control is a tool and does not replace correct electrical protection, supervision or evaluation of battery condition.

## 13. License and source code

KP184control is proprietary software and is not an open-source project. The public repository intentionally does not contain the C# source code. See `LICENSE.txt` for the license terms.
