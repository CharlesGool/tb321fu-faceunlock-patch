# tb321fu-faceunlock-patch

<!-- Uncomment the next line when the project has an icon -->
<!-- <img align="right" src="../resources/logo.png" width="160" alt="tb321fu-faceunlock-patch"> -->

[简体中文](../../README.md) | **English** | [Español](../es/README.md)

The Legion Y700 Gen 3 (TB321FU) has no face unlock after flashing a Pixel Tablet GSI; this project is a TB321FU-GSI-Mods patch that builds ParanoidSense face unlock into the system image, with no root and no platform signature required.

[![License](https://img.shields.io/badge/License-Apache--2.0-orange)](../../LICENSE)

## Documentation

- Project overview: [README](README.md)

- Design rationale: [DESIGN](DESIGN.md)

- Project status: [LOG](LOG.md)
- Historical records: [HISTORY](HISTORY.md)
- Version changelog: [CHANGELOG](CHANGELOG.md)

- Third-party notices: [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md)

## Introduction

- 2D face recognition with the front camera, using the Megvii SDK, at Android biometric class WEAK (Class 2). The app package name is `com.android.faceunlock`.
- The `faceunlock` patch has 3 parts: the Sense framework and permission exemption in `services.jar`, the face app and its permission files, and the settings entry overlay.
- How `services.jar` is modified depends on the base image, so each base image has its own directory, which runs only when the base image ID matches:

| Directory | Base image | Status |
| --- | --- | --- |
| [patch/tangorpro_ap3a.241005.015](../../patch/tangorpro_ap3a.241005.015/README.md) | Pixel Tablet Mystic GSI, Android 15 (AP3A.241005.015) | Verified on a real device |
| [patch/tangorpro_bp1a.250505.005](../../patch/tangorpro_bp1a.250505.005/README.md) | Pixel Tablet official GSI, Android 15 (BP1A.250505.005) | Verified offline; real-device result not recorded |
| [patch/tangorpro_cp3a.260905.009](../../patch/tangorpro_cp3a.260905.009/README.md) | Pixel Tablet GSI, Android 17 (CP3A.260905.009) | Verified offline; this base line cannot boot yet |

For a device-independent porting method and general tools (KernelSU / Magisk modules), see [paranoidsense-gsi-port](https://github.com/CharlesGool/paranoidsense-gsi-port); this repository is its reference implementation on TB321FU.

This repository is the only source of the patch. The `patches/required/tangorpro/<base image>/faceunlock/` directory of [TB321FU-GSI-Mods](https://github.com/CharlesGool/TB321FU-GSI-Mods) is a synchronized copy.

## Requirements

- Legion Y700 Gen 3 (TB321FU), one of the base images in the table above
- Build the system image with [TB321FU-GSI-Mods](https://github.com/CharlesGool/TB321FU-GSI-Mods)
- Build machine: Linux, JDK 17, python3, git, unzip, debugfs (e2fsprogs), and:
  - [dex2jar](https://github.com/ThexXTURBOXx/dex2jar) 2.4.38, r8 8.5.35 (includes d8), [Apktool](https://github.com/iBotPeaches/Apktool) 2.12.1
  - Android build-tools 35 (aapt2, zipalign, apksigner), kotlinc 2.0.21
- Network: before building the APKs, the Megvii libraries, models and settings education page assets must be fetched from [PixelOS ParanoidSense](https://github.com/PixelOS-AOSP/packages_apps_ParanoidSense). These files and the APKs containing them **MUST NOT** be committed to the repository

## Install

This project is not installed on its own; TB321FU-GSI-Mods bakes it into the system image at build time.

### Quick Install

TB321FU-GSI-Mods already ships a synchronized copy. First build the two APKs locally (ap3a as an example; for other base images, switch the directory), then build the image:

```bash
cd <TB321FU-GSI-Mods>/patches/required/tangorpro/tangorpro_ap3a.241005.015/faceunlock/parts
bash 002.app/src/fetch_upstream.sh
bash 002.app/src/build.sh <base image system directory> 002.app/files
bash 003.settings-entry/src/build.sh <base image system directory> 003.settings-entry/files
cd <TB321FU-GSI-Mods>
make 9008 NOTE="faceunlock"
```

`services.jar` does not need to be compiled in advance; it is generated on the fly from the current base image at build time.

### Normal Install

Clone this repository next to Mods (same parent directory), sync the copy in Mods, then build the APKs and the image as in Quick Install. For another location, set `FACEUNLOCK_PATCH_DIR`:

```bash
cd <TB321FU-GSI-Mods>
make sync-faceunlock
```

Syncing does not delete the locally built APKs, the fetched upstream or the signing keys.

## Usage

### Parameters

Set the parameters in `devices/<device>/conf.d/faceunlock.conf` of Mods; an environment variable with the same name takes precedence. The variables are also described in [.env.example](../../.env.example).

| Parameter | Used by | Meaning |
| --- | --- | --- |
| `APKTOOL_JAR` | services-hooks | Apktool 2.12.1 jar (common Mods parameter) |
| `FU_D2J` | services-hooks | `d2j-dex2jar.sh` of dex2jar 2.4.38 |
| `FU_R8` | services-hooks | r8 8.5.35 jar; another version works the same, but the dex bytes differ |
| `FU_JAVA_HOME` | services-hooks | JDK 17, default `/usr/lib/jvm/java-17-openjdk-amd64` |
| `D2J`, `R8`, `APKTOOL`, `BT`, `KOTLIN_HOME` | APK build | The same tools as above, the build-tools 35 directory, the kotlinc 2.0.21 directory |
| `FACEUNLOCK_KEYSTORE` | APK build | Signing keystore (alias `faceunlock`, password `android`); generated automatically when not set |
| `PARANOIDSENSE_DIR` | overlay build | ParanoidSense upstream directory, default `002.app/src/third_party/ParanoidSense` |

### Usage and Checks

Settings → Security & privacy → Device unlock → Face unlock → enter PIN → follow the prompts to enroll. To check:

```bash
adb shell getprop ro.face.sense_service            # true
adb shell dumpsys face                             # has SenseProvider
adb shell cmd overlay list | grep -i faceunlock    # [x]
```

For how to locate the injection points when switching base images, see `parts/001.services-hooks/README.md` in each base image directory.

## Upgrade

After switching the base image (including a new version of the same base line), just rebuild; `services.jar` is generated on the fly from the new base image. When the base image ID changes, create a new directory for it; for how to adapt, see the porting tutorial of [paranoidsense-gsi-port](https://github.com/CharlesGool/paranoidsense-gsi-port). Enrolled face data is stored in the app data and is kept as long as the data is not cleared.

## Uninstall

### Quick Uninstall

Delete the face model in Settings. The patch remains in the image but face unlock is no longer used.

### Full Uninstall

First delete the face model in Settings, then remove this patch in Mods, rebuild and reflash:

```bash
make 9008 WITHOUT=faceunlock NOTE="不带人脸"
```

## Acknowledgements

- [PixelOS-AOSP/packages_apps_ParanoidSense](https://github.com/PixelOS-AOSP/packages_apps_ParanoidSense): app source, Megvii libraries and models
- The `0009-feat-Add-Face-Unlock-with-ParanoidSense` patch of [KLC_OS](https://github.com/LoggingNewMemory/KLC_OS): reference for the framework part

## License

This project is licensed under the Apache License 2.0, SPDX identifier `Apache-2.0`; the full text is in [LICENSE](../../LICENSE). Megvii's libraries and models, and the settings education page assets, are not distributed with this project; see [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md).
