---
title:  How to generate SSH keys on Linux.
description: In this post, we are going to see how to generate SSH keys on Linux. 
tags: 
 - how-to
 - ssh
 - server
---

# Generate SSH keys on Linux

In this post we are going to **generate SSH keys in order to access other machines/services remotely.** The main focus will be on [ed25519](https://www.cryptopp.com/wiki/Ed25519){:target="_blank"}, which is **a cryptography solution (signature scheme) implementing Edwards-curve Digital Signature Algorithm (EdDSA).**

## Why ed25519?

There are a bunch of reasons **why nowadays this signature scheme is the way to go:**

 - **It is more modern.**
 - **It is more secure.**
 - **It is faster to generate and verify.**
 - **It is more collision resilience: against hash-function collision attacks where large numbers of keys are generated with the hope of getting two different keys have matching hashes.**
 - **It has a smaller footprint favoring key transfer/copy/paste.**

{% include alert.html type="info" title="Attention" content="Not all software solutions are supporting 'ed25519' at the moment of writing, but SSH implementations in most modern Operating Systems certainly support it." %}

**If for some reason, you bump into issues, please continue reading or go directly to the RSA section below.**

## The process

**Linux makes it relatively easy to generate keys (any type).** We can just get started with **this command and follow the process step by step** (The email address could be anything, althoug it is recommended to use one in order to **both avoid inconsistency and know whom this key belongs to**). 

```bash
$ ssh-keygen -t ed25519 -C "your@email.com"
```

**Here we have to pick a filename.** As a rule of thumb, **we should store our keys in your** `.ssh` **directory** in order to be consistent and facilitate access to them.

```bash
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/fernando/.ssh/id_ed25519): my_id_ed25519
Enter passphrase (empty for no passphrase): 
```

**We have to pick a password (passphrase) which will add an extra layer of security when accessing our SSH keys. RECOMMENDED.**  

```bash
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
```

**Our keys have been successfully generated:**

```bash
Your identification has been saved in my_id_ed25519
Your public key has been saved in my_id_ed25519.pub
The key fingerprint is:
SHA256:bYLfGZANw8y5u136l+KFdvOwfMI8aLduHM011YckkRU your@email.com
The key's randomart image is:
+--[ED25519 256]--+
|       +o.  o=Eo.|
|        ==  ... +|
|        o..     o|
|       ..o     ..|
|      . S.+    oo|
|       ..+ o... o|
|        .oooo==o |
|        . o.++@=.|
|           +o*==.|
+----[SHA256]-----+
```

**Now our keypair is ready to be deployed to SSH servers or any other service that can use them.**

We can also check **how short our public key is:**

```bash 
$ cat my_id_ed25519.pub
```

```bash
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICVBFoN0z95nHtJCV3uKe7qunIFEZKzTMvJEvpNK+Y5P your@email.com
```

## What about RSA?

[RSA](https://www.cryptopp.com/wiki/RSA_Cryptography){:target="_blank"} **is still a well known player and widely used,** mostly because we are creature of habits. Many platforms are **already migrating and discouraging this signature schema usage** and I'm pretty sure, it will be deprecated at some point. 

Even though the point above, it is around and valid, and **in case of compatbility issues**, we can generate `rsa` keys **the same way** as `ed25519` keys **by only changing the** `-t` **argument in the generation command** (the process is exatly the same as above):

```bash
$ ssh-keygen -t rsa -C "your@email.com"
```

## References

 - [How to generate ed25519 SSH keys](https://www.unixtutorial.org/how-to-generate-ed25519-ssh-key/){:target="_blank"}
 - [how to generate an SSH key pair in Linux](https://www.siteground.com/kb/generate_ssh_key_in_linux/){:target="_blank"}