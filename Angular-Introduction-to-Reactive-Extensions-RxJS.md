## Angular — Introduction to Reactive Extensions (RxJS)

How to use observable sequences in AngularJS

![https://cdn-images-1.medium.com/freeze/max/30/1*5ouFuP2QXBfn9SDAxvErJQ.jpeg?q=20](https://cdn-images-1.medium.com/freeze/max/30/1*5ouFuP2QXBfn9SDAxvErJQ.jpeg?q=20)![https://cdn-images-1.medium.com/max/800/1*5ouFuP2QXBfn9SDAxvErJQ.jpeg](https://cdn-images-1.medium.com/max/800/1*5ouFuP2QXBfn9SDAxvErJQ.jpeg)

[Pt — A code experiment on point, form, and space](http://williamngan.github.io/pt/demo/index.html?name=vector.field).

Reactive Extensions for JavaScript (RxJS) is a reactive streams library that allows you to work with *asynchronous data streams*. RxJS can be used both in the browser or in the server-side using Node.js.

In this post we are going to introduce RxJS basic concepts and how we can use them with AngularJS.

> **Flux**is another popular Data Architecture. We explored how to use Flux with Angular in a previous post.

[https://medium.com/p/6a835c9c0656](https://medium.com/p/6a835c9c0656)[**Angular — building Apps using Flux**
*How to integrate Flux in your AngularJS Applications*medium.com](https://medium.com/p/6a835c9c0656)

### What exactly are asynchronous data streams?

Let’s take each word separately and put it into context.

- ***Asynchronous***, in JavaScript means we can call a function and register a *callback*to be notified when results are available, so we can continue with execution and avoid the Web Page from being unresponsive. This is used for ajax calls, DOM-events, Promises, WebWorkers and WebSockets.
- ***Data***, raw information in the form of JavaScript data types as: Number, String, Objects (Arrays, Sets, Maps).
- ***Streams***, sequences of data made available over time. As an example, opposed to Arrays you don’t need all the information to be present in order to start using them.

*Asynchronous data streams* are not new. They have been around since Unix systems, and come in different flavours and names: streams (Node.js), pipes (Unix) or async pipes (Angular 2).

### Observable sequences

In RxJS, you represent *asynchronous data streams* using observable sequences or also just called observables. Observables are very flexible and can be used using push or pull patterns.

- When using the ***push pattern****,*we **subscribe** to the source stream and react to new data as soon as is made available (emitted).
- When using the ***pull pattern****,*we are using the same operations but synchronously. This happens when using Arrays, Generators or Iterables.

> 🐒 Observables sequences allows us to use both**push**and**pull**patterns

#### Basic example

Let’s start using a simple observable sequence within an Angular Controller. See it running in this [Plunker](http://embed.plnkr.co/ZRxNQfB0DEuUNNlhlScU/preview).

In this example, we used an ***Observable*** (rx.Observable) followed by a chain of **operators** ending with a call to **subscribe**.

The first operator waits for 1 second and emits values (indefinitely) starting with 0 ([interval](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/interval.md), delay is set in ms). The second operator takes the first 3 items ([take](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/take.md)). The third operator, a helper method, let’s us set the *counter* for each value using the current scope ([safeApply](https://github.com/Reactive-Extensions/rx.angular.js/blob/master/src/safeApply.js) uses $scope.$apply only when necessary). Finally, a call to [subscribe](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/subscribe.md) triggers the execution.

We can also use an ASCII [Marble Diagram](http://rxmarbles.com/) to describe it:

Note from the diagram above that each operator creates a new stream that we could also reference separately.

> 🐒Observables programming has two separate stages: setup and execution.

### Observables and Operators

RxJS combines **Observables**and**Operators** so we can subscribe to streams and react to changes using composable operations. Let’s introduce these concepts in more detail.

#### Observables/Observers

Observables get their name from the [Observer](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#observerpatternjavascript) design pattern. The ***Observable*** sends notifications while the ***Observer*** receives them. Let’s create a simple Observer.

You can pass in your observer when calling ***subscribe***or by passing *onNext*, *onError*and *onCompleted*callbacks. These are their behaviours:

- *onNext*, called for each element in the observable sequence.
- *onError*, called only once in case of an error.
- *onCompleted*, called only once when the stream finishes.

If we want to stop listening to changes, we can ***unsubscribe*** by getting a reference and clean up on $destroy.

#### Operators

We have seen some already. These are the main categories: creation, conversion, combine, functional, mathematical, time, exceptions, miscellaneous, selection and primitives. You can explore them [here](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/gettingstarted/categories.md).

A list of the most common: [merge](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/merge.md)[@](http://rxmarbles.com/#merge), [concat](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/concat.md)[@](http://rxmarbles.com/#concat), [defer](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/defer.md), [do](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/do.md), [map](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/select.md)[@](http://rxmarbles.com/#map), [flatMapLatest](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/flatmaplatest.md), [fromPromise](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/frompromise.md), [fromEvent](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/fromevent.md), [takeUntil](https://github.com/Reactive-Extensions/RxJS/blob/f8f795636119143f51fc249d719bdbde1875970c/doc/api/core/operators/takeuntil.md)[@](http://rxmarbles.com/#takeUntil), [throttle](https://github.com/Reactive-Extensions/RxJS/blob/5a37fa289eb88f14d020dd88b7a72cc1f2a75da8/doc/api/core/operators/throttle.md), [delay](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/delay.md)[@](http://rxmarbles.com/#delay), [empty](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/empty.md), [catch](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/catch.md), [if](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/if.md), [timer](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/timer.md), [filter](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/where.md), [zip](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/zip.md)[@](http://rxmarbles.com/#zip).

We have seen how Observables and Operators are a powerful combination. Let’s see how we can use them in Angular.

### Integration with Angular

RxJS plays well with Angular but instead of writing your own helper functions to bridge the two you can use [rx.angular.js](https://github.com/Reactive-Extensions/rx.angular.js), a dedicated library for RxJS and AngularJS interoperability.

Let’s see an example of this integration for: scope, promises and DOM-events.

#### Integration with Scope

Using [***observeOnScope***](https://github.com/Reactive-Extensions/rx.angular.js/tree/ed5446861863b8048d54889d8bfd8f1f80214a47/docs#observeonscopescope-watchexpression-objectequality)*w*e can take a *$watch* expression and turn it into an Observable*.*Let’s use an example using a search box to query Wikipedia articles.

This example will take changes to *$scope.search* and emit objects like the following as the user types

The first operator we used was [throttle](https://github.com/Reactive-Extensions/RxJS/blob/5a37fa289eb88f14d020dd88b7a72cc1f2a75da8/doc/api/core/operators/throttle.md) that delays requests so we don’t overload the server as the user types. Then we used [map](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/select.md) to take only *newValue*or the empty string if *undefined*. We don’t want to repeat queries using the same term so we used [distinctUntilChanged](https://github.com/Reactive-Extensions/RxJS/blob/a2633f3323b2104a3714cdb4e068b29bb9c76d32/doc/api/core/operators/distinctuntilchanged.md). After that, we used [flatMapLatest](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/flatmaplatest.md) so we only take the latest results ignoring out of order and unfinished ajax calls. Finally we got the results into the scope. Try removing some operators using this [Plunker](http://embed.plnkr.co/P8dELQZ6HlglomXSvOcj/preview).

#### Promises

Promises are very helpful for one-off asynchronous operations. If you need a quick overview you can read this post.

[https://medium.com/p/8ecee75d2ffe](https://medium.com/p/8ecee75d2ffe)[**Angular — Promises basics**
*$q service for $http*medium.com](https://medium.com/p/8ecee75d2ffe)

Since version 2.2, RxJS integrates with Promises using [***Rx.Observable.fromPromise***](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/frompromise.md). See an example below:

This function returns an observable that will emit the result of the promise when available. Used with [flatMapLatest](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/flatmaplatest.md) results in a observable containing only the latest values ignoring the rest. You can find a nice graphic explanation [here](https://github.com/baconjs/bacon.js/wiki/Diagrams#user-content-flatmaplatest).

#### DOM-events

Another common use case for RxJS are DOM-events. Let’s build a simple idle user feature using RxJS and Angular. In order to use DOM-events we will use *Rx.DOM* ([HTML DOM bindings for RxJS](https://github.com/Reactive-Extensions/RxJS-DOM)) through *rx.angular*.

> Rx.DOM must be included separately but includes event binding, Ajax requests, Web Sockets, Web Workers, Server-Sent Events and even Geolocation.

In the code above we detect when the user has been idle for a period of 5 seconds. In order to do that we merged the events coming from keystrokes, mouse (clicks, move, scroll) and taps (for mobile users).

Then we buffered all events for 5 seconds ([bufferWithTime](https://github.com/Reactive-Extensions/RxJS/blob/f8f795636119143f51fc249d719bdbde1875970c/doc/api/core/operators/bufferwithtime.md) in ms) and checked when the resulting sequence was empty so we can assume the user has been idle ([filter](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/where.md)). The logic inside subscribe is a simple dialog asking the user to keep working or quit. You can find a working example using a directive in this [Plunker](http://embed.plnkr.co/x7ytJd4G6O1WX8t9lfXm/preview).

### Reactive Programming

We just started scratching the surface, Reactive Programming is a paradigm where asynchronous data streams can be used almost everywhere. Everything is a stream. Repeat with me!

![https://cdn-images-1.medium.com/freeze/max/30/1*j-SOtxql-Sqmvj0i0TWqMg.jpeg?q=20](https://cdn-images-1.medium.com/freeze/max/30/1*j-SOtxql-Sqmvj0i0TWqMg.jpeg?q=20)![https://cdn-images-1.medium.com/max/800/1*j-SOtxql-Sqmvj0i0TWqMg.jpeg](https://cdn-images-1.medium.com/max/800/1*j-SOtxql-Sqmvj0i0TWqMg.jpeg)

Hope you have enough information to continue exploring RxJS on your own. Thanks for reading!

### More resources

- [The introduction to Reactive Programming you’ve been missing](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754), by **André Staltz**[@andrestaltz](https://twitter.com/andrestaltz)
- [RxJS online book](http://xgrommx.github.io/rx-book/index.html), by Denis Stoyanov [@xgrommx](https://twitter.com/xgrommx)
- [AngularJS bindings for RxJS](https://github.com/Reactive-Extensions/rx.angular.js), by **Matthew Podwysocki**[@mattpodwysocki](https://twitter.com/mattpodwysocki)[GitHub](https://github.com/mattpodwysocki)
- [Awesome JavaScript Array Methods](http://jilles.me/awesome-javascript-array-methods/), by **Jilles Soeters**[@jilles](https://twitter.com/jilles)
