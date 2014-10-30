#How to Write Highly Scalable and Maintainable JavaScript: Namespacing

#如何编写高扩展且可维护的 JavaScript系列二：命名空间

##Introduction

##简介

This is the second chapter in a series of articles covering how to write highly scalable and maintainable JavaScript. In the first chapter of this series, we examined the so-called “wild west” syndrome, the root cause of this issue, and mentioned some potential solutions. In this chapter we will discuss the namespacing pattern as applied to JavaScript.

作为这个系列文章中的第二节，包含了如何编写高扩展并且可维护的 JavaScript。在这个系列的第一节中，我们讨论了所谓的“狂野西部”症这个问题的根源所在，并且提到了一些潜在的解决方案。在这一节中，我们会讨论应用于 JavaScript 中的命名空间模式。

Software applications these days, particularly web and mobile-based, are becoming more and more JavaScript intensive. A lot of a the logic and functionality found in web/mobile-based applications has seen a steady and increasing shift to the client-side.

现在的应用软件，尤其是网络和移动端，日益向着 JavaScript 密集型发展。在网络/移动端应用中存在着的大量逻辑和功能，正在稳定且持续增长地转向客户端。

Expanding on this further, consider some traditional non-JavaScript server-based functionality that is responsible for an application’s error handling. Depending on the required functionality and complexity, it would typically be implemented as either a separate library (project/dll in .Net), or as a single class under a specific namespace in a service or business layer. In order to use this functionality on the server, a consumer would either instantiate an instance of this class, or make service calls to the appropriate layer’s error-handling API. In either case, error handling is a cross-cutting concern and will likely be required by most, if not all application layers.

按照这个趋势，考虑到一些传统的、没有 JavaScript 的基于服务的功能设计，通常被用于应用的错误处理。根据所需要的功能和复杂性，通常是作为一个单独的库来实现（.Net 中的 project/dll），也可以作为服务层或者应用层中一个特定命名空间下的一个类。为了在服务端使用这个功能，使用者需要实例化这个类的一个实例，或者通过服务调用相关层的错误处理 API。无论哪种情况，错误处理都是一个横切关注点并且很可能是最需要的，如果不是所有的应用层。

Suppose that we want to have similar error handling capabilities on the client side in a JavaScript- dominant web application. How might we implement this? One way would be to create a single JavaScript file that handles all of our front end code for the given application. This file would include code for “DOM-ready” tasks like wiring up DOM element events, application bootstrapping and state initialization (think AngularJS), declarations (variables, objects, and functions), and any required cross-cutting functionality. This approach is obviously monolithic and not scalable, nor maintainable.

假设我们需要在一个 JavaScript 主导 web 应用的客户端上具备相似的错误处理能力。要如何实现？一种方式就是创建一个单独的 JavaScript 文件，控制了给定应用的所有前端代码。这个文件会包含“DOM-ready”任务的代码，就像和 DOM 元素事件建立了连接，应用引导以及状态初始化（想一下 AngularJS），声明（变量，对象以及函数），和所有需要横切的功能。这个方法很明显是庞大的，并且不方便扩展和维护。

A better approach would be to create a client-side architecture that has a similar layered and/or modular architecture as our server counterpart. How might we do this? Before diving into the architecture, let’s first discuss namespacing in JavaScript applications.

一个更好的方式是创建一个客户端架构，具备相同的分层和（或）模块结构，和服务端相对应。我们要如何做呢？在划分成架构前，我们先讨论一下 JavaScript 应用中的命名空间。

##Namespacing in Scalable and Maintainable JavaScript

##可扩展并且可维护的 JavaScript 中的命名空间

In the absence of a modular format and specification like AMD, namespacing in front-end JavaScript is a best practice to avoid global scope pollution and to organize code modules in a logical, scalable, readable, and maintainable fashion. Often times a top-level application namespace is chosen and this namespace (i.e., object) is the only application object that is attached to the global scope object (window in a web browser, and global in a Node.js application). Once the application’s top-level namespace is chosen, nested namespacing can be used in conjunction with a modular architecture. For example, consider the following:

在没有类似 AMD 这样的模块化的格式和规范时，在前端的 JavaScript 中，命名空间是是一个非常棒的实践，用于避免全局变量污染以及用于组织代码模块的逻辑性，扩展性，可读性和可维护性。通常选择一个顶级的应用命名空间并且这个命名空间（即对象）是唯一一个挂载到全局对象上的对象（在浏览器中是 window，在 Node.js 应用中是 global）。当选择了应用的顶级命名空间，嵌套命名空间可被用于一个模块化的架构。例如，考虑下面：

Example
实例
var myMasterNS = myMasterNS || {};     
myMasterNS.mySubNS = myMasterNS.mySubNS || {};
 
myMasterNS.mySubNS.someFunction = function(){     
    //insert logic 
    //插入逻辑
}; 

Here we have declared the primary global namespace as myMasterNS, and assigned a sub namespace object as a property of the primary namespace, mySubNS in this case. Now we are able to implement any required functionality on the top-level namespace, and on any subsequent sub namespaces without polluting the global scope. Note that this is not the only pattern for implementing namespacing in JavaScript. I chose to use it in this example due to the simplicity and readability of this pattern.

这里我们声明了一个名为 myMasterNS 的简单全局对象，又分配了一个子命名空间对象作为原来命名空间的一个属性，这个例子中是 mySubNS。现在我们能够在顶级命名空间下实现任何所需的功能，并且任何后面的子命名空间都不会污染全局作用域。请注意，这不是使用 JavaScript 实现命名空间的唯一模式。我在这个例子中选择它，是因为这种模式的简单和可读性。

With the server-side analogy, the top level namespace is similar to the entire application, whereas each nested namespace can be thought of as an individual library or module (or dll in .Net). One could break this down even further into sub-modules and so on, but the modularity in general is the key.

和服务端类似，顶级命名空间类似于整个应用，而每个嵌套的命名空间可以认为是单独的库或者模块（或者 .Net 中的 dll）。可以进一步分解为更多的子模块等等，但是模块化通常才是关键。

Again, we must remember that the organizational and architectural considerations and implementations applied to the server should also be applied to the client-side code. In the above namespacing example, we have done that to some extent.

还有，我们必须记住从组织和结构上考虑，并且服务端的代码实现在客户端上同样适用。在前面的命名空间实例中，我们已经一定程度上这样实现了。


##Summary

##总结

In this chapter, we have examined JavaScript namespaces and one way of implementing them. We also discussed some of the reasons to consider using namespaces in JavaScript, and examined some analogies with server implementations.

在这节中，我们仔细讲解了 JavaScript 命名空间以及实现他们的方式。也讨论了在 JavaScript 中使用命名空间的一些原因，并且详细解释了一些服务端类似的实现。

In the example shown, we have used namespacing to implement somewhat similar organization, grouping, and layering as our server-side counterparts. We have also been able to implement our entire application’s functionality with only a single variable attached to the global scope. We are certainly free to implement as many sub-namespaces as we see fit for our application, as well as utilize nested namespaces on our sub-namespaces, e.g., myMasterNS.mySubNS.mySubSubNS.

如例子所示，我们使用命名空间实现了有点类似于与服务端相对应的组织，分组以及分层。我们还通过挂载到全局作用域下的一个单独变量实现了整个应用的功能。当然，我们可以按照应用中的需求实现任意多的子命名空间，也可以在子命名空间里使用嵌套命名空间，即 myMasterNS.mySubNS.mySubSubNS。

We will cover native JavaScript module patterns in the next chapter of this series, which work very well with namespacing, as well as examine module specifications and formats (e.g., AMD) that are available and recommended in many cases. We will also consider cross-cutting concerns and loose-coupling as well.

在这个系列的下一节中会包含原生的 JavaScript 模块模式，功能上和命名空间非常像，也会仔细讲解模块规范和格式（即 AMD）多数情况下都是有效并且推荐的。我们同样会考虑横切关注点和松散耦合。

Stay tuned for more!

敬请期待更多内容！

原文：[How to Write Highly Scalable and Maintainable JavaScript: Namespacing](http://www.innoarchitech.com/scalable-maintainable-javascript-namespacing/)