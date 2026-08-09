---
layout: mermaid
title: Internet Basics
---

# Internet Basics: How Your Data Gets from A to B

When you open a website, dozens of invisible steps happen in under a second. This page explains the building blocks: what an IP address is, how names become numbers, how data travels across the world, and what HTTPS actually protects. It is written for absolute beginners, so no networking experience is required.

## Clients and servers

Your computer is the **client**: it asks for something. The machine that holds the website is the **server**: it answers. Almost everything on the internet follows this pattern — a client sends a request, and a server sends back a response.

## IP addresses and ports

Every device connected to the internet needs an address, just like a house needs a street address. This is the **IP address**. For example, `93.184.216.34` is an IPv4 address; IPv6 addresses are longer, like `2606:2800:220:1:248:1893:25c8:1946`.

A server can run many services at once, so it also uses **ports** to tell them apart. Port `80` is the default for unencrypted web traffic (HTTP), and port `443` is the default for encrypted web traffic (HTTPS). An address plus a port — `93.184.216.34:443` — names one specific service.

## DNS: turning names into numbers

People remember names, but computers need numbers. The **Domain Name System (DNS)** is the phone book of the internet: it translates `example.com` into an IP address. When you type a URL, your computer first asks a DNS resolver, "what is the IP address of this domain?" and only then connects to that address.

<pre class="mermaid">
sequenceDiagram
    participant C as Your computer
    participant R as DNS resolver
    participant D as DNS servers
    participant S as Web server
    C->>R: What is the IP of example.com?
    R->>D: Query example.com
    D-->>R: Answer: 93.184.216.34
    R-->>C: Answer: 93.184.216.34
    C->>S: Connect to 93.184.216.34:443
    S-->>C: Welcome!
</pre>

## Packets and routing

Data does not travel as one continuous stream. It is cut into small chunks called **packets**, each carrying the source address, the destination address, and a piece of the payload. **Routers** pass each packet from one network to the next, like a package moving through multiple post offices, until it reaches its destination.

<pre class="mermaid">
flowchart LR
    A[Your computer] --> B[Home router]
    B --> C[Your ISP]
    C --> D[Backbone routers]
    D --> E[Server's datacenter]
    E --> F[Web server]
</pre>

## TCP and UDP

Two common ways to carry packets:

- **TCP** guarantees delivery and order. If a packet is lost, TCP resends it. Web browsing, email, and file transfers use TCP.
- **UDP** is faster but makes no promises: packets may arrive late, out of order, or not at all. Video calls, gaming, and DNS queries often use UDP.

## HTTP and HTTPS

**HTTP** is the language clients and web servers speak: `GET /index.html` asks for a page. But HTTP itself is plaintext — anyone on the path can read the request.

**HTTPS** wraps HTTP inside **TLS** (Transport Layer Security). The connection is encrypted, so the contents of your requests and responses cannot be read by intermediaries.

However, encryption is not invisibility. Even with HTTPS, a network observer can still see:

- the IP addresses you connect to and the ports you use,
- when and how much data flows,
- the DNS queries you make,
- and — during the TLS handshake — the domain name in the **SNI** field.

These visible "metadata" details matter: the next page explains how a firewall uses exactly these pieces to block traffic.

## Up next

[How the Great Firewall Works](how-the-gfw-works.md) — what blocking techniques look like in practice.
