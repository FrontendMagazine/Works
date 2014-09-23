# Single Div Drawings with CSS

# 基于单个 Div 的 CSS 绘图

## Why A Single Div?

## 为什么只使用一个 Div？

In May of 2013 I attended CSSConf and saw Lea Verou speak about the humble border-radius. It was an eye-opening talk and I realized there was much about CSS behavior I did not fully understand. This reminded me of my time as a fine arts student where I was constantly pushed to become a master of my chosen medium. As a web designer, CSS is my medium and so I challenged myself to learn all I could about it and to explore and experiment with its limits.

2013年5月，我参加了 CSSConf，看到了[Lea Verou 关于 border-radius 的演讲](http://2013.cssconf.com/talk-verou.html)，你可能会认为这个属性很不起眼。但是这个演讲让我大开眼界，认识到 CSS 还有很多行为我是不了解的。回忆起我还是艺术生的那段时光，不断地推动着我成为所选媒介的专家。作为一个 Web 设计师，CSS 是我的媒介，因此我尽我所能地学习，探索它的极限。

### But why a single div?

### 为什么只有一个 Div？

When I was learning to paint, my class did these color mixing exercises where we created the many colors of the spectrum from only the three primary colors: red, yellow, and blue. The purpose of the exercise is to learn the behavior of the medium and the constraints show us the power of combination. You can certainly buy green paint, but you can also create green from blue and yellow. Restricting your available options forces you to re-evaluate the tools you already have.

回忆我以前学画的时候，课堂上还做了混合颜色的实验，我们就使用三原色，红、黄、蓝，创造出了其他颜色的光谱。这个实验的目的是让我们了解颜色的特性，同时这种限制也让我们明白了混合的力量。你当然可以买一只绿色的笔，但是你也可以使用蓝色和黄色把绿色做出来。限制你的可选项，会让你重新评估手头上已有的工具。

I decided to start a CSS drawing project, every few days illustrating something new with only CSS. To further challenge and explore what CSS is capable of, I gave myself the constraint of using only a single div in the markup. Instead of buying green paint (or adding another div), I’d need to stretch and combine CSS properties to achieve my goals.

我决定开始一个使用 [CSS 绘画的项目](http://a.singlediv.com/)，过段时间我就会给出一个只用 CSS 绘制的新东西。为了得到更大的挑战，探索 CSS 的潜力，我给自己定了这个限制，只是用一个 Div。不能直接买一只绿色的笔（添加更多的 Div），我要做的就是尽其所能地结合 CSS 属性来实现我的目的。

## The Toolkit

## 工具箱

With only a single div and browser-supported CSS properties, it may seem like the tools are too limited. I found it’s not always what you have to work with, but how you look at them.

一个 Div 加上浏览器支持的那些 CSS 属性，看起来可用的工具太少了。但是我发现问题不在于你在使用多少东西，而在于你如何看待你在使用的东西。

### Pseudo elements

## 伪元素

With one div in the HTML, I actually have three elements to work with because of CSS pseudo classes. So with div, div:before, and div:after, we can get something like this:

因为 CSS 有伪类，所以虽然只有一个 Div，但实际上我可以使用三个元素。因此，使用 `div`，`div:before`，`div:after`，我们可以这样：

![pseudo elements](https://hacks.mozilla.org/wp-content/uploads/2014/09/asinglediv-1.png)

    div { background: red; }
    div:before { background: yellow; }
    div:after { background: blue; }

It helps to think about these three elements as things that can come in sequence and as three stackable layers. So in my mind, it would usually look more like this:

容易想到，这三个元素可以并排成为三个叠在一起的层。因此，在我的脑海中，它看起来是下面这样的：

![pseudo elements as layers](https://hacks.mozilla.org/wp-content/uploads/2014/09/asinglediv-2.png)

### Shapes

### 形状

With CSS and one element, we are afforded three basic shape types. We can use width and height properties to create squares/rectangles, border-radius to create circles/ellipses, and border to create triangles/trapezoids.

使用 CSS 和单个元素，我们可以制作三种基础图形。使用 width 和 height 属性制作正方形/矩形，使用 border-radius 制作圆/椭圆，使用 border 制作三角形/梯形。

![css shapes](https://hacks.mozilla.org/wp-content/uploads/2014/09/asinglediv-3.png)

There are others we can create with CSS, but most things can be simplified to some combination of these shapes. And these are the easiest to create and manipulate.

我们还可以使用 CSS 创建其他图形，不过大部分都可以简单组合这些基础图形来实现，这些简单的图形最容易制作，也最容易修改。

### Multiples of the same shape

### 多个相同的形状

With multiple box-shadows, we can create many versions of the same shape in varying size, color, and blur. Offsetting them on the x- and y-axes gives us almost endless multiples.

使用叠加的 box-shadow，我们可以创建多个相同的形状，这些形状可以拥有不一样的大小、颜色和模糊效果。我们可以在x或者y轴上移动这些图形，因此几乎可以绘制无限的图形。

![multiple box-shadows](https://hacks.mozilla.org/wp-content/uploads/2014/09/asinglediv-4.png)

    div {
        box-shadow: 170px 0 10px yellow,
                    330px 0 0 -20px blue,
                    330px 5px 5px -20px black;
    }

We can even give our box-shadows box-shadows. Pay attention to the order they are declared. Again, It’s helpful to think of them as layers.

我们甚至可以给 box-shadow 添加 box-shadow。注意它们申明顺序。再者，把它们当做层更容易理解。

### Gradients

### 渐变

Gradients can be used to add shading and depth by implying a light source. This makes the simple, flat shapes feel more realistic. Combining multiple background-images allows us to use many, layered gradients to achieve more complex shading and even more shapes.

渐变通过给定一个光源，可以被用来制造明暗和深浅效果，可以让简单扁平的图形看起来更真实。结合多个 background-image，我们可以使用很多层的渐变来实现更加复杂光影，甚至是更多的图形。

![gradients](https://hacks.mozilla.org/wp-content/uploads/2014/09/asinglediv-5.png)

    div {
        background-image: linear-gradient(to right, gray, white, gray, black);
    }
    
    div:after {
        background-image: radial-gradient(circle, yellow 50%, transparent 50%),
                          linear-gradient(to right, blue, red);
    }

### Visualizing

### 视觉

The most difficult part is visualizing how to piece these parts into a whole recognizable drawing. As much as I focus on the technical aspects of the drawings, this part of the process is critical. To help with this, I’ll often look at a photograph of the subject and break it up visually into pieces. Everything is a shape and everything is a color. I simplify the overall picture into smaller shapes or blocks of color I know can (or suspect can) be achieved with CSS.

最困难的部分视觉，即如何拼凑这些形状成为可被感知的绘图。随着我越来越注重绘图的技巧，发现视觉这一步很重要。为了做到这一点，我常常凝视这主题相关的图片，将其切割为多个可视的部分。都是一个个形状，都是一个个颜色。我把整张图片简化为一些小的带颜色形状或者区块，我知道（大体上）如何使用 CSS 来实现它们。

## Demos

## 实例

Let’s take a closer look at two drawings and break down some of the pieces that make up the larger pictures. First up is the green crayon.

我们一起仔细看看两个绘图，并学习如何分解成不同的区块，合成一个大的图形。第一个就是一支绿色的蜡笔。

A crayon is made up of two primary shapes: the rectangular body and the triangular drawing tip.

蜡笔由两个基础图形构成：矩形的笔身和三角形的笔尖。

![crayon shapes](https://hacks.mozilla.org/wp-content/uploads/2014/09/asinglediv-6.png)

I had to guarantee the following things to capture a realistic image:

	•	the different color of the paper wrapper
	•	the printed graphics and words on the wrapper
	•	the shading that shows the roundness of the crayon
	•	the glossiness that shows roundness and a light source

我必须实现下面这些点来捕获真实蜡笔的感觉：

- 纸质包装上不同的颜色
- 印刷在包装上的形状和文字
- 条纹暗示蜡笔是圆的
- 明暗效果，暗示圆形的蜡笔和光源


So first, I created the main body of the crayon with the div and a background color, top-to-bottom gradient, and box-shadow to show some dimension:

首先，我使用 Div 和 background 颜色制作蜡笔的身体部分，从顶部到底部渐变，并使用 box-shadow 暗示立体感：

![crayon body](https://hacks.mozilla.org/wp-content/uploads/2014/09/asinglediv-7.png)

    div {
        background: #237449;
        background-image: linear-gradient(to bottom,
                                      transparent 62%,
                                      black(.3) 100%);
        box-shadow: 2px 2px 3px black(.3);
    }

Then I added a left-to-right linear-gradient to create the wrapper. It has an alpha value of .6 so some of that previous gradient shows through.

然后，我使用一个从左到右的 linear-gradient 制作纸包装。alpha 值为.6，这样的之前的渐变可以透出来。

![crayon wrapper](https://hacks.mozilla.org/wp-content/uploads/2014/09/asinglediv-8.png)

    div {
        background-image: linear-gradient(to right,
                                      transparent 12px,
                                      rgba(41,237,133,.6) 12px,
                                      rgba(41,237,133,.6) 235px,
                                      transparent 235px);
    }

Next I used the same left-to-right gradient technique to create the printed stripes on the crayon.

接下来，我继续使用同样的方式，从左到右渐变，制作蜡笔上的条纹。

![crayon printing](https://hacks.mozilla.org/wp-content/uploads/2014/09/asinglediv-9.png)

    div {
        background-image: linear-gradient(to right,
                                      transparent 25px,
                                      black(.6) 25px,
                                      black(.6) 30px,
                                      transparent 30px,
                                      transparent 35px,
                                      black(.6) 35px,
                                      black(.6) 40px,
                                      transparent 40px,
                                      transparent 210px,
                                      black(.6) 210px,
                                      black(.6) 215px,
                                      transparent 215px,
                                      transparent 220px,
                                      black(.6) 220px,
                                      black(.6) 225px,
                                      transparent 225px);
    }

And for the printed ellipse, a radial-gradient works great!

纸包装上印刷的椭圆，使用一个 radial-gradient 轻松搞定！

![crayon printing](https://hacks.mozilla.org/wp-content/uploads/2014/09/asinglediv-10.png)

    div {
        background-image: radial-gradient(ellipse at top,
                                      black(.6) 50px,
                                      transparent 54px);
    }

I have it broken up to demonstrate each piece, but keep in mind the background-image would actually look like this:

我刚才单独展示了各个部分，不过别忘了 background-image 看起来是这样的：

    div {
                          // ellipse printed on wrapper
        background-image: radial-gradient(ellipse at top,
                                      black(.6) 50px,
                                      transparent 54px),
                          // printed stripes
                          linear-gradient(to right,
                                      transparent 25px,
                                      black(.6) 25px,
                                      black(.6) 30px,
                                      transparent 30px,
                                      transparent 35px,
                                      black(.6) 35px,
                                      black(.6) 40px,
                                      transparent 40px,
                                      transparent 210px,
                                      black(.6) 210px,
                                      black(.6) 215px,
                                      transparent 215px,
                                      transparent 220px,
                                      black(.6) 220px,
                                      black(.6) 225px,
                                      transparent 225px),
                          // wrapper
                          linear-gradient(to right,
                                      transparent 12px,
                                      rgba(41,237,133,.6) 12px,
                                      rgba(41,237,133,.6) 235px,
                                      transparent 235px),
                          // crayon body shading
                          linear-gradient(to bottom,
                                      transparent 62%,
                                      black(.3) 100%)
    }

So after completing the div, I moved on to the :before pseudo element to create the crayon’s triangular tip. Using solid and transparent borders, I made a triangle and positioned it next to the div I just drew.

完成了 div，我们把注意力转移到 :before 伪类元素上，创建蜡笔的笔头。使用实心和透明的边框，我制作了一个三角形，把它和我之前绘制 的 div 放到一起。

![triangle crayon tip](https://hacks.mozilla.org/wp-content/uploads/2014/09/asinglediv-11.png)

    div:before {
        height: 10px;
        border-right: 48px solid #237449;
        border-bottom: 13px solid transparent;
        border-top: 13px solid transparent;
    }

It looks a little flat next to the crayon’s body, but it will be fixed with the :after pseudo element. With this, I added a top-to-bottom linear-gradient to create a reflective gloss effect that spans the width of the crayon.

比起蜡笔笔杆，笔头看起来有点平，我们可以使用 :after 伪类元素来修复这个问题。我添加一个从顶部到底部的 linear-gradient，制作了一个反光效果，贯穿整只蜡笔。

![crayon gloss](https://hacks.mozilla.org/wp-content/uploads/2014/09/asinglediv-12.png)

    div:after {
        background-image: linear-gradient(to bottom,
                                        white(0) 12px,
                                        white(.2) 17px,
                                        white(.2) 19px,
                                        white(0) 24px);
    }

This adds even more dimension and realism and helps with that flat triangle. As a finishing touch, I added some text content to the :after and positioned it as another printed element on the crayon’s wrapper.

这给那个扁平的三角形添加更多的层次感，更加真实。制作接近尾声，我给 :after 添加一些文字，定位，使得看起来像是印刷在蜡笔包装上的一样。

![crayon color label](https://hacks.mozilla.org/wp-content/uploads/2014/09/asinglediv-13.png)

    div:after {
        content: 'green';
        font-family: Arial, sans-serif;
        font-size: 12px;
        font-weight: bold;
        color: black(.3);
        text-align: right;
        padding-right: 47px;
        padding-top: 17px;
    }

And that’s it!

大功告成！

### Let’s take a look at another one

### 另外一个实例

The crayon is a good example of using background-image and gradients to produce realistic results. Here’s an example that shows the power of multiple box-shadows: a single div camera.

蜡笔作为一个不错的例子，很好地展示了如何使用 background-image 和 gradient 来产生真实的效果。下面这个例子将展示多个 box-shadow 的强大之处：单 div 的照相机。

Here’s the body of the camera, created with background-image and border-image.

这是照相机的主体部分，使用 background-image 和 border-image 制作的。

![camera body](https://hacks.mozilla.org/wp-content/uploads/2014/09/asinglediv-14.png)

Here’s a gif illustrating the :before pseudo element (the black rectangle) and the many details created with its box-shadows.

下面是一张 gif，展示 :before 伪类元素（黑色的那个矩形），以及使用它的 box-shadow 创建出来的很多照相机的细节部分。

![camera rectangle](https://hacks.mozilla.org/wp-content/uploads/2014/09/asinglediv-15.gif)

    div:before {
        background: #333;
        box-shadow: 0 0 0 2px #eee,
                    -1px -1px 1px 3px #333,
                    -95px 6px 0 0 #ccc,
                    30px 3px 0 12px #ccc,
                    -18px 37px 0 46px #ccc,
     
                    -96px -6px 0 -6px #555,
                    -96px -9px 0 -6px #ddd,
     
                    -155px -10px 1px 3px #888,
                    -165px -10px 1px 3px #999,
                    -170px -10px 1px 3px #666,
                    -162px -8px 0 5px #555,
     
                    85px -4px 1px -3px #ccc,
                    79px -4px 1px -3px #888,
                    82px 1px 0 -4px #555;
    }

Similarly, here’s the :after (the grey circle) and its several box-shadow details.

类似的，下面是 :after（灰色的圆）以及使用它的 box-shadow 制作的几个细节部分。

![camera circle](https://hacks.mozilla.org/wp-content/uploads/2014/09/asinglediv-16.gif)

    div:after {
        background: linear-gradient(45deg, #ccc 40%, #ddd 100%);
        border-radius: 50%;
        box-shadow: 0 3px 2px #999,
                    1px -2px 0 white,
                    -1px -3px 2px #555,
                    0 0 0 15px #c2c2c2,
                    0 -2px 0 15px white,
                    -2px -5px 1px 17px #666,
                    0 10px 10px 15px black(.3),
     
                    -90px -51px 1px -43px #aaa,
                    -90px -50px 1px -40px #888,
                    -90px -51px 0 -34px #ccc,
                    -90px -50px 0 -30px #aaa,
                    -90px -48px 1px -28px black(.2),
     
                    -124px -73px 1px -48px #eee,
                    -125px -72px 0 -46px #666,
                    -85px -73px 1px -48px #eee,
                    -86px -72px 0 -46px #666,
                    42px -82px 1px -48px #eee,
                    41px -81px 0 -46px #777,
                    67px -73px 1px -48px #eee,
                    66px -72px 0 -46px #666,
     
                    -46px -86px 1px -45px #444,
                    -44px -87px 0 -38px #333,
                    -44px -86px 0 -37px #ccc,
                    -44px -85px 0 -34px #999,
     
                    14px -89px 1px -48px #eee,
                    12px -84px 1px -48px #999,
                    23px -85px 0 -47px #444,
                    23px -87px 0 -46px #888;
    }
    
A little crazy, but as you can see, multiple box-shadows can add a lot of detail to a single div drawing.

有点疯狂？不过你看到了吧， 多个 box-shadow 确实可以给使用单个 div 绘图添加很多细节部分。

## Biggest challenges

## 最大的挑战

Two of the biggest obstacles I came across were limitations around triangle shapes and the natural behavior of gradients.

我碰到了两个最大的挑战，三角形的限制和 gradient 独特的行为。

### The trouble with triangles

### 三角形的问题

Because triangles are created using borders, it limits how much I can do with them. Adding gradients with border-image apply to all the borders, not just one side. Box-shadows apply to the shape of the box, not the triangle shape the border creates, so creating multiple triangle shapes can be difficult. Here’s an example of what that looks like:

因为三角形是使用 border 创建的，这极大地限制了我对它的利用。使用 border-image 给 border 添加 gradient，不能单独添加其中一边。无法给 border 创建出来的三角形添加 box-shadow，因为 box-shadow 是添加在盒模型上的。因此要创建多个三角形就会很困难。看起来就是下面这样：

![trouble with triangles](https://hacks.mozilla.org/wp-content/uploads/2014/09/asinglediv-17.png)

    div {
        border-left: 80px solid transparent;
        border-right: 80px solid transparent;
        border-bottom: 80px solid red;
    }
     
    div:before {
        border-left: 80px solid transparent;
        border-right: 80px solid transparent;
        border-bottom: 80px solid red;
        border-image: linear-gradient(to right, red, blue);
    }
     
    div:after {
        border-left: 80px solid transparent;
        border-right: 80px solid transparent;
        border-bottom: 80px solid red;
        box-shadow: 5px 5px 5px gray;
    }

### Layering gradients

### 多层渐变

With gradients, their natural behavior is to fill the entire background. This can get a little tricky when layering multiple gradients on top of each other. It takes some extra time to think through transparency, z-index, and understanding what will and won’t be visible. By using this technique effectively, our drawings can have surprising detail.

渐变的行为就是会填满整个 background。在堆叠多个 gradient 的时候就显得很讲技巧。需要花费额外的时间思考透明度、z-index这些事，还要搞清楚什么要可见，什么不要。不过若能有效地使用 gradient，我们的绘图可以包含很多令人惊叹的细节。

The Tardis is a good example of showing and hiding gradients to create a detailed picture. Here’s the drawing mid-process that shows some of the top-to-bottom gradients that span the entire width of the container.

Tardis 就是一个很好的例子，显示或隐藏渐变，创建了一张细节极强的图片。下图显示的是绘制的中间过程，可以看到数个从顶部到底部的渐变，宽度填满整个容器。

![single div tardis in-process](https://hacks.mozilla.org/wp-content/uploads/2014/09/asinglediv-18.png)

Using left-to-right and right-to-left gradients, I was able to cover parts of the gradient and leave some parts exposed.

使用从左到右和从右到左的 gradient，我可以遮住一部分渐变，同时把其他部分渐变显示出来。

![single div tardis in-process](https://hacks.mozilla.org/wp-content/uploads/2014/09/asinglediv-19.png)

The resulting drawing appears to have many shapes making up the front facade of the Tardis, but it’s strategically layered linear-gradients. Sometimes you have to fake it.

最终的结果看上去包含了很多图形来构成 Tardis 的前面，但实际上它就是层叠的 linear-gradient。很多时候不得不伪造呀。


## See them in action

## 动态地查看它们

One awesome thing that popped up because of this project is a really cool and useful Chrome browser extension by Rafael Carício called CSS Gradient Inspector. It extends the developer tools to inspect and toggle on/off each element’s gradients as if they were layers. (It’s very helpful with everyday projects, too.)

源于这个项目，有一个非常酷非常有用的好东西突然出现，那就是 [Rafael Carício](https://twitter.com/rafaelcaricio) 开发的名为 [CSS Gradient Inspector](https://chrome.google.com/webstore/detail/css-gradient-inspector/blklpjonlhpakchaahdnkcjkfmccmdik) 的 Chrome 浏览器插件。这个开发工具可以探测且可以开关元素上的每一个 gradient，看起来就像开关一个个层。（它在日常项目中也非常有用。）

I’m super excited to see designers and developers experimenting and riffing on these drawings with animations and JavaScript functionality. Check out the site and play around with the CSS yourself at a.singlediv.com or on GitHub!

我希望设计师和开发者使用动画或者 JavaScript 的功能来做类似的尝试，或者对这些绘画做一些变形。你可以到 [a.singlediv.com](http://a.singlediv.com/) 或者 [GitHub](https://github.com/lynnandtonic/a-single-div) 上把玩一下这些 CSS。

原文：[Single Div Drawings with CSS](https://hacks.mozilla.org/2014/09/single-div-drawings-with-css/﻿)