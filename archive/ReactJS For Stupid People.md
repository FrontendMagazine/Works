# ReactJS For Stupid People

# 给笨蛋准备的 ReactJS 教程

I struggled for a long time trying to understand what React is and how it fits in the application architecture. This post is what I wish someone had told me.

在相当长的一段时间内，我很努力地去尝试理解 React 是什么以及它在应用架构上的健壮程度。这篇文章解答了我希望别人为我解答的疑惑。


## What is React?

## React 是什么？

How does React compare to Angular, Ember, Backbone, et al? How do you handle data? How do you contact the server? What the heck is JSX? What is a "component"?

和 Angular，Ember，Backbone 等等比起来 React 的表现如何？要如何处理数据？要如何连接服务器？JSX 到底是什么？“组件”又是如何定义的？


Stop.

停。

Stop it right now.

立刻停下来。

**React is ONLY THE VIEW LAYER.**

**React 仅仅是 VIEW 层。**

React is often mentioned in the same breath as other Javascript frameworks, but "React vs Angular" doesn't make sense because they aren't directly comparable things. Angular is a complete framework (including a view layer), React is not. This is why React is so confusing to understand, it's emerging in an ecosystem of complete frameworks, but it's just the view.

React 通常和其他的 JavaScript 框架同时被提及，但是说“React 对比 Angular”却讲不通，因为它们之间是不可比较的。Angular 是一个完整的框架（包括一个 view 层），React 却并不是。这也是 React 很难于理解的原因，它虽然抽离自一个具备完整框架的生态系统中，但仅仅是一个 view 层。

React gives you a template language and some function hooks to essentially render HTML. That's all React outputs, HTML. Your bundles of HTML / Javascript, called "components", are allowed things like storing their own internal state in memory (such as which tab is selected in a tab view), but in the end you just barf out HTML.

React 提供了模板语法以及一些函数钩子用于基本的 HTML 渲染。这就是 React 全部的输出——HTML。你把 HTML / JavaScript 合到一起，被称为“组件”，允许把它们自己内部的状态存到内存中（比如在一个选项卡中哪个被选中），不过最后你只是吐出 HTML。

You absolutely cannot build a fully functional dynamic application with React alone. We'll learn more about why below.

很明显，**你没办法单单使用 React 来创建一个功能完善的动态应用**。下面我们详细看看。

## The Good

## 好处

After working with React for a while, I've seen three very important benefits surface.

使用 React 一段时间后，我发现了三个非常重要的特性。

**1. You can always tell how your component will render by looking at one source file.**

**1. 通过查看一个源文件就可以知道你的组件将会如何渲染。**

This may be the most important benefit, even though it is not different from Angular templates. Let's use a real world implementation example.

这是最大的好处，尽管这和 Angular 模板没什么不同。下面看一个真实的应用实例。

Say you have to update your site's header with the user's name upon log in. If you're not using a Javascript MVC framework, you might do this:

假设你需要在用户登录后更新站点头部的用户名。如果没有使用 JavaScript MVC 框架，可能要这么做：


```html
<header>  
   <div class="name"></div>
</header> 
```
```javascript 
$.post('/login', credentials, function( user ) {
    // Modify the DOM here
    $('header .name').show().text( user.name );
});
```

I can tell you from experience that this code will ruin your life and your friends' lives. How do you debug the output? Who updated the header? Who else has access to the header HTML? Who holds the source of truth for the name being visible? This DOM manipulation is just as bad as a GOTO statement for reasoning about your program.

按照我的经验，这些代码要毁掉你的生活甚至你同事的生活。如何对输出调试？谁来更新头部？谁还可以访问头部的 HTML？谁来维护名字的显示隐藏状态？这个 DOM 操作会让你的项目**像 GOTO 语句那样糟糕**。


Here's how you might do it in React:

在 React 中你可以像下面这样做：

```javascript
render: function() {  
    return <header>
        { this.state.name ? <div>this.state.name</div> : null }
    </header>;
}
```

We can tell instantly how this component will render. If you know the state, you know the rendered output. You don't have to trace program flow. When working on complex applications, especially in teams, this is critically important.

我们会清楚的分辨出这个组件可能会如何渲染。如果你知道这个语句，就会知道渲染后的输出。你没必要去记录程序的流程。在复杂应用中，特别是团队开发中，显得尤为重要。

**2. Bundling Javascript and HTML into JSX makes components easily understandable.**

**2. 将 JavaScript 和 HTML 绑定到 JSX 使组件更易懂**

The weird mix of HTML / Javascript soup above might make you cringe. We've been conditioned to not put raw Javascript in the DOM (like onClick handlers) since we were wee developers. You'll have to trust me, though; working with JSX components is really nice.

上面的那种把 HTML 和 JavaScript 混合在一起的写法可能让你很不适应。我们会很自然地拒绝将 JavaScript 放在 DOM 当中（比如 **onClick** 事件处理函数）即便我们是小小的开发者。但是，请一定要相信我；JSX 组件真的会让你的工作变得很“nice”。

Traditionally you separate views (HTML) from functionality (Javascript). This leads to monolithic Javascript files containing all functionality for one "page", and you have to trace complex flow from JS > HTML > JS > bad-news-sad-time.

按照传统，你会把视图（HTML）从功能（JavaScript）中分离开出来。这会产生一个巨大的 JavaScript 文件，包含了一个“页面”的所有功能需求，并且你必须记录复杂的流程，从 JS 到 HTML 到 JS 到悲痛欲绝。

Tying functionality directly to markup and packaging it in a portable, self contained "component" will make you happier and less filthy in general. Your Javascript has intimate knowledge of your HTML, so mashing them together makes sense.

捆绑功能直接标记和打包成一个可移植的，自主控制的“组件”，让你更开心，且减少脏乱的代码。因为 JavaScript 与 HTML 关系密切，揉到一起也正常。

**3. You can render React on the server.**

**3. 你可以在服务端渲染 React**

If you're building a public facing site or app and you're following the render-it-all-on-the-client path, you've already done it wrong. Client-only rendering is why Soundcloud feels so slow and why Stack Overflow (purely server side rendering) feels so fast. You can render React on the server, and you should.

如果你正在创建一个门户网站或者应用，并且按照 render-it-all-on-the-client 路线，可能已经出错了。只通过客户端渲染使得 [Soundcloud](https://soundcloud.com/) 感觉如此慢，相反 [Stack Overflow](http://stackoverflow.com/)（纯服务端渲染）如此快。你可以在[服务端渲染 React](https://www.npmjs.org/package/react-server-example)，并且你该这么做。

Angular and others encourage you to do disgusting things like render your page with PhantomJS and serve that to search engine crawlers based on user agent, or pay actual cash money for that as a service. Ugh.

Angular 等促使你做一些恶心的事情，比如使用 PhantomJS 来渲染页面，分析请求头中的 user agent ，把这些页面提供给搜索引擎，或者[为此买一个服务](https://prerender.io/pricing)。呃。

## The Bad

## 坏处

Don't forget that React is only the view.

不要忘了 React 仅仅是个** view 层**。

**1. You DO NOT GET any of the following:**

**1. 下面这些都没有：**

- An event system (other than vanilla DOM events)
- Any AJAX capabilities whatsoever
- Any form of a data layer
- Promises
- Any application framework at all
- Any idea how implement the above

React on its own is useless for the real world. Worse yet, as we'll see, this leads to everyone reinventing the wheel.

- 事件系统（除了原生的 DOM 事件）
- AJAX 功能
- 数据层
- Promises
- 应用程序架构
- 实现以上功能的规范

单独的 React 在这个世界上真的没什么用。更糟糕的是，就像我们将要看到的，这迫使每个开发者都要重新造轮子。


**2. The documentation is not "accessible" nor "good." Again, this is a blog post for stupid people. Look at the first part of the sidebar on the documentation page:**

**2. 官方文档既不是“容易理解的”，也不是“良好的”。重申一遍，这是为笨蛋写的一篇博客。请看[文档页](http://facebook.github.io/react/docs)侧边栏中第一部分：**

![React documentation sidebar](http://blog.andrewray.me/content/images/2014/Sep/Screen-Shot-2014-09-22-at-12-08-18-AM.png)

There are three separate, competing quickstart guides. I'm overwhelmed and I'm not even drunk. The sidebar below that is straight out of my nightmares, with sections that obviously shouldn't be there, like "More About Refs" and "PureRenderMixin".

有三个独立的、相互竞争的快速入门指南。我有些不知所措，不过我没有喝多。更下面的侧边栏就像是恶梦一样，很明显一些章节不应该放在那里，像“More About Refs”和“PureRenderMixin”。

**3. React is large for how little you get, including how little cross browser support.**

**3. 相比 React 给你提供的好处，React 实在太大的。而且对浏览器的支持也很限**

Update: React is not 144 KB as I previously wrote. React is about 35 KB gzipped over the wire.

更新：我之前写到 React 大小不到 144KB。通过 gzip 压缩传输后在 35KB 左右。

![React file size](http://blog.andrewray.me/content/images/2014/Oct/react-size.png)

That's without the react-with-addons library you will need to actually develop a real app!

这没有包含任何 React 的插件，而你需要使用那些插件才能实现一个真正的应用！

That's without the ES5 Shim library you need to support IE8!

也不包含 ES5 shim 的类库，你需要支持 IE8 的话。

That's without any sort of application library of any kind!

不包含任何类型的应用类库！

React is a comparable size with Angular, even though Angular is a complete application framework. React is frankly bloated for how little functionality you get. Hopefully this will improve in the future.

React 的大小和 Angular 相当，但 Angular 是一个完整的应用框架。React 显而易见的臃肿，但是你只获得了很少的功能。希望这在将来能得到改善。

## Stop Saying Flux

## 不要说 Flux 

Perhaps the most annoying part of React development is "Flux." It's far more confusing than React. The name "Flux" is a pretentious barrier to understanding.

也许 React 开发中，最让人反感的还是“Flux”。 远比 React 自身混乱。“Flux”这个名字就很让人费解。

There is no such thing as Flux. Flux is a concept, not a library. Well, there is a library, sort of:

Flux 并不是真实存在的。它只是一个概念，而不是个类库。幸运的是，存在一个类库，在某种程度上：

> "Flux is more of a pattern than a framework"

> “相比于一个框架，Flux 更像是一种模式。”

Ugh. The worst part is the name: React didn't reinvent the last 40 years of UI architecture knowledge and come up with some brand new concept for data management.

呃。一个最不恰当的名字：React 并没有重塑最近 40 年的 UI 体系的知识，也没有为数据管理带来新的概念。

The concept "Flux" is simply that your view triggers an event (say, after user types a name in a text field), that event updates a model, then the model triggers an event, and the view responds to that model's event by re-rendering with the latest data. That's it.

“Flux”的概念很简单，view 层触发了一个事件（比如说，用户在文本域中输入了一个姓名），这个事件更新了 model，然后 model 触发了一个事件，view 响应了 model 的事件，使用最新的数据进行渲染。就这样。

This one way data flow / decoupled observer pattern is designed to guarantee that your source of truth always stays in your stores / models. It's a Good Thing™.

这一数据流/解耦观察者模式被设计来保证你的资源总存在于内存/模式中。这是一件好事™。

The bad side of Flux is that everyone is re-inventing it. Since there's no agreed on event library, model layer, AJAX layer, or anything, there are many different "Flux" implementations, and they all compete with each other.

Flux 的坏处是每个人都会重新发明轮子。由于没有在事件库，model 层，AJAX 层等达成一致，出现了很多种“Flux”的实现方式，并且它们彼此之间相互混杂。

## Should I Use React?

## 我该用 React 吗？

Short answer: yes.

简单回答：是。

Long answer: unfortunately, yes, for most things.

详尽的回答：很不幸，是的，在大多数场景中。

Here's why you should use React:

下面是为什么要用 React：

Works great for teams, strongly enforcing UI and workflow patterns
UI code is readable and maintainable
Componentized UI is the future of web development, and you need to start doing it now.

对团队开发来说表现的很出色，加强了 UI 和 工作流模式 UI 代码的可读和可维护性。组件化的 UI 是 web 开发的趋势，并且你现在应该开始了。

Here's why you should think twice before you switch:

下面是为什么在你选择之前需要再考虑一下：

*React will slow you down tremendously at the start. Understanding how props, state, and component communication works is not straightforward, and the docs are a maze of information. This will be countered, in theory, by a grand speed up when your whole team is on board.*

*React does not support any browser below IE8, and never will*

*If your application / website doesn't have very much dynamic page updating, you will be implementing a lot of code for a very small benefit.*

*You will reinvent a lot of wheels. React is young, and because there's no canonical way to do events / component communication, you'll have to build large component libraries from scratch. Does your application have dropdowns, resizable windows, or lightboxes? You'll probably have to write those all from scratch.*

*一开始 React 会极大地减慢你的开发。理解props、state以及组件通信如何工作并不是很简单，并且文档信息错综复杂。理论上，这将会被克服，你的整个团队都上道之后，开发速度上就会有一个很大的提升。*

*React 不支持 IE8 以下的任何浏览器，以后也绝不会。*
*如果你的应用/站点不需要频繁的动态页面更新，你可能为了很小的功能而编写大量的代码。*
*你会改造很多轮子。React 很年轻，并且因为没有权威的方式来处理事件、组件通信，你必须从零开始创建大量的组件库。你的应用是否有下拉菜单，可调整大小的窗口，或者 lightbox？你同样必须从零开始写这些。*

## That's It!

## 就这些了！

Check out the follow-up post, Flux For Stupid People.

下一篇文章，[给笨蛋准备的 Flux 教程 ](http://blog.andrewray.me/flux-for-stupid-people/)

I hope this helped someone as stupid as me understand React better. If this post made your life easier, consider following me on Twitter

我希望它可以帮助像我一样笨的人更好地理解 React。如果这篇文章让你的生活更轻松，可以在 [Twitter](https://twitter.com/andrewray) 上关注我。 

原文：http://blog.andrewray.me/reactjs-for-stupid-people/