# A Baseline for Front-End [JS] Developers: 2015

# 2015 前端[JS]工程师必知必会

It’s been almost three years since I wrote A Baseline for Front-End Developers, probably my most popular post ever. Three years later, I still get Twitter mentions from people who are discovering it for the first time.

上次我写[《前端工程师必知必会》](http://rmurphey.com/blog/2012/04/12/a-baseline-for-front-end-developers/)已经是三年前了，那是我写过最火的文章了。三年了，我仍然会在Twitter上收到关于这篇文章的消息。


In some ways, my words have aged well: there is, shockingly, nothing from that 2012 post that has me hanging my head in shame. Still, though: three years is a long time, and a whole lot has changed. In 2012 I encouraged people to learn browser dev tools and get on the module bandwagon; CSS pre-processors and client-side templating were still worthy of mention as new-ish things that people might not be sold on; and JSHint was a welcome relief from the #getoffmylawn admonitions – accurate though they may have been – of JSLint.

从2012年到现在，一篇文章都没发过让我觉得有点羞羞哒。三年是一段很长的时间，很多东西都发生了改变。2012年，我鼓励同学们去学习浏览器开发者工具和模块化；虽然有很多同学会觉得CSS预编译和客户端模板引擎并不靠谱，但我仍然想要说一说它们；还有JSHint，虽然有#getoffmylawn（滚出我的地盘）的警告，但依然无法阻止JSHint成为一个受欢迎的理念（准确的说，JSLint真的（只是）存在过）。


It’s 2015. I want to write an update, but as I sit down to do just that, I realize a couple of things. One, it’s arguably not fair to call this stuff a “baseline” – if you thought that about the original post, you’ll find it doubly true for this one. One could argue we should consider the good-enough-to-get-a-job skills to be the “baseline.” But there are a whole lot of front-end jobs to choose from, and getting one doesn’t establish much of a baseline. For me, I don’t want to get a job; I want to get invited to great jobs. I don’t want to go to work; I want to go to work with talented people. And I don’t want to be satisfied with knowing enough to do the work that needed to be done yesterday; I want to know how to do the work that will need to get done tomorrow.

已经是2015年了，我想写一篇新的，但是当我坐下来开始动笔的时候，想到了两个事情。一，这些东西被称作“必知必会”可能有人会觉得不太公平——如果你已经觉得[2012年的那篇文章](http://rmurphey.com/blog/2012/04/12/a-baseline-for-front-end-developers/)如此，那本文也是一样的了。也许有同学会说，我们应该把 “足够应付业务需求的技能” 作为 “前端必须掌握的知识”，但考虑到前端行业里也有各种各样的工作可供选择，这么做也只能得到一个并不适合所有人的 “前端基础知识”。对于我来说，我需要的不是工作，我想要的是被邀请去做一份牛逼的工作。我想要的不只是去干活而已，而是想和一群牛逼的人一起做牛逼的事。我不想仅仅满足于用已有的知识来完成现在的工作，而是希望掌握更多的知识来解决未来将会面对的问题。


Two, my world has become entirely JavaScript-centric: knowledge of the ins and outs of CSS has become less and less relevant to my day-to-day work, except where performance is concerned. I know there are plenty of very smart front-end developers for whom this isn’t true, but I have also noticed a growing gulf between those who focus on CSS and those who focus on JavaScript. That’s probably a subject for another blog post, but I bring it up just to say: I am woefully unequipped to make recommendations about what you should know about CSS these days, so I’m not going to try.

第二，我现在已经完全把Javascript作为我的核心了：CSS知识只有在必须关注性能问题时才会用到，其他场景已经用的越来越少。我知道有很多牛逼的前端同学并不是这样的，但我也意识到，关注JS的同学和关注CSS的同学之间的距离也越来越远。这可能需要在另起一篇文章来讨论，不过我想说的是，这篇文章中不会有介绍CSS技能标准的内容，因为我还远远没有达到能那么做的水平。


In short: if this list of things doesn’t fit your vision of the front-end world, that’s OK! We’re both still good people. Promise.

总之，就算这个技能列表并不适合你的前端工作，没关系，不要有压力，地球也不会爆炸。

## Javascript

Remember back in 2009 when you read that HTML5 would be ready to use in 2014, and that seemed like a day that would never come? If so, you’re well prepared for the slow-but-steady emergence of ES6 (which is now called ES2015, a name that is sure to catch on any day now), the next version of JavaScript. Getting my bearings with ES6 – er, ES2015 – is hands-down my biggest JavaScript to-do item at the moment; it is going to be somewhere between game-changing and life-altering, what with classes, real privacy, better functions and arguments, import-able modules, and so much more. Those who are competent and productive with the new syntax will have no trouble standing out in the JS community. Required reading:

## JavaScript

回想2009年，那时候当你知道 HTML5 在2014年才能用的时候，你是不是觉得这辈子基本上都用不到它了？如果是，那么你需要准备好接受进展缓慢但是已经趋于稳定的ES6了，它也是下一代的Javascript（现在叫 [ES2015](https://esdiscuss.org/topic/javascript-2015) 了，嗯，这名字至少表示今年就能用了）。就我而言，ES6，额，ES2015 无疑是我个人现在最关注的 Javascript 内容。在 ES6 中将会出现一些比较大的变化：类，真正的私有，经过改进更易用的函数和参数设定，可导入的模块，等等等等。那些掌握和理解新的语法的同学以后将会在 JS 社区牛逼闪闪。相关阅读：

- Understanding ES6, a work-in-progress book being developed in the open by Nicholas Zakas.
- BabelJS, a tool that lets you write ES6 today and “compile” it to ES5 that will run in current browsers. They also have a good learning section.
- ES6 Rocks, with various posts that explore ES6 features, semantics, and gotchas.

- [Understanding ES6](https://leanpub.com/understandinges6/read/)，Nicholas Zakas 正在写的书。
- BabelJS，一个可以把你写的 ES6 的代码编译成 ES5 并在现代浏览器中运行的工具。他们也有一个不错的[介绍 ES6 的文档](http://babeljs.io/docs/learn-es6/)。
- [ES6 Rocks](http://es6rocks.com/)，里面有大量的文章探索 ES6 的特性，语义和缺陷。


Do you need to be an ES6/ES2015 expert? Probably not today, but you should know at least as much about it as your peers, and possibly more. It’s also worth at least entertaining the possibility of writing your next greenfield project using ES6; the future will be here before you know it.

你也许会问：那我需要成为一个 ES6 专家么？也许现在不需要，但至少你得和你的同事懂的一样多吧？或者比他们稍微多一点？当然，如果能在你的下一个新项目中作为一个娱乐性的技术尝试也是不错的，做好准备肯定没错的，因为我们永远不知道下一刻会发生什么。


New language features aside, you should be able to speak fluently about the asynchronicity of JavaScript, and using callbacks and promises to manage it. You should have well-formed opinions about strategies for loading applications in the browser and communicating between pieces of an application. You should maybe have a favorite application development framework, but not at the expense of having a general understanding of how other frameworks operate, and the tradeoffs you accept when you choose one.

先不说新的语言特性，使用回调和 promises 管理异步 Javascript 至少得背的滚瓜烂熟吧。浏览器端应用加载，以及应用间通信策略得形成一套自己的观点吧。而且你应该知道哪种框架最适合你，而不是现在还把时间花在理解各种框架的实现原理和该选择哪种框架上。


## Modules & Build Tools

There’s no debate that modules should be the building blocks of client-side web applications. Back in 2012, there was lots of debate about what kind of modules we should use for building apps destined for the browser – AMD or CommonJS. The somewhat-gross UMD wrapper arose to try to avoid answering the question while still allowing code reuse – because hey, what’s a few more bytes between friends?

## 模块化和构建工具

毫无疑问，模块化是构建 Web 客户端应用的基石。回到2012年，关于使用哪种模块化（[ADM](https://github.com/amdjs/amdjs-api/blob/master/AMD.md) /[CommonJS](http://webpack.github.io/docs/commonjs.html)）方案构建浏览器端应用还存在很多争论。而最近慢慢火起来的[ UMD ](https://github.com/umdjs/umd)则在保证代码可复用的前提下尝试避免这样的问题。 其实也没什么好争得，毕竟这俩玩意儿之间也就差几个字符吧？


I don’t feel like this debate is anywhere near resolved, but this is the area where I feel like we’ve seen the largest transformation since my 2012 article, though perhaps that’s a reflection of my personal change of heart. I’m not ready to say that I’m done with AMD, but let’s just say I’m floored by how practical it has become to develop and deploy web applications using CommonJS, including modules imported with npm.

我觉得类似这样的争论其实并不都需要有一个答案，这也是我觉得从2012年到现在我们发生的最大的转变，当然，也许只是我自己这么认为。因为我觉得与其说“我再也不用 AMD 了”之类的话，倒不如多去讨论 “在开发和打包过程中使用 CommonJS 和 npm 遇到的各种难题” 来的更有价值。


With much love for all that RequireJS has contributed to the module conversation, I’m a bit enamored of webpack right now. Its features – such as easy-to-understand build flags – feel more accessible than RequireJS. Its hot-swap builds via its built-in dev server make for a fast and delightful development story. It doesn’t force an AMD vs. CommonJS decision, because it supports both. It also comes with a ton of loaders, making it fairly trivial to do lots of common tasks. Browserify is worth knowing about, but lags far behind Webpack in my opinion. Smart people I trust tell me that systemjs is also a serious contender in this space, but I haven’t used it yet, and its docs leave me wanting. Its companion package manager jspm is intriguing, allowing you to pull in modules from multiple sources including npm, but I’m a bit wary of combining those two concerns. Then again, I never thought I’d break up with AMD, yet here I seem to be, so we’ll see.

虽然很感激[ RequireJS ](http://requirejs.org/)曾经对模块化做出的贡献，不过现在我开始有点迷恋[ webpack ](http://webpack.github.io/)了。 webpack 的构建配置比 RequireJS 更加易于理解，也更具访问性。通过它的热插拔特性和内置的本地静态服务器可以让发布更加便捷。它并不强制要求使用 AMD 或者 CommonJS -- 两个它都支持。它还实现了一大堆加载器，用来完成常见的繁琐工作。[ Browserify ](http://browserify.org/)也值得去了解一下，不过我个人认为它比 Webpack 落后很多。一些靠谱的朋友告诉我说[ systemjs ](https://github.com/systemjs/systemjs)也是这个领域的竞争者，不过我还没有用过，而且它的文档烂的我连看都不想看。不过我觉得它的好基友[ jspm ](http://jspm.io/)(包管理器)比较有趣，jspm 可以让你从各种包管理服务器加载你需要的各种组件，(组件必须是符合 ES6, AMD, CommonJS and globals 规范的)，包括 npm, github 等，但是我对于这两个玩意的合体还是有点不太理解。啊，还有，虽然我说了这么多关于模块化之外的内容，但我从来没想过放弃 AMD，我们边走边看吧。


I still long for a day when we stop having module and build tool debates, and there is a single module system and sharing code between arbitrary projects becomes realistic and trivial without the overhead of UMD. Ideally, the arrival of ES6 modules will bring that day – and transpilers will fill in the gaps as the day draws closer – but I find it just as likely that we’ll keep finding ways to make it complicated.

我觉得如果要停止对模块化和构建工具的争论，形成统一的模块化系统，并且在这个系统里面，任何项目的代码都可以共享，而且还不需要 UMD 这样额外的补丁工具，我们还有很长的路要走。理想状况下，[ ES6 modules ](http://www.2ality.com/2014/09/es6-modules-final.html)的到来会解决这些问题，不过在这一天到来之前，类似 UMD 之类的转换器会填补这些空缺，不过貌似这样做我们又把事情变得复杂了，好像我们也总喜欢把事情弄得复杂。


In the meantime, front-end developers need to have an opinion about at least a couple of build tools and the associated module system, and that opinion should be backed up by experience. For better or worse, JavaScript is still in a state where the module decision you make will inform the rest of your project.

与此同时，前端开发人员也需要对构建工具，各种模块化系统有自己的见解和知识储备。不管是好是坏，根据 Javascript 现在的进度，你的模块化策略会对你的项目有比较大的影响。


## Testing

Testing of client-side code has become more commonplace, and a few new testing frameworks have arrived on the scene, including Karma and Intern. I find Intern’s promise-based approach to async testing to be particulary pleasing, though I confess that I still write most of my tests using Mocha – sometimes I’m just a creature of habit.

## 测试

客户端的代码测试变得越来越普遍，最近也诞生了一些新的测试框架：[ Karma](http://karma-runner.github.io/0.12/index.html)，[Intern ](https://theintern.github.io/)。我发现基于 promise 的 Intern 的异步测试方法相当优雅。不过可能是因为习惯，我大多数情况下还是用 Mocha 写测试用例。


The main blocker to testing is the code that front-end devs tend to write. I gave a talk toward the end of 2012 about writing testable JavaScript, and followed up with an article on the topic a few months later.

测试的主要障碍其实是前端开发者的代码编写方式。我在2012年发表过一个关于[《编写可测试的Javascript》](https://www.youtube.com/watch?v=OzjogCFO4Zo)[下载地址](http://savefrom.net/?url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DOzjogCFO4Zo&utm_source=userjs-chrome&utm_medium=extensions&utm_campaign=link_modifier)的演讲，紧接着几个月后又发表了一篇[相关的文章](http://alistapart.com/article/writing-testable-javascript)。


The second biggest blocker to testing remains the tooling. Webdriver is still a huge pain to work with. Continuous automated testing of a complex UI across all supported browsers continues to be either impossible, or so practically expensive that it might as well be impossible – and never mind mobile. We’re still largely stuck doing lightweight automated functional tests on a small subset of supported browser/device/OS combinations, and leaning as hard as we can on lower-level tests that can run quickly and inexpensively. This is a bummer.

测试的第二大障碍是工具。Webdriver 是一个艰难而巨大的工作。目前在各个浏览器端做持续集成的 UI 自动化测试基本上是不可能的，更不用说移动端了。我们仍然停留在局限于某一小部分浏览器和设备上做轻量级的自动化功能测试，尽我们所能去研究怎样快速，低成本的进行这种测试的阶段。


If you’re interested in improving the problem of untested – or untestable – code, the single most valuable book you can read is Working Effectively with Legacy Code. The author, Michael Feathers, defines “legacy code” as any code that does not have tests. On the topic of testing, the baseline is to accept the truth of that statement, even if other constraints are preventing you from addressing it.

如果你对如何改进代码的可测试性感兴趣的话，那么唯一一本最值得看的书是[ Working Effectively with Legacy Code ](http://www.amazon.com/Working-Effectively-Legacy-Michael-Feathers/dp/0131177052)（[中译版：《修改代码的艺术》](http://www.ituring.com.cn/book/536)。作者 Michael Feathers 定义了“遗留代码”的概念：任何未经测试的代码都是遗留代码。在测试领域，最基本的要素就是上面这句话，尽管你可能不这么认为。


## Process Automation

You, hopefully, take for granted the existence of Grunt for task automation. Gulp and Broccoli provide a different approach to automating builds in particular. I haven’t used Broccoli, and I’ve only dabbled in Gulp, but I’ve definitely come to appreciate some of the limitations of Grunt when it comes to automating complex tasks that depend on other services – especially when that task needs to run thousands of times a day.

## 流程自动化

你首先会想到[ Grunt](http://gruntjs.com/)，这也是理所当然的。而[ Gulp ](http://gulpjs.com/)和[ Broccoli ](http://broccolijs.com/)的自动化构建方式也别具匠心。我没用过Broccoli，只玩过Gulp，我也开始意识到Grunt对于依赖其他服务的复杂任务的自动化工作存在局限性，尤其是当这种任务每天需要运行上千次的时候。


The arrival of Yeoman was a mere 45 days away when I wrote my 2012 post. I confess I didn’t use it when it first came out, but recently I’ve been a) starting projects from scratch using unfamiliar tech; and b) trying to figure out how to standardize our approach to developing third-party JS apps at Bazaarvoice. Yeoman really shines in both of these cases. A simple yo react-webpack from the command line creates a whole new project for you, with all the bells and whistles you could possibly want – tests, a dev server, a hello world app, and more. If React and Webpack aren’t your thing, there’s probably a generator to meet your needs, and it’s also easy to create your own.

[Yeoman](http://yeoman.io/)是在我写完2012年的那篇文章仅仅45天之后发布的，我承认当时我并没有及时去尝试一下。不过最近我开始启动一些新项目，这些新项目有两个特点
a) 这些项目都是从零开始
b) 尝试用一些不同的技术方案，试图通过这种方式找到 Bazaarvoice（提供第三方点评服务）上第三方 JS 应用的规范化的开发方式。
Yeoman 在这两方面做的都很好。一个简单的 `yo react-webpack` 命令就可以为你初始化好你的项目，然后各种你想要的玩具也都应有尽有：生成测试用例，本地静态服务器，hello world 入门程序，等等等等。如果 React 和 webpack 不是你想要的，也许你会在 Yeoman 的 generators（项目生成器）里面找到一个你想要的，当然，自己自定义一个这样的构建包也是比较容易的。


Given that Yeoman is a tool that you generally use only at the start of a project, and given that new projects don’t get started all the time, it’s mostly just something worth knowing about. Unless, of course, you’re also trying to standardize practices across projects – then it might be a bit more valuable.

鉴于 Yeoman 只是一个在项目开始时才会用到的构建工具，并且鉴于我们并不是总是做新项目，所以大多情况下了解一下就够了。除非，你也想去规范整个项目开发过程，那么它可能会更有价值一点。


Broccoli has gotten its biggest adoption as the basis for ember-cli, and folks I trust suggest that pairing may get a makeover – and a new name – to form the basis of a Grunt/Yeoman replacement in the future. Development on both Grunt and Yeoman has certainly slowed down, so it will be interesting to see what the future brings there.

Broccoli 已经得到了 ember-cli 的采纳，我觉得他们的配对可能会有一个新名字，这样在未来才比较方便和 Grunt /Yeoman 对抗。而 Grunt 和 Yeoman 的开发进度也放缓了，所以未来会发生什么，我们还是静观其变吧。


## Code Quality

If you, like me, start to twitch when you see code that violates a project’s well-documented style guide, then tools like JSCS and ESLint are godsends, and neither of them existed for you to know about them back in 2012. They both provide a means to document your style guide rules, and then verify your code against those rules automatically, before it ever makes it into a pull request. Which brings me to …

## 代码质量

如果你像我一样，一看见违反代码规范的代码时就开始抓狂，那么[ JSCS ](http://jscs.info/)和
[ ESLint ](http://eslint.org/)就是老天赐给你的礼物，而2012压根就没这些玩意。他们都提供了自定义代码规范的方式，并且可以在代码提交前对你的代码做自动化校验。这让我想起了...


## Git

I don’t think a whole lot has changed in the world of Git workflows since 2012, and I’d like to point out Github still hasn’t made branch names linkable on the pull request page, for f@#$s sake.

## Git

从2012年到现在，github 的使用流程并没有发生很大的变化，比如在 pull request 页面连个分支名都没有（只是恶搞一下）。


You should obviously be comfortable working with feature branches, rebasing your work on the work of others, squashing commits using interactive rebase, and doing work in small units that are unlikely to cause conflicts whenever possible. Another Git tool to add to your toolbox if you haven’t already is the ability to run hooks – specifically, pre-push and pre-commit hooks to run your tests and execute any code quality checks. You can write them yourself, but tools like ghooks make it so trivial that there’s little excuse not to integrate them into your workflow.

你应该非常清楚和流畅地使用功能分支（feature branches）, 使用 rebase 合并别人的代码干活，使用交互式 rebase 命令和 squash 合并提交记录，或者尽可能细颗粒度的划分项目内容，避免引起代码冲突。另一个可用的 Git 工具是钩子，具体而言，就是你可以在 push 前，commit 前，执行你的各种测试用例，检查代码质量。你可以自己写钩子，也可以使用[ ghooks ](https://www.npmjs.com/package/ghooks)，由于 ghooks 使钩子工作变得非常简单，所以你简直没有理由不用它。


##Client-Side Templating
This may be the thing I got the most wrong in my original post, for some definition of “wrong.” Client-side templating is still highly valuable, of course – so valuable that it will be built-in to ES2015 – but there can be too much of a good thing. It’s been a hard-earned lesson for lots of teams that moving all rendering to the browser has high costs when it comes to performance, and thus has the “generate all the HTML client-side” approach rightfully fallen out of favor. Smart projects are now generating HTML server-side – maybe even pre-generating it, and storing it as static files that can be served quickly – and then “hydrating” that HTML client-side, updating it with client-side templates as events warrant.

## 客户端模板

这可能是我在2012年的那篇文章中写的最烂的内容了，某种意义上的“烂”。客户端模板还是很有价值的，而且它已经被内置到 ES2015 里面了，这不仅仅只是一件好事而已。这些年也有一些惨重的教训，不少团队把所有的渲染工作全部丢到浏览器端去做，结果产生了严重的性能问题，所以 “在浏览器端渲染生成所有 HTML” 的做法理所当然的被摒弃了。 而更为聪明的做法则是，把 HTML 生成放在服务器端，或者通过预编译的方式，先将模板做为静态资源储存起来，在需要时快速的编译成 HTML，需要更新时也可以直接在客户端更新模板。


The new expectation here – and I say this to myself as much as to anyone else – is that you are considering the performance implications of your decisions, and maybe not restricting yourself quite so thoroughly to the realm of the browser. Which, conveniently, leads to …

这里会有一些新的展望，不仅是对我自己，也是对所有人，当你在考虑性能问题时，也许没必要把自己完全限定在浏览器范围内。所以，这又让我想起了……


## Node
You say you know JavaScript, so these days I expect that you can hop on over to the Node side of things and at least pitch in, if not get at least knee-deep. Yes, there are file systems and streams and servers – and some paradigms that are fundamentally different from front-end dev – but front-end developers who keep the back end at arm’s length are definitely limiting their potential.

## Node

听说你懂 Javascript，那么我觉得你也应该懂 Node，至少在遇到 Node 问题是能帮得上忙的，如果连忙都帮不上，那也至少深入研究一下吧：Node 的文件系统，流，服务器，完全不同于前端的一些开发模式等等。对后端敬而远之只会限制我们前端的发展潜力。


Even if your actual production back-end doesn’t use Node, it’s an invaluable tool when it comes to keeping you from getting blocked by back-end development. At the very least, you should be familiar with how to initialize a Node project; how to set up an Express server and routes; and how use the request module to proxy requests.

即使你的真实生产环境中后端不用 Node，当你的工作被后端限制或阻碍的时候，Node 也是一个非常有用的工具。最起码，你也应该熟悉怎么去初始化一个 Node 项目，怎么用 Express 搭建服务器设置路由，怎么使用请求模块代理请求。

## The End

Thanks to Paul, Alex, Adam, and Ralph for their thorough review of this post, and for generously pointing out places where I could do better. Thank them for the good parts, and blame any errors on me.

## 最后

感谢 Paul, Alex, Adam, Ralph 对本文的 Review，感谢他们毫不吝啬的指出我的不足之处，并给我提了很好的意见。

With that, good luck. See you again in another three years, perhaps.

就这样，祝你好运。也许，三年之后我们会再见。


原文链接: [A Baseline for Front-End 'JS' Developers: 2015](http://rmurphey.com/blog/2015/03/23/a-baseline-for-front-end-developers-2015/)
