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

## Recommended requirements for this guide 

 - A **Headless Server** running [Arch Linux](https://archlinux.org/){:target="_blank"} or [Manjaro](https://manjaro.org/){:target="_blank"} or any other Arch based distro. 
 - **Linux installation** should be (but not extrictly) based on [Systemd](https://wiki.archlinux.org/title/Systemd){:target="_blank"}:
    - [systemd-boot](https://wiki.archlinux.org/title/Systemd-boot){:target="_blank"} as the UEFI boot manager.
    - [systemd-networkd](../how-to/switch-from-network-manager-to-systemd-networkd) as a network connection manager. 

### Before proceeding

 - [Update/Upgrade your system](../how-to/update-your-system).
 - **As optional**, [switch from Network Manager to systemd-networkd](../how-to/switch-from-network-manager-to-systemd-networkd) in order to be consistent with the approach being showcased here.
 - **As optional** and specially if you are dealing with a headless server, [set a static ip address](../how-to/switch-from-network-manager-to-systemd-networkd#static-ip).

## The Process

The **idea** behind this is simple: we are going to **start an SSH Service** (TinySSH) in our [Initial RAM Disk](https://en.wikipedia.org/wiki/Initial_ramdisk){:target="_blank"} via [mkinitcpio](https://wiki.archlinux.org/title/Mkinitcpio){:target="_blank"} that will let us **connect to our machine at an early stage, so we can decrypt our LUKS partition remotely.** 

### 1 - Install dependencies

We are going to be using a [mkinitcpio](https://wiki.archlinux.org/title/Mkinitcpio){:target="_blank"} hook named [systemd-tool](https://github.com/random-archer/mkinitcpio-systemd-tool){:target="_blank"}, **which provides early remote SSH access before the root partition gets mounted.**. Let's proceed with the following command:

```bash
$ pacman -S mkinitcpio-systemd-tool busybox cryptsetup openssh tinyssh tinyssh-convert mc
```

### 2 - Add 'systemd-tool' HOOK

In order to setup our environment, **it is mandatory to add the 'systemd-tool' hook in the 'HOOKS' section of our** [mkinitcpio](https://wiki.archlinux.org/title/Mkinitcpio){:target="_blank"} **configuration file located at** `/etc/mkinitcpio.conf`. 

As a reference, this is **how my configuration looks like** (you might have other **HOOKS** [depending on your system](https://wiki.archlinux.org/title/Mkinitcpio#HOOKS){:target="_blank"}):

```bash
HOOKS=(base udev autodetect modconf block lvm2 sd-vconsole filesystems keyboard fsck systemd systemd-tool)
```

### 3 - Add network module

**Since the HOOK will setup a network connection before the filesystem is mounted,** we need to make sure **we are adding the right driver** so the [Linux Kernel](https://wiki.archlinux.org/title/Kernel){:target="_blank"} **can recognize our network adapter.** This how the file `/etc/mkinitcpio.conf`, section [MODULES](https://wiki.archlinux.org/title/Mkinitcpio#MODULES){:target="_blank"} should look like:

```bash
MODULES=(r8169 nvme i915 intel_agp)
```

In my case, **the driver for the network adapter is** `r8169`. **You can check yours with the command below:**  

```bash
$ lspci -k
```

**And the output:**

```bash
03:00.0 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller (rev 15)
        Subsystem: Intel Corporation Device 2067
        Kernel driver in use: r8169
        Kernel modules: r8169
```

### 4 - Configure the 'fstab' file

The [fstab](https://wiki.archlinux.org/title/Fstab){:target="_blank"} **defines how disk partitions, various other block devices, or remote file systems should be mounted into the file system.**. At this point you have one, used by the system to mount the filesystem, so let's check it out **(WITHOUT MODIFYING IT):**

```bash
$ sudo vim /etc/fstab
```

**Mine looks like this** (pay **attention** to the `nuc-root`, since this is how I named it when [I installed Arch](https://fernandocejas.com/blog/engineering/2020-12-28-install-arch-linux-full-disk-encryption/){:target="_blank"}, yours could be `root`):

```bash
# Static information about the filesystems.
# See fstab(5) for details.

# <file system> <dir> <type> <options> <dump> <pass>
# /dev/mapper/nuc-root
UUID=2c904c48-4178-44d7-921e-87d23e888888       /               ext4            rw,relatime     0 1

# /dev/mapper/nuc-home
UUID=d32aa547-670c-43b8-b4dd-5c6975888888       /home           ext4            rw,relatime     0 2
```

**Now we have to kind of replicate this** `/etc/fstab` **file and adapt its content to** `/etc/mkinitcpio-systemd-tool/config/fstab`, which is where [systemd-tool](https://github.com/random-archer/mkinitcpio-systemd-tool){:target="_blank"} will dive into in order to **mount the filesystem**. 

{% include alert.html type="info" title="Attention" content="It is IMPORTANT that we MOUNT ONLY THE ROOT PARTION at this point, since the only PID initialized is 1, which indicates that only the ROOT user is AVAILABLE." %} 

We can do this **via this command:**

```bash
$ echo "/dev/mapper/nuc-root    /sysroot    auto    x-systemd.device-timeout=9999h  0 1" > /etc/mkinitcpio-systemd-tool/config/fstab
```

**Here the final result** (cut off) **by checking the content of** `/etc/mkinitcpio-systemd-tool/config/fstab`:

```bash
# This file is part of https://github.com/random-archer/mkinitcpio-systemd-tool

# REQUIRED READING:
# * https://github.com/random-archer/mkinitcpio-systemd-tool/wiki/Root-vs-Fstab
# * https://github.com/random-archer/mkinitcpio-systemd-tool/wiki/System-Recovery

# fstab: mappings for direct partitions in initramfs:
# * file location in initramfs: /etc/fstab
# * file location in real-root: /etc/mkinitcpio-systemd-tool/config/fstab

# ...
/dev/mapper/nuc-root     /sysroot         auto         x-systemd.device-timeout=9999h     0       1
```

### 5 - Configure the 'cryptab' file

The [formal definition of cryptab](https://wiki.archlinux.org/title/Dm-crypt/System_configuration#crypttab){:target="_blank"} says that `/etc/crypttab` (encrypted device table) file **is similar** to the [fstab](https://wiki.archlinux.org/title/Fstab){:target="_blank"} file and **contains a list of encrypted devices to be unlocked during system boot up. This file can be used for automatically mounting encrypted swap devices or secondary file systems.**

In other words, if we are dealing with a headless server, it is very likely that we have an **external RAID HDD plugged in, that needs decryption when mounting the filesystem.** This is the purpose of the `cryptab` file.

So **let's move forard and configure** `/etc/crypttab` **and** `/etc/mkinitcpio-systemd-tool/config/crypttab` according to [systemd-tool requirements](https://github.com/random-archer/mkinitcpio-systemd-tool/blob/master/README.md){:target="_blank"}.

First we need to add our encrypted `sdx`device (yours) to `/etc/crypttab` file. **So let's see what we have:**

```bash
lsblk -f

NAME           FSTYPE      FSVER    LABEL UUID                                   
sda                                                                                                
└─sda2         crypto_LUKS 2    83a6d1c1-8348-4b0c-bb7d-ab1f4366666
```

**Now let's add this information to our** `/etc/crypttab`:

```bash
crypt    UUID=83a6d1c1-8348-4b0c-bb7d-ab1f430368d1   none    luks
```

As a final step, **we have to copy the content of** `/etc/crypttab` **to** `/etc/mkinitcpio-systemd-tool/config/crypttab`: 

```bash
$ cat /etc/crypttab > /etc/mkinitcpio-systemd-tool/config/crypttab
```

By checking our new `/etc/mkinitcpio-systemd-tool/config/crypttab` content, we should get something like this:

```bash
# This file is part of https://github.com/random-archer/mkinitcpio-systemd-tool

# REQUIRED READING:
# * https://github.com/random-archer/mkinitcpio-systemd-tool/wiki/Root-vs-Fstab
# * https://github.com/random-archer/mkinitcpio-systemd-tool/wiki/System-Recovery
# ...

# <mapper-name>  <block-device>                           <password/keyfile>   <crypto-options>
crypt            UUID=83a6d1c1-8348-4b0c-bb7d-ab1f430368d1   none                luks
home-server      /dev/sdb1                                   /root/keyfile       luks,noauto
```

### 6 - Enable required services

Now it is time to **start and enabled the required services**:

```bash
$ sudo systemctl enable initrd-cryptsetup.path
$ sudo systemctl enable initrd-tinysshd
$ sudo systemctl enable initrd-debug-progs
$ sudo systemctl enable initrd-sysroot-mount
```

### 7 - Double check your Kernel Paramters

**This is just a sanity** check to revisit your [systemd-boot](https://wiki.archlinux.org/title/Systemd-boot){:target="_blank"} **configuration and kernel parameters.** My `/boot/loader/entries/arch.conf` look like this:

```bash
title Arch Linux
linux /vmlinuz-linux
initrd /intel-ucode.img
initrd /initramfs-linux.img
options rd.luks.uuid=83a6d1c1-8348-4b0c-bb7d-ab1f430368d1 
        rd.luks.name=83a6d1c1-8348-4b0c-bb7d-ab1f430368d1=cryptlvm 
        root=/dev/mapper/nuc-root rw resume=/dev/mapper/nuc-swap ro intel_iomnu=igfx_off
```

### 8 - Setting up Tiny SSH

Now we have to **setup the SSH service in order to accept remote connections**:

 - The **shell that appears to unlock the partition** is a [tinyssh](https://tinyssh.org/){:target="_blank"}: service.
 - **Tinyssh only recognizes Ed25519 SSH keys**, so [we need to generate an Ed25519 key pair](generate-ssh-keys-on-linux){:target="_blank"} and paste the **public key** to `/root/.ssh/authorized_keys`.
 - Then we create a `symlink` to where [mkinitcpio-systemd-tool](https://github.com/random-archer/mkinitcpio-systemd-tool) pick them up before converting them to **tinyssh** format.
   - `sudo ln -s /root/.ssh/authorized_keys /etc/mkinitcpio-systemd-tool/config/authorized_keys` 
 - At this early stage, **we can ONLY connect as root user, because other users are NOT available yet.**

One more thing is that we need to tell the system that **we want a static ip** (by default is DHCP) at boot. So, inside the `/etc/mkinitcpio-systemd-tool/network/initrd-network.network`:


```bash
$ sudo vim /etc/mkinitcpio-systemd-tool/network/initrd-network.network
```

Add **this content:**

```bash
[Match]
Name=eth0

[Network]
Address=192.168.1.50/24
Gateway=192.168.1.1
DNS=192.168.1.1
```

As we can see, the `[Match]` label **represents the network interface by the usage of kernel interface names (eth0)**. If you cannot find yours, **here is a helper command**:

```bash
$ sudo dmesg | grep -i eth0
```

Which **displays**:

```bash
[   20.157387] e1000e 0000:00:1f.6 eth0: (PCI Express:2.5GT/s:Width x1) 94:c6:91:1f:58:ff
[   20.157395] e1000e 0000:00:1f.6 eth0: Intel(R) PRO/1000 Network Connection
[   20.157459] e1000e 0000:00:1f.6 eth0: MAC: 12, PHY: 12, PBA No: FFFFFF-0FF
[   20.201872] e1000e 0000:00:1f.6 eno1: renamed from eth0
```

### 9 - Build 'initramfs'

As last step, we have to [create and activate our initramgfs image](https://wiki.archlinux.org/title/Mkinitcpio#Manual_generation){:target="_blank"} via [mkinitcpio](https://wiki.archlinux.org/title/Mkinitcpio){:target="_blank"}:

```bash
$ mkinitcpio -P
```

### 10 - Reboot the system

Last but not least, **let's reboot**. Afterwards, **we should see a prompt like this:**

```bash
$ secret> ************* 
```

**Type your passphrase and voilà! The root partition will automatically get decrypted and mounted. Now you have a headless server with encrypted LUKS partitions that you can decrypt remotely via SSH.**

## References

 - [Mkinitcpio Arch Linux Wiki](https://github.com/random-archer/mkinitcpio-systemd-tool){:target="_blank"}
 - [Mkinitcpio-systemd-tool project](https://github.com/random-archer/mkinitcpio-systemd-tool){:target="_blank"}
 - [Systemd-boot Arch Linux Wiki](https://wiki.archlinux.org/title/Systemd-boot){:target="_blank"}
 - [Dm-crypt remote unlock Arch Linux Wiki](https://wiki.archlinux.org/title/Systemd-boot){:target="_blank"}
 - [Home server setup](https://yuankun.me/home-server-setup/){:target="_blank"}
 - [How To Set Up a Raspberry Pi 4 with Archlinux 64-bit](https://gist.github.com/XSystem252/d274cd0af836a72ff42d590d59647928){:target="_blank"}
 - [Decrypt LUKS devices remotely via dropbear SSH](https://adfinis.com/en/blog/decrypt-luks-devices-remotely-via-dropbear-ssh/){:target="_blank"}
