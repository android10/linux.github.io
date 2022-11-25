---
title:  Mount LUKS partitions for System Recovery.
description: In this guide, we are going to see how to mount LUKS partitions in case of troubleshooting in order to work out and recover our system. 
tags: 
 - guides
 - arch
 - manjaro
 - ssh
 - server
 - luks
---

# Mount LUKS partitions for System Recovery

Whenever we have a **system crash** after an update or in any other situation when we have to boot our system via USB drive **in order to repair it**, the first step is to mount our partitions, which could give us a couple of headaches [if they are encrypted](https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system){:target="_blank"}. So in this guide we will see how we can **successfully access to our encrypted drive** (LUKS)..

It is also important that we are clear with our partition table, which will serve as a source of information for mounting the proper drives:

```bash
❯ lsblk -f
NAME        MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINTS
nvme0n1     259:0    0 476.9G  0 disk
├─nvme0n1p1 259:1    0   511M  0 part  /boot
└─nvme0n1p2 259:2    0 476.4G  0 part
  └─luksdev 254:0    0 476.4G  0 crypt /var/log
                                       /home
                                       /var/cache/pacman/pkg
                                       /.snapshots
                                       /
```

## File system: ext4

[Ext4](https://wiki.archlinux.org/title/ext4) is the evolution of the most used Linux filesystem, Ext3. In many ways, Ext4 is a deeper improvement over Ext3 than Ext3 was over Ext2. It is very common and at this point is very likely you are using this fs, so let's have a look on how to proceed with it.

### 1 - Open the encrypted disk

```bash
$ cryptsetup luksOpen /dev/nvme0n1p2 luks

Enter passphrase for /dev/nvme0n1p2: 
```

### 2 - Mount all the partitions

```bash
$ mount /dev/mapper/main-root /mnt
$ mount /dev/mapper/main-home /mnt/home
$ mount /dev/nvme0n1p1 /mnt/boot
$ swapon /dev/mapper/main-swap
```

### 3 - Root into the new system

```bash
$ arch-chroot /mnt
```

### 4 - Unmount and exit

When we are done, we can unmount everything, exit and reboot your system:

```bash
$ exit
$ umount -R /mnt
$ reboot
```

## File system: btrfs

 [Btrfs](https://wiki.archlinux.org/title/Btrfs){:target="_blank"} is a modern copy on write (CoW) filesystem for Linux aimed at implementing advanced features while also focusing on fault tolerance, repair and easy administration. This file system is gaining more and more popularity and in order to mount an encrypted partition backed by a [btrfs](https://btrfs.wiki.kernel.org/index.php/Main_Page){:target="_blank"} file system, we need to perform the following steps. It is important to understand the concept of [sub-volumes](https://fedoramagazine.org/os-chroot-101-covering-btrfs-subvolumes/).

### 1 - Open the encrypted disk

```bash
$ cryptsetup luksOpen /dev/nvme0n1p2 luks

Enter passphrase for /dev/nvme0n1p2: 
```

### 2 - Overview of a btrfs filesystem with subvolumes

#### Method 1

```bash
$ mount /dev/nvme0n1p2 /mnt/
$ ls /mnt/
$ home root

$ ls /mnt/root/
bin dev home lib64 media opt root sbin sys usr
boot etc lib lost+found mnt proc run srv tmp var

$ ls /mnt/home/
user

$ umount /mnt
```

#### Method 2


```bash
$ mount /dev/nvme0n1p2 /mnt/

$ btrfs subvolume list /mnt
ID 256 gen 178 top level 5 path home
ID 258 gen 200 top level 5 path root
ID 262 gen 160 top level 258 path root/var/lib/machines

$ sudo umount /mnt
```

### 3 - Perform chroot with btrfs subvolumes

```bash
$ mount /dev/nvme0n1p2 /mnt/ -t btrfs -o subvol=root

$ ls /mnt/
bin dev home lib64 media opt root sbin sys usr
boot etc lib lost+found mnt proc run srv tmp var

$ ls /mnt/home/
<it's still empty>

$ mount /dev/nvme0n1p2 /mnt/home -t btrfs -o subvol=home

$ ls /mnt/home/
user

$ mount /dev/nvme0n1p1 /mnt/boot
$ mount --bind /dev /mnt/dev
$ mount -t proc /proc /mnt/proc
$ mount -t sysfs /sys /mnt/sys
$ mount -t tmpfs tmpfs /mnt/run
$ mkdir -p /mnt/run/systemd/resolve/
$ echo 'nameserver 1.1.1.1' > /mnt/run/systemd/resolve/stub-resolv.conf
$ chroot /mnt
```

### 4 - Unmount and exit

When we are done, we can unmount everything, exit and reboot your system:

```bash
$ exit
$ umount /mnt/boot
$ umount /mnt/sys
$ umount /mnt/proc
$ umount /mnt/sys
$ umount /mnt/run
$ umount /mnt
```

### 5 - To summarize

In order to summarize all the steps, here is the base for a script that could help you automate the process (make sure you run: `lsblk -f` to get info about your partition table):

```bash
# create directories:

$ mkdir /mnt/home
$ mkdir /mnt/boot
$ mkdir /mnt/btrfs

# decrypt:
$ crypsetup open /dev/nvme0n1p2 

# mount to check btrfs:
$ mount /dev/mapper/luks /mnt/btrfs

# check btrfs subvolumes
$ btrfs subvolume list /mnt/btrfs 

# mount boot 
$ mount /dev/nvme0n1p1 /mnt/boot

# mount root:
$ mount /dev/mapper/luks /mnt -t btrfs -o subvol=@

# mount home:
$ mount /dev/mapper/luks /mnt/home -t btrfs -o subvol=@home 

# test if everything mounted:
$ ls -lha /mnt/home 

# change root
$ arch-chroot 
```

## References

 - [Encrypting and entire system](https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system){:target="_blank"}
 - [Ext4 file system](https://wiki.archlinux.org/title/ext4){:target="_blank"}
 - [Kernel Newbies Ext4 file system](https://kernelnewbies.org/Ext4){:target="_blank"}
 - [Btrfs file system](https://wiki.archlinux.org/title/Btrfs){:target="_blank"}
 - [Btrfs Wiki](https://btrfs.wiki.kernel.org/index.php/Main_Page){:target="_blank"}
 - [Mount LUKS encrypted drive partition in Linux](https://trendoceans.com/mount-luks-encrypted-drive-partition-in-linux/){:target="_blank"}
 - [Covering btrfs sub-volumes](https://fedoramagazine.org/os-chroot-101-covering-btrfs-subvolumes/){:target="_blank"}