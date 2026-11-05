---
name: project-log-en
description: Project bugs, limitations, decisions, and handoff
metadata:
  version: "1.0.0"
  lang: "en"
---

# Log

## Multi-language

[简体中文](../LOG.md) | **English** | [Español](../es/LOG.md)

## Documentation

- Project overview: [README](README.md)

- Design rationale: [DESIGN](DESIGN.md)

- Project status: [LOG](LOG.md)
- Historical records: [HISTORY](HISTORY.md)
- Version changelog: [CHANGELOG](CHANGELOG.md)

- Third-party notices: [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md)

## Records

- [History](HISTORY.md)
- [Changelog](CHANGELOG.md)

## Bugs

- [ ] The bp1a version has only been verified offline (javac, `check_dex`, injection points matched, smali rebuild consistent); no real-device result is recorded.
- [ ] The cp3a version has only been verified offline; the cp3a (Android 17) base line boot-loops on a real device, so it cannot be tested on a device yet.
- [ ] Subclass overrides broken by R8 inlining cannot be detected offline; they can only be found in the logs on a device.

## Limitations

- WEAK (Class 2) biometrics can only be used for unlocking and weak verification; they cannot authorize payments or unlock Keystore credentials that require strong biometrics.
- 2D recognition is less resistant to photo and video attacks than structured light.
- The 3 parts have only been verified when enabled together.

## Decisions

| Date | Decision | Status | Reason |
| --- | --- | --- | --- |
| 2026-10-27 | Grant `USE_BIOMETRIC_INTERNAL` based on system priv-app identity | Accepted | Both permissions required are signature-level, which the privapp allowlist cannot grant |
| 2026-10-27 | SenseService binds with `BIND_FOREGROUND_SERVICE` and `BIND_INCLUDE_CAPABILITIES` | Accepted | Avoids the foreground-only restriction of CAMERA, and needs no persistent notification |
| 2026-11-01 | Merge 3 patches into 3 parts of one patch | Accepted | The parts are functionally indispensable, so selecting them separately is meaningless |
| 2026-11-05 | Change the app package name to `com.android.faceunlock` | Accepted | Avoids detection that treats `co.aospa.sense` as a third-party ROM signature |
| 2026-11-05 | Split directories by base image ID (ap3a, bp1a, cp3a) | Accepted | The class signatures and injection points of `services.jar` vary with the base image |
| 2026-11-05 | Split into a standalone public repository as the only source of the patch; Mods keeps a synchronized copy | Accepted | Consistent with herald-core and tb321fu-tango-patch; the general tutorial stays in paranoidsense-gsi-port |
| 2026-11-05 | `FaceUnlockSettingsOverlay.apk` is no longer committed; education page assets are fetched from upstream at build time | Accepted | That APK contains proprietary assets from Pixel Settings |

## Handoff

The current handoff is maintained in the [Simplified Chinese LOG.md](../LOG.md#交接).
