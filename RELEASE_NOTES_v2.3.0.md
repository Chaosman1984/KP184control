# KP184control v2.3.0

KP184control v2.3.0 is the current stable Windows release.

## Highlights

- Support for CC, CP/CW, CR and CV operating modes.
- Automatic cutoff support.
- Improved pre-start safety checks.
- CV startup current protection and warning.
- CR warning for expected currents below approximately 0.15 A.
- USB disconnect detection and automatic reconnect handling.
- Paused tests can be resumed manually after reconnect.
- Test presets.
- Local test history.
- Improved CSV logging with application version metadata.
- Local mobile/web interface.
- Remote START/STOP support.
- Read-only and full-control mobile access modes.
- Windows x64 self-contained single-file release.

## Tested

The following functions have been tested on the development KP184:

- CC
- CP/CW
- CR
- CV
- Automatic cutoff
- USB disconnect/reconnect
- Manual resume after reconnect
- Presets
- Remote START/STOP

## Windows

This release is built for Windows x64 as a self-contained .NET 10 Windows application. The .NET runtime does not need to be installed separately.

## Download

Release assets:

- `KP184control-v2.3.0-windows-x64.zip`
- `KP184control-v2.3.0-windows-x64-SHA256.txt`

SHA256 of the Windows ZIP:

`F4A143F185722AA454D80B451DFC31CFC033DB0C62AD36EDA1FCB1094072907C`

## Safety

Electronic-load and battery testing can involve high current, heat and battery protection events. Use correctly rated wiring, connectors and fusing, verify polarity, remain within the limits of the battery and KP184, and do not leave tests unattended.

Native CV has no separately configurable hardware current limit. KP184control adds software startup-current monitoring, but this does not replace external/hardware current limiting where required.
