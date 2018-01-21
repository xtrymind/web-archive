---
layout: post
comments: true
title:  "Use BetterBatteryStats on unrooted Xiaomi Devices"
date:   2018-01-22 06:46:00 +0700
author: Dede Dindin Qudsy
---
[BetterBatteryStats](https://play.google.com/store/apps/details?id=com.asksven.betterbatterystats) is one of my must install application on Android. because it can tell me what app that has been sucked my battery. if you know what culprit to app that has been sucked you, you can disable it.

if you have root on your phone, then it's piece of cake, BBS will ask you to grant permission to view battery, but if you unrooted ( which is me because, **** you xiaomi ) you need to use adb a little to grant requested permission. 

On xiaomi device, you need to enable usb debugging and usb debugging (Security Settings) first. go to about phone-> tap on MIUI version 7 times-> then go to Additional settings-> developer options-> and toggle usb debugging and usb debugging(security settings)

![Imgur](https://i.imgur.com/Y45qr2Q.png){:height="60%" width="60%"}

then connect to your PC open terminal and type 
{% highlight shell_session %}
users $ adb shell
{% endhighlight %}
if you have connect to your device, type

{% highlight shell_session %}
mido:/ $ pm grant com.asksven.betterbatterystats android.permission.BATTERY_STATS
mido:/ $ pm grant com.asksven.betterbatterystats android.permission.DUMP
mido:/ $ pm grant com.asksven.betterbatterystats android.permission.PACKAGE_USAGE_STATS
{% endhighlight %}

that's it, you can view in BBS what app has been sucked you battery

![Imgur](https://i.imgur.com/1YHFLUE.png){:height="60%" width="60%"}
