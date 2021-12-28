---
title:  Update your system
description: In this post, we are going to see how to upgrade/update your linux system. 
tags: 
 - how-to
 - arch
 - manjaro
 - debian
---

# Update your system

## Arch Linux based systems

You can fully **update/upgrade** your main system by using `pacman`. Keep in mind, **it does not include** [AUR](https://aur.archlinux.org/){:target="_blank"}

```bash
$ pacman -Syu
```

If you **want to include** [AUR - Arch User Repository Packages](https://aur.archlinux.org/){:target="_blank"} installed, **you have to use an** [AUR Helper](https://wiki.archlinux.org/title/AUR_helpers){:target="_blank"}. 
In my case I use `yay`:

```bash
$ yay -Syu
```

 - The `-S` option tells `pacman` (or `yay` or any other [AUR Helper](https://wiki.archlinux.org/title/AUR_helpers){:target="_blank"}) to **synchronize the local database with the main repository database.**
 - The `-y` option **updates the local system cache.**
 - The `-u` tells the package manager **to actually upgrade the packages.**

**Basically, this whole command synchronizes the local pacman database with the main repository database and then updates the system.**

{% include alert.html type="warning" title="Warning" content="When installing packages in Arch, avoid refreshing the package list without upgrading the system." %}

In practice, **DO NOT** run `pacman -Sy package_name` instead of `pacman -Syu package_name`, as this could lead to **DEPENDENCY ISSUES**.

## Debian based systems

You can **perform a full system update/upgrade** by running this combination of commands:

```bash
$ sudo apt update && sudo apt upgrade -y
```

 - The `apt update` option **updates the local cache from the Debian repository** to check for new available versions of the installed packages.
 - The `apt upgrade` is the command that actually **updates your Debian system**.
 - The `-y` option let the `apt upgrade` command to **run automatically after the first command finishes successfully.**
