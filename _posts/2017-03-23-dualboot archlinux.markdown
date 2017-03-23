---
layout: post
title:  "Dualboot archlinux & windows 10 on Asus A46CB"
date:   2017-03-23 14:20:26 +0700
author: Dede Dindin Qudsy
tags:   [archlinux, windows, Linux]
---
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
	  - [Openbox](#openbox)
	- [Sound](#sound)
	- [Hard Drive](#disk)
	  - [SSD](#ssd)
	  - [HDAPSD](#hdapsd)
	
# Archlinux on Asus A46CB
### Before

1. Disable Windows Fast-Startup
2. Disable Secure Boot

### Partitioning
Windows 10 Efi partitioning

| Partition  | Location | Size       | File system |
| ---------- |:--------:|:----------:|:-----------:|
| Recovery   | sda1     | 450 MB     | ntfs        |
| ESP        | sda2     | 100 MB     | vfat        |
| Reserved   | sda3     | 16 MB      | ?           |
| Windows 10 | sda4     | 53 GB      | ntfs        |
| Arch       | sda5     | 58 GB      | ext4        |

#### Format and mount disks

```
# mkfs.ext4 /dev/sda5
# mount /dev/sda5 /mnt
# mkdir /mnt/boot
# mount /dev/sda2 /mnt/boot
```
### Install base system

{% highlight bash %}
 wifi-menu
 # check connections
 ping  -c 3 www.google.com

 # install base system
 pacstrap /mnt base base-devel zsh
 # generate fstab
 genfstab -U /mnt >> /mnt/etc/fstab
{% endhighlight %}

### Configure base system
#### Chroot
{% highlight bash %}
 arch-chroot /mnt
 
 # password for root
 passwd
 
 # timezone
 ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
 # Hardware clock
 hwclock --systohc
 
 # Locale
 nano /etc/locale.gen
 # uncomment `en_US.UTF-8`
 locale-gen
 echo LANG=en_US.UTF-8 > /etc/locale.conf
 export LANG=en_US.UTF-8
 
 # Hostname
 echo arch >> /etc/hostname
 
 nano /etc/hosts
 # /etc/hosts should look like:

 127.0.0.1   localhost.localdomain   localhost 
 ::1         localhost.localdomain   localhost
 127.0.1.1   arch.localdomain        arch 
{% endhighlight %}

#### Bootloader
{% highlight bash %}
 # install intel ucode 
 pacman -S intel-ucode
 # Initial ramdisk environment
 mkinitcpio -p linux

 #grub support for mbr and gpt partitions, but if it's gpt, better use bootctl

 bootctl --path=/boot install

 #Then add following content to ``/boot/loader/entries/arch.conf``

 tittle    Arch
 linux     /vmlinuz-linux
 initrd    /initramfs-linux.img
 initrd    /intel-ucode.img
 options   root=/dev/sda6 rw

 #note : for ``options   root=`` it's better to use uuid

 # change entries on ``/boot/loader/loader.conf``

 timeout 5
 default arch
{% endhighlight %}

#### Network

{% highlight bash %}

 # Network configuration (Wi-Fi)
 pacman -S wpa_supplicant networkmanager
 systemctl enable NetworkManager
{% endhighlight %}
#### Reboot
{% highlight bash %}
 exit   
 umount -R /mnt/boot
 umount -R /mnt
 reboot
{% endhighlight %}

### After installation

#### Connect to the internet 
{% highlight bash %}
 nmtui
 ping google.co.id 
{% endhighlight %}

#### User Management
{% highlight bash %}
  useradd -m -g users -G wheel -s /bin/zsh xtrymind
  passwd xtrymind
  
 #enable sudo for users
 EDITOR=nano visudo
 #Un-comment this line in nano:
 %wheel ALL=(ALL) ALL
{% endhighlight %}

#### Arch User Repository
``pacaur`` is popular front end for installing aur package
{% highlight bash %}
 $ sh -c "$(curl -fsSL \
   https://raw.githubusercontent.com/xtrymind/dotfiles/master/pacaur.sh)"
{% endhighlight %}

#### Mirror
{% highlight bash %}
 # best mirror
 $ sudo pacman -S reflector
 $ sudo reflector --latest 200 --protocol http --protocol https \
   --sort rate --save /etc/pacman.d/mirrorlist
 
 # enable 32bit support
 # In `/etc/pacman.conf`, uncomment:

 [multilib]
 Include = /etc/pacman.d/mirrorlist

 $ sudo pacman -Sy
{% endhighlight %}

#### Graphics

#### Driver
{% highlight bash %}
 $ sudo pacman -S nvidia
{% endhighlight %}

Laptop trackpad support: `# pacman -S xf86-input-libinput`
#### Xorg
{% highlight bash %}
 $ sudo pacman -S xorg-server xorg-server-utils xorg-xinit xterm
{% endhighlight %}

#### Bumblebee
{% highlight bash %}
 $ sudo pacman -S mesa bbswitch bumblebee
 $ sudo gpasswd -a xtrymind bumblebee
 $ sudo systemctl enable bumblebeed
{% endhighlight %}

{% highlight bash %}
 # Reboot. After rebooting, you should see that bbswitch has disabled the card,
 # which is good for saving the battery on the laptop
 $ cat /proc/acpi/bbswitch
 0000:01:00.0 OFF
{% endhighlight %}

#### Openbox
{% highlight bash %}
 $ sudo pacman -S openbox tint2
 $ cp /etc/X11/xinit/xinitrc ~/.xinitrc
 $ cp -R /etc/xdg/openbox ~/.config
{% endhighlight %}
edit ``.xinitrc`` and add
{% highlight bash %}
 exec openbox-session
{% endhighlight %}

#### Sound

Just install ``alsa-utils``, and use ``alsamixer`` to unmute the master channel. Should just work.

For keyboard hotkeys, add the following to ``xbindkeys`` configuration:
{% highlight bash %}
 $ grep -A1 amixer ~/.xbindkeysrc
 "/usr/bin/amixer set Master 5%+"
    XF86AudioRaiseVolume
--
 "/usr/bin/amixer set Master 5%-"
    XF86AudioLowerVolume
--
 "/usr/bin/amixer set Master toggle"
    XF86AudioMute
{% endhighlight %}
#### Disk
#### SSD
Enable trim support
{% highlight bash %}
 $ sudo systemctl enable fstrim.timer
{% endhighlight %}
#### HDAPSD

Protects your hard drive from sudden shocks

{% highlight bash %}
$ pacman -S hdapsd
$ systemctl enable hdapsd
{% endhighlight %}

