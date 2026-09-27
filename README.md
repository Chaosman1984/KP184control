# KP184control

KP184control is a Windows desktop application for controlling and logging battery discharge tests with the **KUNKIN KP184 electronic load**.

> **Current release:** v2.2.0  
> **Interface languages:** English • Deutsch • Nederlands  
> **Source code:** proprietary / not publicly distributed.  
> **Copyright:** © 2026 Richard Uilenberg. All rights reserved.

## Documentation

- [Nederlandse handleiding](HANDLEIDING_NL.md)
- [English user manual](USER_MANUAL_EN.md)
- [Deutsches Benutzerhandbuch](BENUTZERHANDBUCH_DE.md)

## Device compatibility

**KP184control v2.2.0 is hardware-validated for the KUNKIN KP184 with model ID `0x0730`.**

The application performs a compatibility/read-write check when connecting. Unknown device profiles may be read where possible, but KP184control will not enable the load unless a safe write profile has been confirmed.

## Main features

- Constant Current (**CC**) with software soft-start enabled by default.
- Constant Power (**CP/CW**).
- Constant Resistance (**CR**).
- Automatic KP184 communication/profile detection and compatibility verification.
- Live voltage, current and power; Ah and Wh measurement.
- Automatic cutoff plus optional Ah, Wh and duration stop conditions.
- Communication watchdog and safe reconnect/pause workflow.
- Unexpected current-loss detection.
- Native KP184 slew-rate read/write.
- Runtime current/power watchdog and pre-start safety checks.
- Automatic graph scaling.
- Extended CSV logging.
- Battery Health and remaining-capacity estimation after a valid automatic cutoff.
- Single-page PDF test report with real test graphs.
- Dutch, English and German UI/report support.

### CV status

Constant Voltage (**CV**) is intentionally **not enabled in v2.2.0**.

## Download

For v2.2.0 download:

- `KP184control-v2.2.0-windows.zip`
- `KP184control-v2.2.0-windows-SHA256.txt`

### Requirement

Windows x64 with the **Microsoft .NET 10 Desktop Runtime**.

## Safety

Electronic-load and battery testing can involve high current, heat, arcing, damaged cells, BMS protection events and fire risk. KP184control does not replace proper electrical protection or supervision.

Use correctly rated wiring, connectors and fusing; verify polarity before enabling the load; stay within the limits of both the battery and the KP184; and do not leave a test unattended.

## Source code and license

KP184control is **not open-source software**. The public repository intentionally does not contain the C# source code. The software may be used under the terms in [LICENSE.txt](LICENSE.txt).

## Bugs and security issues

For normal bugs, use GitHub Issues. For security issues, use GitHub private vulnerability reporting where available. See [SECURITY.md](SECURITY.md).
