原文链接：[Introducing RAIL: A User-Centric Model For Performance](http://www.smashingmagazine.com/2015/10/rail-user-centric-model-performance/#a-note-on-measurement)

> 译者注：本篇将使用视频资源，需要大家采用正确的姿势阅读哦。

# Introducing RAIL: A User-Centric Model For Performance
# RAIL，以用户为核心的性能模型

There’s no shortage of **performance advice**, is there? The elephant in the room is the fact that it’s **challenging to interpret**: Everything comes with caveats and disclaimers, and sometimes one piece of advice can seem to actively contradict another. Phrases like “The DOM is slow” or “Always use CSS animations” make for great headlines, but the truth is often far more nuanced.

难道**对性能提建议**还能有不好吗？但其实我们都在**回避这个问题的答案**：每条优化建议的后面都跟着警告和免责声明，有时候这些建议还是相互矛盾的。像“ DOM 太慢了”或者“尽可能地使用 CSS 动画”这样的大标题之下，掩盖着的实际场景要复杂的多。

Take something like loading time, the most common performance topic by far. The problem with loading time is that some people measure Speed Index, others go after first paint, and still others use body.onload, DOMContentLoaded or perhaps some other event. It’s rarely consistent. When it comes to other ways to measure performance, you’ve probably seen enough JavaScript benchmarks to last a lifetime. You may have also heard that 60 FPS matters. But when? All the time? Seems unrealistic.

现在最常见的性能话题大概就是“加载时间”吧，就拿它来举例。有人用 [Speed Index](https://sites.google.com/a/webpagetest.org/docs/using-webpagetest/metrics/speed-index) 作为衡量的标准；有人以页面首次绘制时间为准；仍有人使用 ```body.onload``` 或者 ```DOMContentLoaded``` 或者别的事件为准。衡量的标准并不统一。还有其他方法可以衡量性能，比如你已经看腻了用 JavaScript 基准（JavaScript benchmark）延续页面的生命周期。你可能也听过60 FPS。但是这些标准适用在什么场合？任何时间点吗？听起来有点不现实啊。

Very few of us have unlimited time to throw at optimization work, far from it, and we need criteria that help us decide what’s important to optimize (and what’s not!). When all is said is done, we want and need clear guidance on what “performant” means to our users, because that’s who we’re building for.

我们当中几乎没有人可以把时间完全投入在优化上，所以我们需要一个标准来告诉我们现在要去优化什么东西（或者什么东西暂时不需要优化了）。该说的都说了，现在我们需要一个明确的指引告诉开发者，**在用户的眼里**“性能”意味着什么，毕竟用户才是我们最终服务的对象。


On the Chrome team, we’ve been thinking about this, and we’ve come up with a model to put the user right back in the middle of the performance story. We call it the RAIL model.

Chrome 团队已经考虑过这个问题，并提出了 **RAIL** 模型。用户在这个性能模型中扮演了很重要的角色。

If you want the TL;DR on RAIL to share with your team, here you go!

如果你迫不及待想要把 **RAIL** 分享给你的团队，那可以说说这些：

+ RAIL is a model for breaking down a user’s experience into key actions (for example, tap, drag, scroll, load).
+ [RAIL](http://www.smashingmagazine.com/2015/10/rail-user-centric-model-performance/#rail-perf-model) 将用户体验根据关键动作（比如，轻触、拖拽、滚动、加载）分割到了不同的模块中。

+ RAIL provides performance goals for these actions (for example, tap to paint in under 100 milliseconds).
+ RAIL 给这些动作制定了[性能目标](http://www.smashingmagazine.com/2015/10/rail-user-centric-model-performance/#rail-perf-goals)（比如，轻触后的绘制要在100毫秒之内完成）。

+ RAIL provides a structure for thinking about performance, so that designers and developers can reliably target the highest-impact work.
+ RAIL 提供了一个思考性能问题的结构，所以当设计师和开发者想要处理对用户影响最大的问题的时候有法可依。

Before diving into what RAIL involves and how it could be helpful in your project, let’s step back and look at where it comes from. Let’s start with every performance-minded person’s least favorite word in the whole wide world: “slow.”

在讲如何运用 RAIL 以及如何使用它辅助你的项目时，让我们先看看这个模型是怎么来的。就从大家都很讨厌的词“慢”，开始吧。

## “Slow” 

Is changing the DOM slow? What about loading a ```<script>``` in the ```<head>```? JavaScript animations are slower than CSS ones, right? Also, does a 20-millisecond operation take too long? What about 0.5 seconds? 10 seconds?

![](images/rail/rail-slow-cloud.png)

While it’s true that different operations take different amounts of time to complete, it’s hard to say objectively whether something is slow or fast without the context of when it’s happening. For example, code running during idle time, in a touch handler or in the hot path of a game loop each has different performance requirements. Put another way, the people using your website or app have different **performance expectations** for each of those contexts. Like every aspect of UX, we build for our users, and what they perceive is what matters most. In fact, number one on [Google’s ten things we know to be true](http://www.google.com/about/company/philosophy/) is “Focus on the user and all else will follow.”

Asking **“What does slow mean?,”** then, is really the wrong question. Instead, we need to ask “What does the user feel when they’re interacting with the things we build?”