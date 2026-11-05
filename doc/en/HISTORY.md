---
name: project-history-en
description: Historical Records
metadata:
  version: "1.0.0"
  lang: "en"
---

# Historical Records

## Multi-language

[简体中文](../HISTORY.md) | **English** | [Español](../es/HISTORY.md)

## Documentation

- Project overview: [README](README.md)

- Design rationale: [DESIGN](DESIGN.md)

- Project status: [LOG](LOG.md)
- Historical records: [HISTORY](HISTORY.md)
- Version changelog: [CHANGELOG](CHANGELOG.md)

- Third-party notices: [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md)

## Historical Records

Debugging process on TB321FU (2026-10-27, ap3a base image):

| Version | Result | Cause and fix |
| --- | --- | --- |
| v1 | system_server crash loop | d8 used `--no-desugaring`, causing `IncompatibleClassChangeError`; removed that option |
| v2 | Enrollment page crashes | `USE_BIOMETRIC_INTERNAL` is signature-level; changed 57 checks |
| v3 | Enrollment page still crashes | `MANAGE_BIOMETRIC` is also signature-level; changed to grant by identity |
| v4 | Unlocks the first few times, then the camera is denied | CAMERA is foreground-only; changed the binding flags |
| v5 | All passed | |

Later developments:

- 2026-10-27: The general part was split out as [paranoidsense-gsi-port](https://github.com/CharlesGool/paranoidsense-gsi-port).
- 2026-11-01: In TB321FU-GSI-Mods, 3 patches were merged into the 3 parts of `faceunlock`.
- 2026-11-05: Changed the package name to `com.android.faceunlock`; split into ap3a, bp1a (Android 15 official GSI) and cp3a (Android 17) versions by base image ID; this repository was split out of Mods.
