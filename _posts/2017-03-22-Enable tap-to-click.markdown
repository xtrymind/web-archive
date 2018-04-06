---
layout: post
comments: true
title:  "Enable tap-to-click"
date:   2017-03-22 09:30:26 +0700
author: Dede Dindin Qudsy
tags:   [archlinux, touchpad, libinput]
last_modified_at: 2018-04-06 06:12:00 +0700
desc_update: "change config"
---
Sometimes, it doesn't feel nice if you use touchpad and can't use tap to click, and for me, yes it's annoying to click button.

you need to install ``libinput`` because ``synaptics`` ( based on Arch Wiki ) is in maintenance mode and is no longer updated.
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

To enable it in current session without restart:
```shell_session
users $ sudo libinput-list-devices  
users $ xinput list-props "your touchpad devices"  
Enable tap-click  
users $ xinput set-prop "your touchpad devices" "libinput Tapping Enabled" 1
```
beside with ``xorg.conf.d``, you can automatic enabled it on boot by set ``xinput set-prop`` on ``xinitrc`` or autostart program you use.
