---
layout: post
comments: true
title:  "Hide windows partitions"
date:   2017-03-22 10:53 +0700
author: Dede Dindin Qudsy
---
some people say it doesn't matter but some people say it's matter, YES.

create ``/etc/udev/rules.d/99-hide-ntfs-partitions.rules``
```conf
KERNEL=="sda4", ENV{UDISKS_IGNORE}="1" 
```

which `sda4` is windows partition you want to hide

```shell_session
users $ sudo udevadm trigger 
```

