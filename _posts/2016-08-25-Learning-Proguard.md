---
layout: post
title: Learning proguard by obfuscating and converting external library project to aar
tags: [proguard, aar, android]
---

Sometimes there are some parts of your application which will never change or change not often. For such cases, you may easy convert them to aar or jar libraries and use it. The benefits are increased compilation speed of Android Project and fewer methods to hit the 64k limit.

The procedure is very straightforward. After build you usually can find aar inside <i>/library-project/build/outputs/aar/</i> . Now we can put this aar to libs folder. Then to make it work you need to reference this aar with your project and have a dependency on it.
<pre><code>repositories {
    flatDir {
        dirs 'libs'
    }
}
dependencies {
    compile 'library.package.name:name-release:0.1.0@aar'
}</code></pre>
Now to obfuscate this aar we need to figure which of the public API we are exposing and going to use and we should tell proguard not to touch them; In my case, I had several widget classes to keep off from obfuscating and some interfaces as well
<pre><code>//keep classes and all methods inside
-keep public class package.Utils{*;}
-keep public class package.time.TimePickerView{*;}
-keep public class package.time.Timepoint{*;}
//keep interface and all methods inside
-keep public interface package.time.TimePickerView$OnTimeSetListener{*;}
</code></pre>
Docs on proguard can be found [here](http://proguard.sourceforge.net/manual/usage.html).

We should also provide proguard rules for any external dependencies if we have them. You can find a lot of ready to use rules [here](https://github.com/krschultz/android-proguard-snippets).
<pre><code>release {
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android.txt'),
                'proguard/proguard-project.pro',
                'proguard/proguard-project-libraries.pro'
        }
</code></pre>

After having obfuscated aar and referencing it from the main project we can enjoy increased build time and fewer methods in our project.

That's it for today! Stay tuned! And keep investigating this wonderful Android World!
