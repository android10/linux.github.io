---
title:  Remote host identification has changed.
description: In this post, we are going to see to mitigate the issue 'Remote host identification has changed'.
tags: 
 - troubleshooting
---

# Remote host identification has changed

## Error

```bash
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
SHA256:DZzrKs2Jh0ucl1EQb8bhyqjpr3sdfsdfdYnyQpJMD1oYU.
Please contact your system administrator.
Add correct host key in /home/user/.ssh/known_hosts to get rid of this message.
Offending RSA key in /home/user/.ssh/known_hosts:24
Host key for 192.168.100.10 has changed and you have requested strict checking.
Host key verification failed.
``` 

## Cause

**This is a common escenario when we are trying to connect remotely to a host via SSH and the key of it has changed,** meaning that it is no longer **trustful**. It could also mean as a **'man-in-the-middle attack'**, although it is less likely, but **attention should be paid**.  

## Fix #1

### Remove host public key from client unknown_hosts

Let's edit `~/.ssh/known_hosts` file and **remove the key that represents the host ip you are trying to connect to**:

```bash
$ sudo vim ~/.ssh/known_hosts
```

### Reconnect again and confirm the fingerprint

**After retrying an SSH connection**, we might see a message like this:

```bash
The authenticity of host '[192.168.100.10] ([192.168.100.10])' can't be established.
RSA key fingerprint is SHA256:pHSDFSDF1ewWesRsijHHbrXK556Gj0+g4GTnrwqq8.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? y
```

**Confirm it and problem fixed**:

```bash
Warning: Permanently added '[192.168.1.10]:987' (RSA) to the list of known hosts.
```

## Fix #2 

### Get the 'rsa' public key of the remote host

```bash
$ ssh-keyscan -t rsa 192.168.100.10
```

The **response** should look like this:

```bash
# server_ip SSH-2.0-OpenSSH_4.3
192.168.100.10 ssh-rsa AffAB3NzaC1yc2EAAABIwAAAQEAwH5EXZGEWROMMDSkksdfe
```

### Add remote host public key to client unknown_hosts

Copy the **entire response** line and paste it to the bottom of your** `~/.ssh/known_hosts` **file**:

```bash
192.168.100.10 ssh-rsa AffAB3NzaC1yc2EAAABIwAAAQEAwH5EXZGEWROMMDSkksdfe
```
