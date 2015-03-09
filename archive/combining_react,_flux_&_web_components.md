# Combining React, Flux & Web Components
# 结合 React，Flux & Web Components

I recently had an interesting conversation with some very smart colleagues on the topic of UI component reusability on modern web frontend applications. This led me to spend a few hours on a lazy Sunday afternoon clarifying my thoughts on the status quo - and the road ahead - of web frontend architectures. Specifically, I find it interesting how the short-term future seems to somewhat contradict the assumed long-term future.

最近，我和几个聪明的同事针对于当下的 web 前端应用中 UI 组件重用性的话题进行了一次争论。然后在一个懒散的周末下午花了几个小时，让我在关于 web 前端架构的现状以及未来的路方面，思路更加清晰。值得一提的是，我发现一件很有趣的事，短期的未来似乎和假定的长期愿景有些矛盾。

## The Short-term Future
## 短期未来

React is currently the “new hotness” and seems like the immediate future of the web frontend, and along with it come a breadth of projects that either build on top of the framework itself, or on the ideas it has recently popularized. The most important of these ideas is the concept of a “Virtual DOM”, which reduces the need to directly operate on the massive, complex & sluggish shared data structure that is the traditional DOM. React et al have been covered [thoroughly](https://www.youtube.com/watch?v=nYkdrAPrdcw#t=1453) [elsewhere](http://facebook.github.io/react/docs/why-react.html), so let’s move on.

React 无疑是当下的“新宠”并且似乎就是 web 前端的黎明，伴随而来的项目如雨后春笋，不是建立在 React框架自身之上，就是基于它延伸出的思想。这类思想中最重要的就是“Virtual DOM”的概念，它降低了直接操作笨重、复杂并且性能极差的数据结构，即传统 DOM 的需求。React 等已经[遍地](https://www.youtube.com/watch?v=nYkdrAPrdcw#t=1453)[开花](http://facebook.github.io/react/docs/why-react.html)，我们继续。

In the wake of React comes another important part of the immediate future of frontend development: the Flux architecture and its kin. At this point it’s important to note that neither React’s virtual DOM nor the Flux architecture are necessarily the pinnacles of their respective breeds (e.g. virtual-dom seems an excellent, simplified alternative to React, and there’s no shortage of Flux-like architectures lately), but for the purposes of this discussion, let’s treat them as umbrella terms for the ideas they represent.

对于 React 存在的短板，web 前端开发未来的另一个重要的部分应运而生：Flux 框架和它的同类框架。在这点上，无论是 React 的虚拟 DOM，还是 Flux 框架，都不是同类中的翘楚，知晓这个很重要（比如 [virtual-dom](https://github.com/Matt-Esch/virtual-dom) 看起来很棒，React 简化的替代品，并且[近来](https://github.com/moreartyjs/moreartyjs)并不[缺少](http://fluxxor.com/)[类 Flux](https://github.com/spoike/refluxjs) [的](https://github.com/deloreanjs/delorean)[架构](https://github.com/swannodette/om)），但是出于讨论的目的，我们暂且把它们看成是所代表思想的代名词。

The Flux architecture proposes to simplify complex Single-Page Applications through an unidirectional data flow. This is also explained better elsewhere; the point to take away from this is that things become simpler when you just re-render the entire application based on the current state of the world, a single, consistent source of truth, and do it every time that state changes. This reminds us of the good-old-days of server-side rendering, back in the 90’s: you have a bunch of data (e.g. request parameters, stuff from a database), which you turn into HTML by passing it through a function (e.g. your templating implementation). What makes this simple - and crucially different from traditional DOM wrangling in the browser - is you don’t have to care about any existing state in the DOM, you just overwrite everything with a new one.Traditional string-interpolation templating has been possible in the browser for ages, but constantly dumping and re-creating the DOM has been prohibitively expensive for larger apps. A Virtual DOM changes this.

Flux 框架试图通过一个单向的数据流去简化复杂的单页面应用。关于这一点的[详细](http://facebook.github.io/flux/docs/overview.html)解释有[很多](https://www.youtube.com/watch?v=nYkdrAPrdcw#t=622)；这篇文章的中心思想就是在基于目前的 state ，简单重新渲染整个应用使事情变得更简单，并且在每次状态改变时也只是重新渲染。这让我们想起了服务端渲染的好日子，[回溯到上世纪九十年代](https://www.youtube.com/watch?v=nYkdrAPrdcw#t=1588)：你得到一坨数据（例如：请求参数，数据库里的数据），然后将这些传递到一个函数中（比如：模板的实现）并吐出 HTML。让事情变得简单（同样是与传统的浏览器端 DOM 操作最显著的区别）的是你不必去关心 DOM 中已经存在的 state，仅仅使用一个全新的覆盖之。传统的[字符串式](http://underscorejs.org/#template)[模板](http://mustache.github.io/)已经在浏览器端存在很多年，但是频繁的销毁和重绘 DOM 带来的花销让稍大的应用望而却步。Virtual DOM 改变了这个现状。

So this is what I take the short-term future of web frontends to be, and indeed, the ball is already rolling. But is there something even bigger brewing?

这就是我所描述的 web 前端短期未来的样子，而实际上，车轮已经在滚动。是否还有更大的手笔呢？

## The Long-term Future
## 长期前景

Flux et al are still fully present-generation frontend architectures - that is, they’re working off of the same technical platform which has existed in browsers for a long time. In the distant horizon, however, a tectonic shift to this platform awaits: Web Components. I’ll again leave the introductions to more capable hands, but the gist of the spec is being able to create custom DOM elements. These custom elements are isolated from their neighbors (and parents and children), and they encapsulate some small part of the overall application.The canonical example is the &lt;video&gt; element: all you need to do to bring a self-contained video player “mini-application” - complete with a seek bar, playback control buttons, etc - into your main application is to create a &lt;video &gt; element on your page. The internal logic, DOM content, styling and (importantly!) state is neatly packaged away behind a standard DOM element. My go-to quote with this is from Justin Meyer: "The secret to building large apps is never build large apps. Break your applications into small pieces. Then, assemble those testable, bite-sized pieces into your big application". Web Components allows you to do just this: break your large app into a bunch of independent mini-apps, making the complexity of the whole more manageable.

Flux之流仍然属于本世代的前端架构——也就是说，所有这些前端架构都是基于相同的，已经存在很多年的浏览器技术平台。放眼未来，不管怎样，一个全新的模式正走向这个平台：Web Components。我还是把[详细](http://css-tricks.com/modular-future-web-components/)介绍[留给](https://developers.google.com/events/io/sessions/318907648)[高手](http://webcomponents.org/)们，不过新规范的主旨就是让开发者能创建自定义 DOM 元素。这些自定义元素和他们的兄弟节点相互[隔离](http://www.html5rocks.com/en/tutorials/webcomponents/shadowdom/)（以及父节点和子节点），并且它们封装了整个应用的一小部分。最典型的例子就是 &lt;video&gt; 元素：要想在你的主应用里加入一个自我可控的视频播放器“mini-application”（有进度调，播放按钮等）你只需要在页面上创建一个 &lt;video&gt; 元素。内部的逻辑，DOM 内容，样式和状态（最重要的！）完整地打包成一个标准的 DOM 元素。这里面大多的引用源自 [Justin Meyer](https://twitter.com/justinbmeyer)：[“构建大应用的秘籍就是永远不要一下子做成大应用。要将你的应用分割为小的模块。然后组装那些测试过的，很小的模块到你的大应用中“。](http://addyosmani.com/largescalejavascript/)Web Components 刚好帮你这样做了：把一个很大的应用拆分成很多个独立的小应用，使得复杂的应用更好管理。

Another important implication of being able to abstract these mini-apps behind a standard DOM element is interoperability. With the current state of affairs in frontend development, being able to reuse UI elements within and across projects is quite painful. Taking one JavaScript module and making it work together with one from another framework is hard enough, let alone being able to neatly package other UI assets (such as CSS, icons, images) to go along with it. Web Components (through the related HTML Imports spec) proposes a solution: a standard browser mechanism for pulling in a component - with all its dependencies and assets - and using it as simply as adding a <div> into the DOM. One core part of this promise is the aforementioned isolation: importing a custom element would have minimal effects on any existing elements within the same DOM.Many large frameworks such as Angular, Ember, Polymer and X-Tags are already preparing for this, promising (and even demonstrating) easier interoperability. The importance of this should not be understated: frameworks with very different ideas and implementations could be mixed at matched with relative ease. Given the incredible pace of innovation in the web frontend space, being able to swap out components for better ones without completely rewriting the surrounding application should be very desirable.

互操作性是将那些迷你的应用抽象为一个标准 DOM 元素的另一个重要的结果。从前端开发的现状来看，在项目中以及跨项目时都可以重用 UI 元素是一个痛点。实现一个 JavaScript 模块与另一个框架中的模块同时工作确实很困难，更别说把连同它所需的 UI 资源（比如 CSS， icons, images）一并打包起来。Web Components（包括相关的 [HTML Imports](http://www.html5rocks.com/en/tutorials/webcomponents/imports/) 规范）提出了一个解决方案：在标准的浏览器中注入一个组件（包括它的所有依赖和资源），使用时就像在 DOM 中添加一个 &lt;div&gt; 一样简单。这种实现最核心的部分就是上面提到的分离：引入的自定义元素需要保证对于 DOM 中存在的同一个元素的影响最小。很多框架像 [Angular](https://angularjs.org/)，[Ember](http://emberjs.com/)，[Polymer](https://www.polymer-project.org/) 和 [X-Tags](http://www.x-tags.org/) 已经在这方面做了尝试，[实现](http://www.2ality.com/2013/05/web-components-angular-ember.html)（甚至已经有[实例](https://www.polymer-project.org/articles/demos/polymer-xtag-vanilla/example.html)）了更容易的互操作性。这个重要性不能被忽视：不同应用场景和技术实现的框架需要被相对容易地混合在一起使用。迈出了 web 前端领域最难以置信的的一步创新，实现了更换新的组件时不必要完全重写附属的应用，这非常地可取。

Sound good? I think so. I’m willing to go on record saying Web Components will be huge (thoroughly embarrassing my future self I’m sure, but oh well). Sadly, they’re not there yet: even though the most relevant frameworks in the space - Polymer (backed by Google) and X-Tags (backed by Mozilla) - have polyfills for parts of the spec, it represents such a fundamental shift to how applications are constructed and how browsers work that it won’t be smooth sailing for quite a while.

听起来很棒？我想是这样。我会说 Web Components 不可限量（彻底的改变了我未来的生活。我确信，就是这样）。遗憾的是，现在[还不行](http://caniuse.com/#search=web%20components)：尽管在这个领域中最具代表性的框架已经有了一部分 polyfill （[Polymer](https://www.polymer-project.org/)（背后是 Google）和 [X-Tags](http://www.x-tags.org/)（背后是 Mozilla）-对于部分规范有 polyfills，它代表了应用如何被构建以及浏览器如何工作这样一个根本性的转变，这在相当长的一段时间内不会是一帆风顺的。

## The philosophical mismatch
## 哲学上的不相配

So these are the directions frontend development seems headed, now and later. However, these short- and long-term futures seem incompatible at face value: aren’t the intricacies of maintaining stateful DOM components exactly what Flux et al attempt to rid us of? If we have a “mini-app” with encapsulated internal state, we can’t just re-render the whole DOM whenever we feel like it, as we’ll potentially lose that important state. Or, from another perspective, if we no longer have control over (or even access to!) the full state of the world, how can we ever be sure that that state remains consistent across our UI? This mismatch seems quite fundamental, and I don’t see a simple way to unify these concepts.

所以这些迟早会是前端发展的方向。然而，这些短期和长期的未来看上去和表面价值有些矛盾：准确的维护 DOM 组件带来的错综复杂性不正是 Flux 等尝试去让我们摆脱的么？如果有一个封装了内部 state 的“迷你应用”，我们不能按照我们的需要随时地重新渲染整个 DOM，因为这会潜在地遗漏一些重要的 state。或者，换一个角度，如果无法控制（哪怕是获取）每一个状态，如何保持整个 UI 层面状态的一致性？这种不匹配看上去非常的浅显，并且我还没有发现一个简单的方式可以统一这些概念。

Yet, that is not to say there’s no potential for overlap. Indeed, the promised interop mechanisms of Web Components are too important for the frontend profession to overlook. It should also be noted Flux itself proposes to striking a balance between the simplicity of a single, unidirectional flow of data (with a singular state of the world) and breaking a large application into smaller, more manageable sub-applications (with their own internal states). The reasoning behind this is a familiar one: we human beings are pretty terrible at handling a lot of complexity at once, so it helps to break problems into manageable chunks. So, if these two futures aren’t mutually exclusive after all, how would we go about marrying them, and would we get anything out of it?

不过，那也不是说没有任何的共同点。确实，Web Components 实现的互操作机制对于前端行业的前景非常重要。值得一提的是，Flux 自身[试图](http://facebook.github.io/flux/docs/overview.html#views-and-controller-views)达到存在于一个简单的、单向的数据流（这个领域的一个奇怪的 state）和拆分大的应用为小的、更加可控的子应用（包括它们内部的 state）之间的平衡。背后的原因我们都很熟悉：人类在同时处理很多复杂事物时的表现相当糟糕，所以它帮我们将问题分成了可操作的区块。同样，如果这两种未来终究没有相互排斥，我们又要何去何从，我们从中会得到什么呢？

## A brief aside on loosely connected sub-domains
## 顺便提一下松散连接的子域

Say you were building a web shop, ignoring the choice of frontend architecture for a bit. The domain of that application would probably include displaying product categories, the products that belong to those categories, a shopping cart implementation, that sort of thing. The information within that domain - the price of a product, for instance - has to remain consistent regardless of where the product appears (e.g. in a listing, a shopping cart or a wishlist). Giving this domain the “mini-app” treatment, and breaking it apart into smaller chunks might not reduce the overall complexity of the system at all. The opposite seems likely, in fact, as making sure the state of the world remains consistent across a distributed system is far from trivial.

假如你正在创建一个网上商店，忽略了选择前端架构这一点。应用的域应该会包括展示商品的类目，划分在那些类目下的商品，一个购物车，诸如此类。域所包含的信息（商品的价格，举个例子）不管出现在哪里，必须保证产品属性的一致性（例如：在一个清单，一个购物车或者一个愿望清单）。把这个域看成“迷你应用”，并且把它拆分更小的块可能根本不会减少整个系统的全局复杂性。反过来看，实际上，就像达成这个领域内的 state 在分布式系统上的一致性一样是微不足道的。

Consider, however, having to add a chat-box next to your product details page. You know, the kind where you can talk to a human if you can’t figure out if the product is right for you. Implementing a real-time chat application in the browser is certainly easier than it used to be, but it isn’t trivial, either. Facebook had problems getting theirs right, and they employ pretty smart people. But even though the chat-box contains an interesting domain of engineering challenges, it’s only loosely connected to the domain of the main application. That is, while the sales representative at the other end might be interested in a bit of context - like the current product category - the chat-box can lead a relatively independent life.

考虑一下，不管怎样，都有必要在商品的详情页的侧边添加一个聊天框。如你所知，如果你不能分辨出这个商品是否对你合适时，你可以和其他人讨论。今天在浏览器端实现一个实时的聊天应用比以前要容易得多，不过这也不是重点。Facebook 遇到了获取它们的权限的[问题](https://www.youtube.com/watch?v=nYkdrAPrdcw#t=882)，并且他们雇佣了非常聪明的工程师。但即便这样，chat-box 为工程师带来了一个在域上非常有趣的挑战，它仅仅松散地连接到主应用的域上。也就是说，而在另一端的销售代表可能对环境有一点兴趣（像当前的产品目录），chat-box 可能是一个相对独立的存在。

But I digress; what does any of this have to do with marrying React, Flux & Web Components?

我同意。但这与结合 React，Flux 和 Web Components 有什么关系呢？

## Packaging Flux into Web Components
## 打包 Flux 到 Web Components

One way to leverage both paradigms is to implement the internals of a Web Component using a Flux-like architecture. If you’re able to identify a loosely connected sub-domain in your frontend application - the chat-box for example - you can use all the simplifying power of Flux within that domain. Packaging it up in a Web Component gives you a custom element - say &lt;chat-box&gt; - which is both agnostic to its surroundings, and reusable within and across projects. Pulling it in is as simple as adding a &lt;link rel="import" href="chat-box.html"&gt; to your application, and it’s ready to be used wherever you could drop any other tag, say, &lt;video&gt;.

利用好每个范例的一种方式就是使用类 FLux 结构实现 Web Components 的内部。如果你已经能够在你的前端应用中辨认出一个松散关联的子域（聊天框为例）你可以在那个域内应用 Flux 的简洁化的能力。把它打包成一个 Web Component 得到一个自定义元素 - 称为&lt;chat-box&gt; - 这是一个黑盒，并且在项目内以及跨项目时都是可重用的。使用它就像添加一个 &lt;link rel="import" href="chat-box.html"&gt;到你的应用中一样简单，并且无论何时你丢掉任何其他的标签它都可以使用，比如，&lt;video&gt;。

The <chat-box> element presents itself as a regular DOM element, and exposes any custom functionality through standard DOM API’s, the ones all kinds of libraries already know how to manipulate:

&lt;chat-box&gt;元素看起来就像是普通的 DOM 元素，并且通过标准的 DOM API 暴漏了所有自定义的功能，换句话说所有类型的类库都知道如何使用它：

- To parametrize it during instantiation, you can pass in attributes:
<chat-box product-category="bikes">
- To invoke its encapsulated functionality, you can call methods on the DOM element:
document.querySelector("chat-box").connect()
- To react to interesting changes in state, you can listen for events:
chatBox.addEventListener("customer-rep-available", highlightChatBox)

The true (proposed) power of Web Components is realized when the packaging of such a component is so opaque that it doesn’t really matter which framework reigns inside (or outside!) the component. Let us assume Angular 2.0 comes out and you find it solves the real-time chat domain problems with a lot more elegance than with Flux. As long as the new implementation exposes the same standard DOM interfaces, swapping in an Angular implementation would be as simple as changing the HTML Import for the <chat-box> element.

- 在实例化的过程中实现参数化，你可以在属性中传递：
&lt;chat-box product-category="bikes"&gt;
- 想要调用它所封装的功能，你需要在这个 DOM 元素上调用方法：documment.querySelector("chat-box").connect()
- 想要在 State 里对那些感兴趣的变化做出反应，你需要监听事件：chatBox.addEventListener("customer-rep-avaliable", highlightChatBox)

Web Components 真正的（提出的）能力已经实现了，就是当打包这样一个组件是如此的难懂以至于它根本不在乎哪个框架在[组件内](http://pixelscommander.com/polygon/reactive-elements/example/)（或者组件外！）。我们假定 [Angular 2.0 已经发布](http://www.reddit.com/r/programming/comments/2kl88s/angular_20_drastically_different/)并且你知道它使用了和 FLux 相比更加的优雅的方式解决了实时聊天的域问题。将其替换成 Angular 的实现就会像更换 &lt;chat-box&gt; 元素的 HTML Import一样简单。

## Managing Web Components within Flux
## 在 Flux 中操作 Web Components

The pairing also works the other way around. Since a Virtual DOM implementation essentially allows you to create lightweight DOM elements (which are realized into their heavyweight counterparts only as necessary), there’s no reason why custom elements can’t receive the same treatment. That is, if our React component’s render method looks like this:

这两者也在其它方面发挥着作用。由于 Virtual DOM 的实现本质上允许你创建轻量的 DOM 元素（仅仅在需要的时候才会实现重量级的副本），没有什么原因关于为什么自定义元素不能得到同样的待遇。也就是说，如果我们的 React 组件的渲染方式就像这样：

```
render: function() {
    return <div>
        Hello {this.props.name}!
        <chat-box />
    </div>;
}
```

...then there’s no reason to assume the <chat-box> element has to be implemented as a React component. Indeed, exactly as you’re able to create a virtualized <div> which will later be turned into an actual DOM <div>, we can let React manage the life and death of our custom <chat-box> element. Again, what makes this possible is the fact that custom elements are integrated through standard DOM API’s, which DOM libraries (like React or jQuery) already know well.

然而没有必要去假设&lt;chat-box&gt;元素[被实现](http://facebook.github.io/react/docs/reconciliation.html)为一个 React 组件。实际上，当你创建一个虚拟的&lt;div&gt;，它将会被封装成一个真正的 DOM &lt;div&gt;，我们可以让 React 管理我们这个自定义&lt;chat-box&gt;元素的创建和销毁。再者，自定义元素是通过集成标准 DOM 库（像 React 或者 jQuery）已经所熟知的 API 使它成为了可能。

The problematic part of this approach is that in order to be fast, React will often reuse existing DOM elements to realize DOM updates as cheaply as possible. If our custom element contains internal state, it will likely end up either lost or corrupted as a result. While the problem can be somewhat worked around with keyed children, we still need to be careful to not lose important internal state along with the comings and goings of custom elements.

这种实现存在的问题是：如果想要变得更快，React 就需要频繁地[复用存在的 DOM 元素](http://facebook.github.io/react/docs/multiple-components.html#child-reconciliation)来实现尽可能低廉地更新 DOM。如果我们自定义的元素包含内部 state，它最终的结果可能是丢失或者毁坏。然而这个问题可能有点[边缘化](http://facebook.github.io/react/docs/multiple-components.html#dynamic-children)，我们依旧需要小心不能丢了重要的内部 state 以及自定义元素的来去。

## Managing stateless Web Components
## 使用无状态的 Web Components

The two previous approaches to combining a Virtual DOM with Web Components have been very much concerned with state management, and rightly so: managing the consistency of state in a complex frontend application can be a daunting task. If we forget Flux, state management and our beloved “mini-apps” for a bit, however, we’re still left with an interesting case to consider: Web Components without any internal state.

前面提到的两种结合 Virtual DOM 和 Web Components 的方式都与 state 的管理有关，准确地说：管理状态的一致性在一个复杂的前端应用中可能是一个很可怕的任务。如果我们忘了 Flux，state 管理和我们钟爱的“迷你应用”这一点，我们依旧有一个很有意思的东西需要考虑：没有任何内部 state 的 Web Components。

Let’s say you’ve created a sweet tab implementation as a Web Component, and decided its markup looks like this:

假设你已经创建了一个[清新的选项卡](https://www.polymer-project.org/components/paper-elements/demo.html#paper-tabs)作为一个 Web Component，它的标记看起来可能就像这样：

``
<paper-tabs selected="0">
    <paper-tab>First tab</paper-tab>
    <paper-tab>Second tab</paper-tab>
    <paper-tab>Third tab</paper-tab>
</paper-tabs>
``

The "selected" attribute controls which tab is currently visible. We can easily let React manage this tab implementation, without fears of clobbering delicate internal state, like so:

带有“selected”属性的选项卡当前可见。我们能很容易的让 React 管理这个选项卡的执行，没有必要对内部 state 心存畏惧，就像这样：

``
render: function() {
    return <paper-tabs selected={this.props.selectedTab}>
        {tabContent}
    </paper-tabs>;
}
``

The important difference to the preceding <chat-box> example is that any and all state in the <paper-tabs> component is explicitly controlled through immutable props. This is a good thing. It allows us to simply not care how React manages the DOM, as long as the end result has the content and attributes we specify. It also allows us to fall back to the simple, unidirectional data flow of Flux, while still enjoying many delicious properties of Web Components, such as their isolation semantics, and delivery through simple HTML Imports.

和前面提到的&lt;chat-box&gt;实例最大的区别就是在&lt;paper-tabs&gt;组件中任意一个状态都被不可更改的 props 严格的控制。[这是一件好事](http://facebook.github.io/react/docs/interactivity-and-dynamic-uis.html#what-components-should-have-state)。它允许我们不必去关心 React 如何管理 DOM，只要最终的结果是我们想要的内容和属性。这也允许我们回到 Flux 那种简单的，单向的数据流，不过依旧享受着 Web Components 中很多有趣的属性，如隔离语义，以及通过简单的 HTML Imports 分发。


## Closing thoughts
## 结语

I hope this helped clarify how the two futures of frontend architecture - Flux and Web Components - are fundamentally different, and yet still composable in interesting ways. The simplifying power of Flux can’t be dismissed, but even less so the powerful interoperability characteristics of Web Components. While an acceptable level of Web Components support is still years away, if we align our choices today with what’s coming tomorrow, we’ll be on more solid ground as the craft continues to evolve, often at breakneck speeds. As software craftsmen, we want to use these latest advances in technology to deliver kick-ass user experiences for the browser. Currently, the “new hotness” is React, with a Flux-like architecture backing it up. Still, it’s certainly not the end-all of frontend architectures, and big players are betting big on Web Components. Understanding the interplay between these short- and long-term futures is important to make sure our creations stand the test of time.

我希望这个可以帮助认清 Flux 和 Web Components 这两个前端架构的情况，它们基本上是不同的，并且还能够以更有趣的方式进行组合。Flux 的精简的能力不能被忽视，即便是互操作性不是那么强大的 Web Components 也是如此。然而对 Web Components 支持提升到可接受的水平还要几年时间，如果我们按照明天将会发生的事调整今天的选择，我们的技术将在更加坚实的基础之上持续的发展，很可能以惊人的速度。作为一个软件工匠，我们想要使用那些最前沿的技术在浏览器上提供了不起的用户体验。当前，React 绝对是“新宠”，有一个类 Flux 的架构在它背后。它当然不会是前端架构的终极目标，并且大玩家们在 Web Components 上下了大的赌注。意识到这些短期和长期的未来间的相互作用非常重要，确保我们的作品经得起时间的考验。

原文：[Combining React, Flux & Web Components](http://futurice.com/blog/combining-react-flux-and-web-components)