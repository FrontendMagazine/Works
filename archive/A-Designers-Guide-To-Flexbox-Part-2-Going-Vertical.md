# A Designer’s Guide To Flexbox, Part 2: Going Vertical
# Flexbox 设计指南2：垂直布局

>译者注：本篇接 [Flexbox 设计指南](http://zhuanlan.zhihu.com/FrontendMagazine/19955794)

As I’ve shown previously, flexbox is a powerful CSS layout engine. One of its many advantages is the fact that a horizontal flexbox layout can be displayed vertically with a single line of CSS code, with elements rearranged in any order you wish. This makes flexbox perfectly suited to modern responsive design, where layouts are commonly re-oriented for smaller screens. In this article I’ll look at more of the powerful options this creates for designers.  

一如之前说过的，[flexbox](http://demosthenes.info/blog/css/flexbox) 是一个很强大的 [CSS 布局模块](http://demosthenes.info/blog/css/layouts)。它的优点之一就是，如果你想要将水平布局的 flexbox 改为垂直布局，一行 CSS 代码就能搞定，而且盒子中的元素排列顺序也能用 CSS 来更改。这个特性简直就是为了现在流行的[响应式设计](http://demosthenes.info/blog/mobile/responsive-design)而存在的，我们都知道为了兼顾用户体验，在小屏幕中页面元素会被重新布局。那么在这篇文章里，我们将会讨论到设计师可以如何利用 flexbox 这一特性。

Let’s take a simple arrangement of shapes:  
首先，先创建一个简单的布局：

````
<figure id="flex">
	<div id="triangle"></div>
	<div id="square-vert"></div>
	<div id="circle-vert"></div>
</figure>
````

Shaped with the following CSS:  
再加上点样式：
  
````
#flex { 
	display: flex; 
	padding: 2rem; 
}
#triangle {
	width: 0; height: 0;
	border-bottom: 114px solid hsl(240, 30%, 50%);
	border-left: 63px solid transparent;
	border-right: 63px solid transparent;
}
#square-vert {
	width: 126px; height: 126px;
	background: hsl(300, 30%, 50%);
}
#circle-vert {
	width: 126px; height: 126px;
	border-radius: 50%;
	background: hsl(340, 30%, 50%);
}
````

By default, the elements are arranged along a horizontal axis. To change that, we just need a simple declaration:  
这么写完之后，这三个元素会水平显示。想要改变它们的显示方向，我们只需要加上一行 CSS 语句：  

````
#flex {
	flex-direction: column;
}
````  
![](/images/A-Designers-Guide-To-Flexbox-Part-2-Going-Vertical-01.jpg)

Note that the elements are arranged “top down” by default, and aligned to the left. All the properties I discussed in the previous article can be applied here: it’s just that the context is now vertical, rather than horizontal.  
注意！这三个元素现在是默认地从上向下排列，并且向左对齐。我在上一篇文章中所讨论到的属性，在这个例子里你都能看到：现在上下文是垂直的，而不是水平的。

Placing the elements at the “end” of the flexbox container is easy, so long as the container itself is tall enough to show visible realignment of its children. At the same time, we can align the elements horizontally and reverse their order:  
子元素重新排列后，只要容器的高度依然能够保证其正常显示，那么把子元素放到容器的尾部也很简单。同时让元素垂直对齐，接着倒转它们的顺序。

````
#flex {
	height: 500px;
	justify-content: flex-end;
	flex-direction: column-reverse;
	align-items: center;
}
````

![](/images/A-Designers-Guide-To-Flexbox-Part-2-Going-Vertical-02.jpg)

Combining both horizontal and vertical flexbox alignment finally provides web designers with true, honest-to-goodness perfect centering of elements, one of the techniques shown in my “Seven Ways To Center With CSS” article.  
使用在水平和竖直方向上都对齐的 flexbox，Web 设计师就能实现完美的居中元素。唔，这个方法也是[使用 CSS 实现居中的7种方法](http://demosthenes.info/blog/723/Seven-Ways-of-Centering-With-CSS)之一呢。

## Basic Flexbox For Mobile ##
## 移动端上的 Flexbox 基础篇 ##

Switching a layout between larger and smaller screens becomes very easy with flexbox. Given this pseudo-code:  
使用 flexbox 布局，你能在大小屏幕之间来去自如。举个例子：

````
<header></header>
<div id=wrapper>
	<nav></nav>
	<main></main>
</div>
<footer></footer>
````

We can switch the arrangement of the `<nav>` and `<main>` elements between side-by-side:  
我们可以让 `<nav>` 和 [`<main>`](http://demosthenes.info/blog/648/HTML-51-and-the-main-element) 元素并排布局（水平）：

````
div#wrapper { 
	display: flex; 
}
````

To one above the other at smaller viewport sizes:  
到小屏幕上呈现上下布局：
 
````
@media (max-width: 400px;) {
	div#wrapper { flex-direction: column }
}
````

There’s much more that we can do with flexbox, as I’ll show in upcoming articles.  
到目前为止，你只看到了 [flexbox](http://demosthenes.info/blog/css/flexbox) 的冰山一角，请期待后续的文章，我将展现出更多 flexbox 魔法。

原文：[A Designer’s Guide To Flexbox, Part 2: Going Vertical](http://demosthenes.info/blog/787/A-Designers-Guide-To-Flexbox-Part-2-Going-Vertical)