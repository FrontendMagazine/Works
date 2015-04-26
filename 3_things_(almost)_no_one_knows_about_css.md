# 3 Things (Almost) No One Knows About CSS

# 关于CSS[几乎]没人知道的3件事

Think you know CSS? If the results of a [free CSS test](https://sitthetest.com/tests) I’ve offered online for the past six months are anything to go on, plenty of practicing developers don’t know CSS as well as they think. Out of over 3,000 people who have taken the test so far, the average score was just 55%.

你了解 CSS 吗？在六个月前，我提供了一个在线[免费 CSS 测试](https://sitthetest.com/tests)系统。测试结果表明很多一线开发者并没有如他们所想的那样了解 CSS。目前有超过 3,000 人参加了该项测试，平均成绩只有 55 分。

But hey, an average isn’t that interesting by itself. I was more curious about which questions people were getting wrong. For this article, I’ve run the numbers, and zeroed in on three questions where people scored especially badly. I’ll talk you through each question, show you the answer that most people chose, and explain the correct answer.

但是，嘿，平均分本身并没有什么意思。我更加关心大家都在哪些问题上栽了。这篇文章中，按照出错的程度将其中三个问题列出来。我会和你讨论每个问题，告诉你哪个答案被选择的最多，然后解释正确答案。

It’s safe to say that if you [take the test yourself](https://sitthetest.com/tests) after reading this, you’ll have an unfair advantage!

可以肯定地说，如果你读完这篇文章后[参加测试]((https://sitthetest.com/tests))，将会有不公平的优势！

## Question 1: How Best to Set `line-height`

## Q1：设置 `line-height` 的最佳方式

This first question should have been easy for anyone who deals with text styles on a regular basis:

第一个问题对于有操作过文本样式常规基础的开发者来说应该很简单：

>**You want text on your website to be double-spaced by default. Which of the following line-height values is the best way to achieve this?**
>**想要让站点内文字默认为双倍行距。下面哪个 `line-height` 值是最佳实现方式？**

- 200%
- 2em
- 2
- double

With four answers to choose from, you’d expect 25% of people to get the right answer by luck alone, and only 31% got this one right! Take a minute and pick out an answer for yourself, then read on.

对于这4个答案，你可能觉得仅仅凭运气也会有 25% 的人会答对，但仅仅 31% 的人回答正确！花上一分钟时间选择你认为的正确答案，然后继续。

First off, `double` is a red herring. The only keyword value that `line-height` accepts is `normal`. I’m happy to say that only 9% of people fell for this one. The remaining three answers were all pretty popular, though.

首先排除，`double` 只是用来混淆你。 `line-height` 接受的唯一关键字是  `normal`。我很高兴地说，仅仅有 9% 被这个选项蒙蔽。尽管，剩下的三个答案都非常得普遍。

The answer that most people selected is `2em` (39% chose this). Indeed, `2em` will certainly give you double-spaced text for the element it’s applied to; but so will `200%`, and only 21% liked that answer! Either ems are much more in fashion than percentages, or people don’t really understand them.

大多数测试者选择了 `2em`（39%）。实际上，`2em` 确实会将它所应用的元素内的文本渲染为双倍行距；不过 `200%` ，仅仅有 21% 选择了这个答案！或者 `ems` 相比百分数更加流行， 又或者人们根本就没有理解他们。

The correct answer, though, is `2`.

然而，正确答案是 `2`。

This is a lesson that was drilled into me a long time ago, when I was first learning CSS. **Always specify** `line-height` **as a unitless number**; that way, descendent elements that specify a different `font-size` will inherit that number rather than a fixed line height.

这是一个很久以前的一个教训，那时我第一次学习 CSS。**确保将** `line-height` **指定为一个无单位的数值**；这样一来，指定了不同 `font-size` 的子元素将会集成这个数值而不是一个固定的高度。

Let’s say the page has a default `font-size` of `12pt`, but it also contains a heading with a `font-size` of `24pt`. If you set the `line-height` of the body to `2em` (or `200%`), then you’ll get a line height of exactly `24pt` (twice the body’s `font-size`) everwhere in the document—even in that heading. The heading will therefore be single-spaced, not double-spaced!

我们假设页面默认 `12pt` 的 `font-size`，不过它也会包含一个 `font-size` 为 `24pt` 的头部。如果你将 body 的 `line-height` 设置为`2em` 或者 `200%`，那样在文档中（当然也包括头部）就会得到一个 `24pt` 的行高（body 的 `font-size` 的两倍）。因而，头部就是单倍行距，而不是双倍！

Setting `line-height` to `2` instead tells the browser to preserve the font-size/line-height ratio even when the font size changes. The line height for the body will be `24pt`, but for the heading’s `24pt` font, the line height will automaticlly increase to `48pt`.

相反，将 `line-height` 设置为 `2` 会告知浏览器保持 font-size/line-height 比例，即便文字的尺寸发生变化。body 的行高将是 `24pt`，而头部的文字为 `24pt`，行高也将自动增长为 `48pt`。

## Question 2: How to Make Elements Overlap

## Q2：如何让元素部分重叠

This question was a bit trickier. It called for some experience of the “dirty tricks” that CSS layout often requires:

这个问题有一点棘手。需要一些 CSS 布局总会用到的“奇技淫巧”：

>**Which of the following CSS properties, used by itself, can cause HTML elements to overlap?**
>**仅仅借助下面的哪个 CSS 属性，可以实现 HTML 元素部分重叠？**

- z-index
- margin
- overflow
- background

Got an answer picked out? Okay, let’s dive in.

请选择一个正确答案？OK，我们深入看一下。

Once again, there was an easily-eliminated option: `background`. All but 2% of test takers steered clear of it, knowing that it controls background colors and images.

同样，有一个很容易排除的选项： `background`。除了2%的测试者外都避开了它，知道它控制背景颜色和图像。

Unfortunately, most steered straight into `z-index`. A full 46% of people jumped on that one. I’m guessing it’s because no one really understands how `z-index` works. In fact, setting the `z-index` property by itself has no effect whatsoever; you also need to set the `position` property of an element for z-index to do anything. In short, `z-index` lets you control the stacking order of elements that do overlap, but they need to be overlapping in the first place. MDN has [a really nice article called “Understanding CSS z-index”](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Understanding_z_index) that’s worth a read for more detail.

很不幸，很多测试者之间选择了 `z-index`。几乎有 46% 跳进了这个选项。我猜测是因为他们没有真正地理解 `z-index` 的工作原理。实际上，自身设置 `z-index` 属性根本无济于事；你还需要设置元素的 `position` 属性让 `z-index` 起作用。简而言之， `z-index` 控制了部分重叠元素间的堆放顺序，不过前提是他们要重叠。MDN 上有一篇非常棒的文章叫做[《理解 CSS z-index》](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Understanding_z_index)，非常值得仔细阅读。

`overflow` should have been easy to eliminate too, if you’ve ever used it. It controls how content that doesn’t fit inside a sized box behaves: whether it’s chopped off, whether it flows out past the edges of the box, etc. Again, this depends on the box’s size being constrained with other properties; by itself, it won’t cause overlaps. Still, 22% of people thought it might.

如果你曾用到过，`overflow` 同样也很容易排除。它用来在一个固定尺寸的盒子内控制内部元素的行为：是否被遮盖，是否在盒子边框外部展示，等等。而且，这会需要一些其他 CSS 属性所控制的盒子尺寸；仅仅通过它是不会导致部分遮盖的。然而，22% 的测试者认为它可以。

That leaves us with `margin`, which is the right answer. Only 30% of people got it. You might wonder how on earth a property that creates distance between elements can cause them to overlap. If you’ve done any real-world CSS layout, the answer should be obvious: **negative margins make things overlap.**

剩下的就是 `margin`，也就是正确答案。仅仅 30% 的测试者选择了它。你可能好奇究竟一个属性如何做到元素间的相互遮盖。如果你具备一些 CSS 布局的经验，答案应该很明显：**负值 margin 让他们相互遮盖。**

To demonstrate this, create a page with two `div` elements. Set the `margin-top` on the second `div` to a negative measurement, for example `-100px`. Bam! The second `div` now covers the bottom one hundred pixels of the first `div`.

为了演示这个，我创建了一个仅有两个 `div` 元素的页面。将第二个 `div` 的 `margin-top` 值设置成负值，比如 `-100px`。砰！现在第二个 `div` 覆盖了第一个 `div` 100 像素。

In practice, you’d almost never overlap blocks like this on purpose, but negative margins are extremely useful for squeezing HTML elements into places they don’t normally go. I often use them to push left- or right-floated elements into the padding region of their parent box.

实际上，你几乎不会有意地这样覆盖一个区块，不过负值 margin 在处理外层空间小于 HTML 元素大小的情况时异常地好用。

![](http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2015/04/1429090154fig-negative-margin-float.png)

For web design history buffs, in 2005 overlapping elements with negative margins is what made possible three-column page layouts such as the so-called [One True Layout](http://positioniseverything.net/articles/onetruelayout/) (and later the [Holy Grail layout](http://alistapart.com/article/holygrail)).

对 web 设计历史爱好者来讲，在 2005 年负值 margin 部分覆盖元素使得三列页面布局成为可能，比如所谓的[One True Layout](http://positioniseverything.net/articles/onetruelayout/)（以及后来的[圣杯布局](http://alistapart.com/article/holygrail)）。

## Question 3: Pseudo-elements vs Pseudo-classes
## Q3：伪元素 VS 伪类

This last question is a bit of a cheap shot, I’ll admit. But with only 23% of test takers able to answer it correctly (that’s worse than chance!), it clearly hits on a point of confusion:

最后一个问题有一点卑劣，我承认。不过只有 23% 的测试者回答正确（比碰运气还糟糕！）。毫无疑问，它挖出了一个疑点。

>**Which of the following effects is best achieved using a pseudo-element?**
>**下面哪个效果可以通过伪元素很好地实现？**

- Add a drop-shadow to a hyperlink when a user hovers their mouse over it.
- Display the label of a checkbox in a different color when the checkbox is checked.
- Give the even and odd rows of a table different background colors.
- In a flexible page layout, display the first line of a paragraph in bold text.

- 当用户将鼠标悬停在超链接上时，为其添加一个投影；
- 当复选框选中时，在这个复选框的标签上显示一种不同的颜色；
- 为表格的偶数行和奇数行添加不同的背景色；
- 在弹性布局中，将段落的第一行加粗显示。

Three of these choices are effects you achieve with a pseudo-class; only one involves a pseudo-element. Can you tell the difference?

其中三个效果需要借助伪类实现；仅仅有一个需要伪元素。你能区分出不同吗？

A **pseudo-class** is a particular state in which an actual HTML element can find itself. Think of it as a virtual class that is applied to the element automatically by the browser under certain conditions.

**伪类**是一个真实 HTML 元素上的一个特殊的状态。可以认为是浏览器在特定条件下将一个虚拟的类自动应用于某个元素。

A **pseudo-element** is a part of the document that CSS lets you style even though it isn’t an actual HTML element. It’s like a virtual HTML element—something you get to style even though it doesn’t have actual HTML tags around it.

**伪元素**是 HTML 文档的一部分，尽管它不是真实的 HTML 元素，但是 CSS 允许你为它设置样式。就像是虚拟的 HTML 元素——尽管它没有真实的 HTML 标签，但你仍可以为其添加样式。

With this distinction in mind, let’s run through the options:

先记住这点，我们再来看看下面的选项：

### Add a drop-shadow to a hyperlink when a user hovers their mouse over it.
### 当用户将鼠标悬停在超链接上时，为其添加一个投影

A hyperlink is an actual HTML element. Applying styles to it only in a particular situation (when the mouse is over it) means we’re using a pseudo-class. The pseudo-class you’d use in this case is `:hover`.

超链接是一个真是的 HTML 元素。仅仅在特殊情况下（鼠标悬停）为其应用样式，也就是说我们需要使用伪类。该情况下你应该使用 `:hover` 伪类。

22% of test takers thought this was a pseudo-element.

22% 的测试者误认为这是伪元素。


### Display the label of a checkbox in a different color when the checkbox is checked.

### 当复选框选中时，在这个复选框的标签上显示一种不同的颜色

Again, a `label` is an actual HTML element, not a virtual one. When a checkbox is checked, the browser applies the `:checked` pseudo-class to it. You can then use it in your selectors to style the checkbox, or even a `label` next to it (e.g. using an adjacent sibling selector with `+`).

不过， `label` 是一个真实的 HTML 元素，不是虚拟的。当复选框被选中时，浏览器会将 `:checked` 伪类应用于它。然后，你就能够在选择器中使用它为复选框添加样式，甚至和它相邻的  `label` 元素（比如：使用相邻元素选择器 `+`）。

20% of test takers thought this was a pseudo-element.

20% 的测试者认为这是一个伪元素。

### Give the even and odd rows of a table different background colors.

### 为表格的偶数行和奇数行添加不同的背景色

This is the one that really fooled people, but once again we’re talking about applying styles to actual HTML elements (`tr` elements, in this case). A `tr` being even- or odd-numbered within its parent element’s collection of children is just another circumstance that you can match with a pseudo-class.

这着实愚弄了很多人，不过重申一遍我们讨论的是将样式应用于真是的 HTML 元素（在本例中是 `tr` 元素）。在各自父元素的子元素中的偶数或者奇数行的 `tr` 只是另外一种符合伪类的情景。

In this case, the pseudo-class is `:nth-child(even)` (or `:nth-child(2n)`) for even elements, and `:nth-child(odd)` (or `:nth-child(2n+1)`) for odd ones.

在这个示例中，对于偶数行伪类是 `:nth-child(even)`（或者 `:nth-child(2n)`），对于奇数行则是  `:nth-child(odd)` （或者 `:nth-child(2n+1)`）。

I’m guessing it’s just because `:nth-child` and pseudo-elements in general both sound like really obscure CSS features, but a full 36% of test takers picked this as a pseudo-element.

我猜测它仅仅因为 `:nth-child` 和伪元素听起来都和真实的 CSS 特性很相似，但是 36% 的测试者将它选择成伪元素。

### In a flexible page layout, display the first line of a paragraph in bold text.

### 在弹性布局中，将段落的第一行加粗显示

This, of course, is the right answer. And by now, hopefully, the distinction is clear. In a flexible page layout, you can’t look at the HTML code of a page and say “that element there contains just the first line of the paragraph’s text”. The browser does the word-wrapping depending on the width of the paragraph, which is something you don’t get to control in a flexible page layout.

很明显，这是正确答案。现在，单元我们的讨论足够清晰。在弹性布局中，你无法看到页面中的 HTML 代码只能假想“里面仅仅包含段落文本的第一行”。浏览器会根据段落的宽度进行换行，这在弹性布局总是你无法控制的。

`:first-line` is the pseudo-element that lets you apply styles to the first line of text in a block, no matter where that first line wraps to the second.

`:first-line` 是允许你将样式应用于文本块第一行的伪元素，无论第一行在何处换到第二行。

If you’re thinking “OK, sure, that all makes sense, but c’mon—no one knows the difference between pseudo-elements and pseudo-classes”, well the W3C agrees with you. In the [CSS3 Selectors spec](http://dev.w3.org/csswg/selectors-3/#pseudo-elements), in an attempt to distinguish the two, it changed the syntax so that pseudo-element selectors use two colons (`::first-line`), while pseudo-classes still use one (`:hover`). Of course, for backwards compatibility, browsers must support both versions.

如果你正在想“OK，确实，可以讲得通，但是拜托——每人知道伪元素和伪类之间有什么区别”，确实 W3C 也同意你的管段。在 [CSS3 选择器规范](http://dev.w3.org/csswg/selectors-3/#pseudo-elements)中，在区分二者上做了一次尝试，改变了语法——伪元素选择器使用两个冒号（`::first-line`），而伪类依旧使用一个（`:hover`）。当然，为了向后兼容，浏览器必须支持这两个版本。

So yeah, like I said: cheap shot. But hey, if you’re a CSS geek like me, I imagine you’d know your pseudo-elements from your pseudo-classes.

是滴，如我所说：卑劣。不过，如果你和我一样是一个 CSS 骇客，我确信你了解伪元素和伪类的区别。

## How’d You Do?

## 你做的怎么样？

So that’s it: three tough questions from the test. If you answered just one of them with confidence, you’re doing okay. Got two of them? Not bad at all. If you got all three, I’d love to hear from you! Especially now that I’ve given away the answers to these, I could really use some ideas for more tricky CSS questions. Post ’em in the comments!

这就是测试中三个棘手的问题。如果你仅仅对一个有信心，做的不错。两个？一点也不差。答对了三个，我非常乐意听到！现在，我已经公布了这些问题的答案，同样也可以用这些观点解决更棘手的 CSS 问题。请在评论中留言。

If you enjoyed these questions, maybe you’d like to give the [rest of the test](https://sitthetest.com/tests) a try. Rest assured, the other questions are much easier than these … mostly!

如果你享受这些问题，或许你可以尝试一下[测试的其他部分](https://sitthetest.com/tests)。请放心，其他的问题比这些要简单许多许多！

>![Kevin Yank](http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2015/04/1429112558kevinyank-96x96.jpg)

>**Kevin Yank**

----

>Kevin began developing for the Web in 1995 and is a highly respected technical author. Kev is a world-renowned author, speaker and JavaScript expert. He has a passion for making web technology easy to understand by anyone. Yes, even you!

>Kevin 于 1995 年接触 Web 开发，是一位备受尊重的技术类作者。Kev 更是一名知名作家、演说家和 JavaScript 专家。他热衷于让每个人都能轻易地理解 Web 技术。是的，包括你！