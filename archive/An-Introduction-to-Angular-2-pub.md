# Angular2 简介

声明：Angular2 现在还处于开发预览版，所以会有很多功能缺失，非兼容性升级，并且也会存在各种变化。


那么准备好了么？Yeah！


在这个实例中，我们需要用 `german words` 编写一个应用。如果用户通过认证的话就将这个单词解码。


第一步，从[这里][1]把后端服务KOA抓下来，然后执行：


    $ npm install
    $ node server.js

**注意：**你需要 `node 0.11+` 版本或者 `io.js` 来运行 `koa`。如果选择 `node` 的话，需要用 `--harmony` 来启动服务：


    $ node --harmony server.js

我们边启动服务边把[这个库][2] clone 下来，切到 `before` 目录下，使用命令：


    $ npm install
    $ npm start
    
我从我的朋友[Pawel][3]那里搞到了这个代码模板然后修改了一下，同时也感谢[mgonto][4]和[gdi2290][5]。


让我来简单解释一下：

由于 `Angular 2` 还不是最终版本，所以它需要配置一大堆模板代码，而这个框架就是做这个的。

一方面，我们用经典的 `javascript` 和 `css` 任务流管理工具 `gulpfile` 来处理`angular` 和它的`路由`规则。另一方面，我们用 `index.html` 来加载所有我们需要的`Angular 2` 的库。如我之前所说，在 `Angular 2` 发布最终版之前， 我们需要一大堆库来运行 `Angular 2`， 但不用担心，最终版是不需要这么麻烦的。


另一个有趣的东西是 `System` 模块。`System` 是`ES6`的模块加载器，也是我们的应用启动模块。耶，再也不需要一大堆`script`标签了，耶！


当然，也不会再有 `ng-app` 了 : ) 


那么，`System` 模块应该加载 `index` 来启动我们的应用程序，对吧？那么该怎么做呢？想要启动 `Angular 2` 应用，我们需要将入口组件传入 `bootstrap`（译者注：[`bootstrap`][6]）方法。那入口组件是什么玩意？我们马上就会知道了 :)


    bootstrap(OurMainComponent);

在我们的应用程序中，需要创建一个`App`组件来引导启动我们的应用程序：

    index.js

    import { bootstrap } from 'angular2/angular2';
    import { App } from './app/app';
    
    bootstrap(App);

我们只需要导入我们的 `App` 组件和 `bootstrap` 方法，然后就可以使用 `bootstrap` 方法启动应用了。我们的应用还需要引入路由（译者注：[`router`][7]）组件，由于路由组件是 `angular 2` 的外部组件，所以我们也需要把它作为依赖加载进来。直到今天（2015-05-04），路由模块也还不能导出必要的注入使其更加易用，所以我们需要构造一个新的路由实例然后将它注入进去：

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

我们加载了所有构造路由需要的依赖，并且用 `bind`（译者注：[`bind`][8]）服务创建了一个路由的绑定。希望能尽早修复这个问题。


好了，现在让我们正式开始写 App 组件的代码吧。不过，组件到底是什么？组件只是一个类，可以用来表示一个 `home` 页面，或者一个 `login` 模块，`users` 信息模块……或者可以用来创建一个类似于 `datepicker`，`tabs` 等的`指令`（译者注：[指令][9]）。


    app/app.js
    
    export class App {
    
    }

如我所说，组件只是一个类（我们把它导出以便于可以在其他文件中导入使用，就像我们在 index 中导入其他组件一样）。到目前为止好像我们还是没都没做，让我们动起来！


在 `Angular 2` 中我们可以使用注解（译者注：`[annotations][10]`）。想象一下通过注解的方式给我们的类添加元数据。我们一步一步来。第一步，导入我们需要的两个注解：


    app/app.js

    import {View, Component} from 'angular2/angular2';

然后我们就可以使用了，`Component` 注解用来给组件自身添加元数据，包括组件的选择器，以及需要注入的服务。`View` 注解用来添加 HTML 模板，我们可以给组件指定想要使用的模板，以及`指令`等。我们可以加入多个 `View` 注解（mobile 视图，desktop 视图等）。


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
 

我们给 `Component` 指定了组件的选择器 `words-app`（唉呀妈呀，终于不用再纠结像`Angular 1` 里面是驼峰命名还是下划线命名的问题了），这意味着如果我们想在任何一个地方用这个组件，只需要写上`<words-app></words-app>`就可以了。


这次我们先给 `View` 创建一个简单的模板【注意引号（译者注：这里也可以用单引号，双引号）】。


**注意：**不要在注解后面加分号，那会把`Angular 2`搞哭的 :)


你说过可以把这个选择器放在任何地方？耶，我们来修改一下 `index.html`：

    index.html
    
    <body>
        <div class="container">
            <words-app>
                Loading...
            </words-app>
        </div>
    </body>
 

好了，现在我们来运行下我们的组件，在浏览器中访问 `localhost:3000`


![Hello angular 2][11]


到目前为止，一切都还好，对吧？现在我们需要配置一下从 `bootstrap` 函数中注入的`路由`。


    app/app.js
    
    export class App {
      constructor(router: Router) {
    
      }
    }


构造函数会接收一个 `Router` 类型的参数 `router`：


    app/app.js
    
    export class App {
      constructor(router: Router) {
        router
          .config('/home', Home)
          .then(() => router.navigate('/home'));
      }
    }

    
`router` 有一个 `config` 方法，我们可以通过传递 `path` 为组件配置路由规则，然后通过链式调用 `promise` 的方式将路由导航到 `/home`。


不久以后，我们就可以用注解的方式为每个组件配置路由了。不过现在我们还是得用这种方式配置路由。


当我们通过一个新的路由加载组件时，组件生成的 `View` 放在哪里呢？我们需要为这个新的组件添加一个叫做 `router-outlet `的 `ng-view`。


我们把 `View` 的注解稍作修改：


    app/app.js
    
    @View({
      template: `<router-outlet></router-outlet>`,
      directives: [RouterOutlet]
    })


这一点非常重要：当模板用到指令/组件的时候，我们需要把 `RouterOutlet` 导入进来。注意，我们把导入的 `RouterOutlet` 对象添加到了 `directives` 数组中。说到导入…… 我们还需要导入几个组件。导入这几个组件后，我们的代码如下：


    app/app.js
    
    import {View, Component} from 'angular2/angular2';
    import {Router, RouterOutlet} from 'angular2/router';
    import {Home} from '../home/home';
   

我们加载了 `Router`， `RouterOutlet` 和 `Home` 组件。


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
   

这次我们用引用外部文件的方式代替直接在注解中嵌入模板的方式：


    home/home.html
    
    <h1>This is home</h1>

在启动我们的应用之前，我们把 `bootstrap` 引入到我们的 `index.css` 中：

  
    index.css
    
    @import 'bootstrap';
    
    
现在我们的应用看起来应该是这样的：


![2](http://angular-tips.com/images/posts/angular2intro/2.png)


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
 

服务在 `Angular 2` 中只是一个类，对于这个 `Words` 服务而言，我们只需要一个获取单词的方法而已。到今天为止，`Angular 2` 还没有一个类似于 `$http` 的库，所以我们需要使用一个名为 `fetch` 的库。


我们使用 `fetch` 向服务端发起一个请求，如果有 `JWT` 令牌的话解析之，我们从服务器端响应中提取返回的文本并解析成 `JSON` 对象，然后再下次调用 `.then` 方法的时候，我们就能拿到这个 `JSON` 对象。


我们在 `Home` 中导入这个服务：


    home/home.js
    
    import {Words} from '../services/words';
 

现在我们需要在组件中注入这个服务：


    home/home.js
    
    @Component({
      selector: 'home',
      injectables: [Words]
    })
   

最后，我们给构造函数传入这个服务的实例对象：


    home/home.js
    
    export class Home {
      constructor(words: Words) {
        this.words = words;
      }
    }
   

就像 `router` 对象，我们得到一个 `Words` 类型的实例 `words`。


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


这里有几个 `Angular 2` 的新语法糖。我们用`(click)`代替了 `angular 1` 中的 `ng-click`。圆括号表示这是一个事件（点击事件）。另一个语法糖，`*if` 就是我们经典的 `ng-if`。符号`*`表示这是一个模板，一般用来做一些简写，相当于：


    <template [if]="word">
 

那 `if` 是一个指令（译者注：[指令][12]）么？是的。如果我们需要在模板中使用指令的话，之前怎么说来着？嗯，我们需要导入这些玩意：


    home/home.js
    
    import {View, Component, If} from 'angular2/angular2';


然后：


    home/home.js
    
    @View({
      templateUrl: 'home/home.html',
      directives: [If]
    })
    

下一步，我们需要在点击 `button` 的时候显示服务器端返回的单词：


    home/home.js
    
    getRandomWord() {
      this.words.getWord().then((response) => {
        this.word = response;
      });
    }
    

再试着运行一下我们的应用，我们会看到：


![Alt text](http://angular-tips.com/images/posts/angular2intro/3.png)


嗯，终于像那么回事儿了！


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
    

我们存储两个字段，一个是令牌，一个是解码后的令牌，然后我们需要一个 `login` 方法和`logout` 方法来做登录和登出。没什么特别的东西。`localStorage` 和 `jwt_decode` 都是全局的，不需要导入。我们再次使用 `fetch` 函数在服务器端处理请求。


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
    

就像其他组件，我们需要 `Component` 和 `View` 等注解，还需要注入一个叫做 `Auth` 的新服务。


这个组件只有一个方法：用 `Auth` 服务处理登录并且把 `jwt` 令牌塞到 `localStorage` 里面。如果成功的话，我们就跳转到 `/home` 页面。


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
    

注意我们用 `#username` 代替了 `ng-model="username"` 。这是 `Angular 2` 中的绑定方式。


最后一个步，我们简单修改一下 `css`，因为表单稍微有点宽：


    login/login.css
    
    .login {
      width: 40%;
    }
    

同样需要导入这些 `.css` 文件：


    index.css
    
    @import 'bootstrap';
    @import './src/login/login.css';
    

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
    

我们有了一个名为 `isAuth` 的标识，可以让我们在两个 `div` 之间进行切换。


我们也需要将 `Auth` 服务导入进来：

    
    home/home.js
    
    import {Auth} from '../services/auth';
    
    @Component({
      selector: 'home',
      injectables: [Words, Auth]
    })
    

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

    
在不久的将来，就像基础组件 `ui-sref` 一样，`RouteLink` 组件可以让我们避免 `login` 这种做法。


好的，现在让我们链接到 `login` 页面：

![Alt text](http://angular-tips.com/images/posts/angular2intro/4.png)


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
    

现在它好了：


![Alt text](http://angular-tips.com/images/posts/angular2intro/5.png)


然后我们用示例账户（demo / 12345）登录，我们会看到：


![Alt text](http://angular-tips.com/images/posts/angular2intro/6.png)


爽！

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
 

现在我们终于完成了应用：


![Alt text](http://angular-tips.com/images/posts/angular2intro/7.png)


噢 稍等，我忘了登录：


![Alt text](http://angular-tips.com/images/posts/angular2intro/8.png)


我希望你学习 `Angular 2` 能比学图片中的单词还要快 :)


在结束这篇文章之前，作为一个有趣的探索，你可以把 `Login` 组件导入到 `Home` 中，然后把 `Login` 作为一个指令加入到 `View` 的指令数组中，接着把 `<login></login>` 添加到模板的底部。这样做之后，你就可以在的 `Home` 页面使用 `login` 组件完整的表单和功能了，耶！


我想感谢[Matias Gontovnikas][13]和[PatrickJS][14]的demo [angular2-authentication-sample][15]和他们的解释，这给我理解整个流程提供了巨大的帮助。


Jesus Rodriguez 发表于 2015年5月4日 

原文地址：http://angular-tips.com/blog/2015/05/an-introduction-to-angular-2/

外刊君推荐阅读：

- [5 MIN QUICKSTART - Angular 2][16]
- [ECMAScript 6 入门][17]
- [An Angular TODO App][18]

![Alt text](http://pic1.zhimg.com/133fb0b7c34e868aaa90d276895cb1eb_b.jpg)

关注微博：[前端外刊评论][19]


  [1]: https://github.com/angular-tips/GermanWords-backend-koa
  [2]: https://github.com/angular-tips/GermanWords-frontend-angular-2
  [3]: https://github.com/pkozlowski-opensource
  [4]: https://github.com/mgonto
  [5]: https://github.com/gdi2290
  [6]: https://angular.io/docs/js/latest/api/core/bootstrap-function.html
  [7]: https://angular.io/docs/js/latest/api/router/
  [8]: https://angular.io/docs/js/latest/api/di/
  [9]: https://angular.io/docs/js/latest/api/directives/
  [10]: https://angular.io/docs/js/latest/api/core/ViewAnnotation-class.html
  [11]: http://angular-tips.com/images/posts/angular2intro/1.png
  [12]: https://angular.io/docs/js/latest/api/directives/
  [13]: https://github.com/mgonto
  [14]: https://github.com/gdi2290
  [15]: https://github.com/auth0/angular2-authentication-sample
  [16]: https://angular.io/docs/js/latest/quickstart.html
  [17]: http://es6.ruanyifeng.com/#docs/destructuring
  [18]: https://www.youtube.com/watch?v=uD6Okha_Yj0
  [19]: http://weibo.com/u/5368192199