---
title:  Install and configure Wireguard.
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

## Prepare the terrain

In this example, we are going to configure [WireGuard](https://wiki.archlinux.org/title/WireGuard){:target="_blank"} with the **VPN subnet of 10.0.10.0/24**, and listening port on **42024/UDP** on server side (you can change this if you would like). 

{% include alert.html type="info" title="Attention" content="The default Wireguard port is 51820/UDP, but as a rule of thumb, let's not use the default one in order to create a tiny layer of extra security which will make the process a bit harder to discover if someone is scanning your ports." %}

In order to set up the server and one client, we will have to create the following:

 - **Client private key.**
 - **Client public key.**
 - **Server private key.**
 - **Server public key.**
 - **Pre-shared key per client.**

The pre-shared key (PSK) is an optional security improvement as per the [WireGuard protocol](https://www.wireguard.com/protocol/){:target="_blank"} in order to **add an additional layer of symmetric-key cryptography to be mixed into the already existing public-key cryptography, for post-quantum resistance.**

### Generate Server Keys


### Generate Client Keys




## Install Wireguard Tools

We need the `wireguard-tools` package for **userspace utilites** (**both server and client** will use them, since this is a **peer-to-peer connection** in the end):

```bash
$ sudo pacman -S wireguard-tools
```





## OpenWRT

If you are interested in installing **Wireguard** on your [OpenWRT](https://openwrt.org/){:target="_blank"} router, then **any of these guide are the way to go** (I have tried them out myself):

 - [Installing and configuring Wireguard on OpenWRT](https://jasonschaefer.com/wireguard-vpn-on-openwrt/){:target="_blank"}
 - [Wireguard setup on OpenWRT](https://doc.turris.cz/doc/en/public/wireguard){:target="_blank"}

## Raspbian

Some time ago I published a [Github Repository](https://github.com/android10/RaspberryPi-Wireguard){:target="_blank"} on how to do this and it is still valid. Feel free to contribute if you run into issues. 

## References

 - [Wireguard Website](https://www.wireguard.com/){:target="_blank"}
 - [Wireguard Quick Start Guide](https://www.wireguard.com/quickstart/){:target="_blank"}
 - [Wireguard Command Line Interface](https://www.wireguard.com/quickstart/#command-line-interface){:target="_blank"}
 - [Wireguard Arch Linux Wiki](https://wiki.archlinux.org/title/WireGuard){:target="_blank"}
 - [Create a Point to Point Wireguard VPN on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-create-a-point-to-point-vpn-with-wireguard-on-ubuntu-16-04){:target="_blank"}
 - [What they do not tell about setting a Wireguard VPN](https://medium.com/tangram-visions/what-they-dont-tell-you-about-setting-up-a-wireguard-vpn-46f7bd168478){:target="_blank"}