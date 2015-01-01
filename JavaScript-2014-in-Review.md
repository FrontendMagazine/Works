# JavaScript: 2014 in Review
# JavaScript：回首2014年

I can’t keep up with the libraries that people send to DailyJS – there are just too many! I’ve always felt like this is a good thing: it’s a sign the community is creative and works hard to solve problems in interesting new ways.

大家给 DailyJS 提交的类库是在太多，我完全来不及去处理！但是我一直觉得这是一件好事：这代表着社区非常活跃，大家都在努力使用新的有趣的方式解决者问题。

It’s hard to decide on a framework or library for a particular task: should you use Koa, Express, or Hapi on the server? Gulp or Grunt as a build system? Then there’s client-side development, with its rich set of libraries. This year alone I’ve used React, Angular, Knockout, and Backbone.

无法针对特定的问题选择一个最好的框架或类库：在服务端使用 Koa、Express还是 Hapi？使用 Gulp 还是 Grunt 作为构建系统？还有就是客户端开发，有非常丰富的类库可用。就单单这一年，我用过 React、Angular、Knockout 和 Backbone。

One of the reasons there are so many Node modules is npm is so good. There’s still room for improvement, and the[ npm blog](http://blog.npmjs.org) has been useful for tracking new and upcoming changes to the package manager. It seems like more people than ever are using npm for client-side development as well, so it’ll be interesting to see if Bower still occupies its niche in 2015.

社区拥有这么多 Node 模块的原因之一就是 npm 的优秀。当然 npm 还有进一步的提升空间，[npm 的博客](http://blog.npmjs.org) 值得关注，可以了解到这个模块管理器将来的更新。而且看起来使用 npm 作为客户端模块管理的用户比以往都多，所以 Bower 在2015年是否还能继续坚挺，这值得期待。

Speaking of 2015, I expect to see more people using ES6 features. We’ve already seen several libraries that use generators to make synchronous-style APIs for client-side modules, and server-side databases. Generators seem hard to learn so it’ll take a while for these APIs to catch on.

说起2015年，我希望有更多的人使用 ES6 的特性。我们已经可以看到，有一些类库出现，使用 generator 让客户端模块实现同步风格的 API。generator 看上去很难学，这些 API 要流行起来还需要花点时间。

There’s still scepticism and even irritation in the Node community about ES6 modules. We’ve spent years writing CommonJS modules and happen to like the syntax, so ES6 modules are a hard pill to swallow. There’s a gist from 2013 about [Node and ES6 modules](https://gist.github.com/domenic/4748675) that has comments from well-known Node programmers, and since then [es6-module-loader ](https://www.npmjs.com/package/es6-module-loader)by Guy Bedford has appeared. This library is a polyfill that provides System.import for loading ES6 modules. Guy wrote a great article, [Practical Workflows for ES6 Modules](http://guybedford.com/practical-workflows-for-es6-modules) with lots of details on ES6 modules from a Node programmer’s perspective.

对于 ES6 的模块化体系，Node 社区任然表现出一些不理解，甚至愤怒。这么些年以来，我们编写 CommonJS 模块，碰巧也挺喜欢它的语法的，因此 ES6 的模块体系就像难以下咽的药片。2013年有一份来自著名 Node 开发者的关于 [Node 和 ES6 模块](https://gist.github.com/domenic/4748675) gist 评论，此后，Guy Bedford 发布了 [es6-module-loader ](https://www.npmjs.com/package/es6-module-loader)。这个类库是一个 polyfill，提供 System.import 来加载 ES6 模块。Guy 还撰写了一篇文章  [Practical Workflows for ES6 Modules](http://guybedford.com/practical-workflows-for-es6-modules)，以一个 Node 开发者的视角探讨了很多 ES6 模块体系的细节。

I don’t think 2015 will see a big Node/ES6 module controversy, though. It seems like CommonJS modules are here to stay, and perhaps eventually we’ll start using both formats.

我不认为2015年还会有针对 Node/ES6 模块体系的大讨论。CommonJS 任在，但最终我们会开始同时使用两种模式。

Another potential controversy is the future of Node forks.[ io.js](https://github.com/iojs/io.js) got a of initial attention, but it seems to have cooled off over the last fortnight. But I think forks are positive and I’m excited to see what people do with alternative takes on Node.

另外一个潜在的大讨论就是关于 Node forks 的前途。[ io.js](https://github.com/iojs/io.js)弄了个开门红，但是看起来在过去的几周里，已经归于平静。但我认为 forks 是有益的，我非常有兴趣看看大家如何对待 Node 的替代品的。

If you do anything in 2015, please make more libraries and frameworks. We don’t want a totalitarian open source community, we want a big wonderful mess, because open source is an ongoing conversation with no truly right solutions.

如果你真想在2015年做点事情，请多开发些类库和框架！我们要的不是一个极权的开源社区，要百家争鸣，因为开源是一场与非正确解决方案间的持续对话。

原文：http://dailyjs.com/2014/12/31/year-end/

外刊君推荐阅读：

- [Year 2014 of Node.js](http://blog.rednode.cn/year-2014-of-node-js/)
- [JavaScript Developer Survey 2014: Results](http://dailyjs.com/2014/12/16/javascript-survey-results/)
- [npm’s year in numbers: 2014](http://blog.npmjs.org/post/106746762635/npms-year-in-numbers-2014)



