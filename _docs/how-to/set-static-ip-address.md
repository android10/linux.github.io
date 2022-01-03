---
title:  How to set a static IP address
description: In this post, we are going to see how to set a static IP address on Linux. 
tags: 
 - how-to
 - arch
 - manjaro
 - systemd-networkd
 - network
---

# Set a static IP address

Having a static ip could be useful for **headless servers** for example. I personally use [NetworkManager](https://wiki.archlinux.org/title/NetworkManager){:target="_blank"} to **handle my connections**, so this post will mainly focus on this program, but in case you want to use [systemd-networkd](https://wiki.archlinux.org/title/Systemd-networkd){:target="_blank"} for the same purpose, please check [How to switch from Network Manager to systemd-networkd](../how-to/switch-from-network-manager-to-systemd-networkd), since there is also an example on how to do the same as here. 

Let's proceed **by checking the status of our service**:

```bash
$ sudo systemctl status NetworkManager
```

**Which outputs:** 

```bash
● NetworkManager.service - Network Manager
     Loaded: loaded (/usr/lib/systemd/system/NetworkManager.service; enabled; vendor preset: disabled)
    Drop-In: /usr/lib/systemd/system/NetworkManager.service.d
             └─NetworkManager-ovs.conf
     Active: active (running) since Wed 2021-12-29 11:06:29 CET; 5h 13min ago
       Docs: man:NetworkManager(8)
   Main PID: 534 (NetworkManager)
      Tasks: 3 (limit: 38135)
     Memory: 15.3M
        CPU: 1.353s
     CGroup: /system.slice/NetworkManager.service
             └─534 /usr/bin/NetworkManager --no-daemon
```

 - If you are interested in **Linux services**, [check this other post on how to handle Linux services in a basic way](handle-linux-services).  

## Check current ip address

We have already done this [here](find-your-ip-address), but in summary:

```bash
$ hostname -i
```

## Check current connection

Let's have a look at **our active connections**:

```bash
$ nmcli connection show
```

And **its output:**

```bash
NAME      UUID                                  TYPE      DEVICE 
wired     fa19249d-d349-37ca-b115-b6a7b4fb8639  ethernet  enp3s0 
wireless  855ecace-1f68-46f3-99eb-c18dd26d4636  wifi      --    
```

## Configure connection

We are going to configure **ALL the involved part of our network connection.**

### IP address

```bash
$ nmcli con modify fa19249d-d349-37ca-b115-b6a7b4fb8639 ipv4.addresses 192.168.1.25/24
```

### Gateway

```bash
$ sudo nmcli con modify fa19249d-d349-37ca-b115-b6a7b4fb8639 ipv4.gateway 192.168.1.1
```

### DNS

```bash
$ sudo nmcli con modify fa19249d-d349-37ca-b115-b6a7b4fb8639 ipv4.dns "192.168.1.1"
```

### Method

```bash
$ sudo nmcli con modify fa19249d-d349-37ca-b115-b6a7b4fb8639 ipv4.method manual
```

## Activate connection

```bash
$ sudo nmcli connection up fa19249d-d349-37ca-b115-b6a7b4fb8639
```

## Configuration file 

A configuration file has been generated after we set up everything, and could be checked here:

```bash
$ sudo cat /etc/NetworkManager/system-connections/wired.nmconnection 
```

```bash
[connection]
id=wired
uuid=fa19249d-d349-37ca-b115-b6a7b4fb8639
type=ethernet
autoconnect-priority=-999
interface-name=enp3s0
permissions=
timestamp=1640800022

[ethernet]
mac-address-blacklist=

[ipv4]
address1=192.168.1.25/24,192.168.1.1
dns=192.168.1.1;
dns-search=
method=manual

[ipv6]
addr-gen-mode=eui64
dns-search=
method=ignore

[proxy]
```

## References

 - [Network Manager Arch Linux Wiki](https://wiki.archlinux.org/title/NetworkManager){:target="_blank"}
 - [How to Configure Network Connection Using 'nmcli' Tool](https://www.tecmint.com/nmcli-configure-network-connection/){:target="_blank"}
 - [Configure static IP on arch linux](https://nanxiao.me/en/configure-static-ip-address-on-arch-linux/){:target="_blank"}
 - [Connect to a Network using 'nmcli'](https://docs.fedoraproject.org/en-US/Fedora/25/html/Networking_Guide/sec-Connecting_to_a_Network_Using_nmcli.html){:target="_blank"}