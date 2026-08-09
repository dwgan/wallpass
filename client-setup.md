---
layout: doc
title: Client Setup
---

# How to Use Clients on Different Platforms

Your server is ready — now connect your devices to it. This page lists the recommended client apps for each platform.

## Windows and Linux

Windows and Linux users can download [v2rayN](https://github.com/2dust/v2rayN/releases). For Raspberry Pi, refer to [v2ray-core](https://github.com/v2fly/v2ray-core/releases).

For v2rayN, version `6.60` is recommended for this guide. In v2rayN `6.60`, the default local SOCKS port is `10808` and the HTTP port is `10809`. In newer v2rayN versions, the local inbound is implemented as a mixed proxy, and both HTTP and SOCKS can use the same local port such as `10808`. If Codex login fails while using a newer v2rayN version, try v2rayN `6.60` or manually check that Codex is using the expected HTTP/SOCKS proxy port.

Refer to this [site](https://v2rayn.org/) for a guide. It is recommended to import node information via QR code.

## iOS

iOS users need to install Shadowrocket, which can only be searched using an American Apple account. For a [temporary account](https://zy.weiaj.com/post/65).

## Android

Android users can download [v2rayNG](https://github.com/2dust/v2rayNG/releases).

Refer to this [site](https://v2rayng.org/) for a guide.

## Previous

[How to Set up a Server](server-setup.md) — the server that this client connects to.
