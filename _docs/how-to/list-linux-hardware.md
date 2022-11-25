---
title:  List Linux Hardware.
description: In this post, we are going to see different ways of getting hardware information. 
tags: 
 - how-to
 - arch
 - manjaro
 - systemd
---

# List Linux Hardware

We are going to list different ways of getting information about your installed  hardware on Linux. 

## General Hardaware Information

### neofetch

If it is **not installed**, then install it with your favorite package manager `sudo pacman -S neofetch` and then: 

```bash
$ neofetch
```

You **should see** something like this:

```bash
❯ neofetch
                   -`                    user@arch-linux
                  .o+`                   ----------------------------
                 `ooo/                   OS: Arch Linux x86_64
                `+oooo:                  Host: 20QJCTO1WW ThinkPad T495s
               `+oooooo:                 Kernel: 5.15.79-1-lts
               -+oooooo+:                Uptime: 2 hours, 49 mins
             `/:-:++oooo+:               Packages: 887 (pacman)
            `/++++/+++++++:              Shell: zsh 5.9
           `/++++++++++++++:             Resolution: 3840x2160
          `/+++ooooooooooooo/`           WM: sway
         ./ooosssso++osssssso+`          Theme: Adwaita [GTK2]
        .oossssso-````/ossssss+`         Icons: Adwaita [GTK2]
       -osssssso.      :ssssssso.        Terminal: alacritty
      :osssssss/        osssso+++.       Terminal Font: Hack Nerd Font Mono
     /ossssssss/        +ssssooo/-       CPU: AMD Ryzen 5 PRO 3500U w/ Radeon Vega Mobile Gfx (8) @ 2.100GHz
   `/ossssso+/:-        -:/+osssso+-     GPU: AMD ATI Radeon Vega Series / Radeon Vega Mobile Series
  `+sso+:-`                 `.-/+oso:    Memory: 4714MiB / 13869MiB
 `++:.                           `-/+/
 .`                                 `/
```

## USB and PCI Devices

### lsusb

Install with your package manager `sudo pacman -S usbutils` and then:

```bash
❯ lsusb
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 004: ID 0c45:672a Microdia Integrated_Webcam_HD
Bus 003 Device 003: ID 1050:0407 Yubico.com Yubikey 4/5 OTP+U2F+CCID
Bus 003 Device 002: ID 27c6:533c Shenzhen Goodix Technology Co.,Ltd. FingerPrint
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 006 Device 004: ID 043e:9a68 LG Electronics USA, Inc. LG UltraFine Display Camera
Bus 006 Device 003: ID 043e:9a71 LG Electronics USA, Inc. 
Bus 006 Device 002: ID 043e:9a60 LG Electronics USA, Inc. USB3.1 Hub
Bus 006 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 005 Device 005: ID 043e:9a70 LG Electronics USA, Inc. LG UltraFine Display Controls
Bus 005 Device 004: ID 043e:9a66 LG Electronics USA, Inc. LG UltraFine Display Audio
Bus 005 Device 003: ID 043e:9a73 LG Electronics USA, Inc. 
Bus 005 Device 002: ID 043e:9a61 LG Electronics USA, Inc. USB2.1 Hub
Bus 005 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
```

### lspci

```bash
❯ lspci
00:00.0 Host bridge: Intel Corporation 11th Gen Core Processor Host Bridge/DRAM Registers (rev 01)
00:02.0 VGA compatible controller: Intel Corporation TigerLake-LP GT2 [Iris Xe Graphics] (rev 01)
00:04.0 Signal processing controller: Intel Corporation TigerLake-LP Dynamic Tuning Processor Participant (rev 01)
00:06.0 PCI bridge: Intel Corporation 11th Gen Core Processor PCIe Controller (rev 01)
00:07.0 PCI bridge: Intel Corporation Tiger Lake-LP Thunderbolt 4 PCI Express Root Port #0 (rev 01)
00:07.2 PCI bridge: Intel Corporation Tiger Lake-LP Thunderbolt 4 PCI Express Root Port #2 (rev 01)
00:0a.0 Signal processing controller: Intel Corporation Tigerlake Telemetry Aggregator Driver (rev 01)
00:0d.0 USB controller: Intel Corporation Tiger Lake-LP Thunderbolt 4 USB Controller (rev 01)
00:0d.2 USB controller: Intel Corporation Tiger Lake-LP Thunderbolt 4 NHI #0 (rev 01)
00:0d.3 USB controller: Intel Corporation Tiger Lake-LP Thunderbolt 4 NHI #1 (rev 01)
00:12.0 Serial controller: Intel Corporation Tiger Lake-LP Integrated Sensor Hub (rev 20)
00:14.0 USB controller: Intel Corporation Tiger Lake-LP USB 3.2 Gen 2x1 xHCI Host Controller (rev 20)
00:14.2 RAM memory: Intel Corporation Tiger Lake-LP Shared SRAM (rev 20)
00:15.0 Serial bus controller: Intel Corporation Tiger Lake-LP Serial IO I2C Controller #0 (rev 20)
00:15.1 Serial bus controller: Intel Corporation Tiger Lake-LP Serial IO I2C Controller #1 (rev 20)
00:16.0 Communication controller: Intel Corporation Tiger Lake-LP Management Engine Interface (rev 20)
00:19.0 Serial bus controller: Intel Corporation Tiger Lake-LP Serial IO I2C Controller #4 (rev 20)
00:19.1 Serial bus controller: Intel Corporation Tiger Lake-LP Serial IO I2C Controller #5 (rev 20)
00:1c.0 PCI bridge: Intel Corporation Device a0b8 (rev 20)
00:1d.0 PCI bridge: Intel Corporation Device a0b3 (rev 20)
00:1e.0 Communication controller: Intel Corporation Tiger Lake-LP Serial IO UART Controller #0 (rev 20)
00:1f.0 ISA bridge: Intel Corporation Tiger Lake-LP LPC Controller (rev 20)
00:1f.3 Multimedia audio controller: Intel Corporation Tiger Lake-LP Smart Sound Technology Audio Controller (rev 20)
00:1f.4 SMBus: Intel Corporation Tiger Lake-LP SMBus Controller (rev 20)
00:1f.5 Serial bus controller: Intel Corporation Tiger Lake-LP SPI Controller (rev 20)
01:00.0 Non-Volatile memory controller: Toshiba Corporation XG6 NVMe SSD Controller
02:00.0 PCI bridge: Intel Corporation JHL7540 Thunderbolt 3 Bridge [Titan Ridge DD 2018] (rev 06)
03:02.0 PCI bridge: Intel Corporation JHL7540 Thunderbolt 3 Bridge [Titan Ridge DD 2018] (rev 06)
04:00.0 USB controller: Intel Corporation JHL7540 Thunderbolt 3 USB Controller [Titan Ridge DD 2018] (rev 06)
72:00.0 Unassigned class [ff00]: Qualcomm QCA6390 Wireless Network Adapter
73:00.0 Unassigned class [ff00]: Realtek Semiconductor Co., Ltd. RTS5260 PCI Express Card Reader (rev 01)
```

## Screen Outputs

### xrandr

[xrandr](https://wiki.archlinux.org/title/Xrandr) is an official configuration utility to the RandR (Resize and Rotate) X Window System extension. It can be used to set the size, orientation or reflection of the outputs for a screen.

```bash
xrandr --props
Screen 0: minimum 16 x 16, current 3200 x 1800, maximum 32767 x 32767
XWAYLAND1 connected primary 3200x1800+0+0 (normal left inverted right x axis y axis) 600mm x 340mm
        RANDR Emulation: 1 
        non-desktop: 0 
                supported: 0, 1
   3200x1800     59.96*+
   2048x1536     59.95  
   1920x1440     59.97  
   1600x1200     59.87  
   1440x1080     59.99  
   1400x1050     59.98  
   1280x1024     59.89  
   1280x960      59.94  
   1152x864      59.96  
   1024x768      59.92  
   800x600       59.86  
   640x480       59.38  
   320x240       59.52  
   2560x1600     59.99  
   1920x1200     59.88  
   1680x1050     59.95  
   1440x900      59.89  
   1280x800      59.81  
   720x480       59.71  
   ...
```

### on wayland using Sway

[Sway](https://wiki.archlinux.org/title/Sway) is a tiling Wayland compositor and a drop-in replacement for the i3 window manager for X11.

```bash
❯ swaymsg -t get_outputs
Output DP-2 'Goldstar Company Ltd LG UltraFine 909NTMX7Z723' (focused)
  Current mode: 3200x1800 @ 59.999 Hz
  Position: 3350,70
  Scale factor: 1.000000
  Scale filter: nearest
  Subpixel hinting: unknown
  Transform: normal
  Workspace: 1
  Max render time: off
  Adaptive sync: disabled
  Available modes:
    3840x2160 @ 59.999 Hz
    2560x2880 @ 59.996 Hz
    3200x1800 @ 59.999 Hz
    2560x1440 @ 60.000 Hz
    1920x1200 @ 59.999 Hz
    1920x1080 @ 59.999 Hz
    1600x1200 @ 59.999 Hz
    1680x1050 @ 59.999 Hz
    1280x1024 @ 59.999 Hz
    1440x900 @ 59.999 Hz
    1280x800 @ 59.999 Hz
    1280x720 @ 59.999 Hz
    1024x768 @ 59.999 Hz
    800x600 @ 59.999 Hz
    640x480 @ 59.940 Hz

Output eDP-1 'Chimei Innolux Corporation 0x14F2 0x00000000' (inactive)
  Available modes:
    1920x1080 @ 60.001 Hz
    1680x1050 @ 60.001 Hz
    1280x1024 @ 60.001 Hz
    1440x900 @ 60.001 Hz
    1280x800 @ 60.001 Hz
    1280x720 @ 60.001 Hz
    1024x768 @ 60.001 Hz
    800x600 @ 60.001 Hz
    640x480 @ 60.001 Hz
```

## CPU and Sensors

### lscpu

```bash
❯ lscpu
Architecture:            x86_64
  CPU op-mode(s):        32-bit, 64-bit
  Address sizes:         43 bits physical, 48 bits virtual
  Byte Order:            Little Endian
CPU(s):                  8
  On-line CPU(s) list:   0-7
Vendor ID:               AuthenticAMD
  Model name:            AMD Ryzen 5 PRO 3500U w/ Radeon Vega Mobile Gfx
    CPU family:          23
    Model:               24
    Thread(s) per core:  2
    Core(s) per socket:  4
    Socket(s):           1
    Stepping:            1
    Frequency boost:     enabled
    CPU(s) scaling MHz:  62%
    CPU max MHz:         2100.0000
    CPU min MHz:         1400.0000
    BogoMIPS:            4191.68
Virtualization features:
  Virtualization:        AMD-V
Caches (sum of all):
  L1d:                   128 KiB (4 instances)
  L1i:                   256 KiB (4 instances)
  L2:                    2 MiB (4 instances)
  L3:                    4 MiB (1 instance)
NUMA:
  NUMA node(s):          1
  NUMA node0 CPU(s):     0-7
Vulnerabilities:
  Itlb multihit:         Not affected
  L1tf:                  Not affected
  Mds:                   Not affected
  Meltdown:              Not affected
  Mmio stale data:       Not affected
  Retbleed:              Mitigation; untrained return thunk; SMT vulnerable
  Srbds:                 Not affected
  Tsx async abort:       Not affected
```

### sensors

```bash
❯ sensors
amdgpu-pci-0500
Adapter: PCI adapter
vddgfx:           N/A
vddnb:            N/A
edge:         +48.0°C

thinkpad-isa-0000
Adapter: ISA adapter
fan1:           0 RPM
CPU:          +48.0°C
GPU:           +0.0°C
temp3:         +0.0°C
temp4:         +0.0°C
temp5:         +0.0°C
temp6:         +0.0°C
temp7:         +0.0°C
temp8:            N/A

BAT0-acpi-0
Adapter: ACPI interface
in0:          11.88 V

iwlwifi_1-virtual-0
Adapter: Virtual device
temp1:        +49.0°C

k10temp-pci-00c3
Adapter: PCI adapter
Tctl:         +48.5°C

nvme-pci-0200
Adapter: PCI adapter
Composite:    +37.9°C  (low  =  -5.2°C, high = +83.8°C)
                       (crit = +87.8°C)
```