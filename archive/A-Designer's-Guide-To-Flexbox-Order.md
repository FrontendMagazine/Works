> 译者注：本篇为 Flexbox 设计指南系列第三篇，第一篇[在这里](http://zhuanlan.zhihu.com/FrontendMagazine/19955794)，第二篇[在这](http://zhuanlan.zhihu.com/FrontendMagazine/19978387)。

# A Designer’s Guide To Flexbox: Order #
# Flexbox 设计指南3：排序 #

One of the most powerful features of flexbox is its ability to re-order page content with just a single line of CSS, a feature that previously was only achievable with JavaScript. This makes the flexbox layout module incredibly useful for responsive design, which is much more than “making things smaller”: mobile-first design often includes such features as re-prioritization of content to create different reading orders.

flexbox 强悍之处千千万，有一点不得不提及：你只需要一行 CSS，就可以调换页面内容的顺序，这个效果从前只能借助 [JavaScript](http://demosthenes.info/blog/javascript) 完成呢。这个特性在[响应式设计](http://demosthenes.info/blog/responsive-design)中有多大的便利，相信聪明的你马上就能想象到了。[移动优先的设计](http://demosthenes.info/blog/334/Turn-Web-Development-On-Its-Head-Design-For-Mobile-First)通常需要通过重新优化内容来创建不同的阅读顺序，flexbox 在这样的场景里就很好用了。

It also means that designers have an enhanced ability to easily re-order the elements they’ve crafted for a page. Unfortunately, the vast majority of flexbox support documentation is technical, and aimed towards developers. This series aims to correct that.

这样，设计师也可以轻易地更改设计上内容排版顺序。不过大部分 flexbox 支持文档都比较技术性，主要面向开发者的。我们所撰写的 flexbox 系列就是想尝试扭转这个情况的。

In previous entries I’ve talked about basic horizontal and vertical layout with flexbox. For this example, I’ll use the classical Roman order of columns:

在之前的文章里，我们已经聊过了基本的 flexbox [水平布局](http://demosthenes.info/blog/780/A-Designers-Guide-To-Flexbox)和[垂直布局](http://demosthenes.info/blog/787/A-Designers-Guide-To-Flexbox-Part-2-Going-Vertical)。在今天的话题中，我会用古罗马柱作为例子：

````
<ul id="roman-columns">
	<li><a href="#">Doric</a><img src="doric-column.png" alt>
	<li><a href="#">Ionic</a><img src="ionic-column.png" alt>
	<li><a href="#">Corinthian</a><img src="corinthian-column.png" alt>
</ul>
````

Displaying the columns in their order of development is the default for flexbox layout:

按照 HTML 标签顺序呈现出来的柱子，使用默认的 flexbox 样式：

````
ul#roman-columns { display: flex; padding: 0; }
ul#roman-columns li { display: flex; flex-direction: column-reverse; align-items: center; }
ul#roman-columns li img { width: 100%; height: auto; }
ul#roman-columns li a { text-decoration: none; font-size: 1.5rem; color: #000; margin: 1rem; }
````

>(Note that I’m also using flexbox within each list item to place the images above their linked names; as that is not the focus here, I’ve left that detail for the CodePen repo associated with this article. Interestingly, flexbox layout automatically suppresses style-type (i.e. numbering and decoration) for lists.

>请注意，在*列表项中*我也使用了 flexbox，这是为了保证图片可以在对应名字的上部出现；不过这个效果不是今天的重点，有兴趣的同学可以戳一戳[这里](http://codepen.io/dudleystorey/pen/HwdCf)看完整效果，另外 flexbox 布局会自动忽略列表的 `style-type`（比如序号和点）。

As I’ve shown previously, it’s easy to reverse this layout:

我[从前也用过的](http://demosthenes.info/blog/780/A-Designers-Guide-To-Flexbox)，下面这行代码加上去之后，布局顺序就倒过来了：

```
ul { display: flex; flex-direction: row-reverse; }
```

Which creates:

效果是酱紫的：

![](http://s3-us-west-2.amazonaws.com/s.cdpn.io/4273/ionic-column.png)

It’s important to note that this change in order is not reflected in the DOM.

要注意哦，内容呈现的顺序是不同了，但是 DOM 结构并没有发生变化。

That’s neat and all, but not nearly as great as what comes next. Reverting the CSS to what we had originally, we add the following:

这看起来已经很整齐了，但是比起下面要说的这个方法它还有点逊。先把我们的 CSS 还原到起始状态，然后加上这一句：

````
ul li:last-child { order: -1; }
````

A negative order value will place an element at the start:

`order` 为*负值*可以控制元素调整到起始位置：

![](http://s3-us-west-2.amazonaws.com/s.cdpn.io/4273/doric-column.png)

Whereas a positive value will place an element at the end:

相对的，`order` 为*正值*会让一个元素被放在最后一个位置：

````
ul li:first-child { order: 1; }
````

![](http://s3-us-west-2.amazonaws.com/s.cdpn.io/4273/doric-column.png)

Of course, order can be applied via any selector you wish: class, id, or pseudo-element.

当然，任何 [CSS 选择器](http://demosthenes.info/blog/css/selectors)（class、id 或者 伪元素）都可以控制 `order` 属性。

The way order is applied can get confusing, as it would seem to make sense that a value like order: 1 would place the element first, not last. The easiest way to understand this rather counter-intuitive behaviour is to remember three points:

`order` 的使用方法可能有点反人类，它的值看起来似乎是在告诉浏览器元素呈现的顺序：`order:1` 的意思居然不是“把这个元素放到第一位”而是“把这个元素放到最后一位”。你可以遵循下面这三点来理解 `order`：

+ All flex items have a default order value of 0
+ 所有的 flex 元素默认的 `order` 值都是0

+ By default, flex items will display in source order (i.e. the order the elements appear in the DOM).
+ 默认情况下，flex 元素的呈现顺序就是*资源顺序 (source order)*（也就是 DOM 中元素的顺序）。

+ If they are provided with an order value greater or less than 0, flex items are arranged relative to each other. Therefore the result of using both declarations above would be:
+ 如果存在，元素的 `order` 值大于或者小于0，那么这样的 flex 元素就会被按照它们的 `order` 值相对大小来排列。所以才会呈现出这样的效果：

![](http://s3-us-west-2.amazonaws.com/s.cdpn.io/4273/doric-column.png)

So an element with order: -2 would be placed further “back” in the arrangement of flex items: in this case, on the extreme left.

所以如果有一个元素是 `order:-2`，它会出现在现有 flex-item 的“背部”：在这例子里，它会出现在最左边。

>Note that older browsers require vendor prefixes and/or different property names for flexbox: I’ve only shown the properties and values for the final flexbox specification in these examples. I would recommend using Autoprefixer in your build process to take care of the variations. 

>注意：在早期的浏览器中，使用 flexbox 需要特定的[浏览器前缀](http://demosthenes.info/blog/217/CSS-Vendor-Prefixes-and-Flags)和/或者完全不同的属性名字，在例子里我使用的都是规范所定下的最新的属性和对应值。做兼容的话，建议大家使用 [Autoprefixer](https://github.com/postcss/autoprefixer)。

## Possibilities For Animation ##
## 动画的使用 ##

According to the spec, order is an animatable property, meaning that it should be possible to create Isotope-like animations for flexbox elements as they swap position using CSS transitions. Unfortunately, I’m not aware of any browser that yet supports this feature.

[规范规定](http://www.w3.org/TR/css3-flexbox/#order-property)，`order` 是一个可动画的属性，这说明在调换 flebox 元素顺序时，可以使用 [CSS transitions](http://demosthenes.info/blog/css/animation) 达到 [Isotope](http://isotope.metafizzy.co/) 这样的效果。不过悲剧的是，貌似现在还没有浏览器能支持。

## Conclusion ##
## 结论 ##

While it is a little counter-intuitive at first, order is easy to grasp and apply, making the rearrangement of elements on a page with flexbox layout simple. As I’ll show in the next article, this feature is particularly useful in responsive design.

虽然 `order` 一开始用起来有点反人类，但它还是相当好理解和应用的。有了 `order`，在 flexbox 布局中我们可以轻松地变换元素的布局。也许在下一篇文章中，我可以给各位好好展示一下它在响应式设计当中的特殊作用。

原文：[A Designer’s Guide To Flexbox: Order](http://demosthenes.info/blog/920/A-Designers-Guide-To-Flexbox-Order)

外刊君推荐：