# Border-radius 50% vs 100%
# Border-radius 50% vs 100%

Lately, I’ve been trying to reexamine some of the CSS rules I’ve taken for granted in my career. I’ve written a lot of code without really understanding the reason why; I’ve simply accepted certain ways of doing things as common practice. Or, I may vaguely know the reason, but some of the details of browser implementation are fuzzy because I haven’t really read the specs. I’m trying to ask myself “Why?” as much as possible, and this practice has led to many lightbulb moments that I hope to share with others.

最近我在回顾曾经写过的代码。在过去写代码的时候，我似乎从未认真思考自己为什么要那么写，只是根据经验或者已有的代码写出来。或者是，我大概隐约知道那么写的原因，但是我从没有认真读过规范，所以并不了解具体的细节。所以我下定决心，尽可能多的问自己“为什么”，“为什么要这么写”。现在我发现这样频繁地询问自己，让我拥有了很多“灵光一闪”的瞬间，现在我就要把其中一个分享给你们。

## PERFECT CIRCLES IN CSS
## 用 CSS 画一个完美的圆

A common design pattern we see on the web is a perfect circle, sometimes with a background color or an image inside.
在 Web 上我们常常会看到很多用到圆形的地方，比如带背景色的或者图片是圆形的。

![](http://jessica-eldredge.com/img/assets/2014-09-07/023b.examples.jpg)

CSS circles on [Rdio](http://www.rdio.com/home/unavailable/), [Dribbble](http://www.dribbble.com/), and [Codecademy](https://www.codecademy.com/).

This is often achieved by using CSS’s "border-radius" property. The way I’ve always done it is to create a square, and then set the border radius to 50%. I’ve never really questioned this; I’ve simply accepted that setting the radius to half the height/width of the square on all the corners would create a circle.

通常我们都是用 CSS 的``` border-radius ```属性实现圆形：先画一个方形，然后将它的``` border-radius ```设置成50%。但是为什么偏偏是50%呢？我从来没有思考过这个问题，只是单纯地认为把顶角的半径设置成方形的高度或者宽度的一半就可以得到一个圆形。

Here is a 150px by 150px square that transforms into a circle by setting each corner’s border radius to 50%. According to the W3C's [section on border radius](http://www.w3.org/TR/2010/WD-css3-background-20100612/#the-border-radius), percentage-based units for border radius refer to the width and height of the border box. In my example, the width/height of the border box is 150px, so 50% would return a computed value of 75px.

这是一个 150px x 150px 大小的方形，将它的四个角的半径都设置成 50%。根据 W3C [border-radius 的规范](http://www.w3.org/TR/2010/WD-css3-background-20100612/#the-border-radius)定义，如果``` border-radius ```的值是百分比的话，就是相对于 border box 的宽度和高度的百分比。在我们的例子中，盒子的宽高都是 150px，所以 50% 对应的就是 75px。

![](http://jessica-eldredge.com/img/assets/2014-09-07/c9ff.square-to-circle.jpg)
Transforming a square to a circle using CSS border-radius. [CodePen](http://codepen.io/jessabean/pen/zELqt)

## HOW DOES BORDER-RADIUS REALLY WORK?
## border-radius 的工作原理

But sometimes I see the same design pattern accomplished by setting the border radius to 100%. I did it myself in a project a few days ago, without even thinking. It looks exactly the same, by forming a perfect circle. Why?

但是有时候我看到有的人会用``` border-radius: 100%; ```实现圆形的效果，在前段时间的一个项目中我想都没想就这么使用了，看起来和 50% 并没有什么区别。是什么原因呢？

In Lea Verou’s presentation, [The Humble Border Radius](https://www.youtube.com/watch?v=JSaMl2OKjfQ), she points out the W3C rules on [overlapping curves](http://www.w3.org/TR/css3-background/#corner-overlap): if the total border radius for 2 adjacent corners exceeds the size of the border box, the browser does some additional computation to proportionally scale each corner’s border radius until they don’t overlap.

在 Lea Verou 的演讲 [The Humble Border Radius](https://www.youtube.com/watch?v=JSaMl2OKjfQ) 中，她说到 W3C 对于[重合曲线](http://www.w3.org/TR/css3-background/#corner-overlap)有这样的规范：如果两个相邻的角的半径和超过了对应的盒子的边的长度，那么浏览器要重新计算保证它们不会重合。

If the top left border radius of the square is set to 100%, the radius stretches from the bottom left corner to the top right corner of the square. This is the same as setting the border radius to 150px, the size of the square. And if the top right border radius is also set to 100%, the total of the 2 corners is now 200%. The browser says, “That’s not allowed—make some room for the right corner!” and scales both radii until they fit inside the square. At this point, each corner is now half of 100%.

如果左上角的圆角半径被设置成了100%，那么圆角就会从这个方形左下角跨到右上角，相当于把圆角半径设置成为150px（也就是方形的大小）。如果同时把右上角的圆角半径也设置成为100%，则两个相邻圆角合起来就有200%。这种情况自然是不允许出现的，所以浏览器就会重新就算，匀出空间给右边的圆角，同时缩放两个圆角的半径直到它们可以刚好符合这个方形，所以半径就变成了50%。

![](http://jessica-eldredge.com/img/assets/2014-09-07/d17b.square-to-circle-2.jpg)
How border-radius 100% gets scaled down by the browser. [CodePen](http://codepen.io/jessabean/pen/sJCBx)

The browser will scale all the other corners down as well, so each corner has a 50% radius until we get our circle. Even if you specified 150px for each corner's radius, the actual computed radius would be 75px, because that is the maximum that the browser will allow for each corner.

同样的，浏览器会对其他的圆角应用相同的计算，计算的结果是每个圆角的半径变成了50%，所以我们看到了一个原型。即使将``` border-radius ```设置为150px，浏览器还是会按照75px画圆角，75px是浏览器所允许的这个方形能够拥有的最大的圆角半径。

## CONCLUSION
## 结论

If the border radius for all corners is set to 100%, the browser does some extra work behind the scenes to scale the corners down until they fit. I’m not sure if there are any performance implications to setting border radius to 100% on all your circles.

如果所有圆角的半径都被设置成了100%，浏览器会根据图形的实际情况做一些计算，保证圆角能够刚好适应图形。不过我不确定将所有图形的``` border-radius ```设置成100%对性能会不会有影响。

At the very least, it seems like a pretty pointless practice as long as you know ahead of time that the browser is going to scale the radii down to 50% anyway. Using 50% seems to be the way to go.

最后，我还是要建议在使用圆角的时候，用50%。

Time for me to go fix that random CSS from last week…

好了，现在我要去整理一下上周写的乱七八糟的 CSS 了.....




原文 http://jessica-eldredge.com/2014/09/07/border-radius-50-or-100-percent/