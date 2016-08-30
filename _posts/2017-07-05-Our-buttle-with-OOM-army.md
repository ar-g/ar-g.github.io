---
layout: post
title: Our battle with the OutOfMemory army
tags: [android, OutOfMemory, memory leak, leak]
---

![Leak](/images/7/leak.jpg) 

In the world of startups and rapid mobile development, developers can drown in an ocean of new features and forget about vital things like app stability. We’ve fallen in the same trap too. Our classifieds application, Lalafo is rich on images and different feeds, our users from Asia and many others use affordable devices, almost 20% still run KitKat. One of the most important metrics for us is how much time the user spends inside our app. Therefore, keeping the app healthy is very important.

As our app has grown bigger we’ve started to notice OutOfMemory(OOM) crashes. At some point almost 70% of crashes were caused by OOM errors. We fixed feature-related bugs, and we tried to play with OOM errors. Our bug-fix time is usually 2 days. We fixed some memory leaks and just assumed the OOM errors will disappear. With that said, we continued to develop in this manner, hesitant to give any time to this issue. Finally, our QA came to us and said that he found a sustainable pattern that the app would crash after some period of time. We had a lot of features to develop. Still, we looked closer to crashlytics and realized that it could be that 80% of bugs which cause most of the problems for users which spend the most time. There was no need to think it over.


## LeakCanary

We've used [LeakCanary](https://github.com/square/leakcanary) tool for about half a year. We set it up for our fragments and activities. It captured many memory leaks which we fixed, but we couldn’t reduce the number of OOM errors.

## Android Studio 3.0

The New Android Profiler which is bundled inside of AndroidStudio allowed us to dynamically investigate memory heap of our app. This is where the battle was waged.

![Profiler](/images/7/profiler.png) 


## Treacherous fragments

In our app we use the MVP(Model-View-Presenter) pattern to organize architecture. Our presenters persisted until the user exits the activity or fragment. Unfortunately, that wasn’t the case for nested fragments, the checks `isRemoving() && !isFragmentInBackstack()` didn’t work for them, which led to leaking presenters, which sometimes was heavy and had feeds. The user also can go deeper from the feed to feed, through these nested fragments and will accumulate whole lot of presenters.

## Sneaky adapters

Fixing presenter leaks didn’t change the situation by that much. After spending a couple of days investigating memory heap and doing dumps on several screen transitions, we noticed a suspiciously large amount of [SimpleDraweeView](http://frescolib.org/), which we use in our feeds to load images.

![drawee](/images/7/drawee.png) 

Then we noticed a pattern, that these views were doubling every time we were going to the screen and back in the feed screen. Initially, we thought that the problem was with recyclerView, viewHolder or adapter. Later, it turned out to be because we were reusing adapters in our fragments, they were [holding](https://stackoverflow.com/q/44648308/1823992) RecyclerView and RecycledViewPool. When we looked closer, we saw that RecyclerView takes most of the memory and they are number 5 on the list of a heap dump. After dereferencing, adapter views disappeared altogether from the heap.

<pre><code>
@Override public void onDestroyView() {
  super.onDestroyView();
  recyclerView.setAdapter(null);
}
</code></pre>

## Revisiting image caching with fresco

During this play with the heap, we noticed that our heap usually will grow from 40mb to 150mb after extensively using a feed. It turned out that we were relying on default image caching. In reality, after each refresh, our feed was changing, for example user posts new ads, or apply different search filter. The chance that the user will see the same picture is very small. We decided to clear fresco cache after each search, or exit from similar ads. So we won’t keep unnecessary images in the heap.

<pre><code>
for (Image image : ad.getImages()) {      
  Fresco.getImagePipeline().evictFromCache(Uri.parse(image.getUrl()));
}
</code></pre>

### Conclusion

That was our battle with OutOfMemory errors. After applying these fixes, our OOM errors dropped from 70% to 3% of all crashes. We learned that it’s super important to stand up and look around, from time to time, just to see which of the 80% is causing the majority of the problem for you.