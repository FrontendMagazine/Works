# Angular 2 vs React: 血腥对决

[Angular 2 has reached Beta](https://angular.io/) and appears poised to become the hot new framework of 2016. It’s time for a showdown. Let’s see how it stacks up against 2015’s darling: [React](https://facebook.github.io/react/).

[Angular 2](https://angular.io/) 已经发布 Beta 版，而且似乎很有信心在 2016 年成为热门框架。是时候进行一场巅峰对决了，我们来看看它如何与 [React](https://facebook.github.io/react/) 这个 2015 年的新宠抗衡。

**Disclaimer: **I enjoyed working in Angular 1 but switched to React in 2015. I just published a [Pluralsight course on React and Flux](https://www.pluralsight.com/courses/react-flux-building-applications) ([free trial](http://app.pluralsight.com/signup)). So** yes, I’m biased. But I’m attacking both sides.**

**免责声明：**我之前很喜欢使用 Angular 1，不过在 2015 年转到了 React。最近我也在 Pluralsight 上发布了一门关于 [React 和 Flux 的课程](https://www.pluralsight.com/courses/react-flux-building-applications)（[免费试学](http://app.pluralsight.com/signup)）。所以，**是的，我本人是有偏见的，但我不会偏袒任何一方。**

Alright, let’s do this. There will be blood.

好了，我们开始吧，这场对决将会非常血腥。

![](https://cdn-images-1.medium.com/max/800/1*MRPl_SNuRGJchb6eOAnkSA.jpeg)

图片来源：[@jwcarrol](https://twitter.com/jwcarroll)



## You’re Comparing Apples and Orangutans!

## 两者根本不具有可比性！

Sigh. Yes, Angular is a framework, React is a library. Some say this difference makes comparing them illogical. Not at all!

是的是的，Angular 是框架，React 是类库。所以有人觉得比较这两者没有逻辑性可言。大错特错！

> Choosing between Angular and React is like choosing between buying an off-the-shelf computer and building your own with off-the-shelf parts.

> 选择 Angular 还是 React 就像选择直接购买成品电脑还是买零件自己组装一样。

This post considers the merits of these two approaches. I compare React’s syntax and component model to Angular’s syntax and component model. This is like comparing an off-the-shelf computer’s CPU to a raw CPU. Apples to apples.

两者的优缺点本文都会提及，我会拿 React 语法和组件模型跟 Angular 的组件模型做对比。这就像是拿成品电脑的 CPU 跟零售的 CPU 做对比，没有任何不妥。

## Angular 2 Advantages

## Angular 2 的优点

Let’s start by considering Angular 2’s advantages over React.

我们先看 Angular 相对 React 有哪些优势。

#### **Low Decision Fatigue**

#### **无选择性疲劳**

Since Angular is a framework, it provides significantly more opinions and functionality out of the box. With React, you typically pull a number of other libraries off the shelf to build a real app. You’ll likely want libraries for routing, enforcing unidirectional flows, web API calls, testing, dependency management, and so on. The number of decisions is pretty overwhelming. This is why React has so many starter kits (I’ve [published](https://github.com/coryhouse/react-flux-starter-kit) [two](https://github.com/coryhouse/react-slingshot)).

Angular 是一个完整的框架，本身就提供了比 React 多得多的建议和功能。而要用 React，开发者通常还需要借助别的类库来打造一个真正的应用。比如你可能需要额外的库来处理路由、强制单向流、进行 API 调用、做测试以及管理依赖等等，等等。要做的选择和决定太多了，让人很有压力。这也是为什么 React 有那么多的入门套件（我自己就[写](https://github.com/coryhouse/react-flux-starter-kit)了[两个](https://github.com/coryhouse/react-slingshot)）。

Angular offers more opinions out of the box, which helps you get started more quickly without feeling intimidated by decisions. This enforced consistency also helps new hires feel at home more quickly and makes switching developers between teams more practical.

Angular 提供了更多的建议，所以能够帮助你更快入门，不至于因为要做很多决定而无所适从。这种强制的一致性也能帮助新人更快适应其开发模式，并使得开发者在不同团队间切换更具可行性。

I admire how the Angular core team has embraced TypeScript, which leads to the next advantage…

Angular 核心团队让我非常欣赏的一点是，他们拥抱了 TypeScript，这就造成了另一个优势。

#### TypeScript = Clear Path

#### TypeScript = 阳关大道

Sure, TypeScript isn’t loved by all, but Angular 2’s opinionated take on which flavor of JavaScript to use is a big win. React examples across the web are frustratingly inconsistent — it’s presented in ES5 and ES6 in roughly equal numbers, and it currently offers [three different ways to declare components](http://jamesknelson.com/should-i-use-react-createclass-es6-classes-or-stateless-functional-components/). This creates confusion for newcomers. (Angular also embraces decorators instead of extends — many would consider this a plus as well).

没错，并非所有人都喜欢 TypeScript，但是 Angular 2 毅然决然地选择它确实高出 React 一大截。反观 React，网上的各种示例应用令人咋舌地不一致——ES5 和 ES6 的项目基本上各占一半，而且目前存在[三种不同的组件声明方式](http://jamesknelson.com/should-i-use-react-createclass-es6-classes-or-stateless-functional-components/)。这无疑给初学者造成了困惑。（Angular 还拥抱了装饰器（decorator）而不是继承（extends）——很多人认为这也是个加分项）。

While Angular 2 doesn’t require TypeScript, the Angular core team certainly embraces it and defaults to using TypeScript in documentation. This means related examples and open source projects are more likely to feel familiar and consistent. Angular already provides [clear examples that show how to utilize the TypeScript compiler](https://angular.io/docs/ts/latest/quickstart.html). (though admittedly, [not everyone is embracing TypeScript](http://angularjs.blogspot.com/2015/09/angular-2-survey-results.html) yet, but I suspect shortly after launch it’ll become the de facto standard). This consistency should help avoid the confusion and decision overload that comes with getting started with React.

尽管 Angular 2 并不强制使用 TypeScript，但显然的是，Angular 的核心团队默认在文档中使用 TypeScript。这意味着相关的示例应用和开源项目更有可能保持一致性。Angular 已经提供了[非常清晰的关于如何使用 TypeScript 编译器的例子](https://angular.io/docs/ts/latest/quickstart.html)。（诚然，目前[并非所有人都在拥抱 TypeScript](http://angularjs.blogspot.com/2015/09/angular-2-survey-results.html)，但我有理由相信等到正式发布之后，TypeScript 会成为事实上的标准）。这种一致性应该会帮助初学者避免在学习 React 时遇到的疑惑和选择困难。

#### Reduced Churn

#### 极少的代码变动

2015 was the year of [JavaScript fatigue](https://medium.com/@ericclemmons/javascript-fatigue-48d4011b6fc4#.559iqxb39). React was a key contributor. And since React has yet to reach a 1.0 release, more breaking changes are likely in the future. React’s ecosystem continues to churn at a rapid pace, particularly around the [long list of Flux flavors](https://github.com/kriasoft/react-starter-kit/issues/22) and [routing](https://github.com/rackt/react-router). This means anything you write in React today is likely to feel out of date or require breaking changes when React 1.0 is eventually released.

2015 年是 [JavaScript 疲劳](https://medium.com/@ericclemmons/javascript-fatigue-48d4011b6fc4#.559iqxb39)元年，React 可以说是罪魁祸首。而且 React 尚未发布 1.0，所以未来还可能有很多变数。React 生态圈依旧在快速地变动着，尤其是[各种 Flux 变种](https://github.com/kriasoft/react-starter-kit/issues/22)和[路由](https://github.com/rackt/react-router)。也就是说，你今天用 React 写的所有东西，都有可能在 React 1.0 正式发布后过时，或者必须进行大量的改动。

In contrast, Angular 2 is a careful, methodical reinvention of a mature, comprehensive framework. So Angular is less likely to churn in painful ways after release. And as a full framework, when you choose Angular, you can trust a single team to make careful decisions about the future. In React, it’s your responsibility to herd a bunch of disparate, fast-moving, open-source libraries into a comprehensive whole that plays well together. It’s time-consuming, frustrating, and a never-ending job.

相反，Angular 2 是一个对已经成熟完整框架（Angular 1）的重新发明，而且经过仔细、系统的设计。所以 Angular 不大可能在正式发布后要求已有项目进行痛苦的代码变动。Angular 作为一个完整的框架，你在选择它的时候，也会信任其开发团队，相信他们会认真决定框架的未来。而使用 React，一切都需要你自己负责，你要自己整合一大堆开源类库来打造一个完整的应用，类库之间互不相干且变动频繁。这是一个令人沮丧的耗时工作，而且永远没有尽头。

#### **Broad Tooling Support**

#### **广泛的工具支持**

As you’ll see below, I consider React’s JSX a big win. However, you need to select tooling that supports JSX. React has become so popular that tooling support is rarely a problem today, but new tooling such as IDEs and linters are unlikely to support JSX on day one. Angular 2’s templates store markup in a string or in separate HTML files, so it doesn’t require special tooling support (though it appears tooling to intelligently parse Angular’s string templates is on the way).

后面我会说，我认为 React 的 JSX 是非常耀眼的亮点。然而要使用 JSX，你需要选择支持它的工具。尽管 React 已经足够流行，工具支持不再是什么问题，但诸如 IDE 和 lint 工具等新工具还不大可能很快得到支持。Angular 2 的模版是保存在一个字符串或独立的 HTML 文件中的，所以不要求特殊的工具支持（不过似乎 Angular 字符串模版的智能解析工具已经呼之欲出了）。

#### Web Component Friendly

#### Web Components 友好

Angular 2’s design embraces the web component’s standard. Sheesh, I’m embarrassed I forgot to mention this initially — I recently published a [course on web components](https://www.pluralsight.com/courses/web-components-shadow-dom)! In short, the components that you build in Angular 2 should be much easier to convert into plain, native web components than React’s components. Sure, [browser support is still weak](http://jonrimmer.github.io/are-we-componentized-yet/), but this could be a big win in the long-term.

Angular 2 还拥抱了 Web Component 标准。唉，真尴尬我居然一开始忘记提到这点了——最近我还发布了一门关于[Web Components 课程](https://www.pluralsight.com/courses/web-components-shadow-dom)呢！简单来说，把 Angular 2 组件转换成原生 Web Components 应该会比 React 组件容易得多。固然 Web Components 的[浏览器支持度依然很弱](http://jonrimmer.github.io/are-we-componentized-yet/)，但长期来看，对 Web Components 友好是很大的优势。

Angular’s approach comes with its own set of gotchas, which is a good segue for discussing React’s advantages…

Angular 的实现有其自身的局限和陷阱，这正好让我过渡到对 React 优势的讨论。

