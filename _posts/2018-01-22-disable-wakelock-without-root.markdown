---
layout: post
comments: true
title:  "Disable wakelock without root on Xiaomi Devices"
date:   2018-01-22 12:57:00 +0700
author: Dede Dindin Qudsy
last_modified_at: 2018-01-25 019:55:00 +0700
desc_update: "fix typo all over the place"
---
After succesfully configure [BetterBatteryStats](use-bbs-non-root.html), it's time to see what apps that has lots of wakelocks. Open BetterBatteryStats and go over Partial Wakelocks, you will see an apps that have wakelocks there.

Example on my phone, six hour after installing BetterBatteryStats, Twitter apps have a lots of wakelocks for over 4 minutes, we can see it activity is analytics so we can safely disable this wakelocks.

![Imgur](https://i.imgur.com/vadkMvL.png){:height="60%" width="60%"}

open cmd (if you're on windows) or terminal (if you're on linux) and enter adb shell, after that type this command:
{% highlight shell_session %}
users $ cmd appops set com.android.application WAKE_LOCK ignore
{% endhighlight %}

for Twitter apps, it should be 
{% highlight shell_session %}
users $ cmd appops set com.twitter.android WAKE_LOCK ignore
{% endhighlight %}

after that, all wakelocks requests by the app or in case above Twitter app, will be ignored by the Android system

![Imgur](https://i.imgur.com/EOR9nA8.png){:height="60%" width="60%"}

Source: [XDA](https://www.xda-developers.com/stop-wakelocks-android-without-root/)
