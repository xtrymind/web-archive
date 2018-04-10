---
layout: post
comments: true
title:  "Dualboot archlinux & windows 10 on Asus A46CB"
date:   2017-03-23 14:20:26 +0700
author: Dede Dindin Qudsy
tags:   [archlinux, windows, Linux]
---
`incomplete, need a lot of thing to do`

**Table of Contents**

- [Archlinux on Asus A46CB](#archlinux-on-asus-a46cb)
  - [Before](#before)
  - [Partitioning](#partitioning)
    - [Format and mount disks](#format-and-mount-disks)
  - [Install base system](#install-base-system)
  - [Configure base system](#configure-base-system)
	- [Chroot](#chroot)
	- [Bootloader](#bootloader)
	- [Network](#network)
	- [Reboot](#reboot)
  - [After installation](#after-installation)
	- [Connect to the internet](#connect-to-the-internet)
	- [User Management](#user-management)
	- [Arch User Repository](#arch-user-repository)
	- [Mirror](#mirror)
	- [Graphics](#graphics)
	  - [Driver](#driver)
	  - [Xorg](#xorg)
	  - [Bumblebee](#bumblebee)
	- [Terminal](#terminal)
	- [Touchpad](#touchpad)
	- [Sound](#sound)
	- [Hard Drive](#disk)
	  - [SSD](#ssd)
	  - [HDAPSD](#hdapsd)
	
# Archlinux on Asus A46CB
### Before

1. Disable Windows Fast-Startup
2. Disable Secure Boot

### Partitioning
an example of Windows 10 Efi partitioning on 120GB SSD

| Partition  | Location | Size       | File system |
| ---------- |:--------:|:----------:|:-----------:|
| Recovery   | sda1     | 450 MB     | ntfs        |
| ESP        | sda2     | 100 MB     | vfat        |
| Reserved   | sda3     | 16 MB      | ?           |
| Windows 10 | sda4     | 53 GB      | ntfs        |
| Arch       | sda5     | 58 GB      | ext4        |

#### Format and mount disks

```shell_session
root # mkfs.ext4 /dev/sda5
root # mount /dev/sda5 /mnt
root # mkdir /mnt/boot
root # mount /dev/sda2 /mnt/boot
```
### Install base system

```shell_session
root # wifi-menu
check connections
root # ping  -c 3 www.google.com
```

install system and generate fstab

```shell_session
install base base development zsh and vim to /mnt 
root # pacstrap /mnt base base-devel zsh vim
generate new fstab
root # genfstab -U /mnt >> /mnt/etc/fstab
```


### Configure base system
#### Chroot
```shell_session
enter chroot
root # arch-chroot /mnt

password for root
root # passwd

timezone
root # ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
Hardware clock
root # hwclock --systohc

Locale
root # vim /etc/locale.gen
uncomment `en_US.UTF-8` and `en_GB.UTF-8` for metric system
root # locale-gen
root # echo LANG=en_US.UTF-8 > /etc/locale.conf
root # export LANG=en_US.UTF-8

Hostname
root # echo arch > /etc/hostname
```
```conf
# edit /etc/hosts
 127.0.0.1   localhost.localdomain   localhost
 ::1         localhost.localdomain   localhost
 127.0.1.1   arch.localdomain        arch
```

#### Bootloader
```shell_session
install intel ucode if you have intel processor
root # pacman -S intel-ucode

Initial ramdisk environment
root # mkinitcpio -p linux

install bootloader, using systemd-boot
root # bootctl --path=/boot install

check partuuid of / partition
root # blkid -s PARTUUID -o value /dev/sdxY
```

```conf
# Then add following content to /boot/loader/entries/arch.conf
tittle    Arch
linux     /vmlinuz-linux
initrd    /initramfs-linux.img
initrd    /intel-ucode.img
options   root=PARTUUID=xxxxx-xxx-xx rw
```
```conf
# change entries on /boot/loader/loader.conf
timeout 5
default arch
```

#### Network

```shell_session
Network configuration (Wi-Fi)
root # pacman -S wpa_supplicant networkmanager dialog
root # systemctl enable NetworkManager
```
Asus A46CB use Qualcomm Atheros AR9485 as Wi-Fi chipset, in my experience, if you connect to public wi-fi which you need to login first on web, you will get some random disconnect and decrease bandwidth, to resolve just add `options ath9k nohwcrypt=1` to `/etc/modprobe.d/ath9k.conf `
```shell_session
root # echo options ath9k nohwcrypt=1 >> /etc/modprobe.d/ath9k.conf 
```
#### Reboot
```shell_session
root # exit
root # umount -R /mnt
root # reboot
```

### After installation

#### Connect to the internet
```shell_session
root # nmtui
root # ping google.co.id
```

#### User Management
```shell_session
add user
root # useradd -m -G wheel,users -s /bin/zsh xtrymind
root # passwd xtrymind

enable sudo for users
root # EDITOR=vim visudo
```
```conf
# Uncomment this line in vim:
%wheel ALL=(ALL) ALL
```

#### Mirror
```shell_session
best mirror
users $ sudo pacman -S reflector
users $ sudo reflector --latest 200 --protocol http --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```
```conf
# enable 32bit support
# In `/etc/pacman.conf`, uncomment:
[multilib]
Include = /etc/pacman.d/mirrorlist
```
```shell_session
users $ sudo pacman -Sy
```

#### Display

#### Driver
```shell_session
users $ sudo pacman -S nvidia mesa xf86-video-intel
```

#### Xorg
```shell_session
users $ sudo pacman -S xorg xorg-apps xorg-xinit xbindkeys
```

#### Bumblebee
```shell_session
users $ sudo pacman -S bbswitch bumblebee
users $ sudo gpasswd -a xtrymind bumblebee
users $ sudo systemctl enable bumblebeed
```

```shell_session
Reboot. After rebooting, you should see that bbswitch has disabled the card, which is good for saving the battery on the laptop
users $ cat /proc/acpi/bbswitch
0000:01:00.0 OFF
```

#### WM
install i3 as window manager
```shell_session
users $ sudo pacman -S i3
copy xinit to home dirrectory
users $ cp /etc/X11/xinit/xinitrc ~/.xinitrc
```
```conf
# open .xinitrc and delete started from twm & to exec xterm and add
exec i3
```

#### Terminal
rxvt-unicode or urxvt is a nice terminal emulator, it's small and fast but a little complicated to configure
```shell_session
users $ sudo pacman -S rxvt-unicode urxvt-perls
```

check [arch wiki](https://wiki.archlinux.org/index.php/Rxvt-unicode) for more detail on how to configure urxvt.

#### Touchpad
install libinput because xf86-input-synaptics ( based on Arch Wiki ) is in maintenance mode and is no longer updated.
```shell_session
users $ sudo pacman -S libinput xf86-input-libinput
```

add ``30-touchpad.conf`` to ``/etc/X11/xorg.conf.d/:``
```shell_session
users $ sudo vim /etc/X11/xorg.conf.d/30-touchpad.conf
```

```conf
Section "InputClass"
	Identifier "touchpad"
	Driver "libinput"
	MatchIsTouchpad "on"
	Option "Tapping" "on"
	Option "Natural Scrolling" "on"
	Option "Accel Speed" "0.5"
EndSection
```

#### Power Management
#### Powertop
install it by command
```shell_session
users $ sudo pacman -S powertop
```
and add systemd service, create ``/etc/systemd/system/powertop.service```
```conf
[Unit]
Description=Powertop tunings

[Service]
Type=oneshot
ExecStart=/usr/bin/powertop --auto-tune

[Install]
WantedBy=multi-user.target
```
then enable it
```shell_session
users $ sudo systemctl enable powertop.service
```

#### Laptop Mode
see this [kernel documentation](https://www.kernel.org/doc/Documentation/laptops/laptop-mode.txt) about laptop mode, it help minimize the time that the hard disk needs to be spun up, to conserve battery power on laptops.

create ``/etc/sysctl.d/laptop.conf``
and add :
```conf
vm.laptop_mode = 5
```

#### Backlight

The display’s backlight is a huge power drain, and it is often convenient to have a hotkey to adjust it.

```shell_session
users $ sudo pacman -S xorg-backlight
```
add ``20-intel.conf`` to ``/etc/X11/xorg.conf.d/:``
```shell_session
users $ sudo vim /etc/X11/xorg.conf.d/20-intel.conf
```
```conf
Section "Device"
	Identifier  "Intel Graphics"
	Driver      "intel"
	Option      "DRI" "2"             # DRI3 is now default
	Option 		"TearFree" "true"
	#Option      "AccelMethod"  "sna" # default
	#Option      "AccelMethod"  "uxa" # fallback
EndSection
```
Now, add commands to xbindkeys for manipulating the backlight:

```conf
# Backlight Inc
"/usr/bin/xbacklight -inc 5"
    m:0x0 + c:233
    XF86MonBrightnessUp

# Backligth Dec
"/usr/bin/xbacklight -dec 5"
    m:0x0 + c:232
    XF86MonBrightnessDown
```

#### Sound

Just install ``alsa-utils``, and use ``alsamixer`` to unmute the master channel. Should just work.

For keyboard hotkeys, add the following to ``xbindkeys`` configuration:
```conf
# Volume Up
"/usr/bin/amixer set Master 5%+"
   m:0x0 + c:123
   XF86AudioRaiseVolume

# VOlume Down
"/usr/bin/amixer set Master 5%-"
   m:0x0 + c:122
   XF86AudioLowerVolume

# Mute
"/usr/bin/amixer set Master toggle"
   m:0x0 + c:121
   XF86AudioMute
```
#### Disk
#### SSD
Enable trim support
```shell_session
users $ sudo systemctl enable fstrim.timer
```
#### HDAPSD

Protects your hard drive from sudden shocks

```shell_session
users $ sudo pacman -S hdapsd
users $ sudo systemctl enable hdapsd
```


