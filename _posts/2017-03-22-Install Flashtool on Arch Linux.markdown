---
layout: post
comments: true
title:  "Install Flashtool on Arch Linux"
date:   2017-03-22 10:37 +0700
author: Dede Dindin Qudsy
tags:   [archlinux, flashtool, sony, android, xperia, m4aqua]
last_modified_at: 2017-09-22 08:16:00 +0700
---
Flashtool is a great tool to flash a firmware for your Sony Phone (and sony erriccson too) in case a new firmware come out and you don't want to wait to receive it over OTA or you soft brick your phone.

It includes XperiaFirm to download firmware instead of create bundle from sony software updates(on windows).

#### Aur
Manual way 

clone aur ``git clone https://aur.archlinux.org/xperia-flashtool.git``
{% highlight bash %}
 $ cd xperia-flashtool
 $ makepkg -si
{% endhighlight %}

Using Pacaur
{% highlight bash %}
 $ pacaur -S xperia-flashtool
{% endhighlight %}

#### Manual
Download Flashtool for linux at <a href="http://www.flashtool.net/downloads_linux.php">here</a>

unzip using command
{% highlight bash %}
 $ 7z x flashtool-0.9.23.1-linux.tar.7z && tar -xvf flashtool-0.9.23.1-linux.tar
{% endhighlight %}

create ``/etc/udev/rules.d/63-sonyxperia.rules``
{% highlight bash %}
 SUBSYSTEM=="usb", ACTION=="add", SYSFS{idVendor}=="0fce", SYSFS{idProduct}=="*", MODE="0777"
{% endhighlight %}

restart udev
{% highlight bash %}
 sudo udevadm trigger
{% endhighlight %}

And then just cd to flashtool directory and start with sudo ./Flashtool

<a href="https://xtrymind.files.wordpress.com/2017/03/screenshot-from-2017-03-10-07-29-50.png"><img class="alignnone size-full wp-image-630" src="https://xtrymind.files.wordpress.com/2017/03/screenshot-from-2017-03-10-07-29-50.png" alt="" width="739" height="415" /></a>
