原文地址：[FLIP Your Animations](https://aerotwist.com/blog/flip-your-animations/)

# FLIP Your Animations
# “翻转”你的动画

Animations in your web app should run at 60fps. Not always easy to achieve that, and it really depends on what you're trying to do, but I'm here to help. With FLIP.

网页上的动画应该要达到 60fps 的帧率，这个目标并不是那么容易达到，你需要采取各种方式才能实现这个目标，今天我将介绍 **FLIP** 来帮助你。

Recently I’ve had the pleasure of being part of the team that built building the Google I/O 2015 website, and last year I built the Chrome Dev Summit site. On both sites we used FLIP, which is essentially a principle, and not a framework or a library. It is a way of thinking about animations, and attempting to keep them as cheap as possible for the browser which, all being well, should translate over to 60fps animations.

最近我有幸参与到 [2015 Google I/0 的官方站点](https://events.google.com/io2015/)项目中，而且去年我也参与构建了 [Chrome Dev Summit 站点](https://developer.chrome.com/devsummit/)。这两个项目中我们都使用了 **FLIP**。FLIP 是一个原则，而不是一个框架或者库。它是一种思考动画的方式，通过这个方式，我们希望在正常情况下，浏览器对动画的渲染都可以达到 60fps。

If you prefer watching to reading, this is my talk from Chrome Dev Summit, where I explain FLIP (without naming it explicitly) in lots of detail:

下面是我在 Chrome Dev Summit 上关于 FLIP （当时还没有给它一个正式的名字）的视频，视频中我详细解释了这个原则，你也可以通过视频了解 FLIP。

## The general approach
## 通用方法

What we’re trying and do is to turn animations on their head (flip, see? Gosh darnit, I’m so funneh) and, instead of animating “straight ahead” and potentially doing expensive calculations on every single frame we precalculate the animation dynamically and let it play out cheaply.

我们想要做的是turn animations on their head（看，就是翻转。天了噜，我也太会取名字了吧！），而不是一帧一帧地执行动画，计算每一帧太昂贵了，我们想要预计算动画，然后再执行它，这样效率会高。

FLIP stands for First, Last, Invert, Play.

FLIP 指的是 **F**irst（起始）、**L**ast（终止）、**I**nvert（翻转）还有 **P**lay（播放）。

Let’s break it down:

现在让我们分开来看：

+ **First**: the initial state of the element(s) involved in the transition.
+ **起始**: 元素的初始状态。

+ **Last**: the final state of the element(s).
+ **结束**: 元素的结束状态。

+ **Invert**: here’s the fun bit. You figure out from the first and last how the element has changed, so – say – its width, height, opacity. Next you apply transforms and opacity changes to reverse, or invert, them. If the element has moved 90px down between First and Last, you would apply a transform of -90px in Y. This makes the elements appear as though they’re still in the First position but, crucially, they’re not.
＋ **翻转**: 有趣的来了，现在你已经知道了元素要如何从初始状态转变到结束状态，比如它的宽、高或者透明度要如何变化。下一步你将 transform 和 opacity 运用到调转元素的动画上。如果在初始状态向结束状态的过程中，元素向下移动了 90px，那你需要在 Y 轴平移 －90px 才能在视觉上让元素看起来还是在起始位置。但事实是，它们并不会在起始位置上。

+ **Play**: switch on transitions for any of the properties you changed, and then remove the inversion changes. Because the element or elements are in their final position removing the transforms and opacities will ease them from their faux First position, out to the Last position.
＋ **播放**: 在你改变了的属性上开启过渡，然后移除反转。因为元素（们）在结束状态时的 opacity 和 transform 都会被改变（从假的初始状态到结束状态）。

Ta daaaaa!

## Got Code?
## 代码要怎么写？

Why yes I do. Here’s that same breakdown in code:

来看看下面的代码：

```
  // 获取到元素的初始状态。
  var first = el.getBoundingClientRect();

  // 现在，让元素变到终止状态
  el.classList.add('totes-at-the-end');

  // 注意，这里是强行同步了布局，要小心使用
  var last = el.getBoundingClientRect();

  // 如果需要的话，你可以在其他需要计算的样式上应用这个方法。
  // 但要保证是在复合层上使用，比如 transform、opacity 这样的属性
  var invert = first.top - last.top;

  // 翻转
  el.style.transform = 'translateY(' + invert + 'px)';

  // 下一帧渲染的时候，我们可以保证所有的属性已经发生了改变。
  requestAnimationFrame(function() {

    // 开启动画
    el.classList.add('animate-on-transforms');

    // 动画执行
    el.style.transform = '';
  });

  // 通过 'transitioned' 捕捉到结束点
  el.addEventListener('transitionend', tidyUpAnimations);
```

However, you can also do this with the upcoming Web Animations API, which can make things even easier:
不过，你也可以使用最新的 [Web Animations API](http://w3c.github.io/web-animations/)，代码将会更简洁易懂一些：

```
  // Get the first position.
  // 获取初始状态。
  var first = el.getBoundingClientRect();

  // Move it to the end.
  // 改变到结束状态
  el.classList.add('totes-at-the-end');

  // Get the last position.
  // 获取结束状态
  var last = el.getBoundingClientRect();

  // Invert.
  // 翻转
  var invert = first.top - last.top;

  // Go from the inverted position to last.
  // 从翻转的位置到结束位置
  var player = el.animate([
    { transform: 'translateY(' + invert + 'px)' },
    { transform: 'translateY(0)' }
  ], {
    duration: 300,
    easing: 'cubic-bezier(0,0,0.32,1)',
  });

  // Do any tidy up at the end of the animation.
  // 在动画结束时做点整理
  player.addEventListener('finish', tidyUpAnimations);
```

Right now you’ll need the Web Animations API polyfill to use the code above, though, but it’s pretty lightweight and does make life a lot easier!
不过现在如果你要使用 Web Animations API 的话，还需要结合 [polyfill](https://github.com/web-animations/web-animations-js) 使用，让自己的人生轻松一定！

If you want a more “in-production” context for FLIP check out the source for cards on the Chrome Dev Summit site.
如果你想要一个更“像生产环境”的 FLIP 代码，可以看看 [Chrome Dev Summit 上的代码](https://github.com/GoogleChrome/devsummit/blob/master/src/static/scripts/components/card.js#L263-296)。

## What is it good for?
## 这有什么好处？

It’s absolutely superb for times where you are responding to user input and then animating something in response. So, for example, in the case of Chrome Dev Summit, I was expanding cards that the user tapped on. Often the start and end locations and dimensions of the elements aren’t known, because – say – the site is responsive and things move around. This will help you because it’s explicitly measuring elements and giving you the correct values at runtime.

如果你能够用动画来响应用户输入那自然是极好的。比如在 Chrome Dev Summit 的网站上，当用户点击了卡片，卡片将会展开。通常元素的起始和终止状态的尺寸是未知的。因为站点是响应式的，页面上的元素都是环绕在一块。用 FLIP 这个方法，可以显式地处理元素，在运行时元素的当前值可以被计算出来。

The reason you can afford to do this relatively expensive precalculation is because there is a window of 100ms after someone interacts with your site where you’re able to do work without them noticing. If you’re inside that window users will feel like the site responded instantly! It’s only when things are moving that you need to maintain 60fps.

你之所以能做这样相对昂贵的预计算，是因为当用户与站点发生交互的时候，存在一个 100ms 的时窗，用户是不会察觉到在这 100ms 之内完成的动作的，只要在 100ms 之内，用户都会认为你的站点是很快的！只有当元素在移动的时候，你要保证帧率达到 60fps。

![](images/flip/window.jpg)

We can use that window of time to do all that getBoundingClientRect work (or getComputedStyle if that’s your poison) in JavaScript, and from there we make sure that we’re reducing the animation down to nice-and-fast, compositor-friendly, look-ma-no-paints transform and opacity changes. (Why just those? Check out my Pixels are Expensive post.)

我们可以利用这段时窗完成``` getBoundingClientRect ```（或者是``` getComputedStyle  ```），接着我们就能又快又好地执行动画，