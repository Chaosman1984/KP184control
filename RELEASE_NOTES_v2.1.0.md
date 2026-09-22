# KP184control v2.1.0

KP184control v2.1.0 is the first release baseline in which the tested KP184 modes **CC, CP/CW and CR** are treated as supported release functions.

## Highlights

- CC with soft-start enabled by default.
- CP/CW support.
- CR support with hardware-verified register scaling.
- Battery information, live measurements, Ah/Wh logging and discharge graphs.
- CV is deliberately disabled in this release.

## Download

Attach these two files to the GitHub Release after running the private release build script:

- `KP184control-v2.1.0-windows.zip`
- `KP184control-v2.1.0-windows-SHA256.txt`

## Runtime

Windows x64 with Microsoft .NET 10 Desktop Runtime.

## Safety

Battery discharge testing must be supervised. Start with conservative current/power settings, verify polarity and protection, and stop if the battery/BMS/load behaves unexpectedly.

---
Copyright © 2026 Richard Uilenberg. All rights reserved.
