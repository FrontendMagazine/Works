# A Designer’s Guide To Flexbox

# Flexbox 设计指南

[Flexbox](http://demosthenes.info/blog/css/flexbox) is a new and very powerful CSS layout module, one completely divorced from web practices of the past. Most of the online coverage of the module has focused on details of the specification, which is long, complex, and somewhat arcane. In contrast, there’s relatively little detail **on how flexbox can be used by the designer-developer to solve everyday layout problems:** thus, this article, the first in a three-part series.

[Flexbox](http://demosthenes.info/blog/css/flexbox) 是一个全新并且强大的 CSS 布局模块，完全脱离于传统的 web 开发实践。网上有很多相关的文章，大都关注于规范的细节，导致文章冗长，难懂，甚至有些晦涩。相反，对于**设计师和开发者如何使用 flexbox 解决布局问题**的讨论相当少：至此，这篇文章出现了。

At the basic level, **flexbox has three features that are fundamental to design**, but which have long been difficult or impossible to achieve in CSS: alignment, distribution, and ordering.

从基础层面上来讲，**flexbox 有三个特性是设计之根本**。但也是在很长一段时间内单纯使用 CSS 很难或者不可能完成任务：对齐方式，排布和顺序。

Before we begin, there are a few things to be aware of:

开始之前，需要先了解下面几个事情：

- Flexbox went through three iterations before finally settling into the spec we have today. Each of those iterations had different property names, with appropriate [vendor prefixes](http://demosthenes.info/blog/217/CSS-Vendor-Prefixes-and-Flags) in different browsers. Right now we’re at the point where all current browsers support the unprefixed final spec, but working backwards is something of a minefield. For this reason, I very strongly recommend you **use a recent browser to create and test your code using the final version of flexbox,** then work backwards in each browser to achieve support In older versions. Trying to write flexbox code to support all browsers at the same time is an exercise in frustration.

- Flexbox 在最终形成今天的规范之前，历经了三次迭代。每一次迭代都伴随着不同的属性名，在不同浏览器下有着相应的[特定前缀](http://demosthenes.info/blog/217/CSS-Vendor-Prefixes-and-Flags)。而现在，我们所处在这样的时刻，所有的浏览器都支持无前缀的终极规范，但是想要兼容低版本的浏览器还有很多坑要填。正因如此，我强烈建议你**按照 flexbox 的最终规范编写代码，并且使用最新的浏览器进行测试，**然后再去实现向前兼容。想要让你编写的代码同时兼容所有的浏览器是一件很头疼的事。

- While flexbox can work with other CSS layout systems, it is important to **abandon previous assumptions and practices in web page layout** before starting work in the new system. It has a very different way of working, and you’ll only be stymied if you bring presuppositions to it.

- 尽管 flexbox 可以和其它的 CSS 布局系统一同工作，但是在开始使用新的系统之前，**丢掉以前在 web 布局中的假设和实践**很重要。这是一种全新的工作方式，如果坚持以前的思维，你将受到阻碍。

- You’ll hear occasional claims of “what flexbox is for”. While it is true that other layout systems will quickly supplement flexbox – grids and regions among them – the claim is not exclusively true.  CSS is not semantic, and no part of it has a pre-ordained role. You can use any CSS to achieve anything you wish; the only question is how effectively the code accomplishes your goal. As we’ll see, flexbox solves a lot of layout problems that designers face today.

- 你可能偶尔听到“flexbox 是用来干啥干啥的”。诚然，其它的布局系统会很快的补充上 flexbox——比如 grids 和 regions，但这种称述并不完全准确。CSS 不是语义化的，没有哪一个 CSS 特性就是固定做某件事情的。你可以使用任意的 CSS 来完成你的需求；唯一的问题是什么样的代码才能更高效的实现目标。正如我们看到的，flexbox 解决了设计者在布局上正面临的诸多问题。

- The previous incarnations of flexbox make researching it somewhat treacherous: it’s easy to find an article written without an entry date, only to discover that you’ve been reading an interpretation of the (now obsolete) Flexbox 2009 spec. Play close attention, and be careful.

- Flexbox 以前的几个版本给现在的开发者们带来了一些风险：很可能读到一篇没有指明书写时间的文章，最后发现自己正在看 2009 年的 flexbox 规范说明（现在已经废除）。所以，时刻谨慎小心，提高警惕。


## A Basic Example of Flexbox in Action

## 一个简单的 Flexbox 应用实例

Let’s take three simple ``<div>`` elements and place them inside another container:

将三个简单的 ``<div>`` 元素放在另一个容器内部：

````
<div id="item-container">
	<div class="circle"></div>
	<div class="square"></div>
	<div class="circle"></div>
</div>
````

Now let’s set the inner ``div`` elements to look like their class names, while setting ``display: flex`` on the outer container:

现在按照内层的 ``div`` 元素的类名添加 CSS 样式，为外层容器添加 ``display: flex``：

````
#item-container {
	display: flex;
	background-color: hsl(34,88%,90%);
}
.square {
	height: 200px; width: 200px;
	background-color: hsl(50,88%,50%);
}
.circle {
	border-radius: 50%;
  width: 150px;
  height: 150px;
	background-color: hsl(22,88%,50%);
}
````
The result looks like this:

结果如下所示：

![](http://img.china.alibaba.com/cms/upload/2015/100/472/2274001_975966031.png)

There’s a few things to take note of immediately:

此时，有几件事需要注意：

- The most common flex properties are applied to the parent element, rather than the individual children. This is a frequent cause of confusion, as most web developers are accustomed to controlling individual elements, rather than styling them through their parents.
- Each immediate child of the flex container is considered a “flex item”. However the content inside the children do not inherit anything special, and can take any CSS you wish. Because each child is a flex item, you may find yourself “protecting” content by wrapping it in a div or other element, making it the flex-child rather than the inner content.
- “Block”, “inline”, “float” and “text-align” have no meaning in the context of flex items.
- By default, flex items are aligned at their **tops** and to the **left** of the flex container.

- 很多常见的 flex 相关的属性都被应用于父元素，而不是单个的子元素。这通常会引起一个疑惑，绝大多数开发者习惯于控制单个的元素，而不是通过父元素为子元素添加样式；
- Flex 容器的每个直接子元素被称为一个“flex-item”。然而，子元素里面的所有元素不会继承任何特殊的样式，并且可以添加任何你想要的 CSS。因为每个子元素是一个 flex-item，你会发现自己通过将元素包裹在一个 div 或者其它的元素中，“保护”里面的内容。使该元素成为 flex-child，而不是它里面的内容；
- “Block”，“inline”，“float” 和 “text-align” 在 flex-item 的环境下无意义；
- 默认情况，所有的 flex-item 会按照 flex 容器的 **顶部** 和 **左侧** 对齐。

## Flex Alignment

## Flex 对齐方式

As a first step, let’s change the horizontal alignment of the flex items. The “left” of the flex-item distribution is known as ``flex-start``. We’ll change it to the end:

第一步，我们要改变 flex-item 的水平对齐方式。flex-item 默认是左对齐，对应的 CSS 样式为 ``flex-start``。我们将它改为末端。

````
#item-container { justify-content: flex-end; }
````

![](http://img.china.alibaba.com/cms/upload/2015/200/572/2275002_975966031.png)

Nothing outstanding so far, but let’s take a look at a few other possibilities.

到目前还没什么惊艳的，但是我们再看一下其它几种可能性。

````
#item-container { justify-content: center; }
````

![](http://img.china.alibaba.com/cms/upload/2015/200/372/2273002_975966031.png)

That’s pretty neat: gaining a similar design in traditional CSS would usually require at least one more element and some extra declarations to get the same result. But we’re not done yet:

非常棒：效果与使用传统 CSS 至少一个元素以及一些额外的声明才可能完成的效果一样。实际上，远不止这个。

````
#item-container { justify-content: space-between; }
````
![](http://img.china.alibaba.com/cms/upload/2015/300/272/2272003_975966031.png)

Now we’re talking. ``space-between`` maximizes the distribution of elements within their container. This remains true whether there are two flex items elements or a dozen.

继续讨论。 ``space-between`` 最大化分布了容器内部的元素。不管是两个 flex-item 还是十几个，这都是成立的。

One more:

再来一个：

````
#item-container { justify-content: space-around; }
````

![](http://img.china.alibaba.com/cms/upload/2015/100/872/2278001_975966031.png)

It’s perhaps easiest to think of this as “``margin: 0`` auto that works for everything”. The flex items are evenly distributed by default, but each is centered inside the space it is given.

它很容易让人联想到“已经被用烂了的``margin: 0``”。Flex-item 默认会被均匀的分布，但是每一个都会在其给定的空间内居中显示。

You’ll note that all movement so far has been restricted to the horizontal plane. What about vertically?

你可能已经注意到，目前为止所有的变化都被限定在水平方向上。垂直呢？

````
#item-container { justify-content: space-between; align-items: center; }
````
![](http://img.china.alibaba.com/cms/upload/2015/100/972/2279001_975966031.png)

The flex items move along what the flexbox model refers to as the “cross-axis”.  The central square, having no vertical space in which to move, stays where it is. Effectively, **this aligns elements on their centers, along a horizontal axis.** Other values include ``flex-start`` (the default, equivalent to “align top”), ``flex-end`` (“align bottom”) and ``stretch``.

Flex-item 会沿着 flexbox 模型指向的“十字坐标”移动。中间的正方形，没有可移动的垂直空间，在原位置保持不变。事实上，**会以它们的中心来对其所有元素，沿着 X 轴。**属性值还可能是其它的：``flex-start``（默认，等同于“align top”）， ``flex-end``（“align bottom”）以及 ``stretch``。

I also said that flexbox was **the first CSS layout system that would allow you to order items.** Let’s try a simple example of that: placing the square before the circles, rather than between them:

我还想说，flexbox 也是“首个允许你控制元素顺序的 CSS 布局系统。”为此做一个简单的尝试：将正方形放在两个圆形的前面，而不是两者之间：
````
.square { order: -1; }
````
![](http://img.china.alibaba.com/cms/upload/2015/100/082/2280001_975966031.png)

Now that’s really cool. Flexbox effectively frees our page content from the shackles of source order: traditionally, whatever came first in our HTML had to be displayed first on the page. This change is well out of the reach of traditional CSS, into a space previously occupied solely by [JavaScript](http://demosthenes.info/blog/javascript).  It’s important to understand that your original HTML code remains the same: use View Source in your browser to prove it.

真的很炫酷。Flexbox 有效地摆脱了代码顺序对文档内容显示的束缚：按照惯例，HTML 文档中最先出现的元素往往也最先渲染在页面中。传统的 CSS 对这种改变显得很无力，这个领域也是一直被 [JavaScript](http://demosthenes.info/blog/javascript) 所占据着。需要明白，这么做不会改变原始的 HTML 代码顺序：通过在浏览器上查看源代码可以证明。

There’s a great deal more to explore in [flexbox](http://demosthenes.info/blog/css/flexbox), so much so that it can quickly become overwhelming. Hopefully this article has provided a good start; in future articles I’ll demonstrate other ways in which flexbox can help make previously “impossible” page designs achievable by designers.

关于 [flexbox](http://demosthenes.info/blog/css/flexbox) 值得探讨的有很多，以至于它势不可挡。希望这篇文章是一个很好的开端；我会在接下来的文章中展示，使用 flexbox 实现那些曾被认为“不可能”的页面设计。

>Phillip Walton wrote a great GitHub microsite that also [presents flexbox from a design-oriented perspective](http://philipwalton.github.io/solved-by-flexbox).

>Phillip Walton 的一个很棒的 GitHub 小站同样[以设计的观点透视了 flexbox](http://philipwalton.github.io/solved-by-flexbox)。

原文：http://demosthenes.info/blog/780/A-Designers-Guide-To-Flexbox

外刊君推荐：

http://www.w3.org/TR/css3-flexbox/

http://demosthenes.info/blog/css/flexbox

http://css-tricks.com/snippets/css/a-guide-to-flexbox/