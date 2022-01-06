---
title:  Configure SSH access on your Linux Machine.
description: In this guide, we are going to see how to configure an SSH Server on your Linux machine for remote access. 
tags: 
 - guides
 - arch
 - manjaro
 - ssh
 - security
---

# Configure SSH Access

**SSH remote access is useful for example in the case of a headless server or a remote machine, where you want to have control or administer it.**
Let's see in this guide how we can set up an **SSH server** for the mentioned purpose.

## Set a static IP address

**On* the SERVER SIDE (host you want to control)** and as a rule of thumb, **setting a static IP address is the first step**, since you do not want to have random IPs provided by an DHCP server. Here are 2 sources:

 - [How to set a static IP address.](../how-to/set-static-ip-address){:target="_blank"}
 - [How to switch from 'Network Manager' to 'systemd-networkd'.](../how-to/switch-from-network-manager-to-systemd-networkd){:target="_blank"}

## Install required dependencies dependencies

Not is is time to **install what is needed**, so:

```bash
$ sudo pacman -S openssh
```

## Start and enable services

In this step we want to [start, check status and enable the required services](../how-to/start-stop-enable-linux-services){:target="_blank"}, so **let's follow these commands**, they are **self-explanatory**:

```bash
$ sudo systemctl start sshd
```

**The last command started our SSH service**, so let's double check that is already started:

```bash
$ sudo systemctl status sshd
```

The 'sshd; service should be up and running but it will NOT start automatically, so **let's make sure that happens by enabling it at start-up time:**

```bash
$ sudo systemctl enable sshd
```

## A first SSH test

At this point, **we should be able to connect to our remote machine via SSH** default port **(22)** with **user/pass credentials**, so **ON THE CLIENT SIDE** run:

```bash
# Example: fernando@192.168.10.100
$ ssh USERNAME@IP_ADDRESS
```

If you are asked (very likely), **add a confirmation to add the 'fingerprint' to the 'known_hosts'** and then **type your password.** 

## Securing our SSH server

There are a couple of things **that are 'kind of' mandatory in order to add an extra layer of security to our SSH Server.**

### Change the default SSH Port

We **can achieve this** by editing the file `/etc/ssh/sshd_config`:

```bash
$ sudo vim /etc/ssh/sshd_config 
```

Let's **search and uncomment the option 'Port' and assign any other number**, for example **844**. 

### Disable user/pass authentication

**This is very IMPORTANT**, otherwise we are opening doors to **brute force attacks against our SSH Server**. In order to **mitigate this issue** first check this article:

 - [How to generate SSH keys on Linux](../how-to/generate-ssh-keys-on-linux){:target="_blank"}

Once **we have our SSH keys generated ON THE CLIENT SIDE**, we have to copy the content of our `.pub` file and **paste it at the bottom** of the file `~/.ssh/authorized_keys` on the **SERVER SIDE**. That's it, **we should be able to connect without user/pass credentials.** Please continue reading.

{% include alert.html type="info" title="Attention" content="We will have to generate keys per client willing to connect to our SSH server. We could also share the client keys, but this is NOT A GOOD PRACTICE." %}

### Other considerations

In the file `/etc/ssh/sshd_config`, **we could also polish even more our `sshd` settings,** depending on our needs and setup:

 - **X11Forwarding:** [Enabling X forwarding](https://ostechnix.com/how-to-configure-x11-forwarding-using-ssh-in-linux/){:target="_blank"} **makes our system vulnerable to X11 related issues**. So it’s a good idea to **set it to NO.**
 - **PermitRootLogin:** We should **NOT ALLOW** root users to login directly to the system, so **let's set it to NO.**

## Restart your service

In order for **the changes to take effect, let's restart our service:**

```bash
$ sudo systemctl restart sshd
```

Now **let's try out our setup**:

```bash
# Example: ssh -p 844 fernando@192.168.10.100
$ ssh -p 844 USERNAME@IP_ADDRESS
```

The `-p` argument serves to **specify the custom port** we have already configured. 
**Connection should succeed,** thus allowing us to **fully administer the remote remote machine via SSH.** 

## References

 - [OpenSSH Arch Linux wiki](https://wiki.archlinux.org/title/OpenSSH){:target="_blank"}
 - [Secure Shell Arch Linux wiki](https://wiki.archlinux.org/title/Secure_Shell){:target="_blank"}
 - [Arch Linux SSH server](https://linuxhint.com/arch_linux_ssh_server/){:target="_blank"}
 - [SSH Timeout](https://www.bjornjohansen.com/ssh-timeout){:target="_blank"}
 - [How to configure X11 forwarding using SSH](https://ostechnix.com/how-to-configure-x11-forwarding-using-ssh-in-linux/){:target="_blank"}