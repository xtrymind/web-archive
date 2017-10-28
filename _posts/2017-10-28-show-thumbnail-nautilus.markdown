---
layout: post
comments: true
title:  "Show Thumbnail on nautilus"
date:   2017-10-28 11:37:00 +0700
author: Dede Dindin Qudsy
---
based on [arch wiki](https://wiki.archlinux.org/index.php/GNOME/Files#Thumbnailing_not_working_for_video_files), "There is an ongoing issue with totem (the video thumbnailer for Nautilus) on computers with Intel GPUs. It will crash if gstreamer-vaapi is installed". to resolve this, install ``ffmpegthumbnailer`` and ``gst-libav``

{% highlight shell %}
 $ sudo pacman -S ffmpegthumbnailer gst-libav
 $ rm -r ~/.cache/thumbnails/fail
 $ killall nautilus
{% endhighlight %}

thumbnail will be generated now.



