---
layout: post
title:  "Enable adoptive storage on Sony Xperia M4Aqua"
date:   2017-03-31 09:44:26 +0700
author: Dede Dindin Qudsy
tags:   [android,sony,xperia,m4aqua,adoptive storage]
---
Adoptive storage is with a simple explanation is make your SDcard shared with internal storage to make it bigger.

well, if you want to read more :
 - [greenbot](http://www.greenbot.com/article/3039136/android/adoptable-storage-in-android-6-0-what-it-is-how-it-works.html)
 - [Android](https://source.android.com/devices/storage/adoptable.html)

Sony comes up with Marshmallow firmware for Xperia M4Aqua, but it didn't implemented adoptive storage ( i mean, come on sony, m4aqua just have 8GB storage and fresh install just leave around 2GB left >.<). 

But yes, fortunately sony didn't completely remove adoptive feature, with adb shell you can still use it. You need fast SDcard, the faster the better. Backup you sdcard data and format it with fat32.

You need to enable USB debugging on Developer options first, then do the following command :

{% highlight shell %}
 # check your device connected or not
 $ adb devices
 # login to your phone shell
 $ adb shell
 
 $ sm list-disks
   disk:179,64
 $ sm partition disk:179,64 mixed 50
{% endhighlight %}
 - ``mixed 50`` it's mean your sdcard will be halved, half for your adoptive and half for your sdcard, if you want smaller adoptive use ``mixed 70`` that mean, 30 for adoptive and 70 for your sdcard. 
 - if you want to remove adoptive, use ``sm partition disk:diskid public``

<a href="http://imgur.com/DOQzDWx"><img src="http://i.imgur.com/DOQzDWx.png" title="source: imgur.com" height="50%" width="50%"/></a>
