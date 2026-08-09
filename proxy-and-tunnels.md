---
layout: mermaid
title: Proxies and Tunnels
---

# How Proxies and Tunnels Work

## The core idea

If the direct connection to a blocked site is cut, the fix is to avoid connecting directly. Instead, your computer connects to a **proxy server** located outside the firewall — that connection is allowed — and asks it to fetch the blocked content on your behalf. The proxy server then relays the response back through the same connection.

<pre class="mermaid">
flowchart LR
    subgraph direct[Direct connection - blocked]
        A1[Your computer] -->|request to blocked site| G1{Firewall}
        G1 -->|connection reset| X1[No response]
    end

    subgraph proxied[Via an overseas proxy]
        A2[Your computer] -->|encrypted tunnel| P2[Proxy server abroad]
        P2 -->|normal request| S2[Target website]
        S2 -->|response| P2
        P2 -->|encrypted tunnel| A2
    end
</pre>

## Proxies, VPNs, and tunnels

These words are often used loosely; the practical difference is mostly scope:

- An **HTTP or SOCKS proxy** is a forwarding service for a specific application. Your browser (or other software) sends its traffic to the proxy, and the proxy forwards it to the destination. SOCKS5 can carry many kinds of traffic, not just web pages.
- A **VPN** creates an encrypted tunnel for your whole device (or a large part of its traffic). All traffic inside the tunnel is protected, not just one app.
- A **tunnel** is the general idea: an encrypted connection between two endpoints. Proxy protocols such as VLESS or Trojan run inside a tunnel.

## VLESS and Trojan

Modern proxy protocols separate "how the tunnel is encrypted" from "how the tunnel is disguised."

- **Trojan** is designed to look like a normal HTTPS connection to a real website. To an observer, the traffic is indistinguishable from ordinary web browsing, so keyword filtering does not apply.
- **VLESS** is a lightweight protocol without built-in encryption of its own; it is normally paired with TLS or with **REALITY**. REALITY makes the TLS handshake appear to be a connection to a real, mainstream website (in this guide's setup, `www.amd.com`), and it does not require you to own a certificate or even a domain for the proxy itself. This disguise also resists active probing, because the firewall "sees" a plausible, real handshake.

This is why the server guide configures `VLESS + TCP + REALITY + Vision` with a public target like `www.amd.com`: the traffic looks like an ordinary visit to that site.

## Self-hosted VPS or "airport"?

There are two ways to obtain a proxy server:

| Option | Effort | Cost | Control |
| --- | --- | --- | --- |
| Self-hosted VPS | Higher: you configure and maintain the server | Lower, predictable (one VPS bill) | Full: you know exactly what runs on it |
| Airport service | Low: just import a subscription | Subscription fee | None: someone else operates it |

This guide covers the first option because it is educational, reliable, and gives you full control. If you use the second option, be aware that you are entrusting your traffic to a third party.

## A note on legality

Bypassing internet restrictions may be restricted or illegal in some jurisdictions, and providing these services to others for profit is explicitly outside the scope of this guide. This material is for individual, educational use — the same boundary stated on the home page. Please understand and follow the laws that apply to you.

## Up next

[Server Setup](server-setup.md) — apply these ideas on a real VPS with 3x-ui.
