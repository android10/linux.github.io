---
title:  Decrypt LUKS partition remotely via SSH.
description: In this guide, we are going to see how to decrypt remotely our LUKS partition via SSH by installing and setting up Tiny SSH. 
tags: 
 - guides
 - arch
 - manjaro
 - systemd-networkd
 - ssh
 - server
 - luks
---

# Decrypt LUKS partition remotely via SSH

One of the most common situations **when we want to remotely decrypt our LUKS partitions is in case we have a headless server,** otherwise, we have to be on-site in order to plug a keyboard and a screen and type the decryption key so our machine can load the Linux OS.

## Requirements 

 - A **Headless Server** running [Arch Linux](https://archlinux.org/){:target="_blank"} or [Manjaro](https://manjaro.org/){:target="_blank"} or any other Arch based distro. 
 - Your **Linux installation** should be based on [Systemd](https://wiki.archlinux.org/title/Systemd){:target="_blank"}.

### Before proceeding

 - [Update/Upgrade your system](../how-to/update-your-system).
 - **As optional**, [switch from Network Manager to systemd-networkd](../how-to/switch-from-network-manager-to-systemd-networkd) in order to be consistent with the approach being showcased here.
 - **As optional** and specially if you are dealing with a headless server, [set a static ip address](../how-to/set-static-ip-address).

## The Process

The idea behind this is simple: we are going to start an SSH Service in our [Initial RAM Disk](https://en.wikipedia.org/wiki/Initial_ramdisk){:target="_blank"} via [mkinitcpio](https://wiki.archlinux.org/title/Mkinitcpio){:target="_blank"} 




For my desktop/laptos machines I use Network Manager for handling connections but I would recommend you use systemd-networkd if your system is based on systemd as bla bla bla, in order to make the process easier. Also for this use case scenario, you want to use a static ip address too. 


## Extra Ball: External RAID HDD  

If we are dealing with a headless server, it is very likely that we have an external RAID HDD plugged in to our server so...
 
 - TODO

## Worth reading

Last but not least, **if you are interested in how things work under the hood,** I leave here a bunch of useful concepts/articles/links:

 - Hello
 - Hello 
