---
title:  How to list running services and edit their configuration.
description: In this post, we are going to see how to list existing running services in linux, plus their configuration edition. 
tags: 
 - how-to
 - arch
 - manjaro
 - service
 - systemd
 - network-manager
---

# List and edit service configuration

**Services** or daemons (probably the name we are more familiar with) in linux, **are programms or applications, which run or expect to run in the background**. In essence , they are running without the need for the user to be aware of them all the time.

## Listing running services

In order to **visualize the running services** in our system, we can execute the **following command:**

```bash
$ systemctl --type=service
```

**Which gives us** (I cut it off for just diaplaying purpose):

```bash
  UNIT                                      LOAD   ACTIVE SUB     DESCRIPTION        
  alsa-restore.service                      loaded active exited  Save/Restore Sound Card State
  bolt.service                              loaded active running Thunderbolt system service
  dbus.service                              loaded active running D-Bus System Message Bus
  iio-sensor-proxy.service                  loaded active running IIO Sensor Proxy service
  NetworkManager.service                    loaded active running Network Manager
  sddm.service                              loaded active running Simple Desktop Display Manager
  systemd-journald.service                  loaded active running Journal Service
  systemd-logind.service                    loaded active running User Login Management
  user@1000.service                         loaded active running User Manager for UID 1000
  wpa_supplicant.service                    loaded active running WPA supplicant
  ...

LOAD   = Reflects whether the unit definition was properly loaded.
ACTIVE = The high-level unit activation state, i.e. generalization of SUB.
SUB    = The low-level unit activation state, values depend on unit type.
36 loaded units listed. Pass --all to see loaded but inactive units, too.
To show all installed unit files use 'systemctl list-unit-files'.
```

We also get a piece of help **if we want to see the installed ones**:

```bash
$ systemctl list-unit-files
```

## Editing service configuration

Individually, **per service, we can edit its configuration.** In this example we are using `NetworkManager.service` which is one of the main ones **for handling our network connection**. So let's see its configuration:

```bash
$ sudo systemctl edit --full NetworkManager.service
```

And the **output**:

```bash
[Unit]
Description=Network Manager
Documentation=man:NetworkManager(8)
Wants=network.target
After=network-pre.target dbus.service
Before=network.target 

[Service]
Type=dbus
BusName=org.freedesktop.NetworkManager
#ExecReload=/bin/kill -HUP $MAINPID
ExecStart=/usr/bin/NetworkManager --no-daemon
Restart=on-failure
# NM doesn't want systemd to kill its children for it
KillMode=process

ProtectSystem=true
ProtectHome=read-only

# We require file descriptors for DHCP etc. When activating many interfaces,
# the default limit of 1024 is easily reached.
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
Also=NetworkManager-dispatcher.service

# We want to enable NetworkManager-wait-online.service whenever this service
# is enabled. NetworkManager-wait-online.service has
# WantedBy=network-online.target, so enabling it only has an effect if
# network-online.target itself is enabled or pulled in by some other unit.
Also=NetworkManager-wait-online.service
```

**We edit and save, and bum!**..We are done. 