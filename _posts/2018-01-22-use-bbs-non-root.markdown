---
layout: post
comments: true
title:  "Use BetterBatteryStats on unrooted Xiaomi Devices"
date:   2018-01-22 06:46:00 +0700
author: Dede Dindin Qudsy
last_modified_at: 2018-04-12 09:31:00 +0700
desc_update: "fix writing style"
---
[BetterBatteryStats](https://play.google.com/store/apps/details?id=com.asksven.betterbatterystats) / [XDA Version](https://forum.xda-developers.com/showthread.php?t=1179809) is one of my must install application on Android. Simply because it can tell me complete history on battery.

If you have rooted your phone, then it's piece of cake, BetterBatterStats( BBS ) will ask you to grant permission to view battery, but if you unrooted ( which is me because, **** you xiaomi ) you need to use adb to grant requested permission. 

On xiaomi device, you need to enable usb debugging and usb debugging (Security Settings) first. Go to about phone -> tap on MIUI version 7 times -> then go to Additional settings -> developer options -> and toggle usb debugging and usb debugging(security settings). To enable usb debugging(seccurity settings) you need to login to your xiaomi account.

![Imgur](https://i.imgur.com/Y45qr2Q.png){:height="60%" width="60%"}

open cmd (if you're on windows) or terminal (if you're on linux) and enter adb shell, after that type this command:

{% highlight shell %}
$ pm grant com.asksven.betterbatterystats android.permission.BATTERY_STATS
$ pm grant com.asksven.betterbatterystats android.permission.DUMP
$ pm grant com.asksven.betterbatterystats android.permission.PACKAGE_USAGE_STATS
{% endhighlight %}

Exit adb shell and open BBS Apps, and you can now see detailed battery history.

![Imgur](https://i.imgur.com/1YHFLUE.png){:height="60%" width="60%"}
