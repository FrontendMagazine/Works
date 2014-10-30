#How to Write Highly Scalable and Maintainable JavaScript: Coupling

#编写高扩展并且可维护的 JavaScript：耦合

##Introduction
In the previous chapter of this series, we discussed modular JavaScript. We looked at native JavaScript patterns and some of the prominent modular specifications and formats such as AMD, CommonJS, etc.

##简介
在这个系列的前一章节中，讨论了模块化的 JavaScript。我们看到了原生的 JavaScript 模式以及几个重要的模块化规范和格式，比如：AMD，CommonJS 等等。

In this final chapter, we will examine ways to reduce coupling between JavaScript modules.

在最后一个章节，我们将总结出降低 JavaSript 模块耦合度的几种方式。

##Coupling
Coupling between modules occurs when one module directly references another module. In other words, one module “knows” about another module.

##耦合
当一个模块直接引用另一个模块时会出现模块间的耦合。换句话说，一个模块“知道”关于另一个模块。

For this chapter, assume that we are building a web application that allows people to place food delivery orders. Each time the user places an order, the application creates the order and then sends a confirmation to the user that includes the estimated time of delivery. The user can check the status of the order, or cancel at any time.

在这一节，假定我们要去创建一个 web 应用提供给用户食品发放订单的功能。每次用户下单，应用创建订单号然后给用户发放一个确认信息，包含了预计发货时间。用户可以检查这个订单的状态，或者随时取消订单。

One modular approach to implement this scenario is to create a module that handles orders and another that handles deliveries. The ordering module may have functions to create, update, and delete orders, while the deliveries module may have functions to estimate delivery time, begin delivery, complete delivery, and so on.

一个模块化的方法可以解决这个场景，创建一个模块来维护订单，再创建一个用来维护发货。订单模块可能有这些方法：创建，更新以及删除订单，发货模块可能有这些方法：预计发货时间，开始发货，结束发货等等。

Let’s look at an example of this in action. Note that the code in this post is written primarily for clarity.

看一下相关的例子。注意一下，出于清晰、明确的考虑，文中的代码只写了主要的部分。

Example: Create Order
示例：创建顺序

// Order module definition
// 订单模块定义
var orderModule = (function() {
  var module = {},
    deliveries = myApp.deliveryModule;

  module.createOrder = function(orderData) {
    var orderResult;

    orderResult = // Code to actually create the order
    orderResult.estimatedDeliveryTime = deliveries.getDeliveryTime(orderData);

    return orderResult;
  };

  return module;
})();

The order and delivery modules shown are tightly coupled. For the order module to get the estimated delivery time, it has to “know” about the delivery module, and then call the appropriate delivery module’s API as required.

订单和发货模块表现出重度的耦合。因为订单模块要获得预计发货时间，它必须“知道”发货模块相关内容，然后按照需要去调用恰当的发货模块 API。

There are many reasons to avoid tightly coupling your modules. We discussed some of these in the first chapter of this series. Recall that one of the goals when creating highly scalable and maintainable JavaScript applications is that any module can be easily swapped out at any time for a different module. Reusability is also a major reason to minimize coupling. We would ideally like to maximize code reuse and the ability to test modules independently.

有很多理由要去避免模块间的重度耦合。在这个系列的第一节中，我们已经讨论了一些。回忆一下，创建高度扩展并且可维护的 JavaScript 应用的一个目标就是任意模块都可以随时的轻易换成另外一个模块。可重用性也是降低耦合的一个重要的原因。我们很理想的要去完成最大化代码重用以及独立的模块测试。

Another goal was that there should not be a single point of failure anywhere in the application. Suppose that something went wrong in our call to get the estimated delivery time, this may break the entire application, or at least the successful completion of the ordering process. We would likely prefer that the order is still placed and the application continues to operate, even if we are unable to temporarily provide a delivery time estimate to the customer.

另一个目标就是在应用中任何地方都不能有单点故障。设想一下，在我们调用接口获取预计发货时间时出现了问题，导致整个应用无法运行，或者至少是已经成功结束订单的进程。我们可能想要订单依旧存在，并且应用可以继续运行，尽管我们没办法临时为消费者提供一个发货时间的预算。

Now let’s examine some ways to reduce coupling between modules.

现在，看一下可以降低模块间耦合的几种方式。

##Patterns to Reduce Coupling
##降低耦合的模式
Tightly coupled modules have a variety of disadvantages as noted in this series. Luckily there are ways we can reduce coupling throughout our code. There are many patterns used to achieve loose coupling between JavaScript modules, and often are a variation of the so-called observer pattern. One such variation is referred to as the Pub/Sub or Publish/Subscribe pattern.

模块间重度耦合有很多的弊端，在这个系列中已经说明。幸运的是，有几种方式可以用来降低我们代码中的耦合度。有很多模式可被用于实现 JavaScript 模块之间的松散耦合，通常都是所谓的观察者模式的一个变体。我们要说的就是其中一个变体被称为 Pub/Sub 或者发布/订阅模式。

In some cases the observer registers itself with the event emitter directly in order to be notified whenever a certain event occurs. The downside to this approach is that an observer “knows” about the event emitter object and what observables or events to observe through the registration process.

某些情况下，观察者直接为自己注册了事件触发器，当特定的事件被触发时就可以被通知到。这个方法的弊端就是观察者“知道”关于事件触发对象以及哪些观测值或者事件的观察通过注册进程。

We can do better than this. There are many versions of the Pub/Sub pattern that involve the use of a mediator object, which helps to minimize coupling between modules even further. A mediator object is an object that isolates the publisher from the subscriber. Addy Osmani made an excellent analogy in one of his articles in which he related the mediator to a flight control tower in airplane communications. Airplanes never communicate directly with each other. Airplanes instead only provide information to, and receive information from the tower. The airplanes therefore do not “know” about one another without information from the tower.

我们可以做的更好。有很多版本的 Pub/Sub 模式加入了中介者对象，实现了更深层次的最小化模块之间的耦合。一个中介者对象将发布者从订阅者中隔离出来。Addy Osmani 在他的一篇文章中做了一个绝妙的对比，把中介者比作航空通信中的飞行器控制塔。飞机之间不会直接的相互通信。只会向控制塔发送信息以及从控制塔接收信息。因此，飞机不会“知道”关于另外一个飞机，除了从控制塔获得信息外。

There are a variety of libraries available to implement Pub/Sub-type patterns. We will use PubSubJS from Morgan Roderick, which is a topic-based JavaScript Pub/Sub library, and can be found here. Topic-based simply means that there are topics that a module can either subscribe to, publish to, or both. A module is also able to unsubscribe from a topic if needed.

有很多的类库被用于实现类 Pub/Sub 模式。我们使用了 Morgan Roderick 的 PubSubJS，是一个基于主题的 JavaScript Pub/Sub 类库，可以在[这里](https://github.com/mroderick/PubSubJS)看到。基于出题意味着只要有了主题，一个模块就可以订阅，发布，或两者。模块也可以按照需要从一个主题中取消订阅。


We’ll now revisit our order example from before, but instead implement it using PubSubJS. Here is the code:

现在重新看一下前面的订单实例，不同的是这次使用了PubSubJS来执行它。下面是代码：

Example: Estimated delivery time using Pub/Sub pattern

实例：使用 Pub/Sub 模式估计发货时间

document.addEventListener("DOMContentLoaded", function(event) {
  var orderModule = (function() {
    var orders = {},
      EST_DELIVERY = 'current estimated delivery time',
      estimatedDeliveryTime;

    PubSub.subscribe(EST_DELIVERY, function(msg, data) {
      console.log(msg);
      estimatedDeliveryTime = data;
    });

    return orders;
  })();

  var deliveryModule = (function() {
    var deliveries = {},
      EST_DELIVERY = 'current estimated delivery time';

    deliveries.getEstimatedDeliveryTime = function() {
      var estimatedDeliveryTime = 1; // Hard-coded to 1 hour, but likely an API call.

      PubSub.publish(EST_DELIVERY, estimatedDeliveryTime);
    };

    return deliveries;
  })();

  deliveryModule.getEstimatedDeliveryTime();
});
For simplicity, all of the required code and both module definitions are included in the same document ready event handler. Here we define two modules; one for orders and the other for deliveries respectively.

出于简洁，要用到的所有代码包括模块定义都写在同一个文档加载的事件监听函数当中。这里我们定义了两个模块；一个用于订单，两一个用于发货。

Notice that we have defined a topic with the EST_DELIVERY constant called ‘current estimated delivery time’. Any module can publish and/or subscribe to this topic without ‘knowing’ of each other’s existence. In this case we have directly called the getEstimatedDeliveryTime method of the delivery module.

注意我们定义了一个主题，EST_DELIVERY 常量用来表示“当前预计发货时间”。任何模块都可以同时或者单独订阅、发布这个主题，不必“知道”彼此的存在。在这种情况下我们直接称 getEstimatedDeliveryTime 方法为发货模块。

By calling this method, the delivery module is used to figure out the current estimated delivery wait time, which is likely based on the number of delivery orders currently in the queue, and then publishes the expected estimated delivery time to the EST_DELIVERY topic. At this point, any subscriber to this topic will be notified of the update, and will also receive the latest delivery time estimate.

通过调用这个方法，发货模块可用于计算出当前预计发货等待时间，可能会基于当前的队列中等待发货订单的个数，然后发布者计算出预计发货时间给 EST_DELIVERY 主题。此刻，这个主题的任何一个发布者都会被通知到这个更新，也会收到最新的发货时间估算。

The order module subscribes to this topic. It will therefore always have the most up-to-date estimated delivery wait time to use when orders are placed, but without actually having to “know” about the delivery module and its methods, events, etc. We can even implement a mechanism that regularly updates and publishes the estimated delivery wait time to this topic on regular intervals, or each time an order is placed and the queue is changed.

订单模块订阅了这个主题。因此，当一个新的订单发出时它总会获得最近更新的预计发货等待时间，但是不需要精确的“知道”关于发货模块和它的方法，事件等等。我们甚至可以实现一种机制，在固定的时间间隔内有规律的更新然后发布预计发货等待时间给这个主题，或者每当有新的订单以及队列改变时。

This is just one example of the many ways to use the Publish/Subscribe pattern to your advantage. There are however some potential downsides to this level of loose coupling, and I encourage you to read some of the resources noted in the references section for further discussion on the matter.

这只是使用 Publish/Subscribe 模式的多种方式中的一个实例。当然，在这个松散耦合的层面上也存在一些潜在的缺点。鼓励你看一些资源，已经记录在引用部分中，可以在这个事情上做更深的讨论。

##Summary

##总结

Throughout this series we have discussed ways to write highly scalable and 
maintainable JavaScript code, as well as covered some industry best practices and patterns employed to achieve this goal.

通过这个系列我们讨论了如何编写高度扩展并且可维护的 JavaScript 代码。同时，涵盖了行业内最佳实践和模式用来实现这一目标。

We also discussed the “wild west” syndrome and why it is so important to consider proper JavaScript code design, architecture, organization, and coupling. We considered patterns such as namespacing and modules, as well as the various ways to implement modules using plain JavaScript or by using a specification format such as AMD, CommonJS, and ECMAScript Harmony.

我们也讨论了“狂野西部”综合征，以及为什么去考虑恰当的 JavaScript 代码设计，体系，组织以及耦合是如此的重要。我们讨论的模式像命名空间和模块，以及各种方式去实践模块，使用纯 JavaScript 或者通过一个规范格式像 AMD，CommonJS 以及  ECMAScript Harmony。

In this final chapter we discussed coupling between modules and ways to minimize it. Patterns such as the observer and/or a variation of Publish/Subscribe are excellent choices for reducing coupling. This is very important for promoting code reuse, independent testability, interchangeability, and protection against a single point of failure.

章节的末尾，我们讨论模块间的耦合以及最小化耦合的方式。像观察者模式以及变体的发布者/订阅者一定是降低耦合的最佳选择。这对于提升代码重用，测试上独立，可互换性以及防止单点故障都很重要。

When the above goals and techniques are considered and executed properly, your JavaScript code can most certainly be highly scalable, maintainable, usable, reusable, sustainable, extensible, and so on.

当上面的目标和方法被考虑到并且恰当的被执行时，你的 JavaScript 代码无疑是高度扩展的，可维护的，可用的，可重用的，可持续性的，可扩展的等。

Cheers and happy coding!

欢呼并且快乐的编码！

原文：[How to Write Highly Scalable and Maintainable JavaScript: Coupling](http://www.innoarchitech.com/scalable-maintainable-javascript-coupling)
