# How to Write Highly Scalable and Maintainable JavaScript: Modules

# 编写高扩展并且可维护的 JavaScript：模块

## Introduction

## 简介

So far we have covered the so-called “wild west” syndrome and some of the reasons for needing better organized JavaScript code bases, as well as namespacing as one strategy to achieve this goal. We ended the last chapter by mentioning JavaScript modules and module formats and specifications. This chapter is a deep dive into JavaScript modules.

到目前为止，这个系列已经涵盖：所谓的 “狂野西部”症和用于实现更好的组织 JavaScript 代码库的命名空间策略。我们会在最后一节提到 JavaScript 模块以及模块格式和规范。这一节将深入讲解 JavaScript 模块。

If a method can be thought of as a distinct unit of functionality, then modules should be thought of as a distinct grouping of units of similar functionality. JavaScript modules can be thought as somewhat synonymous to components in many server-side frameworks. Continuing with the classical analogy, a class should abide by the single responsibility principle from SOLID. Likewise, a module should also have a single responsibility, but In this case the single responsibility is that the module should be a one-stop-shop for handling related tasks.

假如把一个方法理解为功能上独立的单元，那么模块就该被认为是功能上相似的单元的一个独立的分组。JavaScript 模块近似等同于服务端框架中的组件。继续这个经典的类比，类应该遵循单一的职责，按照[SOLID](http://en.wikipedia.org/wiki/SOLID_(object-oriented_design))原则。同样的，一个模块也应该只承担单一的职责。就此而言，单一职责意味着一站式地处理了相关的任务。


Returning to our error handling module discussion from a previous chapter, an error handling module may have many distinct units of functionality (i.e., functions), but they should all be related to handling errors. In addition, the module should not contain any functionality not required for handling errors, and likewise be able to handle all error handling requirements for a given application.

回到前一节中我们关于错误处理模块的讨论，错误处理模块可能包含很多功能上不同的单元（比如函数），但是都应该是与处理错误相关的。而且，模块不应该包含任何在功能上对于处理错误无关的代码，此外，需要能够处理给定应用所需要处理的所有错误。

Once the error handling module is complete, it should be consumable by your application, or other modules as required. Even better, it could be loosely coupled from the rest of your application through an event-driven pattern like Pub/Sub, which we will cover in a future chapter.

当完成了错误处理模块，就应该在你的应用中被使用，或者被用到的其他模块中。更进一步，通过一个类似 Pub/Sub 的事件驱动模式实现与应用中其他模块松散地耦合，将会包含在下一个节中。

For now, let us take a close look at actual JavaScript module implementations.

现在，我们近距离的看一下真正 JavaScript 模块的实现。

## Vanilla JavaScript Modules

## 通常的 JavaScript 模块

Modules are relatively easy to implement in plain JavaScript, but there are a variety of patterns at your disposal. Each pattern arguably has its pluses and minuses. We will cover some of these patterns here, but leave out the opinions on which is most suitable for you. Most of the time it boils down to your preference and what works best for your particular situation. There is no shortage of very strong opinions online about what module implementations are best, so feel free to search around a bit if you are still unsure.

使用原生 JavaScript 实现模块是相对容易的，但是已经存在很多的模式随便你使用。每种模式可以说是各有利弊。这里会包含这些模式中的几个，但没有顾及那个最适合你的选择。多数时候，会取决于你的偏好以及哪个在你指定的场景下工作的最好。对于哪个模块实现是最好的，网上没有什么明确的意见。所以，如果不确定可以随便去搜索一下。

Let’s begin.

我们现在开始。

A JavaScript module can be as simple as an plain JavaScript object, where the properties of the object are the individual units of functionality, i.e., functions. Note that without a closure, this module example would wind up polluting the global namespace.

JavaScript 模块可以像 JavaScript 对象一样的简单，对象的原型就是功能上独立的单元，也就是函数。请注意没有闭包，这个模块实例可能会污染全局命名空间。

Example: POJO module pattern

实例：POJO 模块模式

``var myModule = {
    propertyEx: "This is a property on myModule",
    functionEx: function(){
        //insert functionality here
        //在这里插入代码
    }
};``

In order to create a closure and ensure all variables and functions are local to the module, often practitioners will wrap the module with an IIFE, or immediately-invoked function expression. This is very similar to the object method mentioned above.

为了创建闭包，并且确保所有的变量和函数都是这个模块局部的，通常业内会用一个 IIFE（立即执行的函数表达式）包裹这个模块，这与前面提到的对象方法很类似。

Example: Scoped module pattern

实例：Scoped 模块模式

var myModule = (function () {
  var module;
   
  module.varProperty = "This is a property on myModule";
  module.funcProperty = function(){
    //insert code here
  };
     
  return module;
})();
Another pattern is the module pattern itself, along with the so-called revealing module pattern. The revealing module pattern is a special case of the module pattern that implements a form of private members, and thus exposes a public interface only. Here is an example of the revealing module pattern.

另一种模式是模块模式，连同所谓的暴露模块模式。暴露模块模式只是模块模式的一个特例，实现了一种形式的私有成员，这样仅仅暴漏了一个公共接口。下面是暴露模块模式的的一个实例。

Example: Revealing module pattern

实例：暴露模块模式

var myRevealingModule = (function () {
  
    var privateVar = "Alex Castrounis",
        publicVar  = "Hi!";
  
    function privateFunction() {
        console.log( "Name:" + privateVar );
    }
  
    function publicSetName( strName ) {
        privateVar = strName;
    }
  
    function publicGetName() {
        privateFunction();
    }
  
    return {
        setName: publicSetName,
        greeting: publicVar,
        getName: publicGetName
    }; 
  
})();
The last vanilla JavaScript pattern that I will cover is the prototype pattern. It is very similar to the module patterns described above, but embraces the prototypal nature of JavaScript. Here is an example of a module based on the prototype pattern with private members.

我要说的最后一个普遍的 JavaScript 模式是原型模式。和前面描述的模块模式类似，但是使用了 JavaScript 原型。下面是一个带有私有成员基于原型模式的模块的一个实例。

Example: Prototype pattern

实例：原型模式

var myPrototypeModule = (function (){
  
    var privateVar = "Alex Castrounis",
        count = 0;
  
    function PrototypeModule(name){
        this.name = name;
    }
  
    function privateFunction() {
        console.log( "Name:" + privateVar );
        count++;
    }
  
    PrototypeModule.prototype.setName = function(strName){
        this.name = strName;
    };
  
    PrototypeModule.prototype.getName = function(){
        privateFunction();
    };
  
    return PrototypeModule;     
})();

For a more detailed view of different JavaScript module patterns and implementations, including global importing, I highly recommend reading this article by Ben Cherry. In addition, Addy Osmani’s book on JavaScript design patterns is a must read for anyone seriously interested in the topic.

再仔细的看一下不同的 JavaScript 模块模式和实现，包括全局引入，我强烈的建议读一下 Ben Cherry 的这篇[文章](http://www.adequatelygood.com/JavaScript-Module-Pattern-In-Depth.html)。另外，如果对这个问题非常感兴趣，Addy Osmani 的 [JavaScript 设计模式](http://addyosmani.com/resources/essentialjsdesignpatterns/book/)也是一本必读的书。

## Module Formats and Specifications

## 模块格式和规范

While there is nothing technically wrong with implementing vanilla JavaScript modules as per the examples given above, there are some things that are certainly worth considering. The first is that there are many ways to implement modules in plain JavaScript, which often lead to inconsistency and lack of standardization. Remember chapter one of this series on the “Wild West” syndrome? The other important consideration is that the plain JavaScript approach does nothing to account for asynchronous and parallel module loading, dependency management between modules, optimization, etc.

尽管，像前面给出的每个实例那样实现普通的 JavaScript 模块没有什么技术上的错误，不过有一些问题还是非常值得考虑的，那些往往会导致不一致，缺乏标准化。还记得这个系列第一节中关于“狂野西部”症么？另一个值得考虑的是原生 JavaScript 在处理异步和并行模块载入时，模块之间的依赖和优化等等时显得很无力。

In order to address these considerations, a variety of module formats and specifications have surfaced that are intended to promote the following: standardization and consistency, encapsulation, asynchronous and parallel script loading, increased application performance, improve readability and code cleanliness, reduction of HTML script tags, and dependency management. The three biggest players are AMD (Asynchronous Module Definition), CommonJS, and the ECMAScript 6 Harmony module specification.

出于这些考虑，大量的模块格式和规范应运而生，旨在提升以下几个方面：标准和一致，封装，异步和并行载入脚本，提升应用性能，提高可读性和代码整洁，避免出现 HTML 中 script 标签，并且单独维护。有三个最大的参与者分别是 AMD（异步模块定义），CommonJS 以及 ECMAScript 6 Harmony 模块规范。

Again, please note that this series is intended to be an overview of available choices, architectural considerations, and best practices. There are more than enough places to go online for strong opinions regarding these options.

不过，请记住这个系列的意图只是对一些现存的方式做了概述，构建的思考以及最佳实践。网上会有非常多相关的文章。

Let us examine each of these options individually and at a high level. Please refer to the ‘Resources’ section at the end of this chapter for additional resources that cover these topics in greater detail.

现在我们深层次的分别看一下这些选择。可以查阅这一节末尾的“资源”部分，有更多的资源的详细地涵盖了这些主题。

## Asynchronous Module Definition API (AMD)

## 异步模块定义 API（AMD）

Asynchronous Module Definition, or AMD, is a specification of how to define modules and their dependencies, which can be loaded asynchronously by the browser. The Require.js file and module loader by James Burke is a very commonly used framework based on the AMD specification. While it is likely the most popular, it is not the only AMD implementation in the wild.

异步模块定义，或者 AMD 是一个关于如何定义模块以及模块间依赖的规范，使得这些模块可以被浏览器异步载入。James Burke 的 Require.js 文件和模块加载器就是一个基于 AMD 规范的非常通用的框架。它可能是最受欢迎的，但它不是当前环境下唯一的 AMD 实现。

Require.js is highly configurable and customizable, and is very well implemented and widely used. It uses the AMD syntax, which is focused on the define and require functions. Common complaints about Require.js and AMD in general are that it can be fairly complicated to configure and set up, and the syntax required is not exactly terse. Also, by design, Require.js is more of a client-side (a.k.a. browser-based) module implementation and framework.

Require.js 的可配性和可定制性都很高，并且完成度很高，应用广泛。它使用了 AMD 语法，集中于 define 和 require 函数。对于 Require.js 和 AMD 最多的抱怨就是相当繁琐的配置和启动，并且用到的语法也不是很简单。此外，在设计上，Require.js 更像是一个客户端（基于浏览器）模块实现和架构。

You may or may not agree with the above points, but either way, there are certainly some upsides to embracing AMD and Require.js. The first is that Require.js is very good at what it is supposed to do. Assuming that you have everything set up properly, you can rest assured that your modules and their dependencies will be loaded asynchronously and in whatever order required, as per each module’s dependencies and your configuration.

你可能不同意上面的观点，但无论哪种方式，AMD 和 Require.js 肯定有积极的一面。第一点是 Require.js 在它想要做的地方表现的相当优秀。假定你已经完成了所有的初始设置，你会很放心模块和它们的依赖会异步的载入，可以满足任何所需要的规则，按照各个模块的依赖和你的配置。

The other advantage of using Require.js is that it has a built-in optimizer that can be used to combine and minify scripts (via UglifyJS or Google’s Closure Compiler), as well as inlining and optimization of CSS files via the @imports directive and comment removal.

使用 Require.js 的另一个优势就是它拥有内建的控制器，可被用于合并、压缩脚本（通过 UglifyJS 或者 Google 的 Closure Compiler），当然还有内联和优化 CSS 文件，通过 @imports 命令和移除注释。

## CommonJS

CommonJS is another module specification that is widely used. It is intended to specify a module format to be used by both browsers and servers alike, although it is most commonly implemented in server-side frameworks like Node.js. Browserify is a very popular framework intended to implement CommonJS modules on the client-side. You can implement a full stack CommonJS module solution when Browserify (or similar) for the client is combined with Node.js on the server.

CommonJS 是另外一个被广泛应用的模块规范。它试图制定一个模块格式可以在浏览器和服务端通用，尽管它最普遍的实现是服务端的框架，像 Node.js。Browserify 是一个很流行的框架，试图在客户端实现 CommonJS 模块。你能够实现一个全栈的 CommonJS 模块方案，通过把客户端的 Browserify（或者类似的）和服务端的 Node.js 结合在一起就行。

Unlike AMD, CommonJS is based primarily on the exports and require functions. Note that both AMD and CommonJS represent module dependencies as strings.

和 AMD 不同的是，CommonJS 更多是基于 exports 和 require 两个函数。需要注意，AMD 和 CommonJS 全都把模块的依赖表示成字符串。


Some common complaints of the CommonJS specification are reliance on server-side tools and build processes, being too server-driven in general, cross-domain issues, and lack of transport format. That said, CommonJS is beneficial to many people for the following reasons: it is relatively easy to setup and configure, terse syntax and implementation, easy to read, diverse, and can be implemented across the entire stack. It is also able to handle cyclical dependencies rather well.

对 CommonJS 规范最多的抱怨是依赖于服务端工具以及构建过程，服务驱动很重，跨域问题以及缺乏传输格式。尽管如此，CommonJS对很多人来说都是受益的，原因如下：相对容易去安装和配置，简洁的语法和接口，容易阅读，变化多，并且可以遍及全栈来实现。也就能够更好的复用多个依赖。

## ECMAScript 6 Harmony

## ECMAScript 6 Harmony

ECMAScript is a scripting language specification as defined by the ECMA-262 standard and ISO/IEC 16262. The maintenance and standardization of the ECMAScript language specification is the responsibility of ECMA International.

ECMAScript 是一个脚本语言规范，通过 ECMA-262 标准和 ISO/IEC 16262 定义。ECMAScript 语言规范的维护和标准化由 ECMA 国际来负责。

The current edition of the ECMAScript standard is 5.1, although ECMAScript 6 (also known as Harmony and ES.next) is the next edition of the standard. ECMAScript is the specification upon which the JavaScript language itself is based.

ECMAScript 标准的当前版本是 5.1，虽然这个标准的下一个版本 ECMAScript 6（也被称为 Harmony 和 ES.next）已经问世。ECMAScript 是 JavaScript 语言自身基于的规范。

Whenever new features and changes are introduced to the ECMAScript specification, the JavaScript language itself, and engines that interpret and execute it change accordingly (at least in theory).

每当 ECMAScript 规范推出新的特性和更新，JavaScript 语言自身和引擎都会相应的解释和执行它的变化（至少是理论上）。

The obvious benefit of a native module implementation in JavaScript is that it is, well, native! According to Axel Rauschmayer2, ES6 modules are more compact, benefit from static analysis and module structure, implement improved cyclical dependency handling, standardize on a single module specification, eliminate globals, and reduce or eliminate dependency of object-based namespaces.

使用 JavaScript 实现的原生模块最明显的好处当然就是原生！根据 Axel Rauschmayer2 指出，ES6 更加坚实，得益于静态解析和模块结构，实现改进的周期性处理依赖关系，单个模块规范的标准化，消除全局变量，并且降低或者消除基于对象命名空间的依赖。

ES6 modules consist of a declarative importing and exporting syntax, and programmatic loader API for module loading and dependency management.

ES6 模块通过声明导入和导出语法构造而成，还包括编程载入 API 用来载入模块和管理依赖。

Unlike AMD and CommonJS, which are utilized with a couple of very key functions (define, require, and exports), ES6 modules are keyword driven. The keywords import and export are the primary players. When an exported module member has a name declared, it becomes a named export.

与 AMD 和 CommonJS 不同的是，ES6 使用了几个非常关键的函数（define，require 以及 exports），它的模块是关键字驱动的。import 和 export 关键字是最基本的成员。为导出模块声明一个名字，就是一个具名 export。

In addition to named exports, ES6 defines a default export, which is meant to represent the single and most important exported value per module. The value can be whatever you want (function, object, etc.), but the default export is the key exported item for a given module. Note that a module can have a default export along with named exports.

除了具名 exports，ES6 定义了默认 export，用来表示每个模块最关键的一个导出值。可以是你想要的任何值（函数，对象等等），但是默认 export 应该是给定模块的关键导出项。注意，除了具名 export 外模块还可以拥有默认 export。

Additional features of ES6 modules include support for both synchronous and asynchronous loading, as well as a programmatic and configurable module loader API.

ES6 模块的其他特性包括对于同步和异步加载的支持，以及一个可编程和可配置的模块装载 API。

Note that ES6 modules are not implemented yet by all major browsers and may require using a transpiler to implement. This link is a great resource for determining browser compatibility for ES6 and ES7 features.

注意，ES6 模块还没有被所有主流浏览器实现，可能需要使用 transpiler 来实现。这个[链接](http://kangax.github.io/compat-table/es6/)是一个非常好的资源，用于判定浏览器对于 ES6 和 ES7 的兼容性。

## Summary

## 总结

In this chapter we have examined various plain JavaScript module implementations, as well explored the three dominant module formats and specifications. The upcoming native ECMAScript module specification is certainly exciting, but it is still in its infancy, and I highly recommend choosing a module pattern or specification that makes the most sense for you. Once you have settled on a module implementation, be sure to use it consistently throughout your application. Consistency is a key component to maintainability!

在这一节，我们详细讲述了多种原生 JavaScript 的模块实现方式，也主要探索了三个主要的模块格式和规范。当然也包括了即将到来的原生 ECMAScript 模块规范，但只是一个雏形，我强烈建议去选择自己最有感觉的模块模式或者规范。一旦决定了一个模块规范，并且确保在你的项目中的统一性。统一是可维护性的一个关键要素。

In the next chapter we will discuss patterns used to minimize coupling between modules, which can heavily contribute to an application’s scalability, reusability, reliability, and maintainability.

下一节中，我们会讨论用于最小化模块间耦合的模式，这对于一个应用的可扩展性，可重用性，可靠性和可维护性都至关重要。

Stay tuned!

敬请期待！


原文：[How to Write Highly Scalable and Maintainable JavaScript: Modules](http://www.innoarchitech.com/scalable-maintainable-javascript-modules/)