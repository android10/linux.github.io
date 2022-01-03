---
title:  How to switch from Network Manager to systemd-networkd.
description: In this post, we are going to see how to switch from Network Manager to systemd-networkd on Linux. 
tags: 
 - how-to
 - network
 - arch
 - manjaro
---

# Switch from Network Manager to systemd-networkd

**Nowadays, most major Linux distributions adopted** [Systemd](https://wiki.archlinux.org/title/Systemd){:target="_blank"} **as a default init system**, which is basically a **suite of basic building blocks for a Linux system**. It provides a system and service manager that runs as `PID 1` and **starts the rest of the system**. 

It includes different components, and one of those is [systemd-networkd](){:target="_blank"}, which is **responsible for network configuration:**

 - **Basic DHCP/static IP networking for network devices.**
 - **Virtual networking features: bridges, tunnels and VLANs.**

Although wireless networking is not directly handled by [systemd-networkd](https://wiki.archlinux.org/title/Systemd-networkd){:target="_blank"}, you can use [wpa_supplicant](https://wiki.archlinux.org/title/Wpa_supplicant){:target="_blank"} service to configure wireless adapters, and then hook it up with [systemd-networkd](https://wiki.archlinux.org/title/Systemd-networkd){:target="_blank"}.

## Why not Network Manager?

I personally use [NetworkManager](https://wiki.archlinux.org/title/NetworkManager){:target="_blank"} to **handle my network connections**on my desktop/laptops computers because it is easy to setup and [play around with it](set-static-ip-address){:target="_blank"}, but **there are situations where I want full compatibility with** [Systemd](https://wiki.archlinux.org/title/Systemd){:target="_blank"}, for example when [dealing with a headless server](../guides/decrypt-luks-partition-remotely-via-ssh){:target="_blank"}. It is in essence free to the consumer...or even better **"use the best tool for the right job"**.

## Making the move

**It is very straightforward to move from Network Manager to systemd-networkd.** As a first step let's **disable** `Network Manager` service and **enable** `systemd-networkd` service:

```bash
$ sudo systemctl disable NetworkManager
$ sudo systemctl enable systemd-networkd 
```

As a second step, we have to **enable** and **start** `systemd-resolved` service, which is used by `systemd-networkd` for network name resolution. **This service implements a caching DNS server.**

```bash
$ sudo systemctl enable systemd-resolved
$ sudo systemctl start systemd-resolved
```

`Systemd-resolved` will create its own `resolv.conf` under `/run/systemd` directory. However, **it is a common practise to store DNS resolver information in** `/etc/resolv.conf`, and many applications still rely on `/etc/resolv.conf`. That is why **we have to create a symlink to** `/etc/resolv.conf` **for compatibility reasons**:

```bash
$ sudo rm /etc/resolv.conf
$ sudo ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf
```

## Setting up network connections

**Network configuration information are represented by text files with** `.network` **extension in the directory** `/etc/systemd/network`. We can create as many files as we would like to but **keep in mind that they are processed in lexical order so if you have some configuration overlapping inside these files, the last one is the one who wins.** 

If the mentioned folder does not exist, let's proceed to create it:

```bash
$ sudo mkdir /etc/systemd/network
```

Before creating network configuration, **let's display the available network interfaces (we are going to need them)**:

```bash
$ networkctl
```

**Its output:**

```bash
IDX LINK   TYPE     OPERATIONAL SETUP     
  1 lo     loopback carrier     unmanaged
  2 enp3s0 ether    routable    configured

2 links listed.
```

## Static IP

In order to **setup a network interface with a static IP**, we have to create its corresponding file:

```bash
$ sudo vim /etc/systemd/network/00-enp3s0.network
```

With **this content:**

```bash
[Match]
Name=enp3s0

[Network]
Address=192.168.1.50/24
Gateway=192.168.1.1
DNS=192.168.1.1
```

As we can see, the `[Match]` label **represents the network interface by the usage of** [udev](https://wiki.archlinux.org/title/Udev){:target="_blank"} **names (enp3s0) instead of kernel names (eth0)**.

{% include alert.html type="info" title="Attention" content="This distinction is important so the 'udev' module can correctly detect the network interface and configure it properly." %} 

## DHCP

With **DHCP we follow the exact process above and change the configuration file content** to:

```bash
[Match]
Name=enp3s0

[Network]
DHCP=yes
```

## Restarting the service

Once we are done with our changes, **it is required that we restart our service** (if you want to know more about [handling services, please check this](start-stop-enable-linux-services){:target="_blank"}):

```bash
$ sudo systemctl restart systemd-networkd
```

[Now we can see our IP Address](find-your-ip-address){:target="_blank"} and the changes we have applied in this post.  

## References

 - [Systemd Arch Linux Wiki](https://wiki.archlinux.org/title/Systemd){:target="_blank"}
 - [Systemd-networkd Arch Linux Wiki](https://wiki.archlinux.org/title/Systemd-networkd){:target="_blank"}
 - [Wpa_suplication Arch Linux Wiki](https://wiki.archlinux.org/title/Wpa_supplicant){:target="_blank"}
 - [Switch from Network Manager to systemd-network by Dan Nanni](https://www.xmodulo.com/switch-from-networkmanager-to-systemd-networkd.html){:target="_blank"}