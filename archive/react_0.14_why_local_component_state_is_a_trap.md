# React 0.14: Why Local Component State is a Trap
# React 0.14：揭秘局部组件状态陷阱

> Posted on October 29, 2015 by Safari Books Online & filed under Content - Highlights and Reviews, javascript, programming, Programming & Development, Tech, Web Development.

> by Richard Feldman

> 本文于 2015 年 10 月 29 日 发表在 Safari 在线书店的 [Content - Highlights and Reviews](https://www.safaribooksonline.com/blog/category/content-highlights-and-reviews/)、[javascript](https://www.safaribooksonline.com/blog/category/programming/javascript-programming/)、[programming](https://www.safaribooksonline.com/blog/category/programming/)、[Programming & Development](https://www.safaribooksonline.com/blog/category/programming-development/)、[Tech](https://www.safaribooksonline.com/blog/category/tech/)、[Web Development](https://www.safaribooksonline.com/blog/category/programming-development/web-development-2/) 类目下。

> 撰稿人 Richard Feldman

Richard Feldman is a functional programmer who specializes in pushing the limits of browser-based UIs.  He works at NoRedInk building education tools using Elm and React. He is also one of the authors of the upcoming second edition of “Developing a React Edge: The Javascript Library for User Interfaces” from Bleeding Edge Press. The second edition of that book will be available in Safari as an early release title in the near future.

[Richard Feldman](https://twitter.com/rtfeldman) 酷爱函数式编程，专注于突破浏览器端 UI 的限制。他就职于 [NoRedInk](https://www.noredink.com/)，并且使用 Elm 和 React 打造了该教育系统。他同样是即将由 [Bleeding Edge Press](http://bleedingedgepress.com/)出版的第二版「[Developing a React Edge: The Javascript Library for User Interfaces](https://www.safaribooksonline.com/library/view/developing-a-react/9781939902122/)」的作者之一。该书的第二版很快就会出现在 Safari 上。

For years, in the world of JavaScript UI programming, it’s been normal to divide our application state among several components, with each component owning its own local state. With the architectures of the past this was essentially unavoidable, but the rise of React and declarative rendering has opened the door for a more powerful paradigm to take hold: the single state atom.

一直以来，在 JavaScript UI 编程世界中，在多个组件中切分应用的状态都是很寻常的事情，也就是说每个组件拥有自己的状态。在过去的架构体系中这也基本是不可避免的，不过 React 和声明式渲染的出现带来了一个更加健壮的范式：**单一原子状态**。

Now that more and more JavaScript libraries are abandoning local component state in favor of the single state atom, certain questions have begun to arise. Can we control which portions of the single state atom nested components can access? Is it possible to make reusable components without giving them ownership over their own state? Could we implement an entire application like this using React 0.14’s functional stateless components?

现在越来越多的 JavaScript 库抛弃了局部组件状态转而偏爱**单一原子状态**，当然也随之带来了很多问题。我们如何控制嵌套组件让其只能访问单一原子状态的特定部分？是否可以在不借助组件内部状态的情况下重用组件？我们能不能通过 React 0.14 的无状态函数式组件独立完成这样的一个应用？

Perhaps most importantly: now that we have a strong ecosystem that can support a single state atom, has local component state gone from a best practice to an alluring trap?

或许最重要的是：现在已经有足够健康的生态来支撑单一原子状态应用，是否局部组件状态真的从一个最佳实践沦落为一个诱人的陷阱了呢？

## Sources of Truth

## 单一数据源

How many databases does a typical Web App use to store its application state? Almost always the answer is “exactly one.” Many employ special-purpose databases for queuing and caching (e.g. Redis, Memcache), but relatively few Web Apps grow to the point where it becomes appealing to divide application state across multiple databases.

一个常规的 Web 应用到底需要多少个数据库来存储它的状态？答案几乎总是『仅需一个』。很多特殊用途的数据库用于消息队列和缓存（比如，Redis、Memcache），不过几乎很少有 Web 应用庞大到需要切分应用的状态跨数据库存储的。

There’s no technical barrier to carving up a database, so what if we did? Let’s suppose we put our user account information in one MySQL database, their roles and permissions in a second MySQL database, and recent activity in a third MySQL database.

切分数据库并没有什么技术上的障碍，如果我们做了会怎样呢？假设我们把用户账号信息存储在一个 MySQL 数据库中，他们的角色和权限存储在另一个 MySQL 数据库，『最近动态』又存储在一个单独的 MySQL 数据库。

Doing this would have some immediate upsides. We could be certain that the data for each concern was not mixing with the other concerns. When writing queries for each, they would necessarily be small and simple, because it would take extra coordination effort to loop in the others. There’s a certain conceptual appeal to this strict separation.

这样做有一个很明显的好处。我们可以保证每一部分的数据都不会与其他部分混在一起。对每一个进行查询操作，都很轻量和简洁，因为不需要在其他的数据中花费时间索引。在某种概念下，这种严格的隔离很受推崇。

However, there would also be drawbacks. For one, it would introduce synchronization bugs. What happens if we delete a user account right when something else is querying that user’s recent activity from the other database? Someone will get back some crazy data unless we can make that update an atomic transaction—which would be trivial if those values lived in the same database, but which would be more difficult and error-prone with the state distributed like this.

然而，它的弊端也很明显。其一，它会引出同步性的问题。比如，在我们删除一个用户账号的同时有人在另一个数据库中查询该用户的『最近动态』会发生什么？他很可能得到一些很糟糕的数据，除非我们能够做到原子事务更新。如果这些值都在同一个数据库中，这可能并没有什么，不过当状态如此分布的时候会更加的困难并且容易出错。

Another drawback is that backups get way harder. Instead of backing up a single database and being able to restore it on a whim if things go wrong, we would instead have a pile of databases to back up and restore. What if we correctly restore almost all of them, but mess one up? Now our application is in a crazy state – and one we may not realize is crazy until it’s hung around long enough for nonsensical data to propagate and create more broken data.

另外一个弊端是备份变得更难。不仅仅是备份一个单独的数据库，在发生故障时直接恢复它。取而代之的是需要备份和恢复很多数据库。如果我们几乎恢复了所有的数据库，而仅有一个没有恢复又回怎样呢？那么应用的状态将会混乱，并且可能直到它宕机的时间足够长并且产生了很多糟糕的数据污染了其他的数据时，我们才会意识到。

These problems stem from the fact that we’ve taken data that is innately coupled from a business perspective—user accounts, their permissions, and their recent activity—and decoupled it architecturally. Whether we like it or not, these pieces of data depend on one another, and dividing up their storage will make it harder to work with them in tandem.

这些问题来源于我们的数据在业务上是天然地耦合在一起的：用户账号、他们的权限以及他们的『最近动态』，而我们又需要在架构上将它们解耦。你喜欢或不喜欢，那些数据都依赖于另一个，而将它们分开存储只会增加串联它们的难度。

We encounter similar challenges in front-end programming when each component has its own local state. We all know “Undo/Redo” makes for a nice UX, but just like “backup/restore,” it’s difficult and error-prone to implement when your state is spread across multiple sources of truth. It’s also easier for components to incorrectly synchronize with one another, leading to crazy application states such as the infuriating message notification component that displays “1 unread message” even as the message list component insists that all messages have been read.

我们在编写前端代码的过程中，当每个组件都有自己的局部状态时，遇到了类似的的挑战。我们都知道『撤销/恢复』成就了很棒的用户体验，就像数据『备份/恢复』一样，想要在应用状态在多个数据源中传递是很难的而且很容易出错。也很容易出现组件之间错误地传递，导致应用状态的混乱。比如一个很臭名昭著的案例，当一个消息通知组件显示『一个未读消息』时，而实际上消息列表组件认为所有的消息都是已读状态。

A single source of truth eliminates these synchronization problems at the source. The rise of immutable data and declarative rendering, popularized by React, has set the industry on a path to getting rid of these problems once and for all. More and more front-end architectures are embracing the notion of representing application state as an atomic single source of truth, and are seeing the corresponding benefits that back-end databases have enjoyed for years.

**单一的数据来源**完美解决了数据源上的同步问题。伴随 React 而来的『不可变数据』和『声明式渲染』从根本上解决了这些问题。越来越多的前端架构正在拥抱使用单一数据来源来表示应用的状态，并且得到了如后端数据库一样的待遇。

Mozilla’s James Long has embraced the single state atom for Firefox’s developer tools by using Dan Abramov’s Redux architecture for React, which allows hot reloading with time travel. David Nolen’s Om library for ClojureScript uses a single state atom to enable things like trivial undo/redo, which Sean Grove has reaped the benefits of in easily adding undo/redo functionality to Glint. At NoRedInk we have been happily using Elm with the Elm Architecture, around whose single state atom Laszlo Pandy built the original Time-Traveling Debugger.

来自 Mozilla 的 James Long 已经使用 React 借助 Dan Abramov 的 [Redux](http://redux.js.org/) 架构在[开发 Firefox 开发者工具中拥抱单一原子状态](https://www.youtube.com/watch?v=qUlRpybs7_c)，实现了[按照时序热加载的特性](https://www.youtube.com/watch?v=xsSnOQynTHs)。David Nolen 为 ClojureScript 而写的 [Om 库](https://github.com/omcljs/om)借助[单一原子状态](https://twitter.com/swannodette/status/656495791356420096)让逻辑像『撤销/恢复』一样简单，Sean Grove 也[尝到了甜头](https://twitter.com/sgrove/status/504653260146229248)，在 Glint 中很容易地添加了『撤销/恢复』功能。在 [NoRedInk](https://www.noredink.com/) 我们高兴地用着 [Elm 架构](https://github.com/evancz/elm-architecture-tutorial/)，在这些单一原子状态的基础上 [Laszlo Pandy](https://github.com/laszlopandy) 构建了最原始的 [Time-Traveling 调试器](http://debug.elm-lang.org/)。

Although the benefits of a single state atom are becoming clearer as more and more front-end architectures move in that direction, there are still questions in the community around how to accomplish familiar tasks in this new world. How can we maintain the conceptual benefit of local component state—the fine-grained control over which pieces of code can access which parts of state—when state has only one owner? And how do we build reusable components without giving each component ownership over its own state?

尽管在前端架构的动向上看单一原子状态的好处越来越清晰，但社区中关于这一新领域中存在问题的讨论一直没有停止过。我们如何维持局部组件状态的好处，精确地控制那一段代码可以访问哪个状态？以及我们该如何在不让每个组件拥有自身状态的情况下创建可复用的组件？

As with many new technologies, the answer turns out to be surprisingly simple.

随着新技术的到来，这些问题也变得出奇的简单。

## Reusable Stateless Components

## 可复用的无状态组件

Consider an accordion widget. When the user clicks a particular section, that section should expand if it was collapsed, and collapse if it was expanded.

考虑一个折叠组件。当用户点击某一特定的区域，如果该区域处于折叠状态它应该展开，反之亦然。

Clearly there is state involved there. We need to record which sections are expanded and collapsed—in order to tell how to render the accordion, as well as what to do when a user clicks a given section. How do we do this without giving the accordion some local state to call its own?

很明显，这里包含了状态。我们需要记录哪些区域处于展开以及折叠状态，以便渲染组件，以及当用户点击指定区域时应该如何做。如果折叠组件不能调用自身的状态，我们该如何做呢？

On the back-end, we see reusable libraries for common state-based tasks like user authentication. However, these libraries do not come prepackaged with their own MySQL databases; instead, they have an API which allows the application to use them in conjunction with an existing database. The application can store the user data however it pleases, because the authentication library is designed to delegate state storage rather than prepackaging it.

在后端，经常会有很多可复用的库用于基于状态的任务，比如用户授权。然而，这些库并不会与一个自己的 MySQL 数据库打包在一起；相反，应用通常会结合现有的数据库通过一个 API 来使用它们，因为用于授权的库通常被设计为委托状态存储而不是将状态打包起来。

This idea works just as well on the front-end.

在前端，这一思想同样适用。

Our accordion needs state for two things: rendering and handling user actions. It needs to know the current state (namely, which sections are expanded and collapsed) in order to render, and it needs to change that state (namely, changing which sections are expanded and collapsed) when the user clicks.

我们的折叠组件需要状态做两件事情：渲染以及处理用户的动作。它需要知道当前状态（即哪部分被折叠和展开）以便渲染，并且在用户点击时它需要改变状态（即改变展开的和折叠的部分）。

Historically, the common way to meet these needs was to give it a chunk of local state, have it read from that to determine how to render, and modify it when the user clicks.

过去，处理这类需求最常见的方式就是给它一系列的局部状态，然后在渲染时去读取它，在用户点击时修改它。

With a single atomic state, instead we delegate: the accordion’s API accepts as an argument the expanded/collapsed data necessary to render, as well as state-updating functions (“expandSection(),” “collapseSection(),” etc.) that it can invoke in response to user input. So when the user clicks an accordion section and the accordion wants it to be expanded, it invokes the expandSection() function it was passed, which in turn updates the single state atom as appropriate.

在单一原子状态中，存在委托机制：折叠组件的 API 接收用于渲染的所必须的展开或折叠数据作为参数，以及状态更新函数（`expandSection()`、`collapseSection()`等）用于响应用户的输入。所以当用户点击折叠区域并且该区域要展开时，它会调用给它传递的 `expandSection()` 函数，随之更新折叠组件的单一原子状态原子。

Not only does this delegation approach scale, it provides a greater degree of control over which components can impact which parts of state. Parent components can provide children with “read access” (providing portions of current state as arguments) but not “write access” (providing functions to the child which can update specifically-chosen parts of the single state atom) or vice versa. You can even provide access selectively depending on the current state!

不仅仅是这种委派的方式，它还提供了更加强大的方式用于控制哪些组件能够影响状态的哪个部分。父组件可以为子组件提供『读权限』（提供当前状态的一部分作为参数）而不提供『写权限』（为子组件提供了函数可用于更新单一原子状态的指定部分），反过来也一样。你甚至可以根据当前状态选择性地提供访问权限。

How would this architecture impact what we can do for our end users?

这种架构会为我们的终端用户带来哪些不一样的东西？

Suppose we want to implement a UX improvement: if the user refreshes the page, accordions are still expanded and collapsed as they were when the user left. We can easily persist the necessary information in localStorage, but how hard is this to implement using local state versus a single state atom?

假设我们想要实现一个用户体验提升：当用户刷新页面时，折叠组件依旧处于用户操作后的展开或折叠状态。我们可以轻易地使用 `localStorage` 来持久化必要的信息。不过相对于单一原子状态而言，使用局部状态来实现的难点又在哪里呢？

With local component state, every time the user clicks an accordion, we must remember to update localStorage to persist it. If we forget to, even once, our states will be out of sync, and we could end up in a crazy state where things are displaying in a way that should not be possible through normal user interaction. Those bugs are not fun to track down.

使用局部组件状态时，每次用户点击折叠组件时，我们一定要去更新 `localStorage` 以持久化数据。如果我们忘记了，即便只有一次，状态就会不同步。最后我们会得到一个混乱的状态，页面所展示的东西和用户的交互不一致。而且这种问题很难追踪到。

With a single state atom, and an accordion that delegates state ownership to it, this is the easiest thing in the world. Every time our state changes, we save the whole atom to localStorage. Done. It’s never out of sync, and our application is architected such that all we have to do is restore that one atom and all of our UI components Just Work, reusable accordions and all.

一旦有了单一原子状态，一个折叠动作可以从状态的拥有者委派给它，这是很简单的事情。每次状态发生改变，我们将整个原子都存到 `localStorage` 中。这样绝不会出现不同步的问题，并且我们的应用架构仅需要维护一个原子，所有的 UI 组件都会工作，包括可复用的折叠组件等。

This is exactly the approach we took at NoRedInk when we made a reusable accordion in Elm. Its API is designed to delegate state management to a single state atom rather than owning its own local state, giving us the best of both worlds: reusable components and a single state atom.

这也的确是我们在 [NoRedInk](https://www.noredink.com/) 使用 [Elm 创建可复用的折叠组件](https://github.com/NoRedInk/elm-html-widgets/blob/master/src/Widget/Accordion.elm)所使用的方式。

## Example: Stateless Functional Components in React 0.14

## 示例：React 0.14 中的无状态函数式组件

The release of React 0.14 introduced stateless functional components. From the release notes:

最新发布的 React 0.14 提出了[无状态函数式组件](https://facebook.github.io/react/blog/2015/10/07/react-v0.14.html#stateless-functional-components)。它指出：

> In idiomatic React code, most of the components you write will be stateless, simply composing other components…This pattern is designed to encourage the creation of these simple components that should comprise large portions of your apps.
> 在通常的 React 代码中，绝大多数组件都被写成无状态、简单地组合成其他的组件等等。这种通过创建多个简单组件然后组合成一个大应用的设计模式被提倡。

Here is the example of such a component, from that post:
下面是那篇文章中的一个类似的例子：

```
// A functional component using an ES2015 (ES6) arrow function:
// 一个使用 ES2015 (ES6)箭头函数写成的函数式组件
var Aquarium = (props) => {
  var fish = getFish(props.species);
  return {fish};
};
```
Let’s run with that example. What if we want to let user flip a light switch on and off within our fish tank? We could implement that using local component state, with each tank owning whether its light was turned on or off, but let’s try delegating to a single state atom instead.

我们可以运行一下这个示例。用户打开或关闭鱼缸灯光的功能该如何实现呢？我们可以使用局部组件状态，每一个鱼缸都会有一个状态来表示灯的开关状态，但是让我们用委派单一组件状态来试试。

Here’s how that would look in jsFiddle.

下面在 [jsFiddle](https://jsfiddle.net/v0wtjhoc/) 中展示了该示例：

Here we have one traditional React component at the root of the hierarchy, called App. Its job is solely to hold our app’s single state atom, and its children are responsible for updating that state as appropriate.

示例中有一个典型的 React 组件位于组件树的根部，我们称之为 `App`。它的职责仅仅是控制应用的单一原子状态，它的子组件负责按需更新状态。

Note that both the Aquarium and its child Tank components are stateless; you can be sure of this, because they are using 0.14’s stateless functional component style—which does not even support local component state! Nevertheless, the Tank class has no problem updating the application state as necessary, because its parent has provided it with a setLightOn function which allows it to do so selectively.

注意，`Aquarium` 和它的子组件 `Tank` 都是无状态的；我们可以明确这一点，因为它们使用了 0.14 的无状态函数式组件的方式，甚至不支持局部组件状态！不要灰心，鱼缸这个类在需要的时候更新整个应用的状态并没有问题，因为它的父组件提供了 `setLightOn` 方法用于选择性地更新。

Here we can see that the conceptual benefit of local state—fine-grained control over which parts of state components are permitted to access—can be achieved by simple function passing instead, without sacrificing the benefits of the single state atom. Tank does not have access to the entire application state; it only knows about its one single tank, and all it can do to affect that tank is to switch its lights on or off.

这里我们可以看到局部状态的概念上的好处，那就是可以精细地控制哪些组件可以访问哪个状态。而这一点可以通过简单的函数传递来实现，而不必去牺牲单一原子状态的好处。`Tank` 不需要访问整个应用的状态；仅仅需要知道它有一个鱼缸，能对鱼缸做的就是打开或关闭它的照明。

In short, we have avoided mixing its concerns with the rest of our application state simply by being selective in what we pass it!

简而言之，我们简单地通过选择性地传递参数避免了将组件的内容与应用状态的其他部分混合在一起！

This may seem like an unusual way of doing things, but remember what this gets us: now if we ever want to serialize our entire application state in order to store it somewhere, or to roll back to a previous state, we can do so at the drop of a hat. Making transactional updates across different parts of the application state is now trivial, so race conditions are much easier to avoid.

这可能事一种不寻常的做事方式，不过请记住这给我们带来了什么：如果我们想要序列化整个应用的状态并将它存到某个地方，或者切换到前一个状态，都将是不费吹灰之力。跨应用状态的不同部分来更新事务也就变得微不足道了，因而争用状态也很容易避免。

For more on stateless components, the second edition of “Developing a React Edge” discusses them as well as other changes introduced in React 0.14. I also expect single state atoms to be a recurring theme at the Reactive2015 conference in Bratislava, given the number of talks on technologies based around single state atoms, such as Redux, Om, and Elm.

对大多数无状态组件来讲，第二版的『Developing a React Edge』会详细的讨论，当然还包括 React 0.14 中的新特性。我也同样希望单一原子状态会成为在布拉迪斯拉发市举办的 [Reactive2015](https://reactive2015.com/) 中的一个议题，会有很多关于单一原子状态的演讲，包括 [Redux](http://redux.js.org/)，[Om](https://github.com/omcljs/om) 以及 [Elm](https://github.com/evancz/elm-architecture-tutorial/)。

## Takeaways

## 总结

Despite the conceptual allure of local component state, the benefits of the single state atom are too strong to ignore:

尽管局部组件状态很诱人，而单一原子状态的好处绝对强大到不可拒绝：

- A single source of truth makes it easier to perform transactional updates and dodge race conditions.
- Serialization for quick persistence and retrieval of state no longer requires a massive coordination effort.
- Debuggers can reliably support time travel across past UI states.
- Undo/Redo becomes a feature whose implementation is easily within reach.


- 单一数据源使得操作事务更新更加简单并且避开了争用状态
- 快速持久化和检索状态的序列，不再需要大量的判断逻辑
- 调试器能够可靠地跨整个 UI 状态按照时序调试
- 『撤销/恢复』成为一个可以轻易实现的特性

Considering we can use delegation and function passing to build reusable stateless components with even more fine-grained state permissions as what we had with stateful components, even the conceptual benefit of local component state has an easy replacement in the world of the single state atom.

我们能够使用委派和传递函数来创建一个可复用的无状态组件，可以像有状态的组件那样精细地控制状态的访问权限，即便是局部组件状态的好处也可以轻易地移植到单一原子状态的领域中。

So here’s to the next evolution of front-end development, where we avoid the trap of local component state and start reaping the benefits back-end programmers have enjoyed for years.

所以，它很可能就是前端开发界的下一次革命，我们避免了局部组建状态的陷阱并且尝到了后端开发者一直在享用的甜头。
