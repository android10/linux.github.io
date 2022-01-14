---
title:  Disable IPv6 using Network Manager.
description: In this post, we are going to see how to mitigate the issue 'Disable IPv6 using Network Manager'.
tags: 
 - troubleshooting
---

# Disable IPv6 using Network Manager

## Issue

**Many VPNs or network connections support only IPv4.** That means all IPv6 traffic bypasses the VPN and renders it virtually useless. To avoid this, we can tell [Network Manager](https://wiki.archlinux.org/title/NetworkManager){:target="_blank"} to **avoid IPv6 on an specific network connection.**

## Fix

We fix this by **disabling the IPv6 protocol on a system that uses NetworkManager to manage network interfaces.** If we disable IPv6, [Network Manager](https://wiki.archlinux.org/title/NetworkManager){:target="_blank"} automatically sets the corresponding `sysctl` values in the Kernel. 

### Disable IPv6 on a connection using nmcli

Let's display the **list of network connections:**

```bash
$ nmcli connection show
NAME    UUID                                  TYPE      DEVICE
Example 7a7e0151-9c18-4e6f-89ee-65bb2d64d365  ethernet  enp1s0
```

Now we are going to **set the `ipv6.method` parameter of the connection to `disabled`:** 

```bash
 $ nmcli connection modify Example ipv6.method "disabled"
```

As a final step **let's restart both `NetworkManager` and the connection itself:**

```bash
$ sudo systemctl restart NetworkManager
```

```bash
$ nmcli connection up Example
```

### Verify that everything is properly setup

Let's enter the `ip address show` command **to display the IP settings of the device:** 

```bash
$ ip address show enp1s0
2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 52:54:00:6b:74:be brd ff:ff:ff:ff:ff:ff
    inet 192.0.2.1/24 brd 192.10.2.255 scope global noprefixroute enp1s0
       valid_lft forever preferred_lft forever
```

**If no inet6 entry is displayed, IPv6 is disabled on the device.**

Another way to verify the correct setup is by **checking that the `/proc/sys/net/ipv6/conf/enp1s0/disable_ipv6` file now contains the value 1:** 

```bash
$ cat /proc/sys/net/ipv6/conf/enp1s0/disable_ipv6
1
```

## References 

 - [Network Manager Arch Wiki](https://wiki.archlinux.org/title/NetworkManager){:target="_blank"}
 - [Using Network Manager to disable IPv6](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_and_managing_networking/using-networkmanager-to-disable-ipv6-for-a-specific-connection_configuring-and-managing-networking){:target="_blank"}
