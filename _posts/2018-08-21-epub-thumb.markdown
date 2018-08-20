---
layout: post
comments: true
title:  "Epub Thumbnailer"
date:   2018-08-21 05:30:00 +0700
author: Dede Dindin Qudsy
last_modified_at: 2018-08-21 05:30:00 +0700
---
I have found this [epub thumbnailer](https://github.com/marianosimone/epub-thumbnailer) by chance and immediately fall in love as i have lot's of epub on my drive without cover on it and regrets why i didn't meet this script sooner.

To install it pretty easy, just run `sudo python install.py install` and viola, it's done. Supported DE are :
* Gnome
* xfce
* mate
* unity
* openbox
* enlightenment
* i3

if you use archlinux, then use python2 `sudo python2 install.py install` and modify `src/epub-thumbnailer.py` to use python2 or you could use arch wiki way [Dealing with version problem in build scripts](https://wiki.archlinux.org/index.php/Python#Dealing_with_version_problem_in_build_scripts). Either way works fine with me.
```diff
diff --git a/src/epub-thumbnailer.py b/src/epub-thumbnailer.py
index 93e2eac..5984050 100755
--- a/src/epub-thumbnailer.py
+++ b/src/epub-thumbnailer.py
@@ -1,4 +1,4 @@
-#!/usr/bin/python
+#!/usr/bin/python2
 
 #  This program is free software: you can redistribute it and/or modify
 #  it under the terms of the GNU General Public License as published by
```

Screenshot: 
![Imgur](https://i.imgur.com/VrLvwhf.png)
