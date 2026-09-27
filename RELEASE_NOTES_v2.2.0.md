# KP184control v2.2.0

KP184control v2.2.0 is a major safety, reliability, logging and reporting update for the KUNKIN KP184 electronic load.

## Highlights

- Automatic compatibility/read-write verification for the confirmed KP184 profile.
- Extra automatic stop conditions: capacity (Ah), energy (Wh) and test duration.
- Extended test summary with start/end/minimum voltage, current, power and stop reason.
- Native KP184 slew-rate read/write support.
- Communication-loss watchdog with safe pause/reconnect workflow.
- Unexpected current-loss detection for events such as BMS/load interruption.
- Test name and free-form notes.
- Remembered safe application settings.
- Expanded CSV logging and stable-load averages.
- Automatic graph scaling for voltage, current, capacity and energy.
- Runtime current/power safety watchdog.
- Pre-start safety check before LOAD ON.
- Single-page PDF test report with actual graphs and automatic NL/EN/DE language selection.
- Improved Battery Health handling: measured health is only reported after an automatic cutoff.

## Supported load modes

- CC — Constant Current
- CP/CW — Constant Power
- CR — Constant Resistance

CV remains intentionally disabled.

## Tested device profile

- KUNKIN KP184
- Model ID `0x0730`
- 150 V / 40 A / 400 W

Unknown profiles are not allowed to enable LOAD unless a safe write profile has been confirmed.

## Download

- `KP184control-v2.2.0-windows.zip`
- `KP184control-v2.2.0-windows-SHA256.txt`

## Runtime

Windows x64 with Microsoft .NET 10 Desktop Runtime.

## Safety

Battery and electronic-load testing can involve high current, heat, arcing, BMS protection events and fire risk. Use correctly rated wiring, connectors and fusing, verify polarity, stay within the limits of the battery/BMS/KP184, and supervise tests.

---

Copyright © 2026 Richard Uilenberg. All rights reserved.
