---
layout: default
title: How to Set up a Server
---

# How to Set up a Server

This guide uses the following stack:

- VPS provider: [BandwagonHost](https://bwh81.net) GIA CN2, or [Tencent Cloud Lighthouse](https://www.tencentcloud.com/act/pro/lighthouse) as a cost-effective alternative
- Operating system: Debian 13. Ubuntu is also usable, but this guide uses Debian for best performance.
- Server panel: [3x-ui](https://github.com/MHSanaei/3x-ui)
- Protocol: `VLESS + TCP/RAW + REALITY + Vision `(If you need to realize best compatibility, e.g. to use V2rayN_V5.x based on lower than Xray-core 1.8, you can choose `Trojan + TCP/RAW + TLS`)
- Domain: [Alibaba Cloud](https://aliyun.com/), DNS hosted by [Cloudflare](https://cloudflare.com)
- Test result:
  - Test real delay: 150 ~ 200 ms
  - Test download speed: 20 ~ 22 MB/s
  - Video play: load 8K video on [YouTube](https://www.youtube.com/watch?v=XDqTECW7WdU) without waiting
  - Supported websites: ChatGPT, Codex, Netflix
  - Supported platforms: Windows (x86/ARM), Linux (x86/ARM), macOS, iOS, Android
- Tools:
  - [MobaXterm](https://mobaxterm.mobatek.net/download.html)
  - [IP test](https://www.ip2location.com)
- Tutorial, if you want to learn this technology follow the video, the following links are enough for you:
  - [Latest tutorial video](https://www.youtube.com/watch?v=TRVE7sVTgWw)
  - [This series of 4 videos is helpful for understanding this technology.](https://www.youtube.com/watch?v=MuWTmEiNe1g)

## 1. Buy and initialize the VPS

Choose a BandwagonHost GIA CN2 plan from [BandwagonHost](https://bwh81.net) or Tencent Cloud Lighthouse is also a cost-effective VPS option. If you choose Lighthouse, select an overseas region is required.

Install Debian 13 from the VPS control panel.

After the VPS is created, connect to it through SSH:

```bash
ssh root@your_server_ip
```

On Windows, this guide uses MobaXterm as the SSH client:

```text
Session -> SSH
Remote host: your_server_ip
Specify username: root
Port: 22
```

After connecting, enter the root password provided by the VPS provider.

Confirm the Debian version first:

```bash
cat /etc/os-release
```

You should see `Debian GNU/Linux 13` in the output.

Example output:

```text
PRETTY_NAME="Debian GNU/Linux 13 (trixie)"
NAME="Debian GNU/Linux"
VERSION_ID="13"
VERSION="13 (trixie)"
VERSION_CODENAME=trixie
DEBIAN_VERSION_FULL=13.5
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
```

Update the package index and upgrade installed packages:

```bash
apt update && apt upgrade -y
```

Remove unused packages and clean the package cache (optional):

```bash
apt autoremove -y
apt autoclean
```

If the upgrade installs a new kernel or core system packages, reboot the server:

```bash
reboot
```

After rebooting, reconnect through SSH before continuing.

## 2. Move the domain DNS to Cloudflare

This step is optional, but it is recomended if you need to remember the IP easily.

Add your Alibaba Cloud domain to Cloudflare, then copy the two nameservers provided by Cloudflare.

In Alibaba Cloud domain management, replace the domain nameservers with the Cloudflare nameservers. DNS propagation usually takes several minutes to several hours.

In Cloudflare DNS, create an `A` record pointing to your VPS IP:

```text
Type: A
Name: your-subdomain
Content: your_server_ip
Proxy status: DNS only
```

Keep the record as `DNS only`. Do not enable the orange-cloud proxy for this record, because REALITY is not regular HTTP traffic and Cloudflare proxying will break the connection.

## 3. Install 3x-ui

Run the official [3x-ui](https://github.com/MHSanaei/3x-ui) installation script:

```bash
bash <(curl -Ls https://raw.githubusercontent.com/MHSanaei/3x-ui/master/install.sh)
```

When the installer asks interactive questions, use these choices:

```text
Database Selection
Choose [1]: 1

Customize the Panel Port settings?
Choose: y

Please set up the panel port:
Choose: a custom high port, for example 12345

SSL certificate setup method
Choose: 1. Let's Encrypt for Domain

Please enter your domain name:
Choose: your panel domain, for example panel.example.com
```

Before entering the domain here, make sure this domain or subdomain already has an `A` record in Cloudflare pointing to this VPS IP, and the proxy status is `DNS only`.

```text
Type: A
Name: panel
Content: your_server_ip
Proxy status: DNS only
```

Then continue with the certificate port prompt:

```text
Please choose which port to use:
Choose: 80

Modify --reloadcmd for ACME?
Choose: n

Set this certificate for the panel?
Choose: y
```

Save the installer output securely, especially these fields (if you forget to save them, do not worry about it, you can get them from x-ui):

```text
Username
Password
Access URL
```

After installation, run the 3x-ui management menu:

```bash
x-ui
```

Use these menu options for the first check. It is strongly recommended to turn on BBR for better network performance. It is also recommended to change the username and password for x-ui if needed.

```text
26. Enable BBR
Choose: 1. Enable BBR

7. Reset Username & Password
Choose: y
Username: your_username
Password: your_password
Disable currently configured two-factor authentication: y

11. View Current Settings

Panel is secure with SSL
port: your_3xui_panel_port
webBasePath: /your_web_path/
Database: SQLite (/etc/x-ui/x-ui.db)
Access URL: https://your_panel_domain:your_3xui_panel_port/your_web_path/
```

Open the `Access URL` in a browser and log in to the 3x-ui web panel with the username and password you just set. The following steps are configured inside this web panel.

Do not publish the panel username, password, API token, full access URL, or web base path. If they are exposed, reset them from the `x-ui` menu.

## 4. Create a VLESS REALITY inbound

In the 3x-ui panel, create a new inbound with these settings:

```text
Protocol: VLESS
Port: 443
TCP mode: RAW
Security: REALITY
```

Recommended REALITY settings. Choose a website that is accessible from Mainland China. Avoid `microsoft.com` here.

```text
target: www.amd.com:443
SNI / serverName: www.amd.com
Fingerprint: chrome
```

In the 3x-ui panel, create a new client with these settings:

Choose the inbound you just created, then set Flow as follows:

```text
Flow: xtls-rprx-vision
```

## 5. Import the node on the client

Import the generated VLESS link or QR code into the client application. Check these fields carefully if you configure it manually:

```text
Address: your-subdomain.your-domain.com
Port: 443
Protocol: VLESS
Transport: TCP
Security: REALITY
Flow: xtls-rprx-vision
SNI: www.amd.com
Fingerprint: chrome
Public key: generated by 3x-ui
Short ID: generated by 3x-ui
```

Test the connection after importing. If it fails, first check whether the Cloudflare DNS record is `DNS only`, whether port `443/tcp` is open, and whether the client public key, SNI, fingerprint, and short ID match the server inbound.

Enjoy it!

## 6. Basic maintenance

Keep the server and panel updated regularly:

```bash
apt update && apt upgrade -y
x-ui
```

Use the `x-ui` command to manage 3x-ui, including panel settings, updates, and service restart.

For security, do not share the panel address, panel password, private key, or client UUID. If a node is leaked, delete that client in 3x-ui and create a new one.