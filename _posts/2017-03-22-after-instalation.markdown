---
layout: post
title:  "After installation"
date:   2017-03-22 08:11:26 +0700
categories: jekyll update
---
## After installation

### Connect to the internet 
(Wi-fi)
```
# wifi-menu
```
(Eth)
```
# ip link
# systemctl enable dhcpcd@enp3s0
# systemctl start dhcpcd@enp3s0
```

### Install zsh

```
# pacman -S zsh zsh-completions zsh-syntax-highlighting
```

### Create user

```
$ useradd -m -g users -G wheel -s /bin/zsh xtrymind
$ passwd xtrymind
```
### Enable sudo for user

```
# EDITOR=nano visudo
```
Un-comment this line in this file:
```
%wheel ALL=(ALL) ALL
```
### enable 32bit support
In `/etc/pacman.conf`, uncomment:

```
[multilib]
Include = /etc/pacman.d/mirrorlist
```
```
# pacman -Sy
```

### Install X

Laptop trackpad support: `# pacman -S xf86-input-libinput`

```
# pacman -S xorg-server xorg-server-utils xorg-xinit 
```

### Dynamic switching between NVIDA and Intel integrated graphics

```
$ sudo pacman -S mesa xf86-video-intel nvidia bbswitch bumblebee lib32-nvidia-utils lib32-mesa
$ sudo gpasswd -a xtrymind bumblebee
$ sudo systemctl enable bumblebeed
```

Edit `/etc/default/grub`, set `GRUB_CMDLINE_LINUX_DEFAULT="quiet rcutree.rcu_idle_gp_delay=1"`.

Edit `/etc/bumblebee/xorg.conf.nvidia`, uncomment `BusID`. check where the PCI should be the one shown for the Nvidia card in `lspci`.

To turn on Nvidia graphics card:

```
$ sudo tee /proc/acpi/bbswitch <<< ON
$ sudo modprobe nvidia
```

To turn it off:

```
$ sudo modprobe -r nvidia
$ sudo tee /proc/acpi/bbswitch <<< OFF
```

To get information about PCI devices (Shows if Nvidia card is activeted or deactivated and the driver it's using):

```
$ lspci -v
$ lspci -k
$ lspci
```

Test if Bumblebee is working:

```
$ pacman -S xorg-twm xorg-xclock xterm mesa-demos
$ startx
$ optirun glxgears -info
```

### SSD
Enable trim support
```
$ sudo systemctl enable fstrim.timer
```

### Install Samba

```
$ sudo pacman -S samba
$ sudo cp /etc/samba/smb.conf.default /etc/samba/smb.conf
$ sudo systemctl enable smbd nmbd
$ sudo nano /etc/samba/smb.conf
```
Share home directory for each user, add
```
[homes]
    comment = Home Directories
    browseable = no
    writable = yes
+   valid users = %S
```
Share directory with multiple user
```
[share]
   comment = Share directory
   path = /var/lib/share
   read only = no
   guest only = no
   guest ok = no
   share modes = yes
```

```
$ sudo pdbedit -a xtrymind
$ sudo mkdir /var/lib/share
$ sudo chmod 0777 /var/lib/share
$ sudo systemctl start smbd nmbd
```

#### Pacaur

Install AUR packages.

```
gpg --recv-keys --keyserver hkp://pgp.mit.edu 1EB2638FF56C0C53
```
- [cower](https://aur.archlinux.org/cgit/aur.git/snapshot/cower.tar.gz)
- [pacaur](https://aur.archlinux.org/packages/pacaur/)

#### HDAPSD

Protect disk.

```
$ pacman -S hdapsd
$ systemctl enable hdapsd
```
#### Other stuff
```
$ pacaur -y ttf-google-fonts-git wd719x-firmware aic94xx-firmware
```
```
$ sudo pacman -S terminator chromium openssh git reflector ntfs-3g ttf-dejavu noto-fonts-cjk \
               mcomix qbittorrent uget aria2 vlc qt4 reflector alsa-utils
```
```
$ sudo reflector --latest 200 --protocol http --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```
