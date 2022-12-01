---
title:  Encrypt an External Hard Drive.
description: In this post, we are going to see how to encrypt and external hard drive (SSD) in linux. 
tags: 
 - how-to
---

# Encrypt an External SSD Hard Drive

Encrypting an external hard drive (in this case an SSD) can help prevent unauthorized access to your personal data. [Encryption in Arch Linux with LUKS](https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system) can be difficult, so let's see how we can succesfully achieve this this process in an step-by-step how-to guide. 

### Check your drive: in this example we use `sda`

```bash
❯ lsblk
NAME        MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINTS
sda           8:0    0 232.9G  0 disk
nvme0n1     259:0    0 476.9G  0 disk
├─nvme0n1p1 259:1    0   511M  0 part  /boot
└─nvme0n1p2 259:2    0 476.4G  0 part
```

### RECOMMENDED: Wipe all file systems and data from the hard drive

```bash
$ sudo wipefs -a /dev/sda
```

### Run `cryptsetup` to create the encrypted partition

```bash
$ sudo cryptsetup --verbose --verify-passphrase luksFormat /dev/sda

WARNING!
========
This will overwrite data on /dev/sda irrevocably.

Are you sure? (Type 'yes' in capital letters):
```

### Open the encrypted partition

```bash
$ sudo cryptsetup luksOpen /dev/sda sda

Enter passphrase for /dev/sda:
```

### Create a new filesystem on the encrypted partition

In this case we are using [ext4](https://wiki.archlinux.org/title/ext4){:target="_blank"} but we could also use [btrfs](https://wiki.archlinux.org/title/btrfs){:target="_blank"} for example.

```bash
$ sudo mkfs.ext4 /dev/mapper/sda
```

We could also give our **filesystem a label** with the `-L` option: 

```bash 
$ sudo mkfs.ext4 -L MyEncryptedDisk /dev/mapper/sda
```

### Remove the reserved space

By default, **some space has been reserved**, but if you don't intend to run a system from the hard drive, **you can remove it** to have slightly more space on the hard drive.

```bash
$ sudo tune2fs -m 0 /dev/mapper/sda
```

### Check that everything is fine

```bash
❯ lsblk
NAME        MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINTS
sda           8:0    0 232.9G  0 disk
└─sda       254:2    0 232.9G  0 crypt /mnt/ssd
nvme0n1     259:0    0 476.9G  0 disk
├─nvme0n1p1 259:1    0   511M  0 part  /boot
└─nvme0n1p2 259:2    0 476.4G  0 part
```

### Close the encrypted device

In order to **safely remove your drive**, run the following command:

```bash
$ sudo cryptsetup luksClose sda
```

### Mounting the encrypted device

Refer to this guide: [Mount LUKS paritions for System Recovery](../guides/mount-luks-partition-for-system-recovery){:target="_blank"}