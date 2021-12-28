---
title:  Install and configure Wireguard
description: In this guide, we are going to see how to install and setup Wireguard in order to encrypt traffic via a tunnel (VPN). 
tags: 
 - guides
 - arch
 - manjaro
 - wireguard
 - vpn
 - security
---

# Install and configure Wireguard

From the [Wireguard](https://www.wireguard.com/){:target="_blank"} project homepage:

**WireGuard is an extremely simple yet fast and modern VPN that utilizes state-of-the-art cryptography.** It aims to be faster, simpler, leaner, and more useful than IPsec, while avoiding the massive headache. **It intends to be considerably more performant than OpenVPN...**

### TODO

To be continued...

### Install Wireguard Tools

```bash
sudo pacman -S wireguard-tools
```

### OpenWRT

If you are interested in installing **Wireguard** on your [OpenWRT](https://openwrt.org/){:target="_blank"} router, then this guide is the way to go (I have tried it out myself):

 - [Installing and configuring Wireguard on OpenWRT](https://jasonschaefer.com/wireguard-vpn-on-openwrt/){:target="_blank"}

### Raspbian

Some time ago I published a [Github Repository](https://github.com/android10/RaspberryPi-Wireguard){:target="_blank"} on how to do this and it is still valid. Feel free to contribute if you run into issues. 

### References

 - [Wireguard Website](https://www.wireguard.com/){:target="_blank"}
 - [Wireguard Quick Start Guide](https://www.wireguard.com/quickstart/){:target="_blank"}
 - [Wireguard Command Line Interface](https://www.wireguard.com/quickstart/#command-line-interface){:target="_blank"}
 - [Wireguard Arch Linux Wiki](https://wiki.archlinux.org/title/WireGuard){:target="_blank"}