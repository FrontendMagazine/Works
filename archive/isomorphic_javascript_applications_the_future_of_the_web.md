# Isomorphic JavaScript Applications — the Future of the Web?

# 同构 JavaScript 应用 ——  Web 世界的未来？

One of the best known mottos around the web is Java’s Write once, run everywhere. But does this motto apply to Java only? Can we use it to describe JavaScript too? The answer is Yes.

Web 世界有一个至理名言，就是 Java 提出的“Write once, run everywhere”。但这句话只适用于 Java 么？我们能否也用它来形容 JavaScript 呢？答案是 Yes。

In this article, I’ll introduce you to the concept of isomorphic JavaScript applications, describing what they are and pointing to resources that help you develop this kind of application.

我将会在这篇文章中介绍同构 JavaScript 应用的概念，并推荐一些资源帮助你构建此类应用。

## How We Arrived Here

## 一路走来

Many years ago, the web was a bunch of static pages made with HTML and CSS without much interactivity. Each user action required the server to create and serve a complete page. Thanks to JavaScript, developers started to create nice effects, but it was with the advent of Ajax that a revolution started. Web developers began to write code that could communicate with the server to send and receive data without the need to reload the page.

许多年以前，web 只是一些由 HTML 和 CSS 搭建的静态页面，没有太多的交互。用户的每一个动作都需要服务器来创建并返回一个完整的页面。幸而有了 JavaScript，开发者开始创建很棒的效果，不过 Ajax 的到来才是这场革新的真正开始。Web 开发者开始编写能够与服务端进行交互，且在不重载页面的情况下向服务端发送并接受数据的页面。

As the years have passed, the responsibilities of the client-side code have grown a lot, resulting in a new type of application known as the single-page application (SPA). In an SPA, all the necessary assets are retrieved with a single page load, or dynamically loaded and added to the page as necessary. Some examples of SPAs are [Gmail](https://mail.google.com/) and the [StackEdit editor](https://stackedit.io/editor).

随着时间的推移，客户端代码可以做的事情越来越多，催生了被称作单页面应用（SPA）的一类应用。SPA 在首次加载页面时就获取了所有必需的资源，或者再按需动态加载并且渲染到页面上。 [Gmail](https://mail.google.com/) 和 [StackEdit editor](https://stackedit.io/editor) 是很棒的 SPA 示例。

SPAs allow for better interactivity, because almost all their operations are executed on the client, keeping communications with the server to a bare minimum. Unfortunately, they also have some major problems. Let’s discuss some of them.

SPA 准许重度的交互设计，因为几乎所有的操作都在客户端执行，保持最低限度地与服务端进行交流。不幸的是，它们也有一些严重的问题，我们选择几个进行讨论。

## Performance

## 性能

Because SPAs require more client-side code than static pages, the amount of data to download is increased. This leads to slower initial loading times, which can have drastic consequences – such as frustrated end users and loss of revenue. According to one [Microsoft](http://blogs.msdn.com/b/ie/archive/2014/10/08/http-2-the-long-awaited-sequel.aspx) article –

>
A Bing study found that a 10ms increase in page load time costs the site $250K in revenue annually.

因为相对于静态页面，SPA 需要更多的客户端代码，需要下载数据的体积也更大。这使得手机加载速度很慢，可能会导致一些极端的状况 —— 比如糟糕的用户体验以及损失收入。依据 [Microsoft](http://blogs.msdn.com/b/ie/archive/2014/10/08/http-2-the-long-awaited-sequel.aspx) 的一片文章 ——

>
Bing 的一项研究表明：页面的加载时间每增加 10ms，站点年度总收入减少 $250K。

## SEO

## SEO

Because single-page applications rely on JavaScript execution, servers don’t produce all the HTML content they used to. Therefore, web crawlers have a lot of difficulties indexing pages. These crawlers are programs that make requests to a web server and analyze the result as raw text, without interpreting and executing the content like a typical browser running JavaScript would do. Recently, [Google improved its web crawler](http://googlewebmastercentral.blogspot.co.uk/2014/05/understanding-web-pages-better.html) so that it can work with JavaScript-based pages, but what about Bing, Yahoo, and all the other search engines? Good indexing is crucial for any business, as it usually leads to more visits and higher revenue.

因为单页面应用依赖于 JavaScript 的执行，服务器不会提供它们会用到的任何 HTML 内容。因此，web 爬虫很难去索引到这些页面。爬虫就是可以像 web 服务器发送请求，并且将结果分析成原始文本的程序，不需要像一个浏览器运行 JavaScript 那样解释和执行客户端的内容。不久前，[Google 优化了搜索引擎的 web 爬虫](http://googlewebmastercentral.blogspot.co.uk/2014/05/understanding-web-pages-better.html)，现在它也可以抓取基于客户端 JavaScript 所构建的页面了。但是 Bing、Yahoo 以及其他搜索引擎怎么办？一个好的索引对任何公司来说都至关重要，它通常会带来更多的流量以及更多的回报。

## Isomorphic JavaScript Applications

## 同构 JavaScript 应用

Isomorphic JavaScript applications are applications written in JavaScript that can run both on the client and on the server. Because of this, you can write the code once and then execute it on the server to render static pages and on the client to allow for fast interactions. So, this approach takes the best of the two worlds and lets you avoid the two issues described before.

同构 JavaScript 应用基于 JavaScript 编写，可以在客户端和服务端运行。正因为此，你只需要写一次代码，就可以在服务端渲染静态页面，还可以在客户端完成复杂的交互。所以，这种方式互通了两个世界，并且避免了前面提到了两个问题。

Today there are several frameworks that assist you in developing this kind of application. One of them – possibly the most well-known – is [Meteor](https://www.meteor.com/). Meteor is an open-source JavaScript framework, written on top of Node.js, that focuses on real-time web applications. Another project I want to mention is [Rendr](http://rendrjs.github.io/rendr/). It’s a small library [developed by Airbnb](http://nerds.airbnb.com/weve-launched-our-first-nodejs-app-to-product) that allows you to run Backbone.js applications on both the client and the server.

现在，有很多框架可以帮助你开发这类应用。其中可能最著名的一个是[Meteor](https://www.meteor.com/)。Meter 是一个开源 JavaScript 框架，基于 Node.js 编写，专注于实时 web 应用。我想提到的另一个项目是[Rendr](http://rendrjs.github.io/rendr/)，它是[Airbnb 开发](http://nerds.airbnb.com/weve-launched-our-first-nodejs-app-to-product)的一款轻量级类库，准许同时在客户端和服务端运行 Backbone.js。

More companies are adopting Node.js for their products. Sharing code between the client and server is becoming a more common and natural choice, and in my opinion is the future of web development. This trend is enhanced by sharing templates through libraries like [React](http://facebook.github.io/react/).

越来越多的公司将 Node.js 应用到他们的产品中。客户端和服务端的代码共享成为一个更加普通而自然的选择。在我看来，这种做法将是 web 开发的未来。有些类库通过共享模板又增强了这一趋势，比如 [React](http://facebook.github.io/react/)。

## Conclusion

## 结论

In this article I’ve introduced you to the concept of isomorphic JavaScript applications, a new approach to developing applications that combines the best of server-side and client-side programming. We’ve also discussed what problems this approach tries to solve, and some projects that you can employ today to embrace this philosophy.

这篇文章介绍了同构 JavaScript 应用的概念，它是应用开发的一种全新方式，可以最大限度地结合服务端和客户端应用程序。我们还讨论了运用这种方式尝试解决的问题，以及你现在就可以参与实践的一些项目。

Had you already heard of isomorphic JavaScript applications? Have you developed one? What was your experience?

你听说过同构 JavaScript 应用么？你开发过么？你有开发经验么？
 

原文：[Isomorphic JavaScript Applications — the Future of the Web?](http://www.sitepoint.com/isomorphic-javascript-applications/)