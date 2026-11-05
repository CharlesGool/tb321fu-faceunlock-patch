---
name: project-third-party-notices-en
description: Third-party attribution and compliance notices
metadata:
  version: "1.0.0"
  lang: "en"
---

# Third-Party Notices

## Multi-language

[简体中文](../THIRD_PARTY_NOTICES.md) | **English** | [Español](../es/THIRD_PARTY_NOTICES.md)

## Documentation

- Project overview: [README](README.md)

- Design rationale: [DESIGN](DESIGN.md)

- Project status: [LOG](LOG.md)
- Historical records: [HISTORY](HISTORY.md)
- Version changelog: [CHANGELOG](CHANGELOG.md)

- Third-party notices: [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md)

## Third-Party Notices

### Components

| Component | Version or hash | Upstream URL | License | Usage | Copyright holder | Release obligations | Verified on |
| --- | --- | --- | --- | --- | --- | --- | --- |
| ParanoidSense | `a500478bbb0e2933276d57796d9b410f0f8e90f8` (`fifteen` branch) | https://github.com/PixelOS-AOSP/packages_apps_ParanoidSense | Apache-2.0 | `parts/002.app/src/app/` is modified from it | Paranoid Android, PixelOS | Keep the copyright notice, include the license, mark the modifications | 2026-11-05 |
| Megvii face SDK (4 so files including `libmegface.so`, `vendorImplPrebuilt.jar`, `model_file`) | Same as above | Same as above (`lib/`, `res/raw/`) | Unknown (not separately stated upstream) | Not in the repository; fetched at build time and built into the locally built `FaceUnlock.apk` | Megvii | Undetermined; **MUST NOT** enter the repository | 2026-11-05 |
| Settings education page assets (`face_education_lottie.json`, `face_settings.mp4`) | Same as above, SHA-256 `7788f9ec...`, `f93fe186...` | Same as above (`overlay/res/raw/`) | Proprietary (Google Pixel Settings resources, not separately stated upstream) | Not in the repository; fetched at build time and built into the locally built `FaceUnlockSettingsOverlay.apk` | Google | **MUST NOT** enter the repository | 2026-11-05 |
| KLC_OS `0009-feat-Add-Face-Unlock-with-ParanoidSense` patch | Commit `f78e9bc3` inside the patch | https://github.com/LoggingNewMemory/KLC_OS | Not stated by the repository; source file headers are Apache-2.0 | 15 classes in `parts/001.services-hooks/src/java/` are adapted from it | The Android Open Source Project, Paranoid Android | Keep the copyright notices in the source file headers | 2026-11-05 |
| AOSP frameworks/base | Android 15, Android 17 | https://android.googlesource.com/platform/frameworks/base | Apache-2.0 | The classes in `parts/001.services-hooks/src/java/` are derived from AOSP face code | The Android Open Source Project | Keep the copyright notice | 2026-11-05 |

### License Texts

Full text of the Apache License 2.0: [LICENSE](../../LICENSE).

### Source Code Offer

This repository does not distribute binary files. Megvii's libraries and models are provided upstream in binary form, with no source code.

### Compliance Review

There are no copyleft components. The redistribution terms of the Megvii SDK and the settings education page assets cannot be determined, so this repository provides only the fetch scripts; the APKs and flashing packages that users build themselves contain these files, and users **MUST** confirm the authorization themselves before distributing them.
