# An Introduction to Angular 2
# Angular2 简介

DISCLAIMER: Angular 2 is still on a developer preview, so it has a lot of missing features, broken stuff and it is also subject to change.

声明：Angular2 现在还处于开发预览版，所以会有很多功能缺失，非兼容性升级，并且也会存在各种变化。


Ready for Angular 2? You bet!

那么准备好了么？Yeah！


For this demo, we are going to code an application which requests german words. The word will also come with its translation if the user is authenticated.

在这个实例中，我们需要用 `german words` 编写一个应用。如果用户通过认证的话就将这个单词解码。


First, grab the KOA backend from [here][1], then:

第一步，从[这里][2]把后端服务KOA抓下来，然后执行：


    $ npm install
    $ node server.js

NOTE: You need `node 0.11+` or `io.js` to work with `koa`. And if it is node, you need to use the `—harmony` flag like:

**注意：**你需要 `node 0.11+` 版本或者 `io.js` 来运行 `koa`。如果选择 `node` 的话，需要用 `--harmony` 来启动服务：


    $ node --harmony server.js

Leave it running and clone [this repo.][3] Here we are going to work on the before folder. To use it:

我们边启动服务边把[这个库][4] clone 下来，切到 `before` 目录下，使用命令：


    $ npm install
    $ npm start
    
I got this boilerplate from my friend [Pawel][5] and I updated thanks to [mgonto][6] and [gdi2290][7].

我从我的朋友[Pawel][8]那里搞到了这个代码模板然后修改了一下，同时也感谢[mgonto][9]和[gdi2290][10]。


Let me explain it a bit:

让我来简单解释一下：

Since Angular 2 is still not final, it needs a lot of boilerplate to be setup, and this skeleton do it for you.

由于 `Angular 2` 还不是最终版本，所以它需要配置一大堆模板代码，而这个框架就是做这个的。

On one hand, we have the `gulpfile` where apart from the classic tasks to process `javascript` and `css`, we have tasks to build both `angular` and its `router` from the sources. On the other hand, we have the `index.html` file where we load all the needed libraries for `Angular 2`. As I said before, since Angular 2 is still not final, we have to load a lot of libraries to make it work, but worry not, on the final version we won’t need to do that.

一方面，我们用经典的 `javascript` 和 `css` 任务流管理工具 `gulpfile` 来处理`angular` 和它的`路由`规则。另一方面，我们用 `index.html` 来加载所有我们需要的`Angular 2` 的库。如我之前所说，在 `Angular 2` 发布最终版之前， 我们需要一大堆库来运行 `Angular 2`， 但不用担心，最终版是不需要这么麻烦的。


Another interesting thing here is `System`. `System` is the module loader of ES6 and it is the one which will start our application. Yay, no more hundreds of script tags!

另一个有趣的东西是 `System` 模块。`System` 是`ES6`的模块加载器，也是我们的应用启动模块。耶，再也不需要一大堆`script`标签了，耶！


Also, there is no more ng-app :)

当然，也不会再有 `ng-app` 了 : ) 


Alright! `System` is going to load `index` which is supposed to bootstrap our app, right? How do we do that? To bootstrap an `Angular 2` application we need to use the `bootstrap` method passing our main component. Component? Yeah, we will see in a bit :)

那么，`System` 模块应该加载 `index` 来启动我们的应用程序，对吧？那么该怎么做呢？想要启动 `Angular 2` 应用，我们需要将入口组件传入 `bootstrap`（译者注：[`bootstrap`][11]）方法。那入口组件是什么玩意？我们马上就会知道了 :)


    bootstrap(OurMainComponent);

For our app, we will have an `App` component so to bootstrap our application, we can do:

在我们的应用程序中，需要创建一个`App`组件来引导启动我们的应用程序：

    index.js

    import { bootstrap } from 'angular2/angular2';
    import { App } from './app/app';
    
    bootstrap(App);

We just need to import our `App` component and the `bootstrap` method to then use it to bootstrap the app. Another thing we need to do for our app, is to load the router. The router is external to angular 2, so we need to load it as a dependency. As today (2015-05-04) the router is not exporting the needed injectable to make this easy, so we need to construct a new instance of the router and inject it:

我们只需要导入我们的 `App` 组件和 `bootstrap` 方法，然后就可以使用 `bootstrap` 方法启动应用了。我们的应用还需要引入路由（译者注：[`router`][12]）组件，由于路由组件是 `angular 2` 的外部组件，所以我们也需要把它作为依赖加载进来。直到今天（2015-05-04），路由模块也还不能导出必要的注入使其更加易用，所以我们需要构造一个新的路由实例然后将它注入进去：

    index.js
    
    import { bootstrap } from 'angular2/angular2';
    import { RootRouter } from 'angular2/src/router/router';
    import { Pipeline } from 'angular2/src/router/pipeline';
    import { bind } from 'angular2/di';
    import { Router } from 'angular2/router';
    
    import { App } from './app/app';
    
    bootstrap(App, [
      bind(Router).toValue(new RootRouter(new Pipeline()))
    ]);

We loaded all the needed dependencies to construct the router and also the `bind` service to create a Binding for the router. Hopefully this will be fixed really soon.

我们加载了所有构造路由需要的依赖，并且用 `bind`（译者注：[`bind`][13]）服务创建了一个路由的绑定。希望能尽早修复这个问题。


Okay, let’s code the App component. But what’s a component? A component is a just a class which can be used to represent a page like `home`, `login`, `users`… or even used to create a `directive` like `datepicker`, `tabs`, etc.

好了，现在让我们正式开始写 App 组件的代码吧。不过，组件到底是什么？组件只是一个类，可以用来表示一个 `home` 页面，或者一个 `login` 模块，`users` 信息模块……或者可以用来创建一个类似于 `datepicker`，`tabs` 等的`指令`（译者注：[指令][14]）。


    app/app.js
    
    export class App {
    
    }

As I said, the component is just a class (we export it to be able to import it from other files like we did in index). So far it is not doing anything, so let fix that.

如我所说，组件只是一个类（我们把它导出以便于可以在其他文件中导入使用，就像我们在 index 中导入其他组件一样）。到目前为止好像我们还是没都没做，让我们动起来！


In `Angular 2` we can use annotations. Think about annotations as a way to add metadata to our classes. Let’s go step by step. First import the two annotations we need:

在 `Angular 2` 中我们可以使用注解（译者注：`[annotations][15]`）。想象一下通过注解的方式给我们的类添加元数据。我们一步一步来。第一步，导入我们需要的两个注解：


    app/app.js

    import {View, Component} from 'angular2/angular2';

Then we just need to use them. `Component` is an annotation to add metadata about the component itself, that includes its selector, or what services we need to inject. On the other hand, the `View` annotation is used for the HTML templates. Here we can specify the template we want to use with the component, if we need to use directives in it, etc. We can have more than one `View` annotation (mobile view, desktop view, etc).

然后我们就可以使用了，`Component` 注解用来给组件自身添加元数据，包括组件的选择器，以及需要注入的服务。`View` 注解用来添加 HTML 模板，我们可以给组件指定想要使用的模板，以及`指令`等。我们可以加入多个 `View` 注解（mobile 视图，desktop 视图等）。


Let’s use them:

让我们用起来：

    app/app.js
    
    @Component({
      selector: 'words-app'
    })
    @View({
      template: `<h1>Hello angular 2</h1>`
    })
    export class App {
    
    }
 

For `Component` we specified that the selector for this component will be `words-app` (look mum, no more `camelCase` vs `snake-case` like `Angular 1`), that means that to use this component, we just need to drop a `<words-app></words-app>` somewhere!

我们给 `Component` 指定了组件的选择器 `words-app`（唉呀妈呀，终于不用再纠结像`Angular 1` 里面是驼峰命名还是下划线命名的问题了），这意味着如果我们想在任何一个地方用这个组件，只需要写上`<words-app></words-app>`就可以了。


For this `View` we created a simple template (notice the quotes).

这次我们先给 `View` 创建一个简单的模板【注意引号（译者注：这里也可以用单引号，双引号）】。


NOTE: Don’t put semicolons after each annotation, that will make Angular 2 cry :)

**注意：**不要在注解后面加分号，那会把`Angular 2`搞哭的 :)


So you said we can drop that selector somewhere? Yeah, let’s modify the index.html:

你说过可以把这个选择器放在任何地方？耶，我们来修改一下 `index.html`：

    index.html
    
    <body>
        <div class="container">
            <words-app>
                Loading...
            </words-app>
        </div>
    </body>
 

Ok, here we used our new component. Let’s go to localhost:3000

好了，现在我们来运行下我们的组件，在浏览器中访问 `localhost:3000`


![Hello angular 2][16]


So far so good, isn’t it? Inside App we are going to configure the router that we got injected from the bootstrap function:

到目前为止，一切都还好，对吧？现在我们需要配置一下从 `bootstrap` 函数中注入的`路由`。


    app/app.js
    
    export class App {
      constructor(router: Router) {
    
      }
    }


The class constructor will receive a `router` parameter of the type `Router`. Let’s use it:

构造函数会接收一个 `Router` 类型的参数 `router`：


    app/app.js
    
    export class App {
      constructor(router: Router) {
        router
          .config('/home', Home)
          .then(() => router.navigate('/home'));
      }
    }

    
The `router` has a `config` method where we pass the `path` and what `component` to use for that path. We chain the promise it returns to immediately navigate to that `/home` route.

`router` 有一个 `config` 方法，我们可以通过传递 `path` 为组件配置路由规则，然后通过链式调用 `promise` 的方式将路由导航到 `/home`。


Soon we will be able to configure the routes of each component as an annotation, but for the time being, we will use this way of configuring the router.

不久以后，我们就可以用注解的方式为每个组件配置路由了。不过现在我们还是得用这种方式配置路由。


When we load a new route, where do we put the component? We need an `ng-view` which is called `router-outlet` in this new router.

当我们通过一个新的路由加载组件时，组件生成的 `View` 放在哪里呢？我们需要为这个新的组件添加一个叫做 `router-outlet `的 `ng-view`。


Let’s change our `View` annotation like:

我们把 `View` 的注解稍作修改：


    app/app.js
    
    @View({
      template: `<router-outlet></router-outlet>`,
      directives: [RouterOutlet]
    })


There is something important here: If our template uses a directive/component, we need to import it explicitly and that is the case of `RouterOutlet`. Notice that on the `directives` array we put the component object we are importing. Talking about imports… we need to add a few. Our imports are now like:

这一点非常重要：当模板用到指令/组件的时候，我们需要把 `RouterOutlet` 导入进来。注意，我们把导入的 `RouterOutlet` 对象添加到了 `directives` 数组中。说到导入…… 我们还需要导入几个组件。导入这几个组件后，我们的代码如下：


    app/app.js
    
    import {View, Component} from 'angular2/angular2';
    import {Router, RouterOutlet} from 'angular2/router';
    import {Home} from '../home/home';
   

We loaded the `Router`, the `RouterOutlet` component and also the `Home` component.

我们加载了 `Router`， `RouterOutlet` 和 `Home` 组件。


Now our app will navigate directly to that `Home` component. Let’s create it:

现在我们的应用会直接跳转到 `Home` 组件。我们来写这个组件：


    home/home.js
    
    import {View, Component} from 'angular2/angular2';
    
    @Component({
      selector: 'home'
    })
    @View({
      templateUrl: 'home/home.html'
    })
    export class Home {
    
    }
   

This time instead of embedding our template directly on the annotation, we will create an external file for it where we put:

这次我们用引用外部文件的方式代替直接在注解中嵌入模板的方式：


    home/home.html
    
    <h1>This is home</h1>

Before running our app, let’s add bootstrap to our index.css:

在启动我们的应用之前，我们把 `bootstrap` 引入到我们的 `index.css` 中：

  
    index.css
    
    @import 'bootstrap';
    
    
Now we have something like:

现在我们的应用看起来应该是这样的：


![2](http://angular-tips.com/images/posts/angular2intro/2.png)


Let’s do something real, shall we? On this home component, we want to grab a new word every time we click a button. To abstract `Home` from requests and stuff, let’s create a service for that, called `Words`:

现在我们来做点儿有用的东西？在这个 `home` 组件中，我们希望每次点击按钮的时候都可以获取一个新的单词。我们需要创建一个 `Words` 的服务来处理请求和数据：


    services/words.js
    
    export class Words {
      getWord() {
        var jwt = localStorage.getItem('jwt');
    
        return fetch('http://localhost:3001/api/random-word', {
          method: 'GET',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json',
            'Authorization': 'bearer ' + jwt
          }
        })
        .then((response) => {
          return response.text();
        })
        .then((text) => {
          return JSON.parse(text);
        })
      }
    }
 

A service in `Angular 2` is just a class and for this one, we just need a method to grab a word. As today, `Angular 2` doesn’t have anything like `$http` so we are going to use a library called `fetch`.

服务在 `Angular 2` 中只是一个类，对于这个 `Words` 服务而言，我们只需要一个获取单词的方法而已。到今天为止，`Angular 2` 还没有一个类似于 `$http` 的库，所以我们需要使用一个名为 `fetch` 的库。


With `fetch` we make a request to our backend, passing the `JWT` token if there is any, we extract the text from the response and we parse it so the next time we create a new `.then` we will receive the final object.

我们使用 `fetch` 向服务端发起一个请求，如果有 `JWT` 令牌的话解析之，我们从服务器端响应中提取返回的文本并解析成 `JSON` 对象，然后再下次调用 `.then` 方法的时候，我们就能拿到这个 `JSON` 对象。


Let’s import this service in `Home`:

我们在 `Home` 中导入这个服务：


    home/home.js
    
    import {Words} from '../services/words';
 

Now we need to tell our component that we want to inject the service:

现在我们需要在组件中注入这个服务：


    home/home.js
    
    @Component({
      selector: 'home',
      injectables: [Words]
    })
   

Finally, we receive a new instance of the service in the constructor:

最后，我们给构造函数传入这个服务的实例对象：


    home/home.js
    
    export class Home {
      constructor(words: Words) {
        this.words = words;
      }
    }
   

Like with the router, we receive a `words` instance of the type `Words`.

就像 `router` 对象，我们得到一个 `Words` 类型的实例 `words`。


With our service in place, let’s code the template:

服务代码做好之后，现在我们来编写模板代码：


    home/home.html
    
    <div class="jumbotron centered">
      <h1>German words demo!</h1>
      <p>Click the button below to get a random German word with its translation:</p>
      <p><a class="btn btn-primary" role="button" (click)="getRandomWord()">Give me a word!</a></p>
      <div *if="word">
        <pre>Word: {{word.german}}</pre>
      </div>
    </div>


There is a couple of new stuff in `Angular 2`. Instead of `ng-click` we use `(click)`, the parenthesis means that it is an event (click event). On the other hand, we have that `*if` which is our classic `ng-if`. The star means that it is a template, basically a shorter version of doing:

这里有几个 `Angular 2` 的新语法糖。我们用`(click)`代替了 `angular 1` 中的 `ng-click`。圆括号表示这是一个事件（点击事件）。另一个语法糖，`*if` 就是我们经典的 `ng-if`。符号`*`表示这是一个模板，一般用来做一些简写，相当于：


    <template [if]="word">
 

Is that `if` a directive? Yes, and what we said about using directives inside our templates? That we need to import them:

那 `if` 是一个指令（译者注：[指令][17]）么？是的。如果我们需要在模板中使用指令的话，之前怎么说来着？嗯，我们需要导入这些玩意：


    home/home.js
    
    import {View, Component, If} from 'angular2/angular2';


And then:

然后：


    home/home.js
    
    @View({
      templateUrl: 'home/home.html',
      directives: [If]
    })
    

Next, we need that button working so we can show the words:

下一步，我们需要在点击 `button` 的时候显示服务器端返回的单词：


    home/home.js
    
    getRandomWord() {
      this.words.getWord().then((response) => {
        this.word = response;
      });
    }
    

If we now try the app, we can see something like:

再试着运行一下我们的应用，我们会看到：


![Alt text](http://angular-tips.com/images/posts/angular2intro/3.png)


Alright, it is getting shape!

嗯，终于像那么回事儿了！


Let’s move to the authentication. A service you said? Right on!

现在我们来做个身份认证。你说是一个服务？没错！


    export class Auth {
      constructor() {
        this.token = localStorage.getItem('jwt');
        this.user = this.token && jwt_decode(this.token);
      }
    
      isAuth() {
        return !!this.token;
      }
    
      getUser() {
        return this.user;
      }
    
      login(username, password) {
        return fetch('http://localhost:3001/sessions/create', {
          method: 'POST',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            username, password
          })
        })
        .then((response) => {
          return response.text();
        })
        .then((text) => {
          this.token = JSON.parse(text).id_token;
          localStorage.setItem('jwt', this.token);
        });
      }
    
      logout() {
        localStorage.removeItem('jwt');
        this.token = null;
        this.user = null;
      }
    }
    

We store two items, the token and the decoded token, then we have a method to login and a method to logout. Nothing really special. Both `localStorage` and `jwt_decode` are globals so we don’t need to import that. Again, we are using `fetch` to do the request to the backend.

我们存储两个字段，一个是令牌，一个是解码后的令牌，然后我们需要一个 `login` 方法和`logout` 方法来做登录和登出。没什么特别的东西。`localStorage` 和 `jwt_decode` 都是全局的，不需要导入。我们再次使用 `fetch` 函数在服务器端处理请求。


On the other hand, when we talk about login and stuff, we need a way to actually login, right? Let’s create a component for login:

另外，既然我们说到了登录，那么我们就需要一个登录的途径，对吧？我们来创建一个登录组件：


    login/login.js
    
    import {Component, View} from 'angular2/angular2';
    import {Router} from 'angular2/router';
    import {Auth} from '../services/auth';
    
    @Component({
      selector: 'login',
      injectables: [Auth]
    })
    @View({
      templateUrl: 'login/login.html'
    })
    export class Login {
      constructor(router: Router, auth: Auth) {
        this.router = router;
        this.auth = auth;
      }
    
      login(event, username, password) {
        event.preventDefault();
        this.auth.login(username, password).then(() => {
          this.router.parent.navigate('/home');
        })
        .catch((error) => {
          alert(error);
        });
      }
    }
    

Like the other components, we have our `Component` and `View` annotations and we also specified that the new `Auth` service is going to be injected into the component.

就像其他组件，我们需要 `Component` 和 `View` 等注解，还需要注入一个叫做 `Auth` 的新服务。


This component only have one method were we use the `Auth` service to login and set the `jwt` token in the `localStorage`. If we succeed, we just navigate to the `/home` page.

这个组件只有一个方法：用 `Auth` 服务处理登录并且把 `jwt` 令牌塞到 `localStorage` 里面。如果成功的话，我们就跳转到 `/home` 页面。


For the template, we have:

编写我们的模板代码：


    login/login.html
    
    <div class="login jumbotron center-block">
      <h1>Login</h1>
      <form role="form" (submit)="login($event, username.value, password.value)">
        <div class="form-group">
          <label for="username">Username</label>
          <input type="text" #username class="form-control" id="username" placeholder="Username">
        </div>
        <div class="form-group">
          <label for="password">Password</label>
          <input type="password" #password class="form-control" id="password" placeholder="Password">
        </div>
        <button type="submit" class="btn btn-default">Submit</button>
      </form>
    </div>
    

Notice how we have stuff like `#username` instead of `ng-model="username"`. That is how Angular 2 binds our stuff.

注意我们用 `#username` 代替了 `ng-model="username"` 。这是 `Angular 2` 中的绑定方式。


As a last step, let’s modify the css a bit because the form is a bit wide:

最后一个步，我们简单修改一下 `css`，因为表单稍微有点宽：


    login/login.css
    
    .login {
      width: 40%;
    }
    

We also need to import this .css file:

同样需要导入这些 `.css` 文件：


    index.css
    
    @import 'bootstrap';
    @import './src/login/login.css';
    

Having the login in place, we need a link for it in our `Home` component:

为了展示 `login` 组件，我们需要与 `Home` 组件建立连接：


    home/home.html

    <div class="jumbotron centered">
      <h1>German words demo!</h1>
      <p>Click the button below to get a random German word with its translation:</p>
      <p><a class="btn btn-primary" role="button" (click)="getRandomWord()">Give me a word!</a></p>
      <div *if="word">
        <pre>Word: {{word.german}}</pre>
      </div>
    </div>
    <div *if="isAuth">
      <p>Welcome back {{user.username}}</p>
      <a href="#" (click)="logout($event)">Logout</a>
    </div>
    <div *if="!isAuth">
      <a href="#" (click)="login($event)">Login</a>
    </div>
    

So having a flag called `isAuth`, we will switch between two divs.

我们有了一个名为 `isAuth` 的标识，可以让我们在两个 `div` 之间进行切换。


We will need the `Auth` service here as well, so let’s import it:

我们也需要将 `Auth` 服务导入进来：

    
    home/home.js
    
    import {Auth} from '../services/auth';
    
    @Component({
      selector: 'home',
      injectables: [Words, Auth]
    })
    

Then we need a `login` and `logout` methods, the `isAuth` flag and also a reference to the user. Our component is now like:

然后我们需要 `login` 和 `logout` 方法，`isAuth` 标志值，并将其关联到我们的用户。现在我们的组件是这样的：


    home/home.js
    
    export class Home {
      constructor(router: Router, words: Words, auth: Auth) {
        this.router = router;
        this.auth = auth;
        this.words = words;
    
        this.isAuth = auth.isAuth();
    
        if (this.isAuth) {
          this.user = this.auth.getUser();
        }
      }
    
      getRandomWord() {
        this.words.getWord().then((response) => {
          this.word = response;
        });
      }
    
      login(event) {
        event.preventDefault();
        this.router.parent.navigate('/login');
      }
    
      logout(event) {
        event.preventDefault();
        this.auth.logout();
        this.isAuth = false;
        this.user = null;
      }
    }

    
In the near future, we can avoid that `login` method thanks to the `RouteLink` component which is basically like `ui-sref`.

在不久的将来，就像基础组件 `ui-sref` 一样，`RouteLink` 组件可以让我们避免 `login` 这种做法。


Alright, now we have the link to the login page:
好的，现在让我们链接到 `login` 页面：
![Alt text](http://angular-tips.com/images/posts/angular2intro/4.png)


And if we click it, we… Oh wait, it is not working. Ah, we forgot to add the route for it back at app.js:

然后我们点击 `login`，我们就会……哦 稍等，它没反应了。 啊，我们忘了把 `login` 的路由加到 `app.js` 里：


    app/app.js
    
    import {Login} from '../login/login';
    
    export class App {
      constructor(router: Router) {
        router
          .config('/home', Home)
          .then(() => router.config('/login', Login))
          .then(() => router.navigate('/home'));
      }
    }
    

Now it works:

现在它好了：


![Alt text](http://angular-tips.com/images/posts/angular2intro/5.png)


And if we login with the demo credentials (demo / 12345) we see:

然后我们用示例账户（demo / 12345）登录，我们会看到：


![Alt text](http://angular-tips.com/images/posts/angular2intro/6.png)


That’s great!

爽！

The last step is showing the translation if we are logged in and that is easy to do! We are already grabbing the words from the server and now that the server sees that we are authenticated, it will send the translation as well. That means that we just need to update our template:

最后一步，当我们登录进去之后我们需要把单词解码，这个比较简单。我们已经从服务器取到了单词，并且现在服务器认为我们处于登录状态，所以它也会把解码后的单词返回回来。这意味着我们只需要更新我们的模板就可以了：


    home/home.html
    
    <div class="jumbotron centered">
      <h1>German words demo!</h1>
      <p>Click the button below to get a random German word with its translation:</p>
      <p><a class="btn btn-primary" role="button" (click)="getRandomWord()">Give me a word!</a></p>
      <div *if="word">
        <pre>Word: {{word.german}}</pre>
        <pre *if="isAuth">Translation: {{word.english}}</pre>
        <p *if="!isAuth">Please login below to see the translation</p>
      </div>
    </div>
    <div *if="isAuth">
      <p>Welcome back {{user.username}}</p>
      <a href="#" (click)="logout($event)">Logout</a>
    </div>
    <div *if="!isAuth">
      <a href="#" (click)="login($event)">Login</a>
    </div>
 

Now we have our app completed:

现在我们终于完成了应用：


![Alt text](http://angular-tips.com/images/posts/angular2intro/7.png)


Oh wait, I forgot to log in:

噢 稍等，我忘了登录：


![Alt text](http://angular-tips.com/images/posts/angular2intro/8.png)


I bet you can learn `Angular 2` faster than the word on the image :)

我希望你学习 `Angular 2` 能比学图片中的单词还要快 :)


Before closing this article and as a curiosity, you can import the `Login` component in `Home`, add it as a used directive in the `View` component and then add `<login></login>` at the bottom of the template. Doing that, you will attach the entire `login` form and its functionality to the `Home` component, yay!

在结束这篇文章之前，作为一个有趣的探索，你可以把 `Login` 组件导入到 `Home` 中，然后把 `Login` 作为一个指令加入到 `View` 的指令数组中，接着把 `<login></login>` 添加到模板的底部。这样做之后，你就可以在的 `Home` 页面使用 `login` 组件完整的表单和功能了，耶！


I want to thanks [Matias Gontovnikas][18] and [PatrickJS][19] for their demo [angular2-authentication-sample][20] and explanations which helped me a lot understanding the whole process.

我想感谢[Matias Gontovnikas][21]和[PatrickJS][22]的demo [angular2-authentication-sample][23]和他们的解释，这给我理解整个流程提供了巨大的帮助。


Posted by Jesus Rodriguez May 4th, 2015

Jesus Rodriguez 发表于 2015年5月4日 

原文地址：http://angular-tips.com/blog/2015/05/an-introduction-to-angular-2/

  [1]: https://github.com/angular-tips/GermanWords-backend-koa
  [2]: https://github.com/angular-tips/GermanWords-backend-koa
  [3]: https://github.com/angular-tips/GermanWords-frontend-angular-2
  [4]: https://github.com/angular-tips/GermanWords-frontend-angular-2
  [5]: https://github.com/pkozlowski-opensource
  [6]: https://github.com/mgonto
  [7]: https://github.com/gdi2290
  [8]: https://github.com/pkozlowski-opensource
  [9]: https://github.com/mgonto
  [10]: https://github.com/gdi2290
  [11]: https://angular.io/docs/js/latest/api/core/bootstrap-function.html
  [12]: https://angular.io/docs/js/latest/api/router/
  [13]: https://angular.io/docs/js/latest/api/di/
  [14]: https://angular.io/docs/js/latest/api/directives/
  [15]: https://angular.io/docs/js/latest/api/core/ViewAnnotation-class.html
  [16]: http://angular-tips.com/images/posts/angular2intro/1.png
  [17]: https://angular.io/docs/js/latest/api/directives/
  [18]: https://github.com/mgonto
  [19]: https://github.com/gdi2290
  [20]: https://github.com/auth0/angular2-authentication-sample
  [21]: https://github.com/mgonto
  [22]: https://github.com/gdi2290
  [23]: https://github.com/auth0/angular2-authentication-sample