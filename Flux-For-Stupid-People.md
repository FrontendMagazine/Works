# Flux For Stupid People

# 为笨蛋准备的 Flux 教程

TL;DR As a stupid person, this is what I wish someone had told me when I struggled learning Flux. It's not straightforward, not well documented, and has many moving parts.

Flux 很不直观，没什么好的文档，而且一直在更新。作为一个脑子不好使的我，真的希望在我摸索学习 Flux 的时候，有人能告诉我下面这些东西。

This is a follow-up to ReactJS For Stupid People.

这是 [为笨蛋准备的 ReactJS 教程](http://zhuanlan.zhihu.com/FrontendMagazine/19896745)的续篇。

## Should I Use Flux?

## 应该使用 Flux 吗？

If your application deals with dynamic data then yes, you should probably use Flux.

如果你的应用需要处理**动态的数据**，那很可能需要使用 Flux。

If your application is just static views that don't share state, and you never save nor update data, then no, Flux won't give you any benefit.

如果应用都是一些**静态的视图**，它们之间不共享状态，你也没什么数据保存或者更新，就不需要使用 Flux，用了也没什么好处。

## Why Flux?

## 为什么需要 Flux？

Humor me for a moment, because Flux is a moderately complicated idea. Why should you add complexity?

Flux 是一种相对比较复杂的概念，你可能会问，为什么要增加复杂度？

90% of iOS applications are data in a table view. The iOS toolkit has well defined views and data models that make application development easy.

90% 的 iOS 应用本质都是**在把数据塞到 Table View 中**。iOS 的开发工具集包含了设计优秀的 View 和 Data Model，让开发变得非常容易。

On the Front End™ (HTML, Javascript, CSS), we don't even have that. Instead we have a big problem: No one knows how to structure a Front End™ application. Seriously, I've worked in this industry for years, and "best practices" are never taught. Instead, "libraries" are taught. jQuery? Angular? Backbone? Handlebars? The real problem, data flow, still eludes us.

在前端（HTML、JavaScript、CSS），我们没有这些东西。只有**一个很大的问题**：前端项目的架构没有一个统一的标准。而且，我们以及花了很多年的时间摸索，结果好的方法没找到，出现了很多类库。使用 jQuery、Angular、Backbone 还是 Handlebars？但正真的问题是数据流的架构还没有。



## What is Flux?

## 什么是 Flux ？

Flux is a word made up to describe "one way" data flow with very specific events and listeners. There is no specific Flux library, but you'll need the Flux Dispatcher, and any Javascript event library.

Flux 被找来描述“单向”的数据流，且包含某些特殊的事件和监听器。Flux 没有类库，但是你需要 [Flux Dispather](https://github.com/facebook/flux/blob/master/src/Dispatcher.js)，需要某个[ JavaScript 事件库](http://notes.jetienne.com/2011/03/22/microeventjs.html)。

The official documentation is written like someone's stream of conciousness, and is a bad place to start. Once you get Flux in your head, though, it can help fill in the gaps.

官方文档有些意识流，作为读者从何开始。但是一旦你的脑子里有了 Flux 的概念，这些文档可以帮助你补充一些细节。

Don't try to compare Flux to Model View Controller (MVC) architecture. Drawing parallels will only be confusing.

千万别对比 Flux 和 MVC 架构，比来比去只会越看越糊涂。

Let's dive in! I will explain concepts in order and build on them one at a time.

我开始深入点看看吧，我将**逐个解释**相关的概念，每次一个。

## 1. Your Views "Dispatch" "Actions"

## 1. View 层 “Dispatch” “actions”（分发操作）

A "dispatcher" is essentially an event system. It broadcasts events and registers callbacks. There is only ever one, global dispatcher. You should use the Facebook Dispatcher Library. It's very easy to instantiate:

Dispatcher 本质上就是一个事件系统。它负责广播事件和注册 callback。有且只有一个全局的 Dispatcher。推荐使用 [Facebook 的 Dispatcher 库](https://github.com/facebook/flux/blob/master/src/Dispatcher.js)，很简单地举个例子：

    var AppDispatcher = new Dispatcher();

Let's say your application has a "new" button that adds an item to a list.

比方说有一个“新建”按钮，点击把条目添加到一个列表中。

    <button onClick={ this.createNewItem }>New Item</button>

What happens on click? Your view dispatches a very specific event, with the event name and new item data:

点击后会发生什么？View 层触发一个特定事件，包括事件名和新条目的数据：

    createNewItem: function( evt ) {
    
      AppDispatcher.dispatch({
        eventName: 'new-item',
        newItem: { name: 'Marco'}    
      });
    
    }

## 2. Your "Store" Responds to Dispatched Events

## 2. Store 响应事件

Like Flux, "store" is just a word Facebook made up. For our application, we need a specific collection of logic and data for the list. This describes our store. We'll call it a ListStore.

和 Flux 一样，Store 就是 Facebook 随便找的一个词。在应用中，我们需要特定的处理逻辑和数据集合来存储列表，这就是我们所谓的 Store。我们给它起个名字吧——ListStore。

A store is a singleton, meaning you probably shouldn't declare it with new. The ListStore is a global object:

Store 就是一个单例，意味着你不需要使用`new`出来。ListStore 就是一个全局的对象：

    var ListStore = {
    
      items: [],
    
      getAll: function() {
        return this.items;
      }
    
    };

Your store then responds to the dispatched event:

然后 Store 响应分发出来的事件：

    var ListStore = …
        
    AppDispatcher.register( function( payload ) {
    
      switch( payload.eventName ) {
     
        case 'new-item':
    
          ListStore.items.push( payload.newItem );
          break;
      
      }
      
      return true;
    }); 

This is traditionally how Flux does dispatch callbacks. Each payload contains an event name and data. A switch statement decides specific actions.

Flux 就是使用这么传统的方法处理 callback。payload 包含一个事件名和数据。使用 switch 语句来决定调用哪个操作。

 Key Concept: A store is not a model. A store contains models.

关键：Store 不是 model，而是 model 的容器。

 Key concept: A store is the only thing in your application that knows how to update data. This is the most important part of Flux. The event we dispatched doesn't know how to add or remove items.

关键：应用中唯一知道如何更新数据的就是 Store。这是 Flux 最重要的一部分。我们分发的事件是不知如何添加或者删除条目的。

If, for example, a different part of your application needed to keep track of some images and their metadata, you'd make another store, and call it ImageStore. A store represents a single "domain" of your application. If your application is large, the domains will probably be obvious to you already. If your application is small, you probably only need one store.

再比如，应用的其他部分需要保存图片和它们的源信息，你需要使用另外一个 Store，可以取名为 ImageStore。一个 Store 就代表了应用的一个“领域”。如果应用足够大，那领域划分就是比较明显的。如果应用不大，那很可能只需要一个 Store。

Only your stores are allowed to register dispatcher callbacks! Your views should never call AppDispatcher.register. The dispatcher only exists to send messages from views to stores. Your views will respond to a different kind of event.

只有 Store 可以注册 callback。View 永远都不应该调用`AppDispatcher.register`。Dispatcher 的存在就是为了把消息从 View 传递到 Store。而 View 则是响应另外一种事件。

## 3. Your Store Emits a "Change" Event

## 3. Store 触发 “change” 事件

We're almost there! Now that your data is definitely changed, we need to tell the world.

渐入佳境！现在数据已经变化了，我们要告诉全世界。

Your store emits an event, but not using the dispatcher. This is confusing, but it's the Flux way. Let's give our store the ability to trigger events. If you're using MicroEvent.js this is easy:

Store 触发一个事件，但不使用 Dispatcher。迷糊是吧，不过这就是 Flux 的方式。给 Store 插上可以触发事件的翅膀。如果你使用 [MicroEvent.js](http://notes.jetienne.com/2011/03/22/microeventjs.html) ，可以这么写：

    MicroEvent.mixin( ListStore );

Then let's trigger our change event:

接下来就是触发事件：

    case 'new-item':
    
      ListStore.items.push( payload.newItem );
      
      ListStore.trigger( 'change' );
      
      break;

 Key Concept: We don't pass the newest item when we trigger. Our views only care that something changed. Let's keep following the data to understand why.

关键：就触发事件，不传递最新的条目。View 只关心是不是有东西改变了。接下来跟着数据看看为什么。

## 4. Your View Responds to the "Change" Event

## 4. View 层响应 “change” 事件

Now we need to display the list. Our view will completely re-render when the list changes. That's not a typo.

现在我们需要显示列表了，当列表变化的时候，View 就完全重绘。对我打错。

First, let's listen for the change event from our ListStore when the component "mounts," which is when the component is first created:

首先，当 Component “上马”的时候，监听 ListStore 的 change 事件，就是在 Component   创建时：

    componentDidMount: function() {  
      ListStore.bind( 'change', this.listChanged );
    },

For simplicity's sake, we'll just call forceUpdate, which triggers a re-render. A different approach would be to store the entire list in state.

简单点，直接调用 forceUpdate，会触发重绘。另外一种方式，就是把整个列表保存在 state 中。

    listChanged: function() {  
      this.forceUpdate();
    },

Don't forget to clean up your event listeners when your component "unmounts," which is when it goes back to hell:

当 Component 下马时，别忘了清除监听函数，也就是在 Component 下地狱之前：

    componentWillUnmount: function() {  
      ListStore.unbind( 'change', this.listChanged );
    },

Now what? Let's look at our render function, which I've purposely saved for last.

接下来呢？就道 Render 函数了，我故意把它放到最后来看：

    render: function() {
    
          var items = ListStore.getAll();
    
          var itemHtml = items.map( function( item ) {
    
          return <li key={ listItem.id }>
            { listItem.name }
          </li>;
    
        });
    
        return <div>
          <ul>
              { itemHtml }
          </ul>
    
          <button onClick={ this.createNewItem }>New Item</button>
    
        </div>;
    }

We've come full circle. When you add a new item, the view dispatches an action, the store responds to that action, the store updates, the store triggers a change event, and the view responds to the change event by re-rendering.

整个一个环就是这样——添加一个新条目，View 出发一个欣慰，Store 对这个行为作出相应，Store 更新，Store 触发 change 事件，接着 View 响应这个 change 事件重绘。

But here's a problem: we're re-rendering the entire view every time the list changes! Isn't that horribly inefficient?!

但是有一个问题：每次列表变化的时候都重绘整个 View，是不是有些送心病狂无效率？！

Nope.

不是。

Sure, we'll call the render function again, and sure, all the code in the render function will be re-run. But React will only update the real DOM if your rendered output has changed. Your render function is actually generating a "virtual DOM", which React compares to the previous output of render. If the two virtual DOMs are different, React will update the real DOM with only the difference.

没错，render 函数确实被调用了，render 函数中所有代码都一次次的执行了。但是只有在 DOM 真正变化的时候 React 才把变化 render 出来 。render 函数生成了一个 “Virtual DOM”，React 会对比之前一次 render 出来的 DOM。如果这两次的 Virtual DOM 不一样，React 就更新真实 DOM 中变化的部分。

 Key Concept: When store data changes, your views shouldn't care if things were added, deleted, or modified. They should just re-render entirely. React's "virtual DOM" diff algorithm handles the heavy lifting of figuring out which real DOM nodes changed. This makes your life much simpler, and will lower your blood pressure.

关键：当 Store 变化时，View 无需关心条目是添加、删除，还是修改了。它只需要整个重绘，React 的 Virtual DOM diff 算法进行复杂的运算，找出哪些真实的 DOM 节点变化了。这可以帮助你简单生活，降低血压。

## One More Thing: What The Hell Is An "Action Creator"?

## 还有，该死的 “Action Creator” 是什么？

Remember, when we click our button, we dispatch a specific event:

还记得吧，点击按钮，触发事件：

    AppDispatcher.dispatch({  
      eventName: 'new-item',
      newItem: { name: 'Samantha' }
    });

Well, this can get pretty repetitious to type if many of your views need to trigger this event. Plus, all of your views need to know the specific object format. That's lame. Flux suggests an abstraction, called action creators, which just abstracts the above into a function.

不过，如果 View 中有很多地方都需要触发这个事件，这就冗余大了。而且，所有的 View 都需要知道事件对象的特定格式。这有些别扭不是。Flux 提出一个抽象的层，叫做行为创建器，其实就是把上面的代码放到一个函数中。

    ListActions = {
    
      add: function( item ) {
        AppDispatcher.dispatch({
          eventName: 'new-item',
          newItem: item
        });
      }
    
    };

Now your view can just call ListActions.add({ name: '...' }); and not have to worry about dispatched object syntax.

现在 View 只需要调用 `Actions.add({name: '…'})`，不用关心分发对象的语法了。

## Unanswered Questions

## 没有答案的问题

All Flux tells us how to do is manage data flow. It doesn't answer these questions:

如何管理数据流是 Flux 的全部。但它无法回答下面这些问题：

- How do you load and save data to the server?
- How do you handle communication between components with no common parent?
- What events library should I use? Does it matter?
- Why hasn't Facebook released all of this as a library?
- Should I use a model layer like Backbone as the model in my store?

The answer to all of these questions is: have fun!

- 如何加载数据，如何把数据存储到服务端？
- 如何让两个不相干（没有共同父节点）的组件通信？
- 该选择哪一个 event 类库，有什么优劣取舍？
- 为什么 Facebook 没有把所有这些作为一个类库开源出来？
- 可以使用想 Backbone 这样的作为 Store 中存储的 model 么？

上面这几个问题的答案就是：看心情，自己玩吧！

## That's It!

## 就这么多

> 译者注：下面不翻译了。

For additional resources, check out the[ Example Flux Application ](https://github.com/facebook/flux/tree/master/examples/flux-todomvc)provided by Facebook. Hopefully after this article, the files in the js/ folder will be easier to understand.

[The Flux Documentation](http://facebook.github.io/flux/docs/overview.html) contains some helpful nuggets, buried deep in layers of poorly accessible writing.

If this post helped you understand Flux, consider [following me on Twitter](https://twitter.com/andrewray).

原文：http://blog.andrewray.me/flux-for-stupid-people/