---
layout: mermaid
title: Proxies and Tunnels
---

# How Proxies and Tunnels Work

## 1. The core idea: a friend abroad who fetches things for you

Imagine a store only sells in a country you cannot enter, but a friend who lives there can buy anything for you and send it back. That friend is your **proxy server**: a machine outside the firewall that can reach the blocked website, fetches it on your behalf, and sends the result back to you. You never talk to the blocked site directly.

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

## 2. Step by step: what happens inside a proxied connection

<pre class="mermaid">
sequenceDiagram
    participant A as Your app
    participant L as Local client (v2rayN)
    participant P as Proxy server abroad
    participant S as Target website
    A->>L: 1. Request to blocked site
    L->>P: 2. Encrypted tunnel: proxy sees the target
    P->>S: 3. Normal request to the target
    S-->>P: 4. Response
    P-->>L: 5. Encrypted tunnel: response back
    L-->>A: 6. Decrypted response
</pre>

1. Your app asks the local client (for example v2rayN) to fetch a blocked site.
2. The local client wraps the request in an encrypted tunnel and sends it to the proxy server abroad.
3. The proxy server, which can reach anything, sends a normal request to the target.
4. The target responds to the proxy.
5. The proxy wraps the response in the same encrypted tunnel.
6. Your local client decrypts it and hands it to your app.

## 3. What an "encrypted tunnel" actually is

A tunnel is not a wire — it is an agreement to put one message inside another. Your request becomes the payload of a new packet whose destination is the proxy server. On the wire, an observer sees only "a connection to the proxy server", not the message inside. The inner message is encrypted, so only the proxy server can open it.

<pre class="mermaid">
flowchart LR
    A[Your request: GET blocked-site] --> B[Encrypted inside a tunnel packet]
    B --> C[On the wire, only the proxy address is visible]
    C --> D[Proxy decrypts and forwards the inner request]
</pre>

## 4. Proxies, VPNs, and tunnels: what the words mean

- An **HTTP or SOCKS proxy** forwards traffic for a specific application. SOCKS5 can carry many kinds of traffic, not just web pages.
- A **VPN** creates an encrypted tunnel for your whole device (or most of its traffic).
- A **tunnel** is the general idea: an encrypted connection between two endpoints. Protocols such as VLESS or Trojan run inside a tunnel.

## 5. Making the tunnel look normal: VLESS, Trojan, and REALITY

Encryption hides the contents, but a connection to a strange server at a strange port still looks suspicious. The second problem is **disguise**:

- **Trojan** mimics a normal HTTPS connection to a real website. To an observer, the traffic is indistinguishable from ordinary web browsing.
- **VLESS** is a lightweight protocol with no encryption of its own; it is normally paired with TLS or with **REALITY**. REALITY makes the TLS handshake look like a visit to a real, mainstream website (in this guide, `www.amd.com`) — the firewall "sees" a plausible handshake and cannot tell the difference.

<pre class="mermaid">
flowchart LR
    subgraph visitor[Real user visiting amd.com]
        A[Handshake with amd.com] --> B[Looks like normal HTTPS]
    end
    subgraph you[Your REALITY connection]
        C[Handshake presented as amd.com] --> D[Looks like normal HTTPS]
    end
</pre>

## 6. Self-hosted VPS or "airport"?

| Option | Effort | Cost | Control |
| --- | --- | --- | --- |
| Self-hosted VPS | Higher: you configure and maintain it | Lower, predictable (one VPS bill) | Full: you know exactly what runs on it |
| Airport service | Low: import a subscription | Subscription fee | None: someone else operates it |

This guide covers the first option because it is educational, reliable, and gives you full control. If you use the second option, remember that you are entrusting your traffic to a third party.

## 7. A note on legality

Bypassing internet restrictions may be restricted or illegal in some jurisdictions, and providing these services to others for profit is explicitly outside the scope of this guide. This material is for individual, educational use — the same boundary stated on the home page. Please understand and follow the laws that apply to you.

## Up next

[Server Setup](server-setup.md) — apply these ideas on a real VPS with 3x-ui.
