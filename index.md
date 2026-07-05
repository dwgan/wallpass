---
layout: default
title: Home
---

This website introduce the easy way to pass GFW, which can dramatically improve development efficiency.

What is GFW? Please refer to [Wikipedia](https://en.wikipedia.org/wiki/Great_Firewall).

In short,  the firewall exists to protect Chinese citizens from being misled by outside harmful information.

It should be noted that it is illegal to bypass the firewall in legal terms, especially providing services for others to gain benefits.

However, for developers or those with higher education, the firewall significantly reduces work efficiency. Finding reasonable ways to bypass the Great Firewall is a specialized skill.

So, how to pass the Great Firewall? Technically, you need a server outside the Great Firewall, which is not prohibited. You can redirect your internet requests to this server, allowing it to access websites blocked by the Great Firewall on your behalf and then send the data back to you. In this way, you can indirectly visit those restricted websites.

There are mainly two methods to bypass the Great Firewall. The first method is to buy a VPS from overseas VPS servers providers. Developers can set up proxy services, such as SSR or V2Ray, on these servers to bypass the firewall. This method is stable, reliable, and relatively inexpensive, but it requires a higher technical skill level from the developer.

For those without this technical expertise or those who prefer not to spend time on it, they can opt for "airport" services. Frankly speaking, some developer with technical skills purchase multiple VPS servers, set up proxy services on them, and then sell these services to users, usually on a monthly subscription basis.

This guide use first method.

Note this guide can only be used by individual users. It is forbidden for commercial use.

# How to Set up a Server

Refer to [How to Set up a Server](server-setup.md).

# How to use Client on different platforms

## Windows/Linux User

For Windows/Linux user, please go to this [site](https://github.com/2dust/v2rayN/releases) for APP. For RaspberryPI, please refer to this [site](https://github.com/v2fly/v2ray-core/releases).

For v2rayN, version `6.60` is recommended for this guide. In v2rayN `6.60`, the default local SOCKS port is `10808` and the HTTP port is `10809`. In newer v2rayN versions, the local inbound is implemented as a mixed proxy, and both HTTP and SOCKS can use the same local port such as `10808`. If Codex login fails while using a newer v2rayN version, try v2rayN `6.60` or manually check that Codex is using the expected HTTP/SOCKS proxy port.

Refer to this this [site](https://v2rayn.org/) for guide. It is recommended to import nodes information via QR code.

## IOS User

IOS user need to install Shadowrocket, it can only be searched using American Apple account. For [temperal account](https://zy.weiaj.com/post/65)

## Andriod User

Go to this [site](https://github.com/2dust/v2rayNG/releases) for APP.

Refer to this this [site](https://v2rayng.org/) for guide.
