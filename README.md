# KP184control

KP184control is a Windows desktop application for controlling and logging battery discharge tests with the **KUNKIN KP184 electronic load**.

> **Current release:** v2.1.0  
> **Source code:** proprietary / not publicly distributed.  
> **Copyright:** © 2026 Richard Uilenberg. All rights reserved.

## Features in v2.1.0

- Constant Current (**CC**) testing.
- CC soft-start enabled by default to reduce abrupt load application.
- Constant Power (**CP/CW**) testing.
- Constant Resistance (**CR**) testing.
- Live voltage, current and power display.
- Capacity (Ah) and energy (Wh) measurement.
- CSV test logging.
- Discharge-curve graphing and battery-capacity estimation.
- Automatic KP184 connection/profile detection used by the application.
- Dutch, English and German UI support.

### CV status

Constant Voltage (**CV**) is intentionally **not enabled in v2.1.0**. Experimental current-limited CV control was not considered sufficiently stable for release.

## Download

Download KP184control from the **Releases** section of this GitHub repository. For each release, download:

- `KP184control-v2.1.0-windows.zip`
- `KP184control-v2.1.0-windows-SHA256.txt`

Compare the SHA-256 checksum if you want to verify that the ZIP has not changed after release.

### Requirement

The v2.1.0 package requires **Windows x64** and the **Microsoft .NET 10 Desktop Runtime**.

## Basic use

1. Connect the KP184 to the PC and battery/test setup.
2. Start KP184control and connect to the detected KP184.
3. Enter the battery chemistry/capacity/full-charge information.
4. Select CC, CP or CR.
5. Choose conservative load settings and a safe cutoff voltage.
6. Start the test and supervise the hardware.
7. Use STOP immediately if the battery, BMS, wiring, connectors or load behave unexpectedly.

## Safety

Electronic-load and battery testing can involve high current, heat, arcing, damaged cells, BMS protection events and fire risk. KP184control does not replace proper electrical protection or supervision.

Use correctly rated wiring, connectors and fusing; verify polarity before enabling the load; stay within the limits of both the battery and the KP184; and do not leave a test unattended.

## Source code and license

KP184control is **not open-source software**. The public repository intentionally does not contain the C# source code. The software may be used under the terms in [LICENSE.txt](LICENSE.txt). Redistribution, resale, modified redistribution and claiming the software as your own are not permitted without written permission.

## Bugs and security issues

For normal bugs, use GitHub Issues. For a security issue that should not be public, use GitHub's **Security → Report a vulnerability** / private vulnerability reporting feature when available. See [SECURITY.md](SECURITY.md).
