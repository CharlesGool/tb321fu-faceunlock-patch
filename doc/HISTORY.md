---
name: project-history
description: 历史记录
metadata:
  version: "1.0.0"
  lang: "zh-CN"
---

# 历史记录

## 多语言

**简体中文** | [English](en/HISTORY.md) | [Español](es/HISTORY.md)

## 文档

- 项目概览: [README](../README.md)

- 设计思路: [DESIGN](DESIGN.md)

- 项目状态: [LOG](LOG.md)
- 历史记录: [HISTORY](HISTORY.md)
- 变更日志: [CHANGELOG](CHANGELOG.md)

- 第三方声明: [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md)

## 历史记录

在 TB321FU 上调试的过程 (2026-10-27, ap3a 底包):

| 版本 | 结果 | 原因和修复 |
| --- | --- | --- |
| v1 | system_server 循环崩溃 | d8 用了 `--no-desugaring`, `IncompatibleClassChangeError`; 去掉该参数 |
| v2 | 录入页崩溃 | `USE_BIOMETRIC_INTERNAL` 是 signature 级; 改 57 处检查 |
| v3 | 录入页仍崩溃 | `MANAGE_BIOMETRIC` 也是 signature 级; 改为按身份放行 |
| v4 | 头几次能解锁, 之后相机被拒 | CAMERA foreground-only; 改绑定参数 |
| v5 | 全部通过 | |

之后的演变:

- 2026-10-27: 通用部分拆成 [paranoidsense-gsi-port](https://github.com/CharlesGool/paranoidsense-gsi-port).
- 2026-11-01: 在 TB321FU-GSI-Mods 中由 3 个补丁合并为 `faceunlock` 的 3 个部分.
- 2026-11-05: 改包名为 `com.android.faceunlock`; 按底包 ID 分出 ap3a, bp1a (Android 15 官方 GSI) 和 cp3a (Android 17) 版本; 从 Mods 拆出本仓库.
