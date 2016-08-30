---
layout: post
title: How do you find where that Lambda$6 transformed by Retrolambda is?
tags: [retrolambda, bytecode, lambda, android]
---

Sometimes when you analyze the heap dump or maybe just checking stacktrace you may see that something referenced to Lambda$6 but you can't find where this lambda relates to in your source code and if there are too many lambdas in the class it might be very difficult to find it.

The retrolambda transform bytecode of you application with lambdas to bytecode which contains anonymous classes to be compatible with java 5/6 and make it work on Android. To figure out where these lambdas relate to; you will need to look directly into bytecode.

![Lambda](/images/6/lambda.png) 


Fortunately, to find your way around bytecode exist very nifty [plugin](https://plugins.jetbrains.com/plugin/5918) with the help of which you can easily see bytecode of any class of your application.

That's it for today! Stay tuned! And keep investigating this wonderful Android World!

