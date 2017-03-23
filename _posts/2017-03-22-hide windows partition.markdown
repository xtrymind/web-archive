---
layout: post
title:  "Hide windows partitions"
date:   2017-03-22 10:53 +0700
author: Dede Dindin Qudsy
---
create ``/etc/udev/rules.d/99-hide-ntfs-partitions.rules``
{% highlight bash %}
KERNEL=="sda4", ENV{UDISKS_IGNORE}="1" 
{% endhighlight %}

which `sda4` is windows partition you want to hide

{% highlight bash %}
$ sudo udevadm trigger 
{% endhighlight %}

