---
name: project-log
description: 项目缺陷, 限制, 决策和交接
metadata:
  version: "1.0.0"
  lang: "zh-CN"
---

# 日志

## 多语言

**简体中文** | [English](en/LOG.md) | [Español](es/LOG.md)

## 文档

- 项目概览: [README](../README.md)

- 设计思路: [DESIGN](DESIGN.md)

- 项目状态: [LOG](LOG.md)
- 历史记录: [HISTORY](HISTORY.md)
- 变更日志: [CHANGELOG](CHANGELOG.md)

- 第三方声明: [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md)

## 记录

- [历史记录](HISTORY.md)
- [变更日志](CHANGELOG.md)

## 缺陷

- [ ] bp1a 版只做了离线验证 (javac, `check_dex`, 注入点命中, smali 回编一致), 真机结果没有记录.
- [ ] cp3a 版只做了离线验证; cp3a (Android 17) 底包线在真机上开机循环, 还没法上机.
- [ ] R8 内联导致的子类覆盖失效离线查不出, 只能上机看日志.

## 限制

- WEAK (Class 2) 生物识别只能用于解锁和弱校验, 不能授权支付, 也不能解锁要求强生物识别的 Keystore 凭据.
- 2D 识别, 防照片和视频攻击的能力不如结构光.
- 3 个部分只验证过一起启用的组合.

## 决策

| 日期 | 决策 | 状态 | 原因 |
| --- | --- | --- | --- |
| 2026-10-27 | 按系统 priv-app 身份放行 `USE_BIOMETRIC_INTERNAL` | 接受 | 所需的两个权限都是 signature 级, privapp 白名单给不了 |
| 2026-10-27 | SenseService 用 `BIND_FOREGROUND_SERVICE` 和 `BIND_INCLUDE_CAPABILITIES` 绑定 | 接受 | 避开 CAMERA 的 foreground-only 限制, 不需要常驻通知 |
| 2026-11-01 | 3 个补丁合并为一个补丁的 3 个部分 | 接受 | 功能上缺一不可, 分开选没有意义 |
| 2026-11-05 | 应用包名改为 `com.android.faceunlock` | 接受 | 避开把 `co.aospa.sense` 当作第三方 ROM 特征的检测 |
| 2026-11-05 | 按底包 ID 分目录 (ap3a, bp1a, cp3a) | 接受 | `services.jar` 的类签名和注入点随底包变化 |
| 2026-11-05 | 拆成独立公开仓库, 作为补丁的唯一来源, Mods 保留同步副本 | 接受 | 与 herald-core 和 tb321fu-tango-patch 一致; 通用教程留在 paranoidsense-gsi-port |
| 2026-11-05 | `FaceUnlockSettingsOverlay.apk` 不再入库, 教育页素材构建时从上游取 | 接受 | 该 APK 内含 Pixel 设置的专有素材 |

## 交接

- 当前分支: `main`.
- 已完成: 从 TB321FU-GSI-Mods `main` 提取 3 个底包的 `faceunlock/` 到 `patch/<底包 ID>/`; 设置入口 overlay 改为构建时从上游取素材 (重编产物除签名外与原 APK 内容相同); 按项目模板建立文档并翻译为英文和西班牙语.
- 检查: shell 脚本 `bash -n`, Python `py_compile` 通过; 文档链接检查通过; overlay 用 ap3a 的 framework-res 重编验证. 本仓库内没有可运行的构建, `services.jar` 和 `FaceUnlock.apk` 未在本仓库重新构建.
- 剩余步骤:
  - [ ] 在 bp1a 底包上真机验证并记录.
  - [ ] cp3a 底包线能开机后真机验证.
- 阻碍: cp3a 底包线开机循环.
- 临时项目规则: 未发现.
