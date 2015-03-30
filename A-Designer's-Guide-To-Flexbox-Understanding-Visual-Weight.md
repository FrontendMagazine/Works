# A Designer’s Guide To Flexbox: Understanding Visual Weight #
# Flexbox设计指南： 视觉重量#
> 译者注：本篇接[Flexbox 设计指南2：垂直布局](http://zhuanlan.zhihu.com/FrontendMagazine/19978387)

One of the most notoriously difficult parts of the CSS flexbox spec to understand is the eponymous flex property, which uses cryptic values like 1 1 auto. Confusion only deepens when the designer discovers that flex is merely a shortcut for properties with even stranger names like flex-grow and flex-basis.    
[CSS flexbox](http://demosthenes.info/blog/css/flexbox) 标准中最费解的一点就是 ```flex``` 属性，常常我们会看到诸如 ```1 1 auto``` 这样含义模糊的属性值。当发现 ```flex``` 其实只是另外一些都不知道是用来干嘛的属性（比如  ```flex-grow```, ```flex-basis```）的简写时，设计师们就更加摸不着头脑了。

Most explanations of the flexbox spec are fairly technical and dense, with few concessions to visual thinkers. After struggling with the properties and values, I realized that these aspects of flexbox are actually talking about visual weight.  
关于 flexbox 标准大部分的讲解都相对地技术性，这对视觉思考者来讲可能有些难以理解。在属性和值之间纠结了半天之后，我终于意识到了 flexbox 的 ```flex``` 属性其实说的**视觉重量 (Visual Weight)**。

## A Not-So-Egalitarian Algorithm ##
## 不太平均的算法 ##

It would be reasonable to assume that the flexbox model might start by distributing  all flexbox items equally. That is, given a series of <article> elements inside a <section>:    
我们很自然地会去假设伸缩盒模型（flexbox model）会将盒子内的元素平均地分布其中。比如，如果在 ```<section>``` 中有一系列的 ```<article>``` 元素：


````
<h1>The Fortean World Times</h1>
<section>
	<article>
		<img src="earth-vs-the-flying-saucers.jpg" alt>
		<h1>Washington D.C. Attacked By Flying Saucers</h1>
		<h2>Dateline Washington D.C.</h2>
		<h3>Frank Bragg reporting</h3>
		<p>The country was brought to a standstill today when flying saucers appeared over the nation’s capital.
	</article>
	<article>
	…
</section>
````

With the <section> provided with display: flex, you might expect each article to have equal width by default, but that’s not the case:    
在```<section>```上添加```display: flex```样式，你可能希望看到每一篇文章都能按照相同宽度排版，但现实一定会狠狠打击你的：

As you can see, the flex items have different widths, depending on their content.  
你看，flex项其实是根据它们的内容宽度来排版的，所以宽度不同。
![](http://demosthenes.info/assets/images/flexbox-distribution.jpg.pagespeed.ce.MTr_VFZFM9.jpg)

## Providing Flex Items With Equal Width ##
## 给 Flex 元素设置相同的宽度 ##

If we have three articles in the <section> you might go to the instinctive solution of giving each <article> a width of 33% to make them equal. While that will work, it’s a suboptimal solution that removes many of the advantages of flexbox layout. Instead, I would recommend the following:  
如果已经知道在 ```<section>``` 中要放置三篇文章，而且三篇文章的要按照相同宽度来排版，你的直觉应该会告诉你给每一个 ```<article>``` 的宽度设置成33%。虽然说这个方案也是可行的，但在flexbox布局中，这么做绝对是下下之策。设置宽度的方法直接无视了flexbox布局的种种优势，所以，我要推荐下面这样的写法：
  
````
article { flex: 1 0 0px; }
````

Which translates to:   
上面这条样式可以分解为：
  
````
flex-grow: 1;
````  
“make sure this element grows in width equally to all other flex items with a value of flex-grow: 1”  
“flex-grow 的值为1，确保了这个元素的宽度的增长和其他的 flex 项宽度的增长相同”

````
flex-shrink: 0;
````  
“don’t shrink this element more than any others to fit in the available space”)  
“为了适应空间，而将当前元素过度压缩，使其宽度比其他元素要小的方案是不可取的”

````
flex-basis: 0px;
````  
“start this element at 0 pixels wide, and adjust from there”  
“从 0px 开始计算这个元素的宽度”

In turn, this can be shortcut to:  
按照以上顺序呢，我们可以简写为这样：
````
article { flex: 1; }
````

I would recommend using this shortcut in your CSS: not only is it quicker to write, but it has better support cross-browser right now.    
我建议在CSS中使用简写的方式，一是为了快速书写，二是对浏览器兼容更友好。

**Note that if flex is shortcut with a unitless value, it defines flex-grow. When a single flex value is defined with a unit (px, etc), the shortcut defines the flex-basis value.**  
**注意：如果简写的 ```flex``` 属性，对应的值是没有单位的，说明它定义的是 ```flex-grow```。如果有单位（比如px），那么这个简写定义的就是 ```flex-basis``` 的值。**

Written as part of the following CSS:  
````
section { display: flex; }
article { margin: 1rem; flex: 1; }
article img { width: 100%; height: auto; }
````
… the CSS creates equal width and distribution of the flex “columns”:   

使用以下的样式，可以创建出按照相同宽度分布的 flex 列：
````
section { display: flex; }
article { margin: 1rem; flex: 1; }
article img { width: 100%; height: auto; }
````

The advantage of this system is that it’s adaptive: add another <article> element, and the space is distributed automatically between them, with all articles keeping the same width, and no need to adjust any of your CSS.  
这么来布局的好处就是页面可以自适应：如果添加一个新的 ```<article>``` 元素，不需要更改CSS，每个元素所占空间就会自动调整，以满足所有文章依然保持相同宽度。

## Providing A Flex Item With More Visual Weight ##
## 给flex项加上更多的视觉重量 ##

The example we’ve been working on displays articles for an online newspaper. For such a layout, it would make sense that you would occasionally need a larger space for a breaking story, which we’ll apply as a class:  
这篇文章里我们用的例子是在线报纸上的文章展示，在这样的场景下，我们可能会遇到一些“重大事件”的排版，需要用更大的空间来显示它们，所以我们要增加下面这个 class：

````
<article class="breaking">
````

It’s reasonable to assume that breaking stories should always take twice as much space as any other articles on the page. I’ll use another flex shortcut to define a new flex-grow value for this particular class:  
这样的文章大小通常要比其他普通文章的大上一倍，它的 ```flex-grow``` 样式也要特别设置一下：	

````
article.breaking { flex: 2; }
````

However it is applied, flex-grow remains true no matter how big or small the space for the articles becomes: the breaking news article remains always twice as wide as the others: i.e. it always has twice as much visual weight. (The article’s height is determined by it’s content, by default, following standard web layout principles: the one special consideration is that all flex items in a row are the height of the item with the most content).  
这条样式生效之后，无论文章所占空间是变小还是变大 ```flex-grow``` 始终保持这样的关系：重要文章的所占空间始终是普通文章的两倍，也就是说，**重要文章的视觉重量始终两倍于普通文章**。（默认情况下按照 Web 布局标准，文章的高度取决于内容的多少，不过标准中有一个特别的考虑就是，所有水平排列的flex项目的高度由内容最多的那一项来决定）。

## Adding A Solution For Small Screens ##
## 小屏适配 ##

Obviously at a certain point cramming articles side-by-side will create unreadable content. At a reasonable breakpoint, we’ll distribute the articles in a column, rather than a row:  
显然，从某种意义上来说文章的排版过分拥挤会带来很糟糕的阅读体验。所以在某个临界位置，我们要将文章改成竖直排版，而不是水平排版了：

````
@media screen and (max-width: 750px) {
	section { -webkit-box-orient: vertical; flex-direction: column; }
}
````

**The -webkit-box-orient: vertical is for iOS 7, which somehow didn’t carry through the flex-direction: column part of the specification, while it does support all the rest, albeit with vendor prefixes).**  

**```flex-direction: column``` 在iOS7上没有实现，所以需要使用 ``` -webkit-box-orient: vertical``` 完成小屏幕的适配，iOS7可以支持 flexbox 的其他属性，不过要记得加上浏览器前缀哦。**

Note that flex-grow doesn’t make any difference now: all the articles are the same width when they are arranged vertically, with each article’s height determined by its content.  
竖直布局之后，```flex-grow``` 是没有意义的：所有的文章的宽度都是一样的，而高度由每篇文章的内容多少决定。

## Conclusion ##
## 结论 ##

grow is only one part of the visual weight controls in flexbox: the next articles in this series will deal with the two other aspects, flex-shrink and flex-basis.
```grow```只是 **[flexbox](http://demosthenes.info/blog/css/flexbox)** 控制视觉重量的一部分：本系列的下一篇文章会讨论 ```flex-shrink``` 和 ```flex-basis``` 这两个属性，请大家继续关注。

原文：[A Designer’s Guide To Flexbox: Understanding Visual Weight](http://demosthenes.info/blog/901/A-Designers-Guide-To-Flexbox-Understanding-Visual-Weight)