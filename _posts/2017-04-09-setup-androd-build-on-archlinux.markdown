---
layout: post
comments: true
title:  "Alternative way to set up python2 for Android Build on Arch Linux"
date:   2017-04-09 15:45:00 +0700
author: Dede Dindin Qudsy
tags:   [android,archlinux]
last_modified_at: 2017-10-28 11:58:00 +0700
desc_update: "change android build to python2"
---

EDIT :
there's dirty trick on arch wiki to fix problem on [venv](https://wiki.archlinux.org/index.php/Android#Setting_up_the_build_environment)

Well, there's not much to write here.

arch linux guide on set up android build tool [here](https://wiki.archlinux.org/index.php/Android#Building_Android) is pretty much detailed on how to do it.
but since android still use python2, and arch use python3 by default, you will have a little problem using python2 with venv.

since it's like that, i'm using this trick to set up python2 :

{% highlight shell %}
 $ sudo mkdir /opt/python2
 $ cd /opt/python2
 $ sudo ln -s $(which python2) python
{% endhighlight %}

then put this on build script or paste on current terminal or in .zshrc or .bashrc
{% highlight shell %}
 export PATH="/opt/python2:$PATH"
{% endhighlight %}
 
source :
 - [Arch Wiki](https://wiki.archlinux.org/index.php/Android)
 - [XDA Developers](https://forum.xda-developers.com/showthread.php?t=2259929)
