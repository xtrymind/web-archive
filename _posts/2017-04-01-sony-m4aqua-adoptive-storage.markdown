---
layout: post
title:  "Enable adoptive storage on Sony Xperia M4Aqua"
date:   2017-04-01 07:39:26 +0700
author: Dede Dindin Qudsy
tags:   [android,sony,xperia,m4aqua,adoptive storage]
last_modified_at: 2017-04-06 10:53:00 +0700
---
Adoptive storage is with a simple explanation is make your SDcard shared with internal storage to make it bigger.

well, if you want to read more :
 - [Android](https://source.android.com/devices/storage/adoptable.html)
 - [greenbot](http://www.greenbot.com/article/3039136/android/adoptable-storage-in-android-6-0-what-it-is-how-it-works.html)

Sony comes up with Marshmallow (there won't be Nougat) firmware for Xperia M4Aqua, but it didn't implemented adoptive storage ( i mean, come on sony, some variant m4aqua just have 8GB storage and fresh installed firmware just leave around 2GB left for data). 

But yes, fortunately sony didn't completely remove adoptive feature, with adb shell you can still use it. You need fast SDCard, the faster the better. Backup you sdcard data and format it with fat32.

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

Note :
 - ``mixed 50`` it's mean your sdcard will be halved, half for your adoptive and half for your sdcard, if you want smaller adoptive use ``mixed 70`` that mean, 30 for adoptive and 70 for your sdcard. 
 - if you want to remove adoptive, use ``sm partition disk:diskid public`` example ``sm partition disk:179,64 public``
 - doing ``sm partition`` command will format your sdcard, so do not forget to backup content of sdcard.
 - since some M4Aqua variant max storage is 16 GB, you should set your adoptive storage at 8 GB or less, in my experience set adoptive more than 8 GB, i haven't face any problem beside storage total size not counting properly like ``-811228283 GB``. But some people have problem when they set more than 8 GB.

<div class="img-pad">
<a href="http://imgur.com/u9QiztC"><img src="http://i.imgur.com/u9QiztC.png" title="source: imgur.com" /></a>
</div>
<div class="img-pad">
<a href="http://imgur.com/DOQzDWx"><img src="http://i.imgur.com/DOQzDWx.png" title="source: imgur.com" /></a>
</div>
