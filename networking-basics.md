---
layout: mermaid
title: Internet Basics
---

# Internet Basics: How Your Data Gets from A to B

Every time you open a website, dozens of invisible steps happen in under a second. By the end of this page you will be able to explain the whole journey of a single click: how your computer finds the website, how your data travels across the world, and what HTTPS actually protects. No networking experience is needed — we will use everyday analogies.

## 1. Clients and servers: asking and answering

The internet works like a restaurant. Your computer is the **customer**: it places an order. The machine that holds the website is the **kitchen**: it prepares the answer. Your browser (the client) sends a request; the web server answers with the page. Almost everything on the internet follows this pattern.

## 2. IP addresses and ports: the building and the apartment

Every device needs an address, just like a house needs a street address. This is its **IP address** — for example `93.184.216.34` (IPv4) or the longer `2606:2800:220:1:248:1893:25c8:1946` (IPv6).

But a single server hosts many services at once. That is why we also need a **port**, which is like an apartment number inside the building:

- Port `80` → unencrypted web traffic (HTTP)
- Port `443` → encrypted web traffic (HTTPS)

An address plus a port — `93.184.216.34:443` — points to one specific service, not just one machine.

## 3. DNS: the phone book of the internet

Computers talk in numbers, but people remember names. The **Domain Name System (DNS)** is the phone book: it translates `example.com` into an IP address. When you type a URL, your computer first asks a DNS resolver, "what is the IP address of this domain?" and only then connects to that address.

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

## 4. Packets and routing: letters passed between post offices

Data does not travel as one continuous stream. Imagine writing a long letter: you cut it into numbered pages, put each page into its own envelope with the same sender and recipient addresses, and mail them separately. These envelopes are **packets**. Each packet carries the source address, the destination address, and a piece of the data.

**Routers** are the post offices in between. Each router only needs to know roughly where to send the packet next; it does not need to know the whole route. At the destination, the packets are put back in order.

<pre class="mermaid">
flowchart LR
    A[Your computer] --> B[Home router]
    B --> C[Your ISP]
    C --> D[Backbone routers]
    D --> E[Server's datacenter]
    E --> F[Web server]
</pre>

## 5. TCP and UDP: registered mail vs. postcards

Two common ways to send those envelopes:

| | TCP | UDP |
| --- | --- | --- |
| Guarantee | Delivery, order, resend if lost | None |
| Analogy | Registered mail with a receipt | Postcards thrown into the mail |
| Used by | Web pages, email, file transfers | Video calls, games, DNS queries |

## 6. HTTP and HTTPS: postcards and sealed envelopes

**HTTP** is the language clients and servers speak: `GET /index.html` asks for a page. But HTTP is like a postcard — anyone who handles it can read it.

**HTTPS** puts the HTTP message inside a sealed envelope (TLS encryption). Intermediaries can no longer read the contents.

However, a sealed envelope still shows its outside. A network observer can still see:

- the IP addresses and ports involved,
- how much data flows and when,
- the DNS queries you make,
- and — during the TLS handshake — the domain name in the **SNI** field.

<pre class="mermaid">
flowchart LR
    subgraph client[Your computer]
        A[HTTP request]
    end
    subgraph wire[On the wire - visible]
        B[Destination IP and port]
        C[SNI: example.com]
        D[Encrypted content - unreadable]
    end
    subgraph server[Web server]
        E[Decrypts and reads the request]
    end
    A --> B --> C --> D --> E
</pre>

## 7. Putting it all together: what happens when you visit a website

<pre class="mermaid">
sequenceDiagram
    participant B as Browser
    participant R as DNS resolver
    participant S as Web server
    B->>R: 1. What is the IP of example.com?
    R-->>B: 2. 93.184.216.34
    B->>S: 3. TCP connection to :443
    S-->>B: 4. Connection established
    B->>S: 5. TLS handshake (SNI: example.com)
    S-->>B: 6. TLS established - now encrypted
    B->>S: 7. GET /index.html (encrypted)
    S-->>B: 8. Page content (encrypted)
</pre>

This journey — DNS lookup, connection setup, encryption, then the actual request — is exactly what the next page examines from the firewall's point of view.

## Up next

[How the Great Firewall Works](how-the-gfw-works.md) — where and how this journey can be blocked.
