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









