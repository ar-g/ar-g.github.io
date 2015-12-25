---
layout: post
title: Get efficiency and fun from using ADB Shell
tags: [adb, AndroidShell]
---

 In this article I want to show how to use basic adb commands to do things like `install()`, uninstall, copy, check, clean app which used most often during android development and testing. 
 First install any apk, most likely you have more then one devices connected, and need to pick one:

{% highlight java linenos %}
adb devices
List of devices attached
106a6a4f	device
//now specifying device to install simple apk
adb -s 106a6a4f ~/Downloads/sample.apk
{% endhighlight %}

