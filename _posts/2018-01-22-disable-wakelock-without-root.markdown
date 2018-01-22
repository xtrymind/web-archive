---
layout: post
comments: true
title:  "Disable wakelock withour root on Xiaomi Devices"
date:   2018-01-22 12:57:00 +0700
author: Dede Dindin Qudsy
---
after succesfully configure [BetterBatteryStats](use-bbs-non-root.html), it's time to see what apps that has lots of wakelocks. Open BetterBatteryStats and go over Partial Wakelocks, you will see an apps that have wakelocks there.

example on my phone, six hour after installing BetterBatteryStats, Twitter apps have a lots of wakelocks for over 4 minutes, we can see it's activity is analytics so we can safely disable this wakelocks.
![Imgur](https://i.imgur.com/vadkMvL.png){:height="50%" width="50%"}

go to cmd/terminal again and enter adb shell. after that type
{% highlight shell_session %}
users $ cmd appops set com.android.application WAKE_LOCK ignore
{% endhighlight %}

for Twitter apps, it should be 
{% highlight shell_session %}
users $ cmd appops set com.twitter.android WAKE_LOCK ignore
{% endhighlight %}

and that's it, all wakelocks requests by the app will be ignored by the Android system

Source: [XDA](https://www.xda-developers.com/stop-wakelocks-android-without-root/)
