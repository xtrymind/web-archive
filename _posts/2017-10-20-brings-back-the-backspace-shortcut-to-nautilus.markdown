---
layout: post
comments: true
title:  "Bring back the backspace shortcut to nautilus"
date:   2017-10-20 17:55:00 +0700
author: Dede Dindin Qudsy
---
On windows explorer when you want to go back to last place you have been, you'd press backspace on keyboard. 

On nautilus or gnome files, they set that shortcut by alt + arrow left, i'd like it to be one key or backspace again, but unfortunatelly nautilus didn't offer to change shortcut. 

To change it to backspace, github user [riclc](https://github.com/riclc) create python extension to [Brings back the Backspace shortcut to Nautilus](https://github.com/riclc/nautilus_backspace) using nautilus python.

{% highlight shell %}
 $ sudo pacman -S python2-nautilus
 # on ubuntu it command apt-get install python-nautilus
{% endhighlight %}

download ``BackspaceBack.py`` on link above and put it in ``.local/share/nautilus-python/extensions/`` and then restart Nautilus ``killall nautilus``




