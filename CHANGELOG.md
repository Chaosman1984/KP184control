# Changelog

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
