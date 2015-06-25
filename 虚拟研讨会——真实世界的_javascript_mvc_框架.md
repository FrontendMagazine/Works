# 虚拟研讨会——真实世界的 JavaScript MVC 框架

自 HTML5 大行其道，Web 平台也发生了很大变化，开发者开始在 JavaScript 语言中寻求创建复杂应用的解决方案。许多新的 API 应运而生，在众多合适的场景下，浏览器该如何利用好这一切。

[这一系列的文章](http://www.infoq.com/next-generation-html5-javascript/) 将更进一步聚焦于如何将这些强大的技术应用于实践当中，不会创建炫酷的示例和原型，而是帮助开发者如何在项目实践它。在 HTML5 系列文章中，我们将带着这些流行语在专家哪里获得如何在实践中应用它们的方式。我们也会更进一步讨论相关的技术（比如 AngularJS），并且探索未来的标准和 web 开发的趋势。

这篇 InfoQ 文章是“[Next Generation HTML5 and JavaScript](http://www.infoq.com/Next-Generation-HTML5-JavaScript/articles/)”系列的一部分。你可以通过 RSS [订阅](http://www.infoq.com/feed/Next-Generation-HTML5-JavaScript)。

随着越来越多的逻辑在浏览器中执行，JavaScript 前端代码库变得更大也更加难维护。开发者发现了一个解决该问题的方式，即前端 MVC 框架，提高了开发效率并且更容易维护代码。

早在 2013 年，InfoQ 已经发起了社区内 [关于 JavaScript MVC 框架排行](http://www.infoq.com/research/top-javascript-mvc-frameworks) 的调查，依据它们的特点以及它们的完成度：

![](http://www.infoq.com/resource/articles/real-world-javascript-mvc-frameworks/en/resources/61.png)

现在 InfoQ 向专家级实践者发起了一次访谈，关于他们如何使用这些框架以及在开发 JavaScript 应用时使用这些框架的最佳实践。

### InfoQ：你最喜欢哪个 JavaScript MVC 框架，已经使用了多久，以及它帮你解决了那些最基本的问题？



> **Matteo Pagliazzi：** 作为当下的框架，Angular 实至名归，这不仅仅因为 GitHub 上获得的赞同：它具备庞大的类库，模块……并且正在被很多开发者讨论和使用。很明显我们开发者很喜欢它。我觉得这和它很容易上手有关：仅仅需要 5 分钟就能轻易地完成一个完整并且功能强大的任务。不过，随着基础知道到框架的核心，学习曲线变得陡峭，你会发现它非常复杂。

----

> **Julien Knebel：** 在过去 2 年多的时间内，我已经使用 Ember.js 开发了 5 个 web 应用。不得不说，它给我带来了灵活、高效还有自信，因为我可以更多的聚焦在用户体验，而不是在复杂的特性上投入大量的时间，比如一个强大的异步路由。

----

> **Brad Dunbar：**我最喜欢 Backbone。它提供了其他类库都不具备的简洁和灵活。同样，或许更重要的是，代码的简单易读。

----

> **John Munsch：**我最喜欢的框架是 AngularJS，我已经使用它超过了一年，并且依旧在蜜月期一样。

> 我所选择的前端框架会解决一个最基本的问题，给我以足够的骨骼、 肌肉和结缔组织，帮助我建立复杂的 UI 的前端，而不仅仅是比较。不过，相比过去的使用 JSP、PHP、Rails 等后端框架创建请求/响应的方式，这种做法更优美。

----

> **Julio Cesar Ody：** 我使用 Backbone 更多一些，现在逐步使用 ReactJS 替换 Backbone 作为视图层。

> 我建立的所有 web 应用完全运行在浏览器中，并使用 API 和/或 Websocket 与服务器通信。这意味着很多逻辑最终都使用 JavaScript 编写，所以 Backbone 可以协助我保证代码整洁并且可维护。

----

> **Thomas Davis：** 我的第一个正式的 JavaScript MVC 项目在 2010 年末，那次我决定使用 Backbone.js 和 Sproutcore。我写了一篇比较二者的文章，上了 Hacker News 的头条并且获得了超过 100,000 次点击。基于那次反馈，我决定使用 Backbone.js。因为它的体积小，而且无须局限大规模框架的约定也不会妨碍我实现复杂的功能。在学习 Backbone.js 时，我为自己写了一个教程帮我理解这个框架。这些文章获得了上百万的点击，并且仍在增长。目前归档在 [backbonetutorials.com](http://backbonetutorials.com/)。

>现在创建一个应用，用户体验设计和保证网站正常运行一样至关重要。单页应用准许你创建那些传统页面只能通过刷新页面无法企及的用户体验。比如，想象一下在你使用 Facebook 时每次评论都要刷新整个页面的情景。

----

### InfoQ：既然有很多的替代品，你所选用的框架和其他的相比又如何？



>**Julien Knebel：** 它表现的非常棒！我从没想过它会让我自己完整地编写整个应用，尽管 Angular 也非常棒，我更在意一个框架是否能快速搭建并运行。然而我相信随着代码库的增长，它可能变得更加枯燥。另一方面，在我着手学习 Ember 时遇到了很多困难，但是经过几次“哈哈哈哈”，它们就像冬日里的积雪在阳光下慢慢融化。另外一个重点（至少对我）是“组件化“”，我不喜欢 Angular 用指令解决这个问题，Ember Component 似乎才是正确的方式。

----

>**Brad Dunbar:：**我发现几乎没有那个 JavaScript 框架可以恰好满足应用中的所有需求。这意味着我总会围绕它们写一些东西。Backbone 设计的功能很少并且提供了可以相互组合的工具类，而不是增加那些几乎用不到的功能。

----

>**John Munsch：**这个问题很难回答，我的开发经验只在 Backbone.js（大约两年）和 AngularJS（仅仅一年）。不过，这二者我很难取舍。

>Backbone.js 不错，但是在我们共同开发一个大的项目时，最多有六个人。我们遇到了很多问题，我们使用其他类库（双向绑定、验证、模板、AMD 等等）填补了 Backbone.js 在这些方面的空白。如果没有在开始前找到填补这些空白的解决方案，那么你的同事只能选择自己的方式。如果时间很赶并且编码很快的话，很难找到所有的区别并且强制统一。

>我们最终完成了一个项目，并且和原来的解决方案相比更快速，不过它是一个杂交体；它的每部分都想是一个不同的动物。使用 AngularJS，这些部分大多数还是来自于框架自身并且出现这类问题的可能性也会变小。

----

>**Julio Cesar Ody：**不得不说 Backbone 非常的轻量，不过这已经不是我们需要十分在意的点。一个类库优于其他最主要的以及原因，在某些情况下开发者更喜欢使用并且在更加高产的一个。

>所以在 Backbone、Angular 以及 Ember 之间，你不能选错。我没有把 ReactJS 包含在内，主要是因为它仅仅是一个 UI 组件库，所以不应该把它和其他应用框架相混淆。

----

>**Thomas Davis：**自 2010 年发生了很多变化。尽管我总是尝试打破自己的旧习惯，但是目前我依旧在用 Backbone.js。它仍然是最小的并且它的理念和方法几乎没有改变。在我看来，它正逐步被其他竞争者所取代。Angular 这种类库做出的努力让开发者更加容易控制 DOM，在渲染时 Backbone.js 看起来更加的古老。与全新的模块花 JavaScript 相比，也可以成 Backbone.js 在体积上更大。没有任何理由将模块、数据集、路由和视图打包放在同一个类库中， 相反类似 Components.js 将这些部分写成独立的模块。此时还没有真正的使用，我将在另外一个项目中考虑它。

----

### InfoQ：创建更快、更健壮的前端很赞，但是性能如何？尤其在移动端。你们有哪些经验？



>**Igor Minar：**我觉得移动端依旧是 web 开发中需要考虑到的。需要给予更多的关注，Angular 很明显在 2.0 版本中重新聚焦移动。

----

>**Julien Knebel：**Ember 相比其他类库更重，这是事实。尽管可以压缩它，如果你打算在移动端 web 应用中使用它，一定要慎重考虑。流畅地处理 60 FPS 的动画会很困难，你可以用 Ember 或者其他类库实现它们。你需要了解如何处理[常见的渲染上的性能问题](http://jankfree.org/)，否则无论你选择哪个框架都会陷入困难之中。

----

>**Brad Dunbar：**相比其他类库 Backbone 更加高效和轻量。在移动端我没有遇到任何问题，不过我没有为老旧的设备编写适配。

>**John Munsch：**我现在的站点还没有关注移动端，但是之前的岗位确实在移动端做了很多。因为他们需要在仓库和棚厂区为扫码枪运行一个额外的 web 接口。我们创建了一个附属的 UI，仅仅是扫码枪的一个帮助页面。他们在桌面浏览器 UI 下使用相同的技术，并且页面会调用同一个后端 API，不过扫码枪触屏上的页面会更小更简单。

>在那种环境下，尝试为已经存在的页面创建一个响应式版本并不是一个好想法，因为在移动端可能要丢掉 80% 的功能。注意：我们非常幸运，在该项目发起前已经在 HTML5 浏览器下存在高质量的扫码枪。更早之前，这种扫码枪功能仅仅讯在与 IE5.5。*发抖*

>如果我现在想要回到这个项目，我要做的第一件事就是借助 Grunt 为 JavaScript 拼接、压缩、版本等，这可以近一步加速移动端和桌面端。开启头部的 expires，这样浏览器缓存就可以存在一个月或者更多。

----

>**Julio Cesar Ody：** JavaScript 很快，现代的浏览器也是非常的高效并且在持续优化。

>也就是说，当提到好的用户体验性能就是一切。不过 JavaScript 层面的事情只是一部分。如何使用 CSS（以及 transition、animation），小而美的标签是怎样的，这些也会影响性能。你必须关注整体。

>如果你们中有人做过 C 语言中的微控制器编程，就会了解在受限的设备上编写程序。尽管手机已经像电脑一样强大，但它们依旧普遍上比手提电脑要慢。你承受不起过多的错误，并且你最好按照我前面提到的规则，加倍小心。

>检测有问题的东西会有很大帮助。首推 Chrome 开发者工具。

----

>**Thomas Davis：**目前来说，在选择移动端和桌面端之间的前端开发方式上并不存在一个通用方法。有时，原生应用更好，有时前端 JavaScript 应用，有时服务端生成的页面。尽管，性能问题通常与 DOM 操作相关，我敢打赌 React 将大行其道。React 实现的虚拟 DOM与浏览器中 DOM 相比较，仅仅在有必要时更新 DOM，达到了高性能渲染。正因为它的虚拟 DOM，你也可以在服务端渲染，在程序中访问它们。

----

### InfoQ：使用你所选择的框架最有哪些独有的工作流程？使用什么用具开发和调试？



>**Igor Minar：**Webstorm、Karma、Chrome。

----

>**Julien Knebel：**我使用 Grunt 作为构建工具预处理、预编译、后处理、连接、压缩等等。TextMate2 是我一直选用的 IDE。我同样花费了大量的时间在 Chrome 开发工具上使用断点和调试器来进行调试。当然，我离不开 Git。

----

>**Brad Dunbar：**我倾向于使用那些极简的设置，包括一个 CLI 和 vim/node/npm。最近，我正在享受 browserify 的 bundling。对于工作流程，我是很典型的编码、刷新、调试循环往复。我从不是测试优先或同类事情的倡导者。

----

>**John Munsch：**很多前端团队的开发者在 Mac 上使用 WebStorm 或者 Sublime。整个项目通过 Maven 构建和运行，因为后端是 Java，我们最近开始在 Maven 内部使用 Grunt 来优化前端代码。Jsnkins  用于创建和运行单元测试，以及部署到各种测试环境和生产。

>现在，有些开发者在后台运行 Karma 跑 AngularJS 单元测试（不过目前仅仅覆盖了 30% 的代码）。

----

>**Julio Cesar Ody：**我做了很多自己能预见到（在我参与它时）的设计。观察一个页面的样子协助我对将要编写的组件做一次内在的分析。

>我用过自己编写的[Hopla](https://github.com/juliocesar/hopla)，在很长一段时间内，将其作为一个构建工具。我使用 Sprockets 预处理 CoffeeScript 和 SASS，以及将应用便以为静态 HTML/CSS/JS。这样会很方便部署，甚至在创建 Phonegap 应用时。

>然后，我经常通过使用 SCSS 编写一个静态 HTML 页面开始实现一个设计，然后检查每一部分将它们替换成 JavaScript 组件。

----

>**Thomas Davis：**诚然，我一点也不关心别人喜欢如何开发和调试，并且我的方法也会在不同的项目上发生变化。我对开发者最直截了当的建议就是使用 异步模块定义（AMD）。Require.js 非常准确地实现了 AMD，并且在我的经验中它使得调试所有的语言和环境都更加简单。它不仅仅帮助你结构化你的代码，而且减少整个代码库的障碍，因为一切都通过一个文件路径作为依赖引用。

----

### InfoQ: As applications grow bigger, it can be a challenge to keep a robust architecture and maintain a large code base. How does your favourite JavaScript framework scale? Also how does it scale as development teams become bigger and different people need to work on different parts of the functionality?

#### InfoQ：随着应用变大，保持架构的健壮以及维护庞大的代码库是一个挑战。你喜欢的 JavaScript 框架有多大规模？当开发团队膨胀并且不同的开发者需要负责不同的功能时，它的扩展性如何？


>**Igor Minar:** Code reuse, reduction of boilerplate and style guide and conventions are important for reducing complexity of large code-bases. But in our real world experience we've seen that high quality test-suites have even higher impact because they allow for major refactoring while keeping the risk low. And refactoring is a key to keeping the code base clean.

>**Igor Minar：** 代码重用，简化示例代码、样式指南以及一些惯例用法对于压缩庞大代码库的复杂性都很重要。但是在我们的日常经验中，我们发现高质量的测试测试用例会有更大的好处，因为它们允许在低风险的情况下进行重大的重构。代码重构是保持代码库整洁的关键之一。

----

>**Matteo Pagliazzi:** Besides the complexity (like the presence of Services, Factories and Providers which is very confusing), with simplicity a goal of v2 (http://blog.angularjs.org/2014/03/angular-20.html) what I personally don't like is the dirty-checking mechanism used by Angular to see which properties have changed and update the view according (but also this is going away as Object.observe is implemented natively, allowing for notifications to be issued upon change - instead of having to check for every property's changes)

>**Matteo Pagliazzi：** 除了复杂性之外（比如服务，厂商以及供应商呈现出的混乱），[Angular2](http://blog.angularjs.org/2014/03/angular-20.html) 所要解决的问题，也是我最不喜欢的脏检测机制，它用于在属性改变时响应地更新视图（而且这可以通过 `Object.observe` 原生地实现，允许改变后发出通知，而不是在每次属性变化时去检测）。

----

>**To wrap up:** Angular is really great, backed by an awesome community, an enormeous number of external libraries that usually satisfy every need and even in case you didn't find what you need you can always write your own directive. But it is also full of complex concepts that take time to understand.

>**To wrap up：** Angular 非常棒，它有很了不起的社区作为后盾，有大量的外部库可满足每个日常需求。即便没有找到，你也可以写出自己的指令。不过，需要花上一定的时间去理解那些复杂的概念。

----

>**Julien Knebel:** Ember deals really well with growing codebases because of its "enforced good practices" and its strong conventions which let you do a lot with single lines of code (aka convention over configuration). It helps each member of the team debug other's code faster. I saw some developers getting angry because they felt constrained to code how Ember wanted them to code, but I guess the real frustration came from the fact that you first have to understand/learn Ember before trying to do fancy stuff with it. And I do believe that this extra education time really worth it in the long-term. Plus, I tend to trust Yehuda Katz and Tom Dale's work a lot (but this is not really objective I guess :)

>**Julien Knebel：** Ember 确实能处理好日渐增多的代码库，因为它的“强制最佳实践”以及它的强约定，使得你需要处理很多单行代码（也就是通过配置进行约定）。它有助于团队的每个成员都可以快速地 debug 其他人的代码。我看到一些开发者因为感觉到自己代码要受到 Ember 限制而愤怒。不过我猜真正的沮丧来自于在你想要用它尝试一些炫酷的东西之前，必须去理解或者学习 Ember。并且我真的觉得这个额外的学习时间在长远上看真的很值得。此外，我非常相信 Yehuda Katz 和 Tom Dale 的工作（但我想这有点主观了）。

----

>**Brad Dunbar:** I think large JavaScript codebases are challenging regardless of the framework used. It comes down to the team maintaining a set of guidelines and sticking to them. Use of modules of some kind (I mentioned I like npm and browserify) helps tremendously though.

>**Brad Dunbar：**我认为大型 JavaScript 代码库真的是一个挑战，这和用了哪些框架无关。它需要团队来维护一套方案，并坚持下去。借助一些模块化方案（像我提过的 npm 和 browserify）是极好的方案。

----

>**John Munsch:** So far our experiences with scaling are good, front-end JS scales well because there's lots of static files amenable to optimization, caching, and even CDNs. Pretty much the only bad experiences have been with trying to render thousands of items worth of data on pages. We've tried to be encourage the use of pagination and other workarounds but we can't always get agreement to use them and large chunks of data generating thousands of DOM elements is a recipe for problems on old browsers.

>**John Munsch：**目前为止，我们在代码规模增长上有足够的经验，前端 JS 规模通过优化、缓存甚至是 CDN 处理的很好。最坏的一次是尝试在页面上渲染上千条数据。我们也尝试使用翻页和其他方案解决，不过在使用翻页以及在旧版本浏览器上通过大量数据生成数千个 DOM 元素的方案上我们还没有达成一致。

>Other problems have actually completely gone away. For example, AngularJS's default behavior of discarding views and controllers when switching between views keeps memory usage on another view from impacting the present view. That's a simple solution that helps developers avoid a lot of problems as they add more and more functionality to a single page app.

>其他问题实际上已经不是问题。比如，AngularJS 在切换视图时，一个视图保持内存占用会影响当前的视图，它的默认行为是丢弃视图和控制器。这是一个简单的解决方案，可以帮助很多开发者在一个 SPA 中添加愈来愈多的功能时避免很多问题。

----

>**Julio Cesar Ody:** I don't think any of the popular frameworks has a defined scaling path. It's up to the developer to think of something sane and run with it.

---

>**Julio Cesar Ody：** 我认为所有的流行框架都没有一个既定的扩展路线。它需要开发者理性地选择。

>I've always liked the concept of components/modules. It lets me think of writing programs much like building a watch. Each part (or component) has its purpose, and needs to work as independently as possible, exposing a small surface area that other components can interact with.

>我一直很喜欢组件或者模块的概念。它让我觉得写程序就像是创建一个。每部分（或者说组件）都有它的目的，并且需要尽可能地独立运行，暴露可以和其他组件进行交互的一小块。

---

>**Thomas Davis:** As I said above as long as you use a modular JavaScript framework such as Require.js, scaling and maintaining your code base is a walk in the park and will only depend on your coding practises. Though Backbone.js does need to be split into its own modules at this point so instead of requiring Backbone as a dependency, you would only require Backbone.Model for example.

>**Thomas Davis：**就像我前面所提到的，只要你使用了一个模块化 JavaScript 框架比如 Require.js，规模和维护代码库犹如闲庭信步，仅仅取决于代码规范。在这点上，尽管 Backbone.js 不需要被拆分成模块。你仅仅需要想示例那样引入 Backbone.Model，而不用单独地将 Backbone 作为依赖引入。

----

### InfoQ: For teams that are just now considering adopting a JavaScript framework, what would be some of the common pitfalls to worry about?

### InfoQ：对你那些刚刚考虑使用一个 JavaScript 框架的团队，有哪些常见的陷阱需要注意的么？


>**Matteo Pagliazzi:** Working on a big open source project that uses Angular as the client side framework I found that for developers without much experience it's easy to incur in problems with inheritance or to start polluting the $scope and $rootScope with a lot of objects that at a certain point will start to slow the application and grow the RAM used by the browser (we easily use 300+ MB and it can easily get to 1GB with an even small memory leak). The fact that it's made data binding so "simple" is of course fantastic but you have to understand that it will always work as you want just because it's "magic".

>**Matteo Pagliazzi：**曾在一个很大的开源项目中使用 Angular 作为客户端框架，我发现经验不够丰富的开发者很容易陷入继承的问题中或者使用很多对象污染了 $scope 和 $rootScope，这会让应用变得很慢并且增加了浏览器的 RAM 占用（我们一般使用 300+ MB，一个很小的内存泄露都可能轻易地占用 1GB）。如此简单的数据绑定确实很棒，不过你必须理解它才能更好地使用，因为它是很“神奇的”。

----

>**Julien Knebel:** All those MV* frameworks are heavy JavaScript calculations on the front-end, therefore if your app needs to print a large amount of data to the user you'll quickly hit your performance budget (talking about the 60 FPS budget). You'll have to deal with pagination, intelligent scrolling, wisely dosed CSS decorations, subtle sequencing, couple chosen places to display spinners to let network calls retreive data, etc. Eventually, all that stuff will matter a lot.

>**Julien Knebel：**所都的 MV* 框架都在前端进行比较重的 JavaScript 计算，因而应用需要为用户输出大量的数据，可能很快就超出了你的性能预算（假设 60 FPS 的预算）。你必须处理好翻页、智能地滚动、明智的 CSS 声明、细微的顺序差别、下拉菜单的显示位置等等。最终，将这些放在一起就会有非常大的影响。

----

>**Brad Dunbar:** I think the most common issues come before you've even dug into the framework. Your client needs tests, modules, a package manager, and continuous integration just like server. If you can stay true to those, you'll probably be alright.

>**Brad Dunbar：** 我认为最常见的问题来自于框架中的 bug。你的客户端需要需要测试、模块、包管理器以及持续地整合就像服务器一样。如果你做到了这些，应该就够了。

----

>**John Munsch:** I've mentioned the problems we had with Backbone.js, but there are some common things that can always come up. For example SEO is a factor for pages generated entirely via JavaScript on the front-end. I've been fortunate that most of my work has been SaaS so SEO hasn't been anything we've cared much about, but if you're building something for the wider web you need to look at solutions like Prerender.io, BromBone, etc. to see how you're going to deal with that. Google Analytics can present similar problems. None of these are unsolvable, but it's good to go in knowing that there are some issues and you may need to pick a solution.

>**John Munsch：**我之前提到过 Backbone.js 存在的问题，不过还可能会遇见一些常见的问题。比如全部在前端使用 JavaScript 生成整个页面在 SEO 方面存在问题。很幸运，我的绝大部分工作使用了 SaaS，所以基本不用关心 SEO 的问题。不过如果你正在创建需要支持 web 爬虫的项目，你可以去参考一下 Prerender.io、BromBone 等的解决方案，决定你该如何解决。Google Analytics 也有同样的问题。这些问题没有不能解决的，不过最好知道有哪些问题，需要找一个解决方案。

>For us, without a doubt, IE8 has been one of our biggest problems. It's the last browser with a significant chunk of market share out there which does not have a built in JavaScript just-in-time compiler. As a result, JavaScript heavy front-ends can be slow at times, can trigger "unresponsive script" errors when displaying lots of data, etc. The sooner the Lovecraftian horror that is IE8 goes away, the better for all of us.

>对我们而言，毫无疑问 IE8 是最大的问题之一。它也是唯一各个占据着大量浏览器市场并且没有内建的 JavaScript 实时编译器。结果导致了重度 JavaScript 依赖的前端应用有时会很慢。在需要展示很多数据等情况下可能导致“unresponsive script”错误。IE8 越早离开，对我们来说越好。

----

>**Julio Cesar Ody:** The learning curve is definitely the biggest problem I see. Most people have been developing web apps for years, which consist of a server component sending taking, processing requests, and sending HTML back to the browser.

>**Julio Cesar Ody：** 我所看到的最大的问题应该是学习曲线。很多人已经开发 web 应用程序好多年，包括服务端组件接收、处理请求以及将 HTML 返回给浏览器端。

>This is a completely different paradigm which resembles network programming (client/server) a lot, and that's not something many have done before. It has many of the same problems network programming has, and having a background in it surely helps.

>这是一个完全不同的范例，和网络编程（客户端/服务器）有些像，并且和以前做的的那些都有些不同。它有很多和网络编程一样的问题，具备这方面的背景一定会有帮助的。

----

>**Thomas Davis:** The only major pitfall to really worry about is not having server generated content available at the url. This of course stops search engines from being able to directorize your website and you will also encounter problems with sharing your page. A lot of social networks now try to parse your website when users share so that they can bring down extra information. If you don’t have content generated at server time, these share attempts will fail. Though this problem has been solved many times and I recommend prerender.io as nice open source solution, I have also wrote SEO server libraries to mitigate this problem. Though the search engine giants should really take it into their own hands to render your single page applications for content. Some people speculate the entire Chromium project is an attempt by Google to be able to load and execute all pages so that they can tie it into Google Bot and directorize all content rendered by JavaScript.

>**Thomas Davis：**我所担心的唯一一个问题是 URL 上不存在服务端生成的内容。这理所应当就阻碍了搜索引擎抓取站点的能力，在分享页面上也会遇到了问题。现在很多社交网络在用户分享功能中都会对站点进行解析以便读取到其他的信息。如果没有在服务端生成内容，这些尝试都会失败。尽管有很多方式可以解决这个问题，不过我还是推荐 `prerender.io` 这个开源方案，我还写了 SEO 服务起类库来环节这个问题。搜索引擎巨头们真的应该将它内置以便渲染出单页应用的内容。有些人猜测 Google 的 Chromium 项目正在做尝试以便可以加载和执行整个页面。这样就可以把它变成 Google 机器人并且识别出所有由 JavaScript 渲染出的内容。

### InfoQ: For someone that wants to start working with the framework, what do you think would be the fastest and most efficient learning path? Any resources you’d like to recommend?

### InfoQ：如果想要开始使用一个框架，你觉得最快并且最有效的学习路径是什么？有哪些推荐的资源么？

>**Igor Minar:** There are several good books on Angular, newsletters like ng-newsletter.com and many great podcasts here.

>**Igor Minar：** 在 ng-newsletter.com 上有不少 Angular 方面的好书和咨询，而且这里还有很多播客。

---

>**Julien Knebel:** The official guides on emberjs.com are tremendous, every single one of them targets a specific feature of the framework. Otherwise I wrote a beginners in-depth introduction on Smashing Magazine (when Ember 1.0 came out) which, in my humble opinion, is still very relevant.

>**Julien Knebel：**emberjs.com 官方文档很棒，对该框架的每个特性进行了详细解释。另外，我在 Smashing Magazine 上写了一个详细的介绍（Ember 1.0 问世的时候）。非常谦虚地说，这个教程仍有很大的意义。

---

>**Brad Dunbar:** It's maybe not the coolest thing to point out, but I always recommend reading the source. That's where the action is and if it's a mess you'll know it. You'll become very familiar with your choice of MVC framework so if you can't stomach the source you may as well hang it up.

>**Brad Dunbar：** 这可能不是最酷的事，不过我总会推荐阅读源码。这才是关键所在，你会知道是否它很烂。【看不懂呀 这么抽象】

---

>**John Munsch:** If the person in question is already familiar with JavaScript I would recommend picking a small project (four or fewer distinct views making up a web app) and implementing that entire project using the framework in question. Building something, even something very simple, all the way to completion and actually deploying it for use is very different from "playing" with a framework. You are forced to solve some specific problems and really start to understand some things that are otherwise easy to skip because they may not be the fun parts of learning it.

>**John Munsch：** 如果提问者已经很熟悉 JavaScript 了，我会推荐去完成一个小项目（四个或者更少的独立的视图构成一个 web 应用）并且使用刚刚讨论的框架完成整个项目。创建一些东西，即便很简单，完成的方式以及实际部署和单纯“学习”一个框架还是有很大区别的。你会被强制解决一些具体的问题并且会真正的理解一些事情，否则你可能很自然的跳过一些事情，应为学习起来它可能不那么有趣。

----

>**Julio Cesar Ody:** Make as many mistakes as you can in pet projects, but be extra mindful about them. By that I mean it's futile to try and nail everything in the best possible way right out of the bat, and it won't help you get there faster.

>**Julio Cesar Ody：** 在喜欢的项目中可以犯很多错误，不过需要记下它们。我是想说，试错和抓狂可能是个好方式，不过它不会帮助你很快地达到目的。

>Then refactor your own code over and over, keeping in mind what you're looking for is a scenario where you're putting a bunch of components to work together to become a full application, and you need to keep them as independent as possible.

>然后反复修改你的代码，注意你要找到一个场景用来将很多个组件放到一起构成一个完整的应用，然后保证每一个都尽可能的独立。

>I've written a free booklet on this, with an emphasis on Backbone.js, which of course I'll recommend.

>我曾经写了一个免费的手册，包括了 Backbone.js 的重要性，同样是我比较推荐的教程。

---

>**Thomas Davis:** Depends on your learning style, I generally prefer to just jump in and start hacking away. I have received amazing feedback on my video tutorial for Backbone.js which can be found on Youtube.

>**Thomas Davis：** 取决于你的学习方式，我通常比较喜欢跳进去然后开始钻研。我上传到 youtube 上的关于 Backbone.js 是视频教程总会收到很惊奇的反馈。

---

### InfoQ: Now that MVC framework have become mainstream, how do you think they need evolve in order to better fulfil developer needs? For example what features do you think they’re still missing?

### InfoQ：现在 MVC 框架已经成为主流，你觉得他们需要如何演变才能够满足开发者的需求？比如你觉得他们还缺少那些特性？

>**Igor Minar:** Mobile, testability, mobile.

>**Igor Minar：**移动端、可测试性等。

----

>**Julien Knebel:** Real time data synchronized between backend and multiple clients would be the next logical step. Meteor.js seems to go this way so I'll surely give it a try soon.

>**Julien Knebel：**后台和多个客户端之间的实时数据同步可能是下一趋势。Meteor.js 似乎正在这么做，所以我一定要尽快试一试。

---

>**Brad Dunbar:** I'm not sure about this one. I tend to submit patches for things I think Backbone needs and not all of them are good. :)

>**Brad Dunbar：** 这一点不我太确定。我倾向于给 Backbone 不足的地方提交补丁，不过并不是每一个都被采纳。

---

>**John Munsch:** The easiest answer is that something like the usage numbers on ngmodules.org tells you some of the solutions that developers end up having to seek out over and over. Anything that ties the front-end framework to HTML/CSS frameworks (like Bootstrap), file upload, internationalization and localization, etc. If hundreds of developers have had to go find it and then took the time to go mark on a website that they use it, you can bet that thousands more also use it but never bothered to share that fact. That's the easy answer, but I'd actually plug two things I don't think get the love they ought to, validation and no back-end.

>**John Munsch：** 最简单的答案就是参阅 ngmodules.org 这一类站点，有很多开发者反复地在这里寻求答案。任何前端有关的框架都会涉及到 HTML/CSS 框架（像 Bootstrap），文件上传、国际化、本土化等等。如果成百上千的开发者都去探索它，在使用它上面花时间，你也就不用担心社区分享的问题。这是简单的答案，不过实际上还应该有这样两件事，校验以及没有后端。

> #### Validation
> 
> #### 校验

>Client side validation is usually wanted and for some pages can be very complex. We have pages where many dates need to be tested against each other to make sure they occur in the correct sequence, and many other variables have various constraints as well. Then we send them up via an API call to the server, which turns around and tries to do the same stringent validation because you can never rely on validation client side. Our back-end is not written in JavaScript so we can't use a common library of code to do the validation and we get to build two sets of the same code in different languages and give ourselves twice the likelihood of making a mistake in some edge case.

>总会出现客户端校验的情况，并且对某些页面来说会很复杂。有很多页面需要与其他页面上的时间对比以确定它们是否在恰当的场景下出现，有些不可控因素也对其有很多限制。我们将数据通过 API 发送到服务端，将严格验证的数据返回，因为你不能依赖客户端的验证。我们的后算不是用 JavaScript 语言编写的，所以我们没办法使用同一个代码库来验证，因而就需要用不同的语言写相同的逻辑，对某些边缘情况会出现犯同样的错误的可能。

>Why not let me describe the data with a JSON Schema and then make it easy for me to use that for validation on both sides of the API call? I would like to see that made easy.

>为什么不让我描述一下 JSON 模式的数据然后使得它在客户端以及服务端验证上变得都很容易呢？我期待看到它可以变得更简单。

>#### No Back-End

>#### 没有后端

>There was some buzz about this last year as a term of art (for example, nobackend.org) but that's kind of died down. But the idea itself still seems to be going strong with Firebase, Hoodie, Backendless, etc. I think it's a great way for some developers only familiar with front-end work to have a way to do a complete app without needing help from someone to do the back-end. But even for someone like me who could comfortably build the front and back-end of an application it provides an easy way to prototype an idea or get to an MVP.

>在去年，这是一个很火的话题（比如，nobackend.org），不过现在有些平息了。不过 观点本身在 Firebase、Hoddie、Backendless 等技术下依旧很强势。我认为这对仅仅熟悉前端的开发者来说是一件好事，可以不需要后端就完成一个完整的应用。这样，很多开发者都可以轻易地创建一个应用的前后端，这提供了更简洁的方式来实现一个想法的原型。

---

>**Julio Cesar Ody:** It's hard to say. I think a lot of ideas are born out of misconceptions about modularity, for one, so the more features mindset is one I've historically fought in the past.

>**Julio Cesar Ody：**很难说。我认为很多想法源自于对模块化的误解，所以很多特性的模式只是在和以前的想法争辩。

>But I think React.js has the right idea about it, in that it gives you a component-driven approach to building apps, and it's hard to do anything terribly wrong if you just follow the examples. It also abstracts some hard problems such as DOM performance and it pretty much gives you that for free.

>不过我认为 ReactJS 在这一点上很正确，它提倡组件驱动的方式来创建应用，如果你仅仅跟着示例模仿就不会出现很糟糕的错误。它也改善了很多难题，比如 DOM 性能，这样确实带来了很多便利。

>That's the kind of problem no one wants to spend any time solving in the course of building an application, so that's a step in the right direction. 

>这也是在创建应用时很少有人愿意花时间来解决的一类问题，所以这一步的方向很正确。

---

>**Thomas Davis:** Client-side tooling and MVC’s are making fast progress in the right direction. I actually believe that server API’s are lagging behind at this point. There is no real convention that people are following when it comes to building RESTful API’s which makes it difficult for client-side models and collections to bring down data efficiently. Error logging on the client-side still needs some work but attempts such as trackjs.com are making progress there. How events are handled on the client side could also use some work. 

>**Thomas Davis：**客户端 MVC 正沿正确的方向快速前进。我确信在这点上服务端 API 正在落后。目前，还没有一个开发者共同遵循的既定的规范，当创建 RESTFul API 的时候客户端数据模型和集合很难高效的获取数据。客户端的 Error 日志同样需要一定的工作量，不过尝试使用 trackjs.com 会好些。如何处理好客户端的事件同样有很多工作要完成。

## 讨论会成员

![Igor Minar](http://www.infoq.com/resource/articles/real-world-javascript-mvc-frameworks/en/resources/no-pic.jpg)

----

**Igor Minar** 是一名来自 Google 的软件工程师。他是 AngularJS 的带头人、测试驱动开发的实践者、开源项目热衷者、骇客。
 
----
 
 ![Matteo Pagliazzi](http://www.infoq.com/resource/articles/real-world-javascript-mvc-frameworks/en/resources/Matteo.jpg)

----

**Matteo Pagliazzi** 是一名热情的软件开发者和开源贡献者。
 
----

 ![ Julien Knebel](http://www.infoq.com/resource/articles/real-world-javascript-mvc-frameworks/en/resources/1no-pic.jpg)

----

**Julien Knebel** 是一名自学的接口设计者和前端开发者，居住在巴黎。在是一个自由职业者，主要在法国的几个最大的公司。

----

![Brad Dunbar](http://www.infoq.com/resource/articles/real-world-javascript-mvc-frameworks/en/resources/Brad.jpg)

----

**Brad Dunbar** 是一名 JavaScript 工程师，同时也是 Backbone 和 Underscore 项目的贡献者。

---- 

![John Munsch](http://www.infoq.com/resource/articles/real-world-javascript-mvc-frameworks/en/resources/John.jpg)

---- 

**John Munsch** 是一名具备 27 年开发经验的专业软件开发者。最近，他带领团队使用 AngularJS 创建新的 web 应用，在此之前的几年内，他们使用过类型的 Backbone.js、Underscore.js 和 Handlebars.js。

----
 
![Julio Cesar Ody](http://www.infoq.com/resource/articles/real-world-javascript-mvc-frameworks/en/resources/Julio.jpg)

----

**Julio Cesar Ody** 是一名软件开发者、设计师、主持人并且渴望成为作家，居住在澳大利亚·悉尼。他主要工作在移动 web 开发，并且开发了一个自己很得意的实用的工具。

----
 
 ![Thomas Davis](http://www.infoq.com/resource/articles/real-world-javascript-mvc-frameworks/en/resources/1Thomas.jpeg)

----

**Thomas Davis** 是 backbonetutorials.com 的创始人、cdnjs.com 联合创始人以及 taskforce.is 的一名开发者。同样，还是很多开源项目的每日贡献者，可以在 github.com/thomasdavis 找到。
 


原文：[Virtual Panel: Real-world JavaScript MVC Frameworks](http://www.infoq.com/articles/real-world-javascript-mvc-frameworks)