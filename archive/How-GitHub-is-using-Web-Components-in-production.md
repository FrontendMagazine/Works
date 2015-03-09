# How GitHub is using Web Components in production

# Web Components 在 GitHub 中的应用

![http://img.readitlater.com/i/webcomponents.org/img/stories/interview-with-joshua-peek/RS/w512.jpg](http://img.readitlater.com/i/webcomponents.org/img/stories/interview-with-joshua-peek/RS/w512.jpg)

More and more people have been using Web Components. Some just want to play with it, while others have been using it in really big projects.

使用 Web Components 的人越来越多，有些人可能只是玩一玩，而有些人已经在真实的大项目中使用起来了。

In order to keep pushing this community forward we decided to start a series of interviews with some early adopters.

为了推动社区的发展，我们将推出一个专题，采访一些早期的尝试者。

Today, we'll start by asking some questions to Joshua Peek, a programmer who has been working on GitHub for the past three years.

这次，我们从访问 [Joshua Peek](https://twitter.com/joshpeek/) 开始，他本人是一个开发者，在 [GitHub](https://github.com/) 已经工作了三年。

## Even with your high-traffic volume of visitors worldwide, GitHub.com is using Web Components in production. How hard was it to convince them to adopt a brand new technology like that?

## 尽管你们有巨量的来自世界各地的访问者，GitHhub.com 还是已经使用了一些 Web Components 在线上的产品上。费了多大了力气才说服大家使用这一类新技术？

We've actually been really cautious about jumping onto the latest client side MVC library of the month. jQuery is pretty much the only framework we're still using. When it comes to standardized web technologies, we're usually on the bleeding edge.

这个月，我们确实非常谨慎地考虑过使用最新的客户端 MVC 类库。jQuery 差不多是唯一一个我们还在使用的框架。但是说到标准化的 Web 技术，我们一直都是走在前面的。

The initial element extension we wrote were a pretty minimal impact to start testing the Custom Element APIs. If for some reason it didn't work out, we could easily roll back to our original relative time rewriting script.

为了开始测试 Custom Element API，我们[最初的元素扩展](https://github.com/github/time-elements)是小范围的。因为如果出了什么问题，我们可以很容易地使用与原来差不多的时间，重写代码。

![GitHub.com's Custom Element](http://webcomponents.org/img/stories/github-custom-element.jpg)

## The <time> element extensions you created are using only Custom Elements. What do you think about other specs such as HTML Imports, Templates, and Shadow DOM?

## 你们对 time 元素做的扩展只使用了 Custom Elements，那你对其他的规范 HTML Imports、Templates 和 Shadow DOM 有什么看法？

Template looks really wonderful and I'd love to use it today but the polyfills can't quite handle all the edge cases. Just little things like inert scripts, table element fragments and duplicate ids are all the pain points we have with just using a dummy <div> element today. But if we can't trust the polyfill to be consistent with the native template element then its just going to cause more headaches.

Template 看起来很赞，我很想现在就用它。不过，目前有很多边缘情况 polyfill 都无法处理。很简单的，比如不执行的脚本，table 元素标记和重复的 id 都是痛点，我们目前的解决方案都是使用 div 元素来模拟。但是，如果polyfill 无法实现与原生模板元素一样的功能，一定会产生很多头疼的问题。

Shadow DOM is definitely interesting but it seems to be the least well defined spec-wise. I'm a little cautious to even use the polyfills since they need to hijack almost every DOM query API to define the custom Shadow DOM behavior. There could be some performance issues there.

Shadow DOM 确实很有趣，但是充其量就是规范已经定义好了。谨慎考虑，我可能不会用 polyfill，因为它们需要劫持几乎所有的 DOM 查询的 API，来定实现自定义的 Shadow DOM 行为。这可能会产生性能问题。

HTML Imports polyfills rely on eval() to run embed scripts which we block for security reasons and just require more undeveloped tooling to inline and rewrite URLs. It's just a lot of work and complexity for not much benefit right now. We're fine just importing both a script and style resource for a component we want to use on a page.

HTML Imports polyfill 目前依赖于 eval() 来运行内嵌的脚本，于是被我们拒之门外。因为缺乏安全性，且需要很多亟待开发的工具来做内联和重写URL。这很复杂，需要大量的工作，有点得不偿失。我们现在就是直接把页面上需要用到的脚本和样式直接引入就行了，没啥问题。

I think we'll be sticking with just Custom Elements for now. The polyfills work and the plain old JS apis work nicely with all our existing infrastructure.

我想，我们目前就先使用 Custom Element。polyfill ，原来的老的 JS API 和我们目前有的架构基础三者可以很好的在一起工作。

## Is there any particular fallback strategy that you had to implement for users with old browsers?

## 针对还在使用过时浏览器的用户，你们有特定的降级策略么？

Luckily the Custom Elements polyfill goes back to IE 9 which is the oldest browser we support. We do still care about the degraded no-js experience beyond that. For <time> elements its simple, if you don't run any JS you just get a static date which is acceptable in those cases. I think we'll try to continue the pattern of putting "noscript" content inside the Custom Element tag. Say you wanted a custom element version of<select>. You could include a native control inside the custom element then upgrade it when the JS actually runs. So if the browser is too old or blocks JS, you still have some functioning control.

庆幸的是，Custom Element polyfill 可以支持到 IE 9，它同时是我们支持的最老的浏览器。除此之外，我们确实还关心不支持 JavaScript 的浏览器该如何降级。对于 time 元素来说，这一点不难，如果 JavaScript 没有运行，用户可以看到一个静态的日期，这是可以接受的。我想我们可以尝试在 Custom Element 标签中放一个 noscript 来解决这个问题。假如，你想自定义select 元素，你可以在自定义元素内部放一个原生的 select 元素，JavaScript 运行的话就可以更新之。总之，如果浏览器很老，或者不支持 JavaScript，你还是可以做一些功能性的控制。

## Besides extending that particular tag, does GitHub have plans for using any other Custom Element?

## 除了扩展这些标签，GitHub 还有使用其他 Custom Element 的打算么？

Yeah, I think we're going to be moving forward in this direction. Next up probably some simple controls like menus and modals. I'm really hopeful for a simple XHR async submission pattern around forms. I've been experimenting on a project called async-form-element. Not quite production ready yet. But this is the kind of thing I hope gets pushed down the HTML spec itself one day.

是的，我想我们会朝着这个方向继续向前。下次可能是一些简单的控件，比如 menu  和 modal。我真心希望表单有 XHR 异步提交的功能。我已经在一个名为 [async-form-element](https://github.com/josh/async-form-element) 的项目上做实验了，目前还未成熟，不能用在真实的产品中。但是我希望将来某一天可以加入到 HTML 标准中。

I don't ever see us going all in on Custom Elements for every possible thing. We're not planning on boiling everything down to a single magic<github-app></github-app>. Use native elements and controls when possible and supplement with custom elements.

当然，我们不会在任何可能的组件上都是用 Custom Element。我们不会把所有东西揉到一起变成一个谜一般的 `<github-app></github-app>`。有可能就使用原生的元素和控件，自定义元素作为补充。

[原文](http://webcomponents.org/articles/interview-with-joshua-peek/)
