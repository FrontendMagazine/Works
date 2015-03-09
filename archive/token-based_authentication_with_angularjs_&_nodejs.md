# Token-Based Authentication With AngularJS & NodeJS
# 使用 AngularJS & NodeJS 实现基于 token 的认证应用

![Final product image](https://cms-assets.tutsplus.com/uploads/users/487/posts/22543/final_image/token-based-real-life-architecture%20(3).png)
我们将要创建的

Authentication is one of the most important parts of any web application. In this tutorial, we'll be discussing token-based authentication systems and how they differ from traditional login systems. At the end of this tutorial, you'll see a fully working demo written in AngularJS and NodeJS

认证是任何 web 应用中不可或缺的一部分。在这个教程中，我们会讨论基于 token 的认证系统以及它和传统的登录系统的不同。这篇教程的末尾，你会看到一个使用 AngularJS 和 NodeJS 构建的完整的应用。

## Traditional Authentication Systems
## 传统的认证系统

Before proceeding with a token-based authentication system, let's have a look at a traditional authentication system first.

在开始说基于 token 的认证系统之前，我们先看一下传统的认证系统。

1. The user provides a __username__ and __password__ in the login form and clicks __Log In__.
2. After the request is made, validate the user on the backend by querying in the database. If the request is valid, create a session by using the user information fetched from the database, and then return the session information in the response header in order to store the session ID in the browser.
3. Provide the session information for accessing restricted endpoints in the application.
4. If the session information is valid, let the user access specified end points, and respond with the rendered HTML content.

1. 用户在登录域输入 __用户名__ 和 __密码__ ，然后点击 __登录__ ；
2. 请求发送之后，通过在后端查询数据库验证用户的合法性。如果请求有效，使用在数据库得到的信息创建一个 session，然后在响应头信息中返回这个 session 的信息，目的是把这个 session ID 存储到浏览器中；
3. 在访问应用中受限制的端点时提供这个 session 信息；
4. 如果 session 信息有效，允许用户访问受限制的端点，并且把渲染好的 HTML 内容返回。

![](https://cms-assets.tutsplus.com/uploads/users/487/posts/22543/image/traditional-authentication-system-png.png)

Everything is fine until this point. The web application works well, and it is able to authenticate users so that they may access restricted endpoints; however, what happens when you want to develop another client, say for Android, for your application? Will you be able to use the current application to authenticate mobile clients and to serve restricted content? As it currently stands, no. There are two main reasons for this:

在这之前一切都很美好。web 应用正常工作，并且它能够认证用户信息然后可以访问受限的端点；然而当你在开发其他终端时发生了什么呢，比如在 Android 应用中？你还能使用当前的应用去认证移动端并且分发受限制的内容么？真相是，不可以。有两个主要的原因：

1. Sessions and cookies do not make sense for mobile applications. You cannot share sessions or cookies created on the server-side with mobile clients.
2. In the current application, the rendered HTML is returned. In a mobile client, you need something like JSON or XML to be included as the response.

1. 在移动应用上 session 和 cookie 行不通。你无法与移动终端共享服务器创建的 session 和 cookie。
2. 在这个应用中，渲染好的 HTML 被返回。但在移动端，你需要包含一些类似 JSON 或者 XML 的东西包含在响应中。

In this case, you need a client-independent application.
在这个例子中，需要一个独立客户端服务。

## Token-Based Authentication
## 基于 token 的认证

In token-based authentication, cookies and sessions will not be used. A token will be used for authenticating a user for each request to the server. Let's redesign the first scenario with token-based authentication.

在基于 token 的认证里，不再使用 cookie 和session。token 可被用于在每次向服务器请求时认证用户。我们使用基于 token 的认证来重新设计刚才的设想。

It will use the following flow of control:
将会用到下面的控制流程：

1. The user provides a __username__ and __password__ in the login form and clicks __Log In__.
2. After a request is made, validate the user on the backend by querying in the database. If the request is valid, create a token by using the user information fetched from the database, and then return that information in the response header so that we can store the token browser in local storage.
3. Provide token information in every request header for accessing restricted endpoints in the application.
4. If the token fetched from the request header information is valid, let the user access the specified end point, and respond with JSON or XML.

1. 用户在登录表单中输入 __用户名__ 和 __密码__ ，然后点击 __登录__ ；
2. 请求发送之后，通过在后端查询数据库验证用户的合法性。如果请求有效，使用在数据库得到的信息创建一个 token，然后在响应头信息中返回这个的信息，目的是把这个 token 存储到浏览器的本地存储中；
3. 在每次发送访问应用中受限制的终端的请求时提供 token 信息；
4. 如果从请求头信息中拿到的 token 有效，允许用户访问受限制的端点，并且返回 JSON 或者 XML。

In this case, we have no returned session or cookie, and we have not returned any HTML content. That means that we can use this architecture for any client for a specific application. You can see the architecture schema below:

在这个例子中，我们没有返回的 session 或者 cookie，并且我们没有返回任何 HTML 内容。那意味着我们可以把这个架构应用于特定应用的所有客户端中。你可以看一下面的架构体系：

[](https://cms-assets.tutsplus.com/uploads/users/487/posts/22543/image/token-based-authentication-system-png.png)

So, what is this JWT?
那么，这里的 JWT 是什么？

## JWT

JWT stands for __JSON Web Token__ and is a token format used in authorization headers. This token helps you to design communication between two systems in a secure way. Let's rephrase JWT as the "bearer token" for the purposes of this tutorial. A bearer token consists of three parts: header, payload, and signature.

JWT 代表 __JSON Web Token__ ，它是一种用于认证头部的 token 格式。这个 token 帮你实现了在两个系统之间以一种安全的方式传递信息。出于教学目的，我们暂且把 JWT 作为“不记名 token”。一个不记名 token 包含了三部分：header，payload，signature。

- The header is the part of the token that keeps the token type and encryption method, which is also encrypted with base-64.
- The payload includes the information. You can put any kind of data like user info, product info and so on, all of which is stored with base-64 encryption.
- The signature consists of combinations of the header, payload, and secret key. The secret key must be kept securely on the server-side.

- header 是 token 的一部分，用来存放 token 的类型和编码方式，通常是使用 base-64 编码。
- payload 包含了信息。你可以存放任一种信息，比如用户信息，产品信息等。它们都是使用 base-64 编码方式进行存储。
- signature 包括了 header，payload 和密钥的混合体。密钥必须安全地保存储在服务端。

You can see the JWT schema and an example token below:
你可以在下面看到 JWT 刚要和一个实例 token：
![](https://cms-assets.tutsplus.com/uploads/users/487/posts/22543/image/jwt-schema-png.png)

You do not need to implement the bearer token generator as you can find versions that already exist in several languages. You can see some of them below:

你不必关心如何实现不记名 token 生成器函数，因为它对于很多常用的语言已经有多个版本的实现。下面给出了一些：

Language	Library URL
NodeJS	http://github.com/auth0/node-jsonwebtoken
PHP	http://github.com/firebase/php-jwt
Java	http://github.com/auth0/java-jwt
Ruby	http://github.com/progrium/ruby-jwt
.NET	http://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
Python	http://github.com/progrium/pyjwt/

## A Practical Example
## 一个实例

After covering some basic information about token-based authentication, we can now proceed with a practical example. Take a look at the following schema, after which we'll analyze it in more detail:

在讨论了关于基于 token 认证的一些基础知识后，我们接下来看一个实例。看一下下面的几点，然后我们会仔细的分析它：

![](https://cms-assets.tutsplus.com/uploads/users/487/posts/22543/image/real-life-architecture-png.png)

1. The requests are made by several clients such as a web application, a mobile client, etc., to the API for a specific purpose.
2. The requests are made to a service like ``[https://api.yourexampleapp.com](https://api.yourexampleapp.com)``. If lots of people use the application, multiple servers may be required to serve the requested operation.
3. Here, the load balancer is used for balancing requests to best suit the application servers at the back-end. When you make a request to ``[https://api.yourexampleapp.com](https://api.yourexampleapp.com)``, first the load balancer will handle a request, and then it will redirect the client to a specific server.
4. There is one application, and this application is deployed to several servers (server-1, server-2, ..., server-n). Whenever a request is made to ``[https://api.yourexampleapp.com](https://api.yourexampleapp.com)``, the back-end application will intercept the request header and extract token information from the authorization header. A database query will be made by using this token. If this token is valid and has the required permission to access the requested endpoint, it will continue. If not, it will return a 403 response code (which indicates a forbidden status).

1. 多个终端，比如一个 web 应用，一个移动端等向 API 发送特定的请求。
2. 类似 ``[https://api.yourexampleapp.com](https://api.yourexampleapp.com)`` 这样的请求发送到服务层。如果很多人使用了这个应用，需要多个服务器来响应这些请求操作。
3. 这时，负载均衡被用于平衡请求，目的是达到最优化的后端应用服务。当你向 ``[https://api.yourexampleapp.com](https://api.yourexampleapp.com)`` 发送请求，最外层的负载均衡会处理这个请求，然后重定向到指定的服务器。
4. 一个应用可能会被部署到多个服务器上（server-1, server-2, ..., server-n）。当有请求发送到 ``[https://api.yourexampleapp.com](https://api.yourexampleapp.com)`` 时，后端的应用会拦截这个请求头部并且从认证头部中提取到 token 信息。使用这个 token 查询数据库。如果这个 token 有效并且有请求终端数据所必须的许可时，请求会继续。如果无效，会返回 403 状态码（表明一个拒绝的状态）。

## Advantages
## 优势

Token-based authentication comes with several advantages that solve serious problems. Some of them are as follows:

基于 token 的认证在解决棘手的问题时有几个优势：

- __Client Independent Services__. In token-based authentication, a token is transferred via request headers, instead of keeping the authentication information in sessions or cookies. This means there is no state. You can send a request to the server from any type of client that can make HTTP requests.
- __Client Independent Services__ 。在基于 token 的认证，token 通过请求头传输，而不是把认证信息存储在 session 或者 cookie 中。这意味着无状态。你可以从任意一种可以发送 HTTP 请求的终端向服务器发送请求。
- __CDN__. In most current web applications, views are rendered on the back-end and HTML content is returned to the browser. Front-end logic depends on back-end code. There is no need to make such a dependency. This comes with several problems. For example, if you are working with a design agency that implements your front-end HTML, CSS, and JavaScript, you need to take that front-end code and migrate it into your back-end code in order to do some rendering or populating operations. After some time, your rendered HTML content will differ greatly from what the code agency implemented. In token-based authentication, you can develop a front-end project separately from the back-end code. Your back-end code will return a JSON response instead of rendered HTML, and you can put the minified, gzipped version of the front-end code into the CDN. When you go to your web page, HTML content will be served from the CDN, and page content will be populated by API services using the token in the authorization headers
- __CDN__ 。在绝大多数现在的应用中，view 在后端渲染，HTML 内容被返回给浏览器。前端逻辑依赖后端代码。这中依赖真的没必要。而且，带来了几个问题。比如，你和一个设计机构合作，设计师帮你完成了前端的 HTML，CSS 和 JavaScript，你需要拿到前端代码并且把它移植到你的后端代码中，目的当然是为了渲染。修改几次后，你渲染的 HTML 内容可能和设计师完成的代码有了很大的不同。在基于 token 的认证中，你可以开发完全独立于后端代码的前端项目。后端代码会返回一个 JSON 而不是渲染 HTML，并且你可以把最小化，压缩过的代码放到 CDN 上。当你访问 web 页面，HTML 内容由 CDN 提供服务，并且页面内容是通过使用认证头部的 token 的 API 服务所填充。
- __No Cookie-Session (or No CSRF)__. CSRF is a major problem in modern web security because it doesn't check whether a request source is trusted or not. To solve this problem, a token pool is used for sending that token on every form post. In token-based authentication, a token is used in authorization headers, and CSRF does not include that information.
- __No Cookie-Session (or No CSRF)__ 。CSRF 是当代 web 安全中一处痛点，因为它不会去检查一个请求来源是否可信。为了解决这个问题，一个 token 池被用在每次表单请求时发送相关的 token。在基于 token 的认证中，已经有一个 token 应用在认证头部，并且 CSRF 不包含那个信息。
- __Persistent Token Store__. When a session read, write, or delete operation is made in the application, it will make a file operation in the operating system's ``temp`` folder, at least for the first time. Let's say that you have multiple servers and a session is created on the first server. When you make another request and your request drops in another server, session information will not exist and will get an "unauthorized" response. I know, you can solve that with a sticky session. However, in token-based authentication, this case is solved naturally. There is no sticky session problem, because the request token is intercepted on every request on any server.
- __Persistent Token Store__ 。当在应用中进行 session 的读，写或者删除操作时，会有一个文件操作发生在操作系统的 ``temp`` 文件夹下，至少在第一次时。假设有多台服务器并且 session 在第一台服务上创建。当你再次发送请求并且这个请求落在另一台服务器上，session 信息并不存在并且会获得一个“未认证”的响应。我知道，你可以通过一个粘性 session 解决这个问题。然而，在基于 token 的认证中，这个问题很自然就被解决了。没有粘性 session 的问题，因为在每个发送到服务器的请求中这个请求的 token 都会被拦截。

Those are the most common advantages of token-based authentication and communication. That's the end of the theoretical and architectural talk about token-based authentication. Time for a practical example.
这些就是基于 token 的认证和通信中最明显的优势。基于 token 认证的理论和架构就说到这里。下面上实例。

## An Example Application
## 应用实例

You will see two applications to demonstrate token-based authentication:

你会看到两个用于展示基于 token 认证的应用：

1. token-based-auth-backend
2. token-based-auth-frontend

In the back-end project, there will be service implementations, and service results will be in JSON format. There is no view returned in services. In the front-end project, there will be an AngularJS project for front-end HTML and then the front-end app will be populated by AngularJS services to make requests to the back-end services.

在后端项目中，包括服务接口，服务返回的 JSON 格式。服务层不会返回视图。在前端项目中，会使用 AngularJS 向后端服务发送请求。

#### token-based-auth-backend
In the back-end project, there are three main files:
在后端项目中，有三个主要文件：

- ``package.json`` is for dependency management.
- ``models\User.js`` contains a User model that will be used for making database operations about users.
- ``server.js`` is for project bootstrapping and request handling.

- ``package.json`` 用于管理依赖；
- ``models\User.js`` 包含了可能被用于处理关于用户的数据库操作的用户模型；
- ``server.js`` 用于项目引导和请求处理。

That's it! This project is very simple, so that you can understand the main concept easily without doing a deep dive.

就是这样！这个项目非常简单，你不必深入研究就可以了解主要的概念。
````
{
    "name": "angular-restful-auth",
    "version": "0.0.1",
    "dependencies": {
        "express": "4.x",
        "body-parser": "~1.0.0",
        "morgan": "latest",
        "mongoose": "3.8.8",
        "jsonwebtoken": "0.4.0"
    },
    "engines": {
        "node": ">=0.10.0"
    }
}
````

``package.json`` contains dependencies for the project: ``express`` for MVC, ``body-parser`` for simulating post request handling in NodeJS, ``morgan`` for request logging, ``mongoose`` for our ORM framework to connect to MongoDB, and ``jsonwebtoken`` for creating JWT tokens by using our User model. There is also an attribute called ``engines`` that says that this project is made by using NodeJS version >= 0.10.0. This is useful for PaaS services like Heroku. We will also cover that topic in another section.

``package.json``包含了这个项目的依赖：``express`` 用于 MVC，``body-parser`` 用于在 NodeJS  中模拟 post 请求操作，``morgan`` 用于请求登录，``mongoose`` 用于为我们的 ORM 框架连接 MongoDB，最后 ``jsonwebtoken`` 用于使用我们的 User 模型创建 JWT 。如果这个项目使用版本号 >= 0.10.0 的 NodeJS 创建，那么还有一个叫做 ``engines`` 的属性。这对那些像 HeroKu 的 PaaS 服务很有用。我们也会在另外一节中包含那个话题。

````
var mongoose     = require('mongoose');
var Schema       = mongoose.Scema;

var UserSchema   = new Schema({
    email: String,
    password: String,
    token: String
});

module.exports = mongoose.model('User', UserSchema);
````

We said that we would generate a token by using the user model payload. This model helps us to make user operations on MongoDB. In ``User.js``, the user-schema is defined and the User model is created by using a mongoose model. This model is ready for database operations.

上面提到我们可以通过使用用户的 payload 模型生成一个 token。这个模型帮助我们处理用户在 MongoDB 上的请求。在 ``User.js``，user-schema 被定义并且 User 模型通过使用 mogoose 模型被创建。这个模型提供了数据库操作。

Our dependencies are defined, and our user model is defined, so now let's combine all those to construct a service for handling specific requests.

我们的依赖和 user 模型被定义好，现在我们把那些构想成一个服务用于处理特定的请求。

````
// Required Modules
var express    = require("express");
var morgan     = require("morgan");
var bodyParser = require("body-parser");
var jwt        = require("jsonwebtoken");
var mongoose   = require("mongoose");
var app        = express();
````

In NodeJS, you can include a module in your project by using ``require``. First, we need to import the necessary modules into the project:

在 NodeJS 中，你可以使用 ``require`` 包含一个模块到你的项目中。第一步，我们需要把必要的模块引入到项目中：

````
var port = process.env.PORT || 3001;
var User     = require('./models/User');
// Connect to DB
mongoose.connect(process.env.MONGO_URL);
````

Our service will serve through a specific port. If any port variable is defined in the system environment variables, you can use that, or we have defined port ``3001``. After that, the User model is included, and the database connection is established in order to do some user operations. Do not forget to define an environment variable—``MONGO_URL``—for the database connection URL.

服务层通过一个指定的端口提供服务。如果没有在环境变量中指定端口，你可以使用那个，或者我们定义的 ``3001`` 端口。然后，User 模型被包含，并且数据库连接被建立用来处理一些用户操作。不要忘记定义一个 ``MONGO_URL`` 环境变量，用于数据库连接 URL。

````
app.use(bodyParser.urlencoded({ extended: true }));
app.use(bodyParser.json());
app.use(morgan("dev"));
app.use(function(req, res, next) {
    res.setHeader('Access-Control-Allow-Origin', '*');
    res.setHeader('Access-Control-Allow-Methods', 'GET, POST');
    res.setHeader('Access-Control-Allow-Headers', 'X-Requested-With,content-type, Authorization');
    next();
});
````

In the above section, we've made some configurations for simulating an __HTTP__ request handling in NodeJS by using Express. We are allowing requests to come from different domains in order to develop a client-independent system. If you do not allow this, you will trigger a CORS (Cross Origin Request Sharing) error in the web browser.

上一节中，我们已经做了一些配置用于在 NodeJS 中使用 Express 模拟一个 __HTTP__ 请求。我们允许来自不同域名的请求，目的是建立一个独立的客户端系统。如果你没这么做，可能会触发浏览器的 CORS（跨域请求共享）错误。

- ``Access-Control-Allow-Origin`` allowed for all domains.
- You can send ``POST`` and ``GET`` requests to this service.
- ``X-Requested-With`` and ``content-type`` headers are allowed.

- ``Access-Control-Allow-Origin`` 允许所有的域名。
- 你可以向这个设备发送 ``POST`` 和 ``GET`` 请求。
- 允许 ``X-Requested-With`` 和 ``content-type`` 头部。


````
app.post('/authenticate', function(req, res) {
    User.findOne({email: req.body.email, password: req.body.password}, function(err, user) {
        if (err) {
            res.json({
                type: false,
                data: "Error occured: " + err
            });
        } else {
            if (user) {
               res.json({
                    type: true,
                    data: user,
                    token: user.token
                }); 
            } else {
                res.json({
                    type: false,
                    data: "Incorrect email/password"
                });    
            }
        }
    });
});
````

We have imported all of the required modules and defined our configuration, so now it's time to define request handlers. In the above code, whenever you make a ``POST`` request to ``/authenticate`` with username and password, you will get a ``JWT`` token. First, the database query is processed by using a username and password. If a user exists, the user data will be returned with its token. But, what if there is no such user matching the username and/or password?

我们已经引入了所需的全部模块并且定义了配置文件，所以是时候来定义请求处理函数了。在上面的代码中，当你提供了用户名和密码向 ``/authenticate`` 发送一个 ``POST`` 请求时，你将会得到一个 ``JWT``。首先，通过用户名和密码查询数据库。如果用户存在，用户数据将会和它的 token 一起返回。但是，如果没有用户名或者密码不正确，要怎么处理呢？


````
app.post('/signin', function(req, res) {
    User.findOne({email: req.body.email, password: req.body.password}, function(err, user) {
        if (err) {
            res.json({
                type: false,
                data: "Error occured: " + err
            });
        } else {
            if (user) {
                res.json({
                    type: false,
                    data: "User already exists!"
                });
            } else {
                var userModel = new User();
                userModel.email = req.body.email;
                userModel.password = req.body.password;
                userModel.save(function(err, user) {
                    user.token = jwt.sign(user, process.env.JWT_SECRET);
                    user.save(function(err, user1) {
                        res.json({
                            type: true,
                            data: user1,
                            token: user1.token
                        });
                    });
                })
            }
        }
    });
});
````

When you make a ``POST`` request to ``/signin`` with username and password, a new user will be created by using posted user information. On the ``19th`` line, you can see that a new JSON token is generated by using the ``jsonwebtoken`` module, which has been assigned to the ``jwt`` variable. The authentication part is OK. What if we try to access a restricted endpoint? How can we manage to access that endpoint?

当你使用用户名和密码向 ``/signin`` 发送 ``POST`` 请求时，一个新的用户会通过所请求的用户信息被创建。在 ``第 19`` 行，你可以看到一个新的 JSON 通过 ``jsonwebtoken`` 模块生成，然后赋值给 ``jwt`` 变量。认证部分已经完成。我们访问一个受限的终端数据会怎么样呢？我们又要如何访问那个数据终点呢？

````
app.get('/me', ensureAuthorized, function(req, res) {
    User.findOne({token: req.token}, function(err, user) {
        if (err) {
            res.json({
                type: false,
                data: "Error occured: " + err
            });
        } else {
            res.json({
                type: true,
                data: user
            });
        }
    });
});
````

When you make a ``GET`` request to ``/me``, you will get the current user info, but in order to continue with the requested endpoint, the ``ensureAuthorized`` function will be executed.

当你向 ``/me`` 发送 ``GET`` 请求时，你将会得到当前用户的信息，但是为了继续请求端点， ``ensureAuthorized`` 函数将会执行。

````
function ensureAuthorized(req, res, next) {
    var bearerToken;
    var bearerHeader = req.headers["authorization"];
    if (typeof bearerHeader !== 'undefined') {
        var bearer = bearerHeader.split(" ");
        bearerToken = bearer[1];
        req.token = bearerToken;
        next();
    } else {
        res.send(403);
    }
}
````

In this function, request headers are intercepted and the ``authorization`` header is extracted. If a bearer token exists in this header, that token is assigned to ``req.token`` in order to be used throughout the request, and the request can be continued by using ``next()``. If a token does not exist, you will get a 403 (Forbidden) response. Let's go back to the handler ``/me``, and use ``req.token`` to fetch user data with this token. Whenever you create a new user, a token is generated and saved in the user model in DB. Those tokens are unique.

在这个函数中，请求头部被拦截并且 ``authorization`` 头部被提取。如果头部中存在一个不记名  token，通过调用 ``next()`` 函数，请求继续。如果 token 不存在，你会得到一个 403（Forbidden）返回。我们回到  ``/me`` 事件处理函数，并且使用 ``req.token`` 获取这个 token 对应的用户数据。当你创建一个新的用户，会生成一个 token 并且存储到数据库的用户模型中。那些 token 都是唯一的。

We have only three handlers for this simple project. After that, you will see;

这个简单的例子中已经有三个事件处理函数。然后，你将看到；

````
process.on('uncaughtException', function(err) {
    console.log(err);
});
````

The NodeJS app may crash if an error occurs. With the above code, that crash is prevented and an error log is printed in the console. And finally, we can start the server by using the following code snippet. 

当程序出错时 NodeJS 应用可能会崩溃。添加上面的代码可以拯救它并且一个错误日志会打到控制台上。最终，我们可以使用下面的代码片段启动服务。

````
// Start Server
app.listen(port, function () {
    console.log( "Express server listening on port " + port);
});
````

To sum up:
总结一下：

- Modules are imported.
- Configurations are made.
- Request handlers are defined.
- A middleware is defined in order to intercept restricted endpoints.
- The server is started.

- 引入模块
- 正确配置
- 定义请求处理函数
- 定义用来拦截受限终点数据的中间件
- 启动服务

We are done with the back-end service. So that it can be used by multiple clients, you can deploy this simple server application to your servers, or maybe you can deploy in Heroku. There is a file called ``Procfile`` in the project's root folder. Let's deploy our service in Heroku.

我们已经完成了后端服务。到现在，应用已经可以被多个终端使用，你可以部署这个简单的应用到你的服务器上，或者部署在 Heroku。有一个叫做 ``Procfile`` 的文件在项目的根目录下。现在把服务部署到 Heroku。

#### Heroku Deployment
#### Heroku 部署

You can clone the back-end project from this [GitHub repository](http://github.com/cubuzoa/token-based-auth-backend).

你可以在这个 [GitHub 库](http://github.com/cubuzoa/token-based-auth-backend)下载项目的后端代码。

I will not be discussing how to create an app in Heroku; you can refer [to this article](http://devcenter.heroku.com/articles/creating-apps) for creating a Heroku app if you have not done this before. After you create your Heroku app, you can add a destination to your current project by using the following command:

我不会教你如何在 Heroku 如何创建一个应用；如果你还没有做过这个，你可以查阅[这篇文章](http://devcenter.heroku.com/articles/creating-apps)。创建完 Heroku 应用，你可以使用下面的命令为你的项目添加一个地址：

````
git remote add heroku <your_heroku_git_url>
````

Now you have cloned a project and added a destination. After ``git add`` and ``git commit``, you can push your code to Heroku by performing ``git push heroku master``. When you successfully push a project, Heroku will perform the ``npm install`` command to download dependencies into the ``temp`` folder on Heroku. After that, it will start your application and you can access your service by using the HTTP protocol.

现在，你已经克隆了这个项目并且添加了地址。在 ``git add`` 和 ``git commit`` 后，你可以使用 ``git push heroku master`` 命令将你的代码推到 Heroku。当你成功将项目推送到仓库，Heroku 会自动执行 ``npm install`` 命令将依赖文件下载到 Heroku 的 ``temp`` 文件夹。然后，它会启动你的应用，因此你就可以使用 HTTP 协议访问这个服务。

#### token-based-auth-frontend
In the front-end project, you will see an AngularJS project. Here, I'll only mention the main sections in the front-end project, because AngularJS is not something that can be covered within a single tutorial.

在前端项目中，将会使用 AngularJS。在这里，我只会提到前端项目中的主要内容，因为 AngularJS 的相关知识不会包括在这个教程里。

You can clone the project from this [GitHub repository](https://github.com/cubuzoa/token-based-auth-frontend). In this project, you will see the following folder structure:

你可以在这个 [GitHub 库](https://github.com/cubuzoa/token-based-auth-frontend)下载源码。在这个项目中，你会看下下面的文件结构：
[Folder Structure](https://cms-assets.tutsplus.com/uploads/users/487/posts/22543/image/folder%20structure.png)

``ngStorage.js`` is a library for AngularJS to manipulate local storage operations. Also, there is a main layout ``index.html`` and partials that extend the main layout under the ``partials folder``. ``controllers.js`` is for defining our controller actions in the front-end. ``services.js`` is for making service requests to our service that I mentioned in the previous project. We have a bootstrap-like file called ``app.js`` and in this file, configurations and module imports are applied. Finally, ``client.js`` is for serving static HTML files (or just ``index.html``, in this case); this helps us to serve static HTML files when you deploy to a server without using Apache or any other web servers.

``ngStorage.js`` 是一个用于操作本地存储的 AngularJS 类库。此外，有一个全局的 layout 文件 ``index.html`` 并且在 ``partials folder`` 文件夹里还有一些用于扩展全局 layout 的部分。 ``controllers.js`` 用于在前端定义我们 controller 的 action。 ``services.js`` 用于向我们在上一个项目中提到的服务发送请求。还有一个 ``app.js`` 文件，它里面有配置文件和模块引入。最后，``client.js`` 为静态 HTML 文件（或者仅仅 ``index.html``，在这里例子中）；当你没有使用 Apache 或者任何其他的 web 服务器时，它可以为静态的 HTML 文件提供服务。


````
...
<script src="//cdnjs.cloudflare.com/ajax/libs/jquery/2.1.1/jquery.min.js"></script>
<script src="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/js/bootstrap.min.js"></script>
<script src="//cdnjs.cloudflare.com/ajax/libs/angular.js/1.2.20/angular.min.js"></script>
<script src="//cdnjs.cloudflare.com/ajax/libs/angular.js/1.2.20/angular-route.min.js"></script>
<script src="/lib/ngStorage.js"></script>
<script src="/lib/loading-bar.js"></script>
<script src="/scripts/app.js"></script>
<script src="/scripts/controllers.js"></script>
<script src="/scripts/services.js"></script>
</body>

````

In the main layout HTML file, all of the required JavaScript files are included for AngularJS-related libraries, as well as our custom controller, service, and app file.

在全局的 layout 文件中，AngularJS 所需的全部 JavaScript 文件都被包含，包括自定义的控制器，服务和应用文件。

````
'use strict';

/* Controllers */

angular.module('angularRestfulAuth')
    .controller('HomeCtrl', ['$rootScope', '$scope', '$location', '$localStorage', 'Main', function($rootScope, $scope, $location, $localStorage, Main) {

        $scope.signin = function() {
            var formData = {
                email: $scope.email,
                password: $scope.password
            }

            Main.signin(formData, function(res) {
                if (res.type == false) {
                    alert(res.data)    
                } else {
                    $localStorage.token = res.data.token;
                    window.location = "/";    
                }
            }, function() {
                $rootScope.error = 'Failed to signin';
            })
        };

        $scope.signup = function() {
            var formData = {
                email: $scope.email,
                password: $scope.password
            }

            Main.save(formData, function(res) {
                if (res.type == false) {
                    alert(res.data)
                } else {
                    $localStorage.token = res.data.token;
                    window.location = "/"    
                }
            }, function() {
                $rootScope.error = 'Failed to signup';
            })
        };

        $scope.me = function() {
            Main.me(function(res) {
                $scope.myDetails = res;
            }, function() {
                $rootScope.error = 'Failed to fetch details';
            })
        };

        $scope.logout = function() {
            Main.logout(function() {
                window.location = "/"
            }, function() {
                alert("Failed to logout!");
            });
        };
        $scope.token = $localStorage.token;
    }])
````

In the above code, the ``HomeCtrl`` controller is defined and some required modules are injected like ``$rootScope`` and ``$scope``. Dependency injection is one of the strongest properties of AngularJS. ``$scope`` is the bridge variable between controllers and views in AngularJS that means you can use ``test`` in view if you defined it in a specified controller like ``$scope.test=.... ``

在上面的代码中，``HomeCtrl`` 控制器被定义并且一些所需的模块被注入（比如 ``$rootScope`` 和 ``$scope``）。依赖注入是 AngularJS 最强大的属性之一。 ``$scope`` 是 AngularJS 中的一个存在于控制器和视图之间的中间变量，这意味着你可以在视图中使用 ``test``，前提是你在特定的控制器中定义了  ``$scope.test=.... ``。

In this controller, some utility functions are defined, such as:
在控制器中，一些工具函数被定义，比如：

- ``signin`` to set up a sign-in button on the sign-in form
- ``signup`` for sign-up form handling
- ``me`` for assigning the Me button in the layout

- ``signin`` 可以在登录表单中初始化一个登录按钮；
- ``signup`` 用于处理注册操作；
- ``me`` 可以在 layout 中生生一个 Me 按钮。


In the main layout, in the main menu list, you can see the ``data-ng-controller`` attribute with a value ``HomeCtrl``. That means that this menu ``dom`` element can share scope with ``HomeCtrl``. When you click the sign-up button in the form, the sign-up function in the controller file will be executed, and in this function, the sign-up service is used from the ``Main`` service that is already injected in this controller. 

在全局 layout 和主菜单列表中，你可以看到 ``data-ng-controller`` 这个属性，它的值是 ``HomeCtrl``。那意味着这个菜单的 ``dom`` 元素可以和 ``HomeCtrl``共享作用域。当你点击表单里的 ``sign-up`` 按钮时，控制器文件中的 ``sign-up`` 函数将会执行，并且在这个函数中，使用的登录服务来自于已经注入到这个控制器的 ``Main`` 服务。

The main structure is ``view -> controller -> service``. This service makes simple Ajax requests to the back-end in order to get specific data.
主要的结构是 ``view -> controller -> service``。这个服务向后端发送了简单的 Ajax 请求，目的是获取指定的数据。

````
'use strict';

angular.module('angularRestfulAuth')
    .factory('Main', ['$http', '$localStorage', function($http, $localStorage){
        var baseUrl = "your_service_url";
        function changeUser(user) {
            angular.extend(currentUser, user);
        }

        function urlBase64Decode(str) {
            var output = str.replace('-', '+').replace('_', '/');
            switch (output.length % 4) {
                case 0:
                    break;
                case 2:
                    output += '==';
                    break;
                case 3:
                    output += '=';
                    break;
                default:
                    throw 'Illegal base64url string!';
            }
            return window.atob(output);
        }

        function getUserFromToken() {
            var token = $localStorage.token;
            var user = {};
            if (typeof token !== 'undefined') {
                var encoded = token.split('.')[1];
                user = JSON.parse(urlBase64Decode(encoded));
            }
            return user;
        }

        var currentUser = getUserFromToken();

        return {
            save: function(data, success, error) {
                $http.post(baseUrl + '/signin', data).success(success).error(error)
            },
            signin: function(data, success, error) {
                $http.post(baseUrl + '/authenticate', data).success(success).error(error)
            },
            me: function(success, error) {
                $http.get(baseUrl + '/me').success(success).error(error)
            },
            logout: function(success) {
                changeUser({});
                delete $localStorage.token;
                success();
            }
        };
    }
]);
````

In the above code, you can see service functions like making requests for authenticating. In controller.js, you may have already realised that there are functions like ``Main.me``. This ``Main`` service has been injected in the controller, and in the controller, the services belonging to this service are called directly. 

在上面的代码中，你会看到服务函数请求认证。在 controller.js 中，你可能已经看到了有类似 ``Main.me`` 的函数。这里的 ``Main`` 服务已经注入到控制器，并且在它内部，属于这个服务的其他服务直接被调用。

These functions are simply Ajax requests to our service which we deployed together. Do not forget to put the service URL in ``baseUrl`` in the above code. When you deploy your service to Heroku, you will get a service URL like ``appname.herokuapp.com``. In the above code, you will set ``var baseUrl = "appname.herokuapp.com"``. 

这些函数式仅仅是简单地向我们部署的服务器集群发送 Ajax 请求。不要忘记在上面的代码中把服务的 URL 放到 ``baseUrl`` 。当你把服务部署到 Heroku，你会得到一个类似 ``appname.herokuapp.com`` 的服务 URL。在上面的代码中，你要设置 ``var baseUrl = "appname.herokuapp.com"``。

In the sign-up or sign-in part of the application, the bearer token responds to the request and this token is saved to local storage. Whenever you make a request to a service in the back-end, you need to put this token in the headers. You can do this by using AngularJS interceptors.

在应用的注册或者登录部分，不记名 token 响应了这个请求并且这个 token 被存储到本地存储中。当你向后端请求一个服务时，你需要把这个 token 放在头部中。你可以使用 AngularJS 的拦截器实现这个。

````
$httpProvider.interceptors.push(['$q', '$location', '$localStorage', function($q, $location, $localStorage) {
            return {
                'request': function (config) {
                    config.headers = config.headers || {};
                    if ($localStorage.token) {
                        config.headers.Authorization = 'Bearer ' + $localStorage.token;
                    }
                    return config;
                },
                'responseError': function(response) {
                    if(response.status === 401 || response.status === 403) {
                        $location.path('/signin');
                    }
                    return $q.reject(response);
                }
            };
        }]);
        
 ````
In the above code, every request is intercepted and an authorization header and value are put in the headers.
在上面的代码中，每次请求都会被拦截并且会把认证头部和值放到头部中。

In the front-end project, we have some partial pages like ``signin``, ``signup``, ``profile details``, and ``vb``. These partial pages are related with specific controllers. You can see that relation in ``app.js``:

在前端项目中，会有一些不完整的页面，比如 ``signin``，``signup``，``profile details`` 和 ``vb``。这些页面与特定的控制器相关。你可以在 ``app.js`` 中看到：


````
angular.module('angularRestfulAuth', [
    'ngStorage',
    'ngRoute'
])
.config(['$routeProvider', '$httpProvider', function ($routeProvider, $httpProvider) {

    $routeProvider.
        when('/', {
            templateUrl: 'partials/home.html',
            controller: 'HomeCtrl'
        }).
        when('/signin', {
            templateUrl: 'partials/signin.html',
            controller: 'HomeCtrl'
        }).
        when('/signup', {
            templateUrl: 'partials/signup.html',
            controller: 'HomeCtrl'
        }).
        when('/me', {
            templateUrl: 'partials/me.html',
            controller: 'HomeCtrl'
        }).
        otherwise({
            redirectTo: '/'
        });
````

As you can easily understand in the above code, when you go to ``/``, the ``home.html`` page will be rendered. Another example: if you go to ``/signup``, ``signup.html`` will be rendered. This rendering operation will be done in the browser, not on the server-side.

如上面代码所示，当你访问 ``/``，``home.html`` 将会被渲染。再看一个例子：如果你访问 ``/signup``，``signup.html`` 将会被渲染。渲染操作会在浏览器中完成，而不是在服务端。

## Conclusion
## 总结

You can see how everything we discussed in this tutorial works into practice by checking out this [working demo](http://token-based-auth.herokuapp.com/).

你可以通过检出这个[实例](http://token-based-auth.herokuapp.com/)看到我们在这个教程中所讨论的项目是如何工作的。

Token-based authentication systems help you to construct an authentication/authorization system while you are developing client-independent services. By using this technology, you will just focus on your services (or APIs). 

基于 token 的认证系统帮你建立了一个认证/授权系统，当你在开发客户端独立的服务时。通过使用这个技术，你只需关注于服务（或者 API）。

The authentication/authorization part will be handled by the token-based authentication system as a layer in front of your services. You can access and use services from any client like web browsers, Android, iOS, or a desktop client.

认证/授权部分将会被基于 token 的认证系统作为你的服务前面的层来处理。你可以访问并且使用来自于任何像 web 浏览器，Android，iOS 或者一个桌面客户端这类服务。

原文：[Token-Based Authentication With AngularJS & NodeJS](http://code.tutsplus.com/tutorials/token-based-authentication-with-angularjs-nodejs--cms-22543)