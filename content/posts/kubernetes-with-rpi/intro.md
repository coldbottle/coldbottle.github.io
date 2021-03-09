---
author: "coldbottle"
title: "Intro"
date: 2021-03-07
draft: true
tags: []
categories: []
series: []
ShowToc: true
TocOpen: true
weight: 99999
---



### 준비물
raspberry pi 4 8GB 8개
POE
SSD
등등등

### rasbian os

```
apt update && apt full-upgrade -y
apt install vim -y
```

### firmware update

[링크](https://www.raspberrypi.org/documentation/hardware/raspberrypi/bcm2711_bootloader_config.md)

```bash
cat > /etc/default/rpi-eeprom-update << EOF
FIRMWARE_RELEASE_STATUS="stable"
EOF
```

```bash
cd /lib/firmware/raspberrypi/bootloader/stable
sudo rpi-eeprom-update -d -f pieeprom-2021-02-16.bin
```

```bash
sudo reboot
```

```bash
vcgencmd bootloader_version

Feb 16 2021 13:23:36
version d6d82cf99bcb3e9a166a34cfde53130957a36bd3 (release)
timestamp 1613481816
update-time 1615091055
capabilities 0x0000001f
```

boot order는 Default가 0xf41, Booting순서가 SD다음 USB입니다.
SD card를 제거할 예정이기 때문에 바꾸지 않아도 됩니다.


### ubuntu

balencher

### modify

[링크](https://www.raspberrypi.org/forums/viewtopic.php?t=278791)

```bash
lsblk

NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda           8:0    0  1.8T  0 disk
|-sda1        8:1    0  256M  0 part
`-sda2        8:2    0  2.8G  0 part
mmcblk0     179:0    0 14.9G  0 disk
|-mmcblk0p1 179:1    0  256M  0 part /boot
`-mmcblk0p2 179:2    0 14.6G  0 part /
```

```bash
sudo su
```

```bash
mkdir -p /mnt/boot && mount /dev/sda1 /mnt/boot
mkdir -p /mnt/principal && mount /dev/sda2 /mnt/principal/
```

```bash
zcat /mnt/boot/vmlinuz > /mnt/boot/vmlinux
```
```bash
cat > /mnt/boot/config.txt << EOF
# Please DO NOT modify this file; if you need to modify the boot config, the
# "usercfg.txt" file is the place to include user changes. Please refer to
# the README file for a description of the various configuration files on
# the boot partition.

# The unusual ordering below is deliberate; older firmwares (in particular the
# version initially shipped with bionic) don't understand the conditional
# [sections] below and simply ignore them. The Pi4 doesn't boot at all with
# firmwares this old so it's safe to place at the top. Of the Pi2 and Pi3, the
# Pi3 uboot happens to work happily on the Pi2, so it needs to go at the bottom
# to support old firmwares.

[pi4]
max_framebuffers=2
dtoverlay=vc4-fkms-v3d
boot_delay
kernel=vmlinux
initramfs initrd.img followkernel

[pi2]
kernel=uboot_rpi_2.bin

[pi3]
kernel=uboot_rpi_3.bin

[all]
arm_64bit=1
device_tree_address=0x03000000

# The following settings are "defaults" expected to be overridden by the
# included configuration. The only reason they are included is, again, to
# support old firmwares which don't understand the "include" command.

enable_uart=1
cmdline=cmdline.txt

include syscfg.txt
include usercfg.txt
EOF
```


```bash
cat > /mnt/boot/auto_decompress_kernel << \EOF
#!/bin/bash -e

#Set Variables
BTPATH=/boot/firmware
CKPATH=$BTPATH/vmlinuz
DKPATH=$BTPATH/vmlinux

#Check if compression needs to be done.
if [ -e $BTPATH/check.md5 ]; then
	if md5sum --status --ignore-missing -c $BTPATH/check.md5; then
	echo -e "\e[32mFiles have not changed, Decompression not needed\e[0m"
	exit 0
	else echo -e "\e[31mHash failed, kernel will be compressed\e[0m"
	fi
fi

#Backup the old decompressed kernel
mv $DKPATH $DKPATH.bak

if [ ! $? == 0 ]; then
	echo -e "\e[31mDECOMPRESSED KERNEL BACKUP FAILED!\e[0m"
	exit 1
else 	echo -e "\e[32mDecompressed kernel backup was successful\e[0m"
fi

#Decompress the new kernel
echo "Decompressing kernel: "$CKPATH".............."

zcat $CKPATH > $DKPATH

if [ ! $? == 0 ]; then
	echo -e "\e[31mKERNEL FAILED TO DECOMPRESS!\e[0m"
	exit 1
else
	echo -e "\e[32mKernel Decompressed Succesfully\e[0m"
fi

#Hash the new kernel for checking
md5sum $CKPATH $DKPATH > $BTPATH/check.md5

if [ ! $? == 0 ]; then
	echo -e "\e[31mMD5 GENERATION FAILED!\e[0m"
	else echo -e "\e[32mMD5 generated Succesfully\e[0m"
fi

#Exit
exit 0
EOF
```

```bash
chmod +x auto_decompress_kernel
```

```bash
cat > /mnt/principal/etc/apt/apt.conf.d/999_decompress_rpi_kernel << EOF
DPkg::Post-Invoke {"/bin/bash /boot/firmware/auto_decompress_kernel"; };
EOF
```

```bash
chmod +x 999_decompress_rpi_kernel
```

```bash
poweroff
```

ubuntu / ubuntu
```bash
sudo apt update && sudo apt full-upgrade -y
```

### poe fan control

[링크](https://jjj.blog/2020/06/raspberry-pi-4-ubuntu-20-04-poe-hat-fan-control/)

```bash
cat > /etc/udev/rules.d/50-rpi-fan.rules << EOF
SUBSYSTEM=="thermal"
KERNEL=="thermal_zone3"

# If the temp hits 81C, highest RPM
ATTR{trip_point_0_temp}="82000"
ATTR{trip_point_0_hyst}="3000"
#
# If the temp hits 80C, higher RPM
ATTR{trip_point_1_temp}="81000"
ATTR{trip_point_1_hyst}="2000"
#
# If the temp hits 70C, higher RPM
ATTR{trip_point_2_temp}="71000"
ATTR{trip_point_2_hyst}="3000"
#
# If the temp hits 60C, turn on the fan
ATTR{trip_point_3_temp}="61000"
ATTR{trip_point_3_hyst}="5000"
#
# Fan is off otherwise
EOF
```

```
hostnamectl set-hostname rpi-node1
```