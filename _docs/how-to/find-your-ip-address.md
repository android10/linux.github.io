---
title:  How to find your IP Address
description: In this post, we are going to see how to find your IP Address in Linux. 
tags: 
 - how-to
---

# Find your IP Address

Many times, **it is useful to identify our different network interfaces and figure out what are their IP Addresses.**

## Local IP Address

You can figure out your **Local IP Address** with the following command:

```bash
$ ip addr
```

This output **represents the different existent interfaces** which you can use to visualize the resultant IP Addresses:

```bash
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: wlp114s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 4096
    link/ether 9c:b6:d0:3e:4b:02 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.115/24 brd 192.168.1.255 scope global dynamic noprefixroute wlp114s0
       valid_lft 83691sec preferred_lft 83691sec
    inet6 2a01:c23:6491:1101:8b69:e75e:68fd:9cdc/64 scope global dynamic noprefixroute 
       valid_lft 297sec preferred_lft 117sec
    inet6 fe80::3b77:46e0:a344:4ebc/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```

Another **alternative would be to use the** `hostname` **command:**

```bash
$ hostname -i
```

And **here the output:**

```bash
$ 192.168.1.115
```

## Public IP Address

Figuring out your **Public IP Address** is as easy as **visiting any website from this list:**

 - [https://ip.me/](https://ip.me/){:target="_blank"}
 - [https://whatismyipaddress.com/](https://whatismyipaddress.com/){:target="_blank"}