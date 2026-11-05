# tb321fu-faceunlock-patch

**简体中文** | [English](doc/en/README.md) | [Español](doc/es/README.md)

拯救者 Y700 三代 (TB321FU) 刷 Pixel Tablet GSI 后没有人脸解锁; 本项目是把 ParanoidSense 人脸解锁做进 system 镜像的 TB321FU-GSI-Mods 补丁, 不需要 root, 也不需要 platform 签名.

[![License](https://img.shields.io/badge/License-Apache--2.0-orange)](LICENSE)

## 文档

- 项目概览: [README](README.md)

- 设计思路: [DESIGN](doc/DESIGN.md)

- 项目状态: [LOG](doc/LOG.md)
- 历史记录: [HISTORY](doc/HISTORY.md)
- 变更日志: [CHANGELOG](doc/CHANGELOG.md)

- 第三方声明: [THIRD_PARTY_NOTICES](doc/THIRD_PARTY_NOTICES.md)

## 简介

- 前置摄像头 2D 人脸识别, 用旷视 (Megvii) SDK, Android 生物识别等级 WEAK (Class 2). 应用包名是 `com.android.faceunlock`.
- 补丁 `faceunlock` 由 3 个部分组成: `services.jar` 的 Sense 框架和权限放行, 人脸应用和权限文件, 设置入口 overlay.
- `services.jar` 的改法依赖底包, 所以每个底包一个目录, 只在底包 ID 匹配时执行:

| 目录 | 底包 | 状态 |
| --- | --- | --- |
| [patch/tangorpro_ap3a.241005.015](patch/tangorpro_ap3a.241005.015/README.md) | Pixel Tablet Mystic GSI, Android 15 (AP3A.241005.015) | 真机验证通过 |
| [patch/tangorpro_bp1a.250505.005](patch/tangorpro_bp1a.250505.005/README.md) | Pixel Tablet 官方 GSI, Android 15 (BP1A.250505.005) | 离线验证通过, 真机结果未记录 |
| [patch/tangorpro_cp3a.260905.009](patch/tangorpro_cp3a.260905.009/README.md) | Pixel Tablet GSI, Android 17 (CP3A.260905.009) | 离线验证通过; 该底包线尚不能开机 |

不针对具体设备的移植方法和通用工具 (KernelSU / Magisk 模块) 见 [paranoidsense-gsi-port](https://github.com/CharlesGool/paranoidsense-gsi-port), 本仓库是它在 TB321FU 上的参考实现.

本仓库是该补丁的唯一来源. [TB321FU-GSI-Mods](https://github.com/CharlesGool/TB321FU-GSI-Mods) 的 `patches/required/tangorpro/<底包>/faceunlock/` 是同步副本.

## 要求

- 拯救者 Y700 三代 (TB321FU), 上表中的一个底包
- 用 [TB321FU-GSI-Mods](https://github.com/CharlesGool/TB321FU-GSI-Mods) 构建 system 镜像
- 构建机: Linux, JDK 17, python3, git, unzip, debugfs (e2fsprogs), 以及:
  - [dex2jar](https://github.com/ThexXTURBOXx/dex2jar) 2.4.38, r8 8.5.35 (含 d8), [Apktool](https://github.com/iBotPeaches/Apktool) 2.12.1
  - Android build-tools 35 (aapt2, zipalign, apksigner), kotlinc 2.0.21
- 网络: 构建 APK 前要从 [PixelOS ParanoidSense](https://github.com/PixelOS-AOSP/packages_apps_ParanoidSense) 拉取旷视的库, 模型和设置教育页素材. 这些文件和含有它们的 APK **禁止**提交到仓库

## 安装

本项目不单独安装, 由 TB321FU-GSI-Mods 在构建时打进 system 镜像.

### 快速安装

TB321FU-GSI-Mods 已带同步副本. 先在本地编出两个 APK (以 ap3a 为例, 其他底包换目录), 再构建:

```bash
cd <TB321FU-GSI-Mods>/patches/required/tangorpro/tangorpro_ap3a.241005.015/faceunlock/parts
bash 002.app/src/fetch_upstream.sh
bash 002.app/src/build.sh <底包 system 目录> 002.app/files
bash 003.settings-entry/src/build.sh <底包 system 目录> 003.settings-entry/files
cd <TB321FU-GSI-Mods>
make 9008 NOTE="faceunlock"
```

`services.jar` 不用预先编译, 构建时从当前底包现场生成.

### 常规安装

把本仓库克隆到 Mods 旁边 (同一父目录), 在 Mods 中同步副本后再按快速安装编 APK 和构建. 其他位置用 `FACEUNLOCK_PATCH_DIR` 指定:

```bash
cd <TB321FU-GSI-Mods>
make sync-faceunlock
```

同步不会删除本地编好的 APK, 拉下的上游和签名密钥.

## 指南

### 参数

参数在 Mods 的 `devices/<设备>/conf.d/faceunlock.conf` 中设置, 同名环境变量优先. 变量说明也见 [.env.example](.env.example).

| 参数 | 用于 | 含义 |
| --- | --- | --- |
| `APKTOOL_JAR` | services-hooks | Apktool 2.12.1 的 jar (Mods 公共参数) |
| `FU_D2J` | services-hooks | dex2jar 2.4.38 的 `d2j-dex2jar.sh` |
| `FU_R8` | services-hooks | r8 8.5.35 的 jar; 换版本功能相同, dex 字节不同 |
| `FU_JAVA_HOME` | services-hooks | JDK 17, 默认 `/usr/lib/jvm/java-17-openjdk-amd64` |
| `D2J`, `R8`, `APKTOOL`, `BT`, `KOTLIN_HOME` | 编 APK | 同上的工具, build-tools 35 目录, kotlinc 2.0.21 目录 |
| `FACEUNLOCK_KEYSTORE` | 编 APK | 签名 keystore (别名 `faceunlock`, 口令 `android`); 不设时自动生成 |
| `PARANOIDSENSE_DIR` | 编 overlay | ParanoidSense 上游目录, 默认 `002.app/src/third_party/ParanoidSense` |

### 使用和检查

设置 → 安全与隐私 → 设备解锁 → 人脸解锁 → 输入 PIN → 按提示录入. 检查:

```bash
adb shell getprop ro.face.sense_service            # true
adb shell dumpsys face                             # 有 SenseProvider
adb shell cmd overlay list | grep -i faceunlock    # [x]
```

换底包时的定位方法见各底包目录的 `parts/001.services-hooks/README.md`.

## 升级

换底包 (包括同一底包线的新版本) 后重新构建即可, `services.jar` 从新底包现场生成. 底包 ID 变了时要新建对应目录, 改法见 [paranoidsense-gsi-port](https://github.com/CharlesGool/paranoidsense-gsi-port) 的移植教程. 已录入的人脸数据存在应用数据里, 不清数据就保留.

## 卸载

### 快速卸载

在设置里删除人脸模型. 补丁仍在镜像里, 只是不再使用人脸解锁.

### 完全卸载

先在设置里删除人脸模型, 再在 Mods 中去掉本补丁重新构建并刷机:

```bash
make 9008 WITHOUT=faceunlock NOTE="不带人脸"
```

## 致谢

- [PixelOS-AOSP/packages_apps_ParanoidSense](https://github.com/PixelOS-AOSP/packages_apps_ParanoidSense): 应用源码, 旷视库和模型
- [KLC_OS](https://github.com/LoggingNewMemory/KLC_OS) 的 `0009-feat-Add-Face-Unlock-with-ParanoidSense` 补丁: 框架部分的参考

## 许可证

本项目以 Apache License 2.0 授权, SPDX 标识符 `Apache-2.0`, 全文见 [LICENSE](LICENSE). 旷视的库和模型, 以及设置教育页素材不随本项目分发, 见 [THIRD_PARTY_NOTICES](doc/THIRD_PARTY_NOTICES.md).
