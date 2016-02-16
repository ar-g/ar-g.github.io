---
layout: post
title: AndroidStudio tips and tricks from Android Dev Summit
tags: [AndroidStudio, ide, tools]
---

The more time we spending developing the more efficient we need to be. Knowing tips, tricks and shortcuts will help us develop much faster in the long run.
  
Recently on Android Dev Summit AndroidTools team make a huge step forward and gave an amazing talk and recap all the tricks we can use. I decided to put them down.

<iframe width="560" height="315" src="https://www.youtube.com/embed/Y2GC6P5hPeA?list=PLWz5rJ2EKKc_Tt7q77qwyKRgytF1RzRx8" frameborder="0" allowfullscreen></iframe>

1. Using **TAB** instead **ENTER** for autocompletion will provide completion without breaking syntax.
2. **CTRL-SHIFT-SPACEx2** smart autocompletion.
3. Intention actions **ALT-ENTER**
4. Live templates just start typing **logd, sout, fori** and hit ENTER
5. One of my favourite ones after this talk become **postfix autocompletion**. Basically, get a list of methods from variables and at the bottom of the list, we will see live templates.

![Yummy](/images/4/postfix.gif "Yummy")   

6. Filter search works everywhere whenever we start typing
7. Replace structurally [details](https://youtu.be/Y2GC6P5hPeA?t=403) . Tip replace structurally legacy code and deprecated APIs.
8. Add own inspection and turn into a quick fix. Continue watching point 7.
9. Tools namespace for preview, ignored when building(e.g. listItem in GridView)
10. Add **shrinkResources** to strip resources + use mode to tools:strictMode, tools:keep, tools:discard control it.
10. **CTRL+SHIFT+ALT** to find any command + see shortcut
11. **Analyse stacktrace** even obfuscated + see who committed it :)
12. Condition on breakpoint if we know condition when we want to stop. Log expression on break point.
13. Finally Enable all artefacts in AndroidStudio 2.0 with benefits such as Refactoring, etc.
14. **ALT+SHIFT+T** create a test class with unit tests in according to package and names.
15. **CTRL+N** generate test method

## Conclusion

This was most of the tricks I found very useful. Some of them dramatically improved development speed.
  
Couple of things I wanted also to mention adding own shortcuts might be very useful for such actions like:

- *Git -> Resolve conflicts*
- *Git -> Compare with the same repository version*
- *Attach debugger*
- *Sync gradle files*

And there is also very useful stats hidden in AndroidStudio in *Help -> Productivity Guide*. See what we used and may experiment with more. That's it for today stay tuned and continue exploring this wonderful Android World :)

#### Author
Andrii Rakhimov - *Android Developer* @ [Uklon](http://uklon.com.ua/)