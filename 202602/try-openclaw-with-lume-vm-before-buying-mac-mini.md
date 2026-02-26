---
title: "想买一个 Mac Mini 来养小龙虾(OpenClaw)？你可以先等等"
date: 2026-02-26
tags:
  - openclaw
  - vm
  - macos
  - lume
---

OpenClaw 最近很火，很多人在讨论要不要专门买台 Mac Mini 来跑它。先别急，
有个更省事的方案——用 [Lume](https://cua.ai/docs/lume/guide/getting-started/introduction)
在你现有的 Mac 上跑一台 macOS 虚拟机，体验相当不错。

在聊 Lume 之前，说说为什么其他选项不够好：

- **便宜的 VPS**：通常只有 2GB 内存，OpenClaw 跑起来很卡，体验差。
- **UTM**：能跑，但每次都得从图形界面启动，界面偶尔还会卡住；
  创建 macOS 虚拟机也需要手动做不少配置，麻烦。

## 什么是 Lume

Lume 是一个基于 Apple 官方 Virtualization.framework 的 macOS/Linux
虚拟机管理工具，专为 Apple Silicon（M1 及以上）设计。

几个关键特点：

- **接近原生的性能**：直接使用硬件虚拟化，不走 QEMU 等软件模拟层
- **CLI 优先，没有 GUI**：通过命令行或 REST API 管理，headless 场景是第一公民
- **轻量**：不依赖额外的 Hypervisor，本身资源占用小
- **无人值守安装**：`--unattended` 参数全自动完成 macOS 安装，不需要手动点选

## 安装 Lume

```bash
brew install lume
```

硬盘空间有限的话，可以把虚拟机存到外部硬盘：

```bash
# 添加外部存储位置
lume config storage add external /Volumes/HS-C2000/lume
# 设为新虚拟机的默认存储
lume config storage default external
```

## 创建虚拟机

完整流程参考 [Lume 官方 Quickstart 文档](https://cua.ai/docs/lume/guide/getting-started/quickstart)，
包括 ipsw 镜像的下载方式。准备好镜像后，一条命令完成安装：

```bash
lume create vm-oc \
  --os macos \
  --ipsw /Volumes/HS-C2000/Downloads/UniversalMac_26.2_25C56_Restore.ipsw \
  --storage external \
  --unattended tahoe
```

`--unattended` 会全自动完成系统安装，静等结束就好。

## 启动并连接

安装完成后，headless 模式后台启动：

```bash
lume run vm-oc --no-display > /dev/null 2>&1 &
```

命令执行后，系统会自动弹出「屏幕共享」连接到虚拟机，就可以像操作一台普通
Mac 一样，在里面安装 OpenClaw 了。OpenClaw 的安装直接参考
[官方文档](https://openclaw.ai/)，在 VM 里没有额外的坑。

## 跑起来之后

性能比我预期的好，OpenClaw 日常使用响应不慢。和 UTM 比，最大的差别是省心——
不需要打开一个 GUI 程序来管虚拟机，命令行启停，干净。

说实话，OpenClaw 还在快速迭代，现在就花几千块买台专用机器，多少有点赌。
但这个方案可以让你现在就跑起来，真的用顺手了、确认值得，再考虑买硬件不迟。
