---
layout: mermaid
title: How the GFW Works
---

# How the Great Firewall Works

## 1. What it is

The **Great Firewall (GFW)** is the name commonly used for a set of filtering systems positioned between networks inside mainland China and the rest of the internet. Think of it as a checkpoint on the highway: every connection that passes into or out of the country is inspected, and some are turned away. Understanding how it blocks traffic tells us what a working bypass must do.

## 2. Following a connection: what the firewall sees at each step

Let us follow a single connection to a website, step by step, and mark what the firewall can observe at each stage:

<pre class="mermaid">
flowchart TD
    A[1. DNS query for example.com] -->|visible: the domain name| B[2. TCP connection to server IP:443]
    B -->|visible: the IP and port| C[3. TLS handshake - SNI carries the domain]
    C -->|visible: SNI, certificate, sizes| D[4. Encrypted data flows]
    D -->|visible: timing and volume, not content| E[5. Connection closes]
</pre>

- **Step 1, DNS:** the firewall sees which domain you are asking about.
- **Step 2, TCP:** it sees which server IP and port you are connecting to.
- **Step 3, TLS handshake:** encryption has not started yet — the **SNI** field still carries the domain name in plaintext, along with details like the TLS version and fingerprint.
- **Steps 4–5:** the content is encrypted, but the firewall can still measure when traffic flows and how much of it there is.

This is the key insight: **the firewall does not need to read your data. It only needs to recognize the connection.**

## 3. The blocking techniques

**1. IP blocklisting — a bouncer with a list.** Known IP addresses or whole ranges are dropped or not routed. It is coarse: it blocks everything hosted at those addresses.

**2. DNS poisoning — directory assistance gives a wrong number.** The firewall answers DNS queries for blocked domains with a fake IP. Your computer connects to a dead address and the site appears unreachable.

**3. TCP RST injection — a forged "return to sender" notice.** To kill an active connection, an attacker on the path sends a fake TCP **RST** packet that looks like it comes from one of the two endpoints. Both sides believe the connection closed and give up.

**4. SNI-based filtering — reading the name on the envelope.** During the TLS handshake the domain name is visible in the SNI field. The firewall can match it against a blocklist and inject a reset, so the connection dies before anything is exchanged.

**5. Deep packet inspection — opening unsealed mail.** For unencrypted traffic, the firewall reads the payload directly and blocks requests containing filtered keywords.

<pre class="mermaid">
flowchart TD
    subgraph normal[Without interference]
        C1[Your computer] -->|DNS query| D1[DNS server]
        D1 -->|Real IP| C1
        C1 -->|HTTPS with SNI: example.com| S1[Target website]
    end

    subgraph blocked[When the firewall interferes]
        C2[Your computer] -->|DNS query| D2[DNS server]
        D2 -.->|Fake IP returned| C2
        C2 -->|HTTPS with blocked SNI| F[Firewall]
        F -.->|injects TCP RST| C2
        F -->|connection killed| S2[Target website]
    end
</pre>

## 4. Why encryption alone is not enough

HTTPS hides the contents of your messages, but the firewall still sees the outside of the envelope: the destination IP, the SNI domain, and the traffic pattern. Blocking is a recognition problem, not a reading problem. That is why merely "using HTTPS" does not bypass anything — the connection still looks like a connection to a blocked service.

## 5. What a working bypass must do

1. a **server outside the firewall** — the blocked content is fetched where it is reachable;
2. **encryption** — so the content cannot be read or keyword-filtered;
3. **traffic that does not look blocked** — so the firewall does not recognize the connection and reset it.

The third point is the hard part, and it is exactly what the next page is about.

## Up next

[How Proxies and Tunnels Work](proxy-and-tunnels.md) — turning these ideas into a working setup.
