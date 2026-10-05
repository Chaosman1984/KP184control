# Changelog

## 2.3.0 — 2026-10-05

### Added
- Constant Voltage (CV) support for the tested KP184 profile.
- Extra CV startup-current monitoring and safety warning.
- Low-current CR warning below approximately 0.15 A expected current.
- Local test presets.
- Local test history.
- Local mobile/web interface for monitoring and control on the same network.
- Mobile access modes: off, read-only and full control.
- Remote START/STOP support.
- Application version metadata in new CSV logs.

### Improved
- Disconnect/reconnect workflow and manual resume after reconnect.
- Pre-start safety checks and runtime safety monitoring.
- Release hardening for Windows and local web access.
- Documentation for CV, CR low-current behaviour and remote control.

### Tested for this release
- CC
- CP/CW
- CR
- CV
- Automatic cutoff
- USB disconnect/reconnect
- Manual resume after reconnect
- Presets
- Remote START/STOP

### Packaging
- Windows x64 Release build.
- Self-contained deployment; separate .NET Desktop Runtime installation is not required.
- Single-file publish configuration.
- SHA256 checksum supplied with the release ZIP.

## 2.2.0 — 2026-09-27

### Added
- Compatibility Check with safe read/write confirmation for the known KP184 profile.
- Extra automatic stop conditions for Ah, Wh and test duration.
- Extended test summary and stable-load averages.
- Native slew-rate read/write controls.
- Communication watchdog, pause/reconnect workflow and interruption counters.
- Unexpected current-loss/BMS interruption detection.
- Test name and free-form test note.
- Remembered safe user settings.
- Expanded CSV metadata, event logging and final results.
- Runtime current/power safety watchdog.
- Pre-start safety check before LOAD ON.
- Single-page PDF test report with four graphs.
- Automatic report language matching the selected UI language (NL/EN/DE).

### Improved
- Settings become editable again after STOP.
- Battery Health is only calculated as measured health after automatic cutoff.
- Result panel layout and translated dynamic status text.
- Automatic X/Y scaling for all graphs.
- Software soft-start remains enabled by default and is kept separate from native slew-rate control.

### Supported release modes
- CC
- CP/CW
- CR

### Not enabled
- CV remains intentionally disabled.

## 2.1.0 — 2026-09-22

### Added / completed
- Hardware-tested CR mode support.
- Hardware-confirmed KP184 load modes: CC = 1, CR = 2, CP/CW = 3.
- CR resistance scaling for the tested KP184: 0.1 Ω per register step.
- CC soft-start retained and enabled by default.
- UI alignment cleanup for the battery full-charge voltage field.

### Supported release modes
- CC
- CP/CW
- CR

### Not enabled
- CV is intentionally disabled in this release after experimental CVL testing showed unstable behaviour with the test battery/BMS setup.

### Packaging / protection
- Release version metadata set to 2.1.0.
- Proprietary license included.
- Protected release process uses Obfuscar.
- Public release excludes source code, PDB debug symbols and Obfuscar mapping files.
