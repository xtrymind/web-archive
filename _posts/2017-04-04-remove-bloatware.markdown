---
layout: post
title:  "Remove Bloatware on Sony Xperia M4Aqua"
date:   2017-04-04 19:29:26 +0700
author: Dede Dindin Qudsy
tags:   [android,sony,xperia,m4aqua,bloatware]
last_modified_at: 2017-04-04 20:22:26 +0700
---
Sony ship Marshmallow firmware with a lot of bloatware (not as lot as samsung though), it's eat space, memory and battery life even though we don't use it.

if you already root you can always remove them one by one with your choice of software, but it's take a lot of time doing it one by one.

i'm make flashable zip to remove some of sony marshmallow bloatware, not all bloatware i remove because i'm still using some of it like sony mail, gmail etc. if you wanna add some bloatware to remove, just edit `updater-script` and add what you wanna remove.

Download [here](https://www.androidfilehost.com/?fid=817550096634759712)

Note :
1. it's use mv not rm in case you wanna re-add what you wanna remove, just copy from ``/system/__backups/app/your-wanna-restored-software`` to ``/system/app``
2. if you don't need it, just delete __backup folder to free some space.
3. List of some deleted bloatware :

```
 - amazon
 - AR effect
 - AVG Antivirus
 - Bubble
 - Google Chrome
 - Google Drive
 - Google Hangouts
 - Google Maps
 - Google Music
 - Xperia Lounge
 - Google Photos
 - Google Play Movies
 - Sketch
 - Movie Creator
 - Support
 - Xperia Transfer Mobile
 - Youtube
 - File Commander
 - Sony Music
 - Sony Calendar
 - Sony Video
 - Sony Podcast
 - Simple Home
 - My Xperia
 - Xperia Chinese Keyboard
 - Sony Playstation App
 - Screen Recording
 - Etc
 
```
