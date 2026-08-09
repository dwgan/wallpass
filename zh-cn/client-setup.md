---
layout: doc
title: 客户端配置
lang: zh
---

# 各平台客户端使用

服务器已经就绪——现在把你的设备连接上去。本页列出每个平台推荐的客户端应用。

## Windows 和 Linux

Windows 和 Linux 用户可以下载 [v2rayN](https://github.com/2dust/v2rayN/releases)。Raspberry Pi 请参考 [v2ray-core](https://github.com/v2fly/v2ray-core/releases)。

本指南推荐使用 v2rayN `6.60` 版本。在 v2rayN `6.60` 中，默认的本地 SOCKS 端口是 `10808`，HTTP 端口是 `10809`。在新版本的 v2rayN 中，本地入站被实现为混合代理，HTTP 和 SOCKS 可以共用同一个本地端口（例如 `10808`）。如果使用较新版本的 v2rayN 导致 Codex 登录失败，请尝试 v2rayN `6.60`，或手动检查 Codex 使用的是否是预期的 HTTP/SOCKS 代理端口。

参考这个[网站](https://v2rayn.org/)的指南。建议通过二维码导入节点信息。

## iOS

iOS 用户需要安装 Shadowrocket，它只能使用美区 Apple 账号搜索。临时账号可参考[这里](https://zy.weiaj.com/post/65)。

## Android

Android 用户可以下载 [v2rayNG](https://github.com/2dust/v2rayNG/releases)。

参考这个[网站](https://v2rayng.org/)的指南。

## 上一节

[如何搭建服务器](server-setup.md) — 这台客户端所连接的服务器。
