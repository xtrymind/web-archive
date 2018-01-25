---
layout: post
comments: true
title:  "Unlock Bootloader Xiaomi Redmi Note 4"
date:   2018-01-17 09:08:00 +0700
author: Dede Dindin Qudsy
---
Got Xiaomi Redmi Note 4 to replace my dead Sony Xperia M4 Aqua, which i got for IDR 2.275.000 for 3/32 version. The first thing i want on new android phone is of course to have unlocked bootloader, but unfortunatelly ( i didn't read anything about unlock bootloader on xiaomi before as i thought it will be easy based on lot's of kernel and rom development) it's not simple as it be.

first, you need to apply request for unlock bootloader on [en.miui.com/unlock](http://en.miui.com/unlock/) and you need to give a good reason for why you want to unlock your xiaomi phone bootloader. as it need to be checked it need quite some times to get approved, sometimes it need only minutes, or sometimes hours, or sometimes days even weeks. but there are who get request rejected too. I'm lucky to get approval only 3 minutes.



see which partitions have been encrypted
{% highlight shell_session %}
root # blkid | grep crypto
/dev/sdc2: UUID="68b5a97c-9e39-4713-946b-4325ffbeec88" TYPE="crypto_LUKS" PARTUUID="fd09a229-c4fc-4f5d-b378-8bdcaf8e4d57"
{% endhighlight %}

open luks partition and set up mapper name
{% highlight shell_session %}
root # cryptsetup luksOpen /dev/sdc2/ vg0
Enter passphrase for /dev/sdc2:
{% endhighlight %}

run `lsblk` to see partitions layout
{% highlight shell_session %}
root # lsblk
NAME           MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINT
sdc              8:32   0  74.5G  0 disk  
├─sdc1           8:33   0   512M  0 part  
└─sdc2           8:34   0    74G  0 part  
  └─vg0        254:0    0    74G  0 crypt 
    ├─vg0-swap 254:1    0     8G  0 lvm   
    └─vg0-root 254:2    0    66G  0 lvm
{% endhighlight %}

mount `vg0root`
{% highlight shell_session %}
root # mount /dev/mapper/vg0-root /home/user/vg0-root/
{% endhighlight %}

after you have done copying or modifying file on encrypted partitions, umount partition and close luks with command
{% highlight shell_session %}
root # cryptsetup close vg0
{% endhighlight %}
