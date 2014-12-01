# Flux For Stupid People

# 给笨蛋准备的 Flux 教程

TL;DR As a stupid person, this is what I wish someone had told me when I struggled learning Flux. It's not straightforward, not well documented, and has many moving parts.

Flux 很不直观，没什么好的文档，而且一直在更新。作为一个脑子不好使的我，真的希望在我摸索学习 Flux 的时候，有人能告诉我下面这些东西。

This is a follow-up to ReactJS For Stupid People.

这是 [给笨蛋准备的 ReactJS 教程]的续篇。

## Should I Use Flux?

## 应该使用 Flux 吗？

If your application deals with dynamic data then yes, you should probably use Flux.

如果你的应用需要处理动态的数据，那很可能需要使用 Flux。

If your application is just static views that don't share state, and you never save nor update data, then no, Flux won't give you any benefit.

如果应用都是一些静态的视图，它们之间不共享状态，你也没什么数据保存或者更新，就不需要使用 Flux，用了也没什么好处。

## Why Flux?

## 为什么需要 Flux？

Humor me for a moment, because Flux is a moderately complicated idea. Why should you add complexity?

Flux 是一种相对比较复杂的概念，你可能会问，为什么要增加复杂度？

90% of iOS applications are data in a table view. The iOS toolkit has well defined views and data models that make application development easy.

90% 的 iOS 应用本质都是在把数据塞到 Table View 中。iOS 的开发工具集包含了设计优秀的 View 和 Data Model，让开发变得非常容易。

On the Front End™ (HTML, Javascript, CSS), we don't even have that. Instead we have a big problem: No one knows how to structure a Front End™ application. Seriously, I've worked in this industry for years, and "best practices" are never taught. Instead, "libraries" are taught. jQuery? Angular? Backbone? Handlebars? The real problem, data flow, still eludes us.

在前端（HTML、JavaScript、CSS），我们没有这些东西。只有一个很大的问题：前端项目的架构没有一个统一的标准。而且，我们已经花了很多年的时间摸索，结果好的方法没找到，出现了很多类库。使用 jQuery、Angular、Backbone 还是 Handlebars？但正真的问题是数据流的架构还没有。



## What is Flux?

## 什么是 Flux ？

Flux is a word made up to describe "one way" data flow with very specific events and listeners. There is no specific Flux library, but you'll need the Flux Dispatcher, and any Javascript event library.

Flux 被找来描述“单向”的数据流，且包含某些特殊的事件和监听器。Flux 没有类库，但是你需要 Flux Dispather，需要某个 JavaScript 事件库。

The official documentation is written like someone's stream of conciousness, and is a bad place to start. Once you get Flux in your head, though, it can help fill in the gaps.

Don't try to compare Flux to Model View Controller (MVC) architecture. Drawing parallels will only be confusing.

Let's dive in! I will explain concepts in order and build on them one at a time.

## 1. Your Views "Dispatch" "Actions"

## 1. View 层 “Dispatch” “actions”（分发操作）

A "dispatcher" is essentially an event system. It broadcasts events and registers callbacks. There is only ever one, global dispatcher. You should use the Facebook Dispatcher Library. It's very easy to instantiate:

    var AppDispatcher = new Dispatcher();

Let's say your application has a "new" button that adds an item to a list.

    <button onClick={ this.createNewItem }>New Item</button>

What happens on click? Your view dispatches a very specific event, with the event name and new item data:

    createNewItem: function( evt ) {
    
      AppDispatcher.dispatch({
        eventName: 'new-item',
        newItem: { name: 'Marco'}    
      });
    
    }

## 2. Your "Store" Responds to Dispatched Events

## 2. Store 响应事件

Like Flux, "store" is just a word Facebook made up. For our application, we need a specific collection of logic and data for the list. This describes our store. We'll call it a ListStore.

A store is a singleton, meaning you probably shouldn't declare it with new. The ListStore is a global object:

    var ListStore = {
    
      items: [],
    
      getAll: function() {
        return this.items;
      }
    
    };

Your store then responds to the dispatched event:

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

 Key Concept: A store is not a model. A store contains models.

 Key concept: A store is the only thing in your application that knows how to update data. This is the most important part of Flux. The event we dispatched doesn't know how to add or remove items.

If, for example, a different part of your application needed to keep track of some images and their metadata, you'd make another store, and call it ImageStore. A store represents a single "domain" of your application. If your application is large, the domains will probably be obvious to you already. If your application is small, you probably only need one store.

Only your stores are allowed to register dispatcher callbacks! Your views should never call AppDispatcher.register. The dispatcher only exists to send messages from views to stores. Your views will respond to a different kind of event.

## 3. Your Store Emits a "Change" Event

## 3. Store 触发 “change” 事件

We're almost there! Now that your data is definitely changed, we need to tell the world.

Your store emits an event, but not using the dispatcher. This is confusing, but it's the Flux way. Let's give our store the ability to trigger events. If you're using MicroEvent.js this is easy:

    MicroEvent.mixin( ListStore );

Then let's trigger our change event:

    case 'new-item':
    
      ListStore.items.push( payload.newItem );
      
      ListStore.trigger( 'change' );
      
      break;

 Key Concept: We don't pass the newest item when we trigger. Our views only care that something changed. Let's keep following the data to understand why.

## 4. Your View Responds to the "Change" Event

## 4. View 层响应 “change” 事件

Now we need to display the list. Our view will completely re-render when the list changes. That's not a typo.

First, let's listen for the change event from our ListStore when the component "mounts," which is when the component is first created:

    componentDidMount: function() {  
      ListStore.bind( 'change', this.listChanged );
    },

For simplicity's sake, we'll just call forceUpdate, which triggers a re-render. A different approach would be to store the entire list in state.

    listChanged: function() {  
      this.forceUpdate();
    },

Don't forget to clean up your event listeners when your component "unmounts," which is when it goes back to hell:

    componentWillUnmount: function() {  
      ListStore.unbind( 'change', this.listChanged );
    },

Now what? Let's look at our render function, which I've purposely saved for last.

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

But here's a problem: we're re-rendering the entire view every time the list changes! Isn't that horribly inefficient?!

Nope.

Sure, we'll call the render function again, and sure, all the code in the render function will be re-run. But React will only update the real DOM if your rendered output has changed. Your render function is actually generating a "virtual DOM", which React compares to the previous output of render. If the two virtual DOMs are different, React will update the real DOM with only the difference.

 Key Concept: When store data changes, your views shouldn't care if things were added, deleted, or modified. They should just re-render entirely. React's "virtual DOM" diff algorithm handles the heavy lifting of figuring out which real DOM nodes changed. This makes your life much simpler, and will lower your blood pressure.

## One More Thing: What The Hell Is An "Action Creator"?

## 还有，该死的 “Action Creator” 是什么？

Remember, when we click our button, we dispatch a specific event:

    AppDispatcher.dispatch({  
      eventName: 'new-item',
      newItem: { name: 'Samantha' }
    });

Well, this can get pretty repetitious to type if many of your views need to trigger this event. Plus, all of your views need to know the specific object format. That's lame. Flux suggests an abstraction, called action creators, which just abstracts the above into a function.

    ListActions = {
    
      add: function( item ) {
        AppDispatcher.dispatch({
          eventName: 'new-item',
          newItem: item
        });
      }
    
    };

Now your view can just call ListActions.add({ name: '...' }); and not have to worry about dispatched object syntax.

## Unanswered Questions

## 没有答案的问题

All Flux tells us how to do is manage data flow. It doesn't answer these questions:

How do you load and save data to the server?
How do you handle communication between components with no common parent?
What events library should I use? Does it matter?
Why hasn't Facebook released all of this as a library?
Should I use a model layer like Backbone as the model in my store?
The answer to all of these questions is: have fun!

## That's It!

## 就这样了

For additional resources, check out the Example Flux Application provided by Facebook. Hopefully after this article, the files in the js/ folder will be easier to understand.

The Flux Documentation contains some helpful nuggets, buried deep in layers of poorly accessible writing.

If this post helped you understand Flux, consider following me on Twitter.

原文：http://blog.andrewray.me/flux-for-stupid-people/