---
title:  How to change your hostname
description: In this post, we are going to see how to change your hostname in linux. 
tags: 
 - how-to
---

# Change your hostname

**The hostname of a Linux system is used to identify the device on a network,** thus highlighting which system you’re interacting with. In the end, everything drills down into [ip addresses](find-your-ip-address), **but those can change frequently,** for instance, **hostnames give us a way to know which device we’re working with**, either on the network or physically, without having to remember the mentioned [ip addresses](find-your-ip-address).

## Checking your current hostname

Run the following **command:**

```bash
$ hostname
```

Here is the **output:**

```bash
$ android10-xps-arch
```

If you want more detailed information, **you could also use:**

```bash
$ hostnamectl
```

Which **displays:**

```bash
 Static hostname: android10-xps-arch
       Icon name: computer-laptop
         Chassis: laptop
      Machine ID: ...
         Boot ID: ...
Operating System: Arch Linux                      
          Kernel: Linux 5.15.7-zen1-1-zen
    Architecture: x86-64
 Hardware Vendor: Dell Inc.
  Hardware Model: XPS 13 9310
```

As we can see, our hostname is `android10-xps-arch` according to **both commands above.** 

## Modifying your hostname

Now we are going to change our hostname to `android10-linux` by running the **following command:**

```bash
$ sudo hostnamectl set-hostname android10-linux
```

We can double check our changes by **running again:**

```bash
$ hostnamectl
```

```bash
 Static hostname: android10-linux
       Icon name: computer-laptop
         Chassis: laptop
      Machine ID: ...
         Boot ID: ...
Operating System: Arch Linux                      
          Kernel: Linux 5.15.7-zen1-1-zen
    Architecture: x86-64
 Hardware Vendor: Dell Inc.
  Hardware Model: XPS 13 9310
```