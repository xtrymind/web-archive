---
layout: post
comments: true
title:  "Alternative way to set up python2 for Android Build on Arch Linux"
date:   2017-04-09 15:45:00 +0700
author: Dede Dindin Qudsy
tags:   [android,archlinux]
last_modified_at: 2017-04-09 13:53:00 +0700
---
Well, there's no much to write here beside some alternative way to set up python 2.

arch linux guide on set up android build tool [here](https://wiki.archlinux.org/index.php/Android#Building_Android) is pretty much detailed on how to do it.
but since android still use python2, i have a little problem on set up python2 with virtual environment ( i don't know it's only me or other have a same problem),
since it's like that, i'm using alternative way to set up python2 :

{% highlight shell %}
 $ sudo mkdir /opt/android-build
 $ cd /opt/android-build
 $ sudo ln -s $(which python2) python
{% endhighlight %}

then put this on build script or paste on current terminal or in .zshrc or .bashrc
{% highlight shell %}
 export PATH="/opt/android-build:$PATH"
{% endhighlight %}

source :
 - [Arch Wiki](https://wiki.archlinux.org/index.php/Android)
 - [XDA Developers](https://forum.xda-developers.com/showthread.php?t=2259929)
