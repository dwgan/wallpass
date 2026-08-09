---
layout: mermaid
title: How the GFW Works
---

# How the Great Firewall Works

## What it is

The **Great Firewall (GFW)** is the name commonly used for the set of filtering systems that sit between networks inside mainland China and the rest of the internet. Its goal is to restrict access to specific websites and services. Understanding how it blocks traffic tells us what a working bypass must do.

## What an observer can see

From the previous page, even encrypted HTTPS traffic leaves metadata in the open. A firewall watching a connection sees:

- destination **IP address** and **port**,
- **DNS queries** for domain names,
- packet timing and sizes,
- the **SNI** field in the TLS handshake, which usually contains the domain name,
- the plaintext content of any unencrypted traffic.

All of these can be used to make blocking decisions.

## Blocking techniques

**1. IP blocklisting.** Known IP addresses or whole ranges belonging to blocked services are simply not routed, or connections to them are dropped. This is coarse: it blocks everything hosted at those addresses.

**2. DNS poisoning.** The firewall answers DNS queries for blocked domains with a fake IP address. The client connects to the fake address and gets nothing, so the site appears unreachable.

**3. TCP reset injection.** To kill an active connection, an attacker on the path can send a forged TCP **RST** packet that looks like it comes from one of the two endpoints. Both sides see the connection as "closed" and give up.

**4. SNI-based filtering.** When a client starts a TLS connection, it sends the domain name in the SNI field before encryption begins. A firewall can read that field and inject a reset — so connections to blocked domains die during the handshake.

**5. Deep packet inspection.** For unencrypted traffic, the firewall can read the payload directly and block requests that contain filtered keywords.

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

## What this means for a bypass

A working bypass needs three things:

1. a **server outside the firewall** — the blocked content is fetched there, where it is reachable;
2. **encryption** — so the content of your traffic cannot be read or filtered by keyword;
3. **traffic that does not look blocked** — otherwise the firewall will simply recognize the connection and reset it.

That third point is the hard part. Simply encrypting a connection is not enough if the firewall can still see the destination IP or read the SNI field. The next page explains how proxies and tunnels handle all three.

## Up next

[How Proxies and Tunnels Work](proxy-and-tunnels.md) — turning these ideas into a working setup.
