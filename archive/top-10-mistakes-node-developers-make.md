# Top 10 Mistakes Node.js Developers Make

# 10个Node.js开发者最易犯的错误

## Introduction

##简介

Node.js has seen an important growth in the past years, with big companies such as Walmart or PayPal adopting it. More and more people are picking up Node and publishing modules to NPM at such a pace that exceeds other languages. However, the Node philosophy can take a bit to get used to, especially if you have switched from another language.

Node.js 在过去的几年中迅猛发展，沃尔玛，PayPal 等大公司均开始采用它。越来越多的人使用 Node 开发并在 NPM 上发布模块，如此快的速度已经[超越了其他语言][1]。然而，Node 的理念需要花点时间适应，尤其是对于那些从其他语言切换过来的工程师而言。

In this article we will talk about the most common mistakes Node developers make and how to avoid them. You can find the source code for the examples on github.

这篇文章中，我们将要探讨那些 Node 开发者常犯的错误，以及如何避免它们。你可以在 github 上找到实例中的代码。

## 1 Not using development tools

## 1 不使用开发工具

- nodemon or supervisor for automatic restart
- In-browser live reload (reload after static files and/or views change)

- 使用 nodemon 或者 supervisor 来自动重启
- 浏览器端的 live reload（当静态文件或模板文件改变后重新加载）

Unlike other languages such as PHP or Ruby, Node requires a restart when you make changes to the source code. Another thing that can slow you down while creating web applications is refreshing the browser when the static code changes. While you can do these things manually, there are better solutions out there.

当你改变源代码时需要重启 Node，这点与ruby或者php等语言不同。此外当你开发 web 应用时，有件事情会拖累你：当静态代码更新时需要刷新网页。现在有一些更好的方案来代替手动执行这些操作。

### 1.1 Automating restarts

### 1.1 自动重启

Most of us are probably used to saving a file in the editor, hit [CTRL+C] to stop the application and then restart it by pressing the [UP] arrow and [Enter]. However you can automate this repetitive task and make your development process easier by using existing tools such as:

- nodemon
- node-supervisor
- forever

我们经常做的操作：在编辑器中保存文件，按下 CTRL+C 终止应用，按一下[上]，再按一下[Enter]键再次启动。你可以自动化这些操作，使用以下的工具来解放你的双手：

 - nodemon 
 - node-supervisor 
 - forever

What these modules do is to watch for file changes and restart the server for you. Let us take nodemon for example. First you install it globally:

这些模块会监听文件改变并帮你重启应用。我们以 nodemon 为例，首先你需要全局安装这个模块：

    npm i nodemon -g

Then you should simply swap the node command for the nodemon command:

接下来你需要使用 nodemon 命令，替代 node 命令来启动应用：

    $ nodemon server.js 
    14 Nov 21:23:23 - [nodemon] v1.2.1 
    14 Nov 21:23:23 - [nodemon] to restart at any time, enter `rs`     14 Nov 21:23:23 - [nodemon] watching: *.* 
    14 Nov 21:23:23 - [nodemon] starting `node server.js` 
    14 Nov 21:24:14 - [nodemon] restarting due to changes... 
    14 Nov 21:24:14 - [nodemon] starting `node server.js`

Among the existing options for nodemon or node-supervisor, probably the most popular one is to ignore specific files or folders.

此外 nodemon 或 node-supervisor 也提供了一些选项，最常用的是忽略特定文件或文件夹。
  
### 1.2 Automatic browser refresh

### 1.2自动刷新浏览器

Besides reloading the Node application when the source code changes, you can also speed up development for web applications. Instead of manually triggering the page refresh in the browser, we can automate this as well using tools such as livereload.

除了当源代码更改时重启应用,你还有另外的途径来提高 web 应用的开发效率。你也可以通过使用 livereload 等工具代替手动刷新。


They work similarly to the ones presented before, because they watch for file changes in certain folders and trigger a browser refresh in this case (instead of a server restart). The refresh is done either by a script injected in the page or by a browser plugin.

这些工具跟之前介绍的工具原理类似，通过监听特定文件夹中的文件改变，来触发浏览器的刷新(不需要重启服务)。浏览器的刷新是通过脚本注入页面或浏览器插件完成。

Instead of showing you how to use livereload, this time we will create a similar tool ourselves with Node. It will do the following:

- Watch for file changes in a folder;
- Send a message to all connected clients using server-sent events; and
- Trigger the page reload.

这次我们不展示如何使用livereload，而是创建一个类似的工具。它将执行以下操作:

 - 监听文件夹中文件的变化;
 - 使用[服务器端事件](http://html5doctor.com/server-sent-events/)发送消息到所有连接的客户端；
 - 触发页面重新加载。

First we should install the NPM dependencies needed for the project:

express - for creating the sample web application
watch - to watch for file changes
sendevent - server-sent events, SSE (an alternative would have been websockets)
uglify-js - for minifying the client-side JavaScript files
ejs - view templates

Next we will create a simple Express server that renders a home view on the front page:

首先我们应该安装项目所需的 NPM 依赖:

 - express——用于创建示例web应用程序 
 - watch——监听文件改变
 - sendevent——服务器端事件,SSE(另一种是选择是websockets)
 - uglify-js——压缩客户端JavaScript文件
 - ejs——视图模板

接下来我们建立一个简单的express服务器，并渲染一个首页：

    var express = require('express'); 
    var app = express(); 
    var ejs = require('ejs'); 
    var path = require('path'); 
    var PORT = process.env.PORT || 1337; 
    
    // view engine setup 设置渲染引擎
    app.engine('html', ejs.renderFile); 
    app.set('views', path.join(__dirname, 'views')); 
    app.set('view engine', 'html'); 
    
    // serve an empty page that just loads the browserify bundle
    // 渲染一个空页面
    app.get('/', function(req, res) {
        res.render('home');
    }); 
    app.listen(PORT); 
    console.log('server started on port %s', PORT);
    
Since we are using Express we will also create the browser-refresh tool as an Express middleware. The middleware will attach the SSE endpoint and will also create a view helper for the client script. The arguments for the middleware function will be the Express app and the folder to be monitored. Since we know that, we can already add the following lines before the view setup (inside server.js):

由于我们使用 express，我们把这个浏览器自动刷新的工具作为 express 的中间件使用。中间件将带有 SSE 功能，并且创建一个模板的 helper 来引入浏览器端脚本。而中间件的参数有两个：express 应用和要监视的文件夹。依据这样的设计，我们可以预先在模板设置前使用以下代码来加载这个中间件(server.js内):

    var reloadify = require('./lib/reloadify'); 
    reloadify(app, __dirname + '/views');

We are watching the /views folder for changes. And now for the middleware:

我们需要监听 views 目录下的改动。中间件则长这样：

    var sendevent = require('sendevent');
    var watch = require('watch');
    var uglify = require('uglify-js');
    var fs = require('fs');
    var ENV = process.env.NODE_ENV || 'development'; 
    
    // create && minify static JS code to be included in the page
    //创建并压缩要嵌入到页面中的js静态文件
    var polyfill = fs.readFileSync(__dirname + '/assets/eventsource-polyfill.js', 'utf8');
    var clientScript = fs.readFileSync(__dirname + '/assets/client-script.js', 'utf8');
    var script = uglify.minify(polyfill + clientScript, {
            fromString : true
        }).code;
    function reloadify(app, dir) {
        if (ENV !== 'development') {
            app.locals.watchScript = '';
            return;
        }
        // create a middlware that handles requests to `/eventstream`  建立一个中间件来处理来自/eventstream的请求
        var events = sendevent('/eventstream');
        app.use(events);
        watch.watchTree(dir, function (f, curr, prev) {
            events.broadcast({
                msg : 'reload'
            });
        });
    
        // assign the script to a local var so it's accessible in the view 把这个脚本挂到应用的local上，以便view可以获取到
    
        app.locals.watchScript = '<script>' + script + '</script>';
    }
    
    module.exports = reloadify;

As you might have noticed, if the environment isn't set to 'development' the middleware won't do anything. This means we won't have to remove it for production.

The frontend JS file is pretty simple, it will just listen to the SSE messages and reload the page when needed:


你可能已经注意到,若环境变量不为“development”，中间件不会做任何事情。这意味着我们不需要在生产环境中删除这个中间件。

前端 JS 文件非常简单,它只会听 SSE 消息并在需要时重新加载页面:

     (function() {
    
        function subscribe(url, callback) {
          var source = new window.EventSource(url);
    
          source.onmessage = function(e) {
            callback(e.data);
          };
    
          source.onerror = function(e) {
            if (source.readyState == window.EventSource.CLOSED) return;
    
            console.log('sse error', e);
          };
    
          return source.close.bind(source);
        };
    
        subscribe('/eventstream', function(data) {
          if (data && /reload/.test(data)) {
            window.location.reload();
          }
        });
    
      }());
      
      
The eventsource-polyfill.js is Remy Sharp's polyfill for SSE. Last but not least, the only thing left to do is to include the frontend generated script into the page (/views/home.html) using the view helper:

SSE 的 polyfill，eventsource-polyfill.js 是由 Remy Sharp 提供的。最后我们需要使用 helper 来把这个前端脚本插入到页面中:

     ...
      <%- watchScript %>
      ...

Now every time you make a change to the home.html page the browser will automatically reload the home page of the server for you

这样当你每次修改home.html时，浏览器都会帮你重新加载页面。
  
  
## 2 Blocking the event loop

## 2 阻塞[event loop][2] 

Since Node.js runs on a single thread, everything that will block the event loop will block everything. That means that if you have a web server with a thousand connected clients and you happen to block the event loop, every client will just...wait.

由于Nodejs运行在一个单线程上,当event loop被阻塞后将阻塞一切。这意味着,如果你的web服务器与1000个客户端同时链接，event loop被阻塞后,每个客户只会……傻等。


Here are some examples on how you might do that (maybe unknowingly):

Parsing a big json payload with the JSON.parse function;
Trying to do syntax highlighting on a big file on the backend (with something like Ace or highlight.js); or
Parsing a big output in one go (such as the output of a git log command from a child process).
The thing is that you may do these things unknowingly, because parsing a 15 Mb output doesn't come up that often, right? It's enough for an attacker to catch you off-guard and your entire server will be DDOS-ed.

这里有一些例子告诉你如何做到(可能在不知情的情况下):

 - 使用JSON.parse函数解析超大json。
 - 想在后台做大文件的语法高亮显示(类似Ace或highlight.js);
 - 解析一个庞大的输出流(如git命令产生的日志，从子进程输出)。

这意味着,你可能不知不觉地处理这些事情,因为解析15 Mb的输出并不经常出现。这足以让攻击者打你个措手不及，以至于整个服务器被DDOS掉。


Luckily you can monitor the event loop delay to detect anomalies. This can be achieve either via proprietary solutions such as StrongOps or by using open-source modules such as blocked.

The idea behind these tools is to accurately track the time spend between an interval repeatedly and report it. The time difference is calculated by getting the time at moment A and moment B, subtracting the time at moment A from moment B and also subtracting the time interval.

幸运的是，你可以监视eventloop延迟来检测异常。这可以通过StrongOps等专有的解决方案，通过使用开源模块比如blocked 来解决。

这些工具背后的原理是准确地反复跟踪一个interval所消耗的时间并报告出来。时差是通过获取A和B时刻的时间,减去A时刻到B时刻的时间，再减去设定的时间间隔后计算而来。


Below there's an example on how to achieve that. It does the following:

Retrieve the high-resolution time between the current time and the time passed as a param;
Determines the delay of the event loop at regular intervals;
Displays the delay in green or red, in case it exceeds the threshold; then
To see it in action, each 300 miliseconds a heavy computation is executed.
The source code for the example is the following:

下面的一个例子描述了如何实现:

- 获取高精度的当前时间与参数传递的时间的差;
- 定义定期事件循环的延迟;
- 使用绿色显示延迟,如果它超过阈值则使用红色显示。

为了展示实际效果,我们每隔300ms执行一次大计算量的代码。
源代码示例如下:

    var getHrDiffTime = function(time) {
        // ts = [seconds, nanoseconds]
        var ts = process.hrtime(time);
        // convert seconds to miliseconds and nanoseconds to miliseconds as well
        return (ts[0] * 1000) + (ts[1] / 1000000);
      };
    
      var outputDelay = function(interval, maxDelay) {
        maxDelay = maxDelay || 100;
    
        var before = process.hrtime();
    
        setTimeout(function() {
          var delay = getHrDiffTime(before) - interval;
    
          if (delay < maxDelay) {
            console.log('delay is %s', chalk.green(delay));
          } else {
            console.log('delay is %s', chalk.red(delay));
          }
    
          outputDelay(interval, maxDelay);
        }, interval);
      };
    
      outputDelay(300);
    
      // heavy stuff happening every 2 seconds here
      setInterval(function compute() {
        var sum = 0;
    
        for (var i = 0; i <= 999999999; i++) {
          sum += i * 2 - (i + 1);
        }
      }, 2000);
  

You must install the chalk before running it. After running the example you should see the following output in the terminal:

在跑这段代码前你需要安装[chalk][3]。跑完代码后你会在终端看到以下输出：

![此处输入图片的描述][4]

As said before, existing open source modules are doing it similarly so use them with confidence:

像之前说的一样，使用一些现成的开源模块也可以完成这些，可以参考以下链接：


https://github.com/hapijs/heavy/blob/bbc98a5d7c4bddaab94d442210ca694c7cd75bde/lib/index.js#L70
https://github.com/tj/node-blocked/blob/master/index.js#L2-L14



If you couple this technique with profiling, you can determine exactly which part of your code caused the delay.

通过这种技术来分析,你可以准确地确定哪一部分的代码导致了延迟。

## 3 Executing a callback multiple times

## 3 多次执行一个回调

How many times have you saved a file and reloaded your Node web app only to see it crash really fast? The most likely scenario is that you executed the callback twice, meaning you forgot to return after the first time.
　
有多少次你保存一个文件，重新启动nodejs web应用程序，然后程序很快崩溃了?
　
最可能的情况是,你的回调执行了两次,你忘了第一次后返回。

Let's create an example to replicate this situation. We will create a simple proxy server with some basic validation. To use it install the request dependency, run the example and open (for instance) http://localhost:1337/?url=http://www.google.com/. The source code for our example is the following:

让我们创建一个示例来重现这种情况。我们将创建一个简单的代理服务器，带有一些基本的验证。跑这个例子之前请先安装依赖,之后运行示例，打开浏览器输入http://localhost:1337/?url=http://www.google.com/
　　

我们的示例的源代码如下:
    
    var request = require('request');
    var http = require('http');
    var url = require('url');
    var PORT = process.env.PORT || 1337;
    var expression =/[-a-zA-Z0-9@:%_\+.~#?&//=]{2,256}\.[a-z]{2,4}\b(\/[-a-zA-Z0-9@:%_\+.~#?&//=]*)?/gi;
    var isUrl = new RegExp(expression);
    var respond = function(err, params) {
        var res = params.res;
        var body = params.body;
        var proxyUrl = params.proxyUrl;
        res.setHeader('Content-type', 'text/html; charset=utf-8');
        if (err) {
            console.error(err);
            res.end('An error occured. Please make sure the domain exists.');
        } else {
            res.end(body);
        }
    };
    http.createServer(function(req, res) {
                var queryParams = url.parse(req.url, true).query;
                var proxyUrl = queryParams.url;
                if (!proxyUrl || (!isUrl.test(proxyUrl))) {
                    res.writeHead(200, {
                        'Content-Type': 'text/html'
                    });
                    res.write("Please provide a correct URL param. For ex: ");
                    res.end("<a href='http://localhost:1337/?url=http://www.google.com/'>http://localhost:1337/?url=http://www.google.com/</a>");
                } else { 
        // ------------------------ 
        // Proxying happens here  在这里进行代理
        // TO BE CONTINUED  看下文
        // ------------------------ 
        }
     }).listen(PORT);
     
The source code above contains almost everything except the proxying itself, because I want you to take a closer look at it:

上面的源代码除了代理部分都包括了，现在我希望你仔细研究下代理部分:

    request(proxyUrl, function(err, r, body) {
    if (err) {
        respond(err, {
            res: res,
            proxyUrl: proxyUrl
        });
    }
    respond(null, {
        res: res,
        body: body,
        proxyUrl: proxyUrl
    });
    });
    
In the callback we have handled the error condition, but forgot to stop the execution flow after calling the respond function. That means that if we enter a domain that doesn't host a website, the respond function will be called twice and we will get the following message in the terminal:

在回调中我们处理了错误,但执行respond函数后我们忘了停止这个流程。这意味着如果我们输入一个域名,但这个域名不是任何网站的主机,则respond函数会调用两次,我们将在终端得到以下信息:
　　

> Error: Can't set headers after they are sent. at    
> ServerResponse.OutgoingMessage.setHeader (http.js:691:11) at respond  
> (/Users/alexandruvladutu/www/airpair-2/3-multi-callback/proxy-server.js:18:7)
> This can be avoided either by using the `return` statement or by
> wrapping the 'success' callback in the `else` statement:

因此正确的做法如下：

    request(.., function(..params) {
        if (err) {
            return respond(err, ..);
        }
        respond(..);
    }); 
    // OR: 
    request(.., function(..params) {
        if (err) { 
            respond(err, ..); 
        } else {
        respond(..); 
        } 
    });
    
    
## 4 The Christmas tree of callbacks (Callback Hell)

## 4 圣诞树回调(回调地狱callback hell)

Every time somebody wants to bash Node they come up with the 'callback hell' argument. Some of them see callback nesting as unavoidable, but that is simply untrue. There are a number of solutions out there to keep your code nice and tidy, such as:

每当有人吐槽nodejs，肯定会提到“回调地狱”的论点。他们中的一些人认为回调函数嵌套是不可避免的,但这是错误的。Nodejs有许多解决方案可以让代码清楚明了,如:

- Using control flow modules (such as async);
- Promises; and
- Generators.

- 使用流程控制模块（[比如async][5]）
- 使用[Promises][6]
- 使用[Generator][7]

We are going to create a sample application and then refactor it to use the async module. The app will act as a naive frontend resource analyzer which does the following:

- Checks how many scripts / stylesheets / images are in the HTML code;
- Outputs the their total number to the terminal;
- Checks the content-length of each resource; then
- Puts the total length of the resources to the terminal.
- Besides the async module, we will be using the following npm modules:

我们将创建一个示例应用程序,然后使用异步模块重构代码。这个应用程序将作为一个简单的前端静态资源分析器，实现以下功能:
　　

 - 检查HTML代码中有多少脚本/样式表/图像; 
 - 输出全部资源到终端; 　　
 - 检查每个资源的内容长度 　　
 - 将资源的总长度输出到终端。

　　
除了 async 模块,我们将使用以下 npm 模块:

- request for getting the page data (body, headers, etc).
- cheerio as jQuery on the backend (DOM element selector).
- once to make sure our callback is executed once.

 - request 用于获取页面数据(body, headers, etc).
 - cheerio 类似JQuery的底层框架(DOM element selector).
 - once 保证回调只执行一次.

    var URL = process.env.URL;
    var assert = require('assert');
    var url = require('url');
    var request = require('request');
    var cheerio = require('cheerio');
    var once = require('once');
    var isUrl = new RegExp(/[-a-zA-Z0-9@:%_\+.~#?&//=]{2,256}\.[a-z]{2,4}\b(\/[-a-zA-Z0-9@:%_\+.~#?&//=]*)?/gi);
    assert(isUrl.test(URL), 'must provide a correct URL env variable');
    request({
      url: URL,
      gzip: true
    }, function(err, res, body) {
      if (err) {
        throw err;
      }
      if (res.statusCode !== 200) {
        return console.error('Bad server response', res.statusCode);
      }
      var $ = cheerio.load(body);
      var resources = [];
      $('script').each(function(index, el) {
        var src = $(this).attr('src');
        if (src) {
          resources.push(src);
        }
      });
      // ..... 
      // 处理样式表跟图片的代码跟以上代码类似
      // 可以去github看所有代码   
      var counter = resources.length;
      var next = once(function(err, result) {
        if (err) {
          throw err;
        }
        var size = (result.size / 1024 / 1024).toFixed(2);
        console.log('There are ~ %s resources with a size of %s Mb.', result.length, size);
      });
      var totalSize = 0;
      resources.forEach(function(relative) {
        var resourceUrl = url.resolve(URL, relative);
        request({
          url: resourceUrl,
          gzip: true
        }, function(err, res, body) {
          if (err) {
            return next(err);
          }
          if (res.statusCode !== 200) {
            return next(new Error(resourceUrl + ' responded with a bad code ' + res.statusCode));
          }
          if (res.headers['content-length']) {
            totalSize += parseInt(res.headers['content-length'], 10);
          } else {
            totalSize += Buffer.byteLength(body, 'utf8');
          }
          if (!--counter) {
            next(null, {
              length: resources.length,
              size: totalSize
            });
          }
        });
      });
    });  

This doesn't look that horrible, but you can go even deeper with nested callbacks. From our previous example you can recognize the Christmas tree at the bottom, where you see indentation like this:

这代码看起来还没那么糟糕，但之后你可能会陷入更深的回调嵌套中。在之前的例子中你可以在代码底部看到一段像圣诞树的部分

                if (!--counter) {
                    next(null, {
                        length : resources.length,
                        size : totalSize
                    });
                }
            });
        });
    });

After a bit of refactoring using async our code might look like the following:
    
我们使用 async 重构这段代码：

    var async = require('async');
    var rootHtml = '';
    var resources = [];
    var totalSize = 0;
    var handleBadResponse = function (err, url, statusCode, cb) {
        if (!err && (statusCode !== 200)) {
            err = new Error(URL + ' responded with a bad code ' + res.statusCode);
        }
        if (err) {
            cb(err);
            return true;
        }
        return false;
    };
    async.series([function getRootHtml(cb) {
                request({
                    url : URL,
                    gzip : true
                }, function (err, res, body) {
                    if (handleBadResponse(err, URL, res.statusCode, cb)) {
                        return;
                    }
                    rootHtml = body;
                    cb();
                });
            }, function aggregateResources(cb) {
                var $ = cheerio.load(rootHtml);
                $('script').each(function (index, el) {
                    var src = $(this).attr('src');
                    if (src) {
                        resources.push(src);
                    }
                }); 
                // 完整代码可以参考github
                
                setImmediate(cb);
            }, function calculateSize(cb) {
                async.each(resources, function (relativeUrl, next) {
                    var resourceUrl = url.resolve(URL, relativeUrl);
                    request({
                        url : resourceUrl,
                        gzip : true
                    }, function (err, res, body) {
                        if (handleBadResponse(err, resourceUrl, res.statusCode, cb)) {
                            return;
                        }
                        if (res.headers['content-length']) {
                            totalSize += parseInt(res.headers['content-length'], 10);
                        } else {
                            totalSize += Buffer.byteLength(body, 'utf8');
                        }
                        next();
                    });
                }, cb);
            }
        ], function (err) {
        if (err) {
            throw err;
        }
        var size = (totalSize / 1024 / 1024).toFixed(2);
        console.log('There are ~ %s resources with a size of %s Mb.', resources.length, size);
    });


## 5 Creating big monolithic applications

## 5 开发一个庞大的应用程序

Developers new to Node come with mindsets from different languages and they tend to do things differently. For example including everything into a single file, not breaking things into their own modules and publishing to NPM, etc.

Take our previous example for instance. We have pushed everything into a single file, making it hard to test and read the code. But no worries, with a bit of refactoring we can make it much nicer and more modular. This will also help with 'callback hell' in case you were wondering.

If we extract the URL validator, the response handler, the request functionality and the resource processor into their own files our main one will look like so:
　　
开发者在刚刚接触nodejs时喜欢从不同的语言角度去思考,他们往往用不同的方式来做事情。例如一个单独的文件包含所有代码,而不去拆散代码到自己的模块，并发布到NPM,等等。

比如我们之前的例子。我们已经把一切功能写到一个文件,使代码难以测试和阅读。但不用担心,重构可以使代码更好，更模块化。当你陷入“回调地狱”时此举也会对你有所帮助。

我们提取URL验证的部分,响应处理部分，以及资源的请求处理部分到独立的文件后，主入口函数看起来会是这样:

    // ...
    var handleBadResponse = require('./lib/bad-response-handler');
    var isValidUrl = require('./lib/url-validator');
    var extractResources = require('./lib/resource-extractor');
    var request = require('./lib/requester');
    // ...
    async.series([function getRootHtml(cb) {
                request(URL, function (err, data) {
                    if (err) {
                        return cb(err);
                    }
                    rootHtml = data.body;
                    cb(null, 123);
                });
            }, function aggregateResources(cb) {
                resources = extractResources(rootHtml);
                setImmediate(cb);
            }, function calculateSize(cb) {
                async.each(resources, function (relativeUrl, next) {
                    var resourceUrl = url.resolve(URL, relativeUrl);
                    request(resourceUrl, function (err, data) {
                        if (err) {
                            return next(err);
                        }
                        if (data.res.headers['content-length']) {
                            totalSize += parseInt(data.res.headers['content-length'], 10);
                        } else {
                            totalSize += Buffer.byteLength(data.body, 'utf8');
                        }
                        next();
                    });
                }, cb);
            }
        ], function (err) {
        if (err) {
            throw err;
        }
        var size = (totalSize / 1024 / 1024).toFixed(2);
        console.log('\nThere are ~ %s resources with a size of %s Mb.', resources.length, size);
    });

在这里面，request函数可能长这样：

    module.exports = function getSiteData(url, callback) {
        request({
            url : url,
            gzip : true,
            // lying a bit
            headers : {
                'User-Agent' : 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/38.0.2125.111 Safari/537.36'
            }
        }, function (err, res, body) {
            if (handleBadResponse(err, url, res && res.statusCode, callback)) {
                return;
            }
            callback(null, {
                body : body,
                res : res
            });
        });
    };

Note: you can check the full example in the github repo.

Now things are simpler, way easier to read and we can start writing tests for our app. We can go on with the refactoring and extract the response length functionality into its own module as well.

The good thing about Node is that it encourages you to write tiny modules and publish them to NPM. You will find modules for all kinds of things such as generating a random number between an interval. You should strive for modularity in your Node applications and keeping things as simple as possible.

An interesting article on how to write modules is the one from substack.

注意:你可以在github上看到完整的[示例][8]。

现在事情变得更简单了,代码更容易阅读,我们就可以开始为我们的应用程序编写测试。我们可以继续重构代码，提取处理响应长度的函数到自己的模块中。


nodejs的特色是鼓励你写小的模块并发布NPM。你会发现模块能做各种各样的事情，例如从一个区间中产生随机数。你应该尽可能使node应用程序模块化,让事情尽可能的简单。

一篇[有趣的文章][9]，来自[substack][10]，讲述了如何编写模块。

## 6 Poor logging

## 不够强健的日志记录

Many Node tutorials show you a small example that contains console.log here and there, so some developers are left with the impression that that's how they should implement logging in their application.

You should use something better than console.log when coding Node apps, and here's why:

- No need to use util.inspect for large, complex objects;
- Built-in serializers for things like errors, request and response objects;
- Support multiple sources for controlling where the logs go;
- Automatic inclusion of hostname, process id, application name;
- Supports multiple levels of logging (debug, info, error, fatal etc);
- Advanced functionality such as log file rotation, etc.
- You can get all of those for free when using a production-ready logging module such as - bunyan. On top of that you also get a handy CLI tool for development if you install the module globally.

Let's take a look at one of their examples on how to use it:

许多Nodejs教程展示例子的时候都带有 console.log 代码。所以一些开发人员缺少一个概念：他们应如何实现应用程序中的日志记录。
在写nodejs应用时你应该使用一些比console.log更加牛逼的日志模块,原因如下:

 - 对于大型、复杂的对象不需要使用util.inspect进行检查;
 - 内置的序列化工具可以处理错误,请求和响应对象之类;
 - 支持多个日志的输出源; 　　
 - 自动包含主机名,进程id、应用程序名称;
 - 支持多个级别的日志记录(debug、info、error、fatal等);
 - 高级功能,如日志文件切割等。


你可以在一个适合生产环境的日志模块中完全获得这些功能,如[bunyan][11]模块。除此之外，如果你全局安装这个模块，你还可以拥有一个命令行工具。

让我们看个例子，了解模块如何使用:
    
    var bunyan = require('bunyan')；
    var log = bunyan.createLogger({
            name : 'myserver',
            serializers : {
                req : bunyan.stdSerializers.req,
                res : bunyan.stdSerializers.res
            }
        });
    var server = http.createServer(function (req, res) {
            log.info({
                req : req
            }, 'start request');// <-- this is the guy we're testing
            res.writeHead(200, {
                'Content-Type' : 'text/plain'
            });
            res.end('Hello World\n');
            log.info({
                res : res
            }, 'done response');// <-- this is the guy we're testing
        });
    server.listen(1337, '127.0.0.1', function () {
        log.info('server listening');
        var options = {
            port : 1337,
            hostname : '127.0.0.1',
            path : '/path?q=1#anchor',
            headers : {
                'X-Hi' : 'Mom'
            }
        };
        var req = http.request(options, function (res) {
                res.resume();
                res.on('end', function () {
                    process.exit();
                })
            });
        req.write('hi from the client');
        req.end();
    });　　
　　
　　If you run the example in the terminal you will see something like the following:
　　
如果你在终端运行这个例子，可以看到如下信息：
　　
    
    $ node server.js
        {"name":"myserver","hostname":"MBP.local","pid":14304,"level":30,"ms    g":"server listening","time":"2014-11-16T11:30:13.263Z","v":0}
        {"name":"myserver","hostname":"MBP.local","pid":14304,"level":30,"req":{"method":"GET","url":"/path?q=1#anchor","headers":{"x-hi":"Mom","host":"127.0.0.1:1337","connection":"keep-alive"},"remoteAddress":"127.0.0.1","remotePort":61580},"msg":"start request","time":"2014-11-16T11:30:13.271Z","v":0}
        {"name":"myserver","hostname":"MBP.local","pid":14304,"level":30,"res":{"statusCode":200,"header":"HTTP/1.1 200 OK\r\nContent-Type: text/plain\r\nDate: Sun, 16 Nov 2014 11:30:13 GMT\r\nConnection: keep-alive\r\nTransfer-Encoding: chunked\r\n\r\n"},"msg":"done response","time":"2014-11-16T11:30:13.273Z","v":0}
    
But in development it's better to use the CLI tool like in the screenshot:
开发过程中更适合使用命令行工具，如下图所示

![此处输入图片的描述][12]


As you can see, bunyan gives you a lot of useful information about the current process, which is vital into production. Another handy feature is that you can pipe the logs into a stream (or multiple streams).

如你所见,bunyan提供了很多关于当前进程的有用信息,这对于生产环境是至关重要的。还有一个很方便的功能就是你可以将输出pipe到一个stream中(或多个stream)。

7 No tests

## 7 没有测试

We should never consider our applications 'done' if we didn't write any tests for them. There's really no excuse for that, considering how many existing tools we have for that:

Testing frameworks: mocha, jasmine, tape and many other
Assertion modules: chai, should.js
Modules for mocks, spies, stubs or fake timers such as sinon
Code coverage tools: istanbul, blanket
The convention for NPM modules is that you specify a test command in your package.json, for example:
　　
如果我们没有写任何测试，不能把我们的应用程序称之为“完成”。没有借口不写测试,看看我们有多少工具:
　　

 - 测试框架:[mocha][13]、[jasmine][14]、[tape][15]等等 
 - 断言模块:[chai][16],[should.js][17] 　　
 - 模拟数据的模块[sinon][18]
 - 代码测试覆盖率工具:[istanbul][19],[blanket][20]

对于NPM模块有一个约定，你需要**在package.json**里指定一个测试命令,例如:

    { "name": "express", 
    ... 
    "scripts": { "test": "mocha --require test/support/env --reporter spec --bail --check-leaks test/ test/acceptance/", 
    ... }
    
    Then the tests can be run with npm test, no matter of the testing framework used.

Another thing you should consider for your projects is to enforce having all your tests pass before committing. Fortunately it is as simple as doing npm i pre-commit --save-dev.

You can also decide to enforce a certain code coverage level and deny commits that don't adhere to that level. The pre-commit module simply runs npm test automatically for you as a pre-commit hook.

In case you are not sure how to get started with writing tests you can either find tutorials online or browse popular Node projects on Github, such as the following:

然后使用**npm test**可以运行测试,不管使用任何的测试框架。

另一件应该考虑的是必须执行项目所有测试并保证通过。幸运的是这很简单，使用**npm i pre-commit --save-dev** 即可。

你也可以设置覆盖率水平需要达到什么等级，若没有达到这个级别则拒绝代码提交。**pre-commit**模块在提交代码之前，以钩子的方式自动运行**npm test**命令。

如果你不了解如何给自己的项目编写测试用例，你可以看一些在线教程或者或浏览Github上受欢迎的node项目,比如:

 - [express][21]
 - [loopback][22]
 - [ghost][23]  
 - [hapi][24]  
 - [haraka][25]

## 8 Not using static analysis tools

## 8 不使用静态分析工具

Instead of spotting problems in production it's better to catch them right away in development by using static analysis tools.

Tools such as ESLint help solve a huge array of problems, such as:

Possible errors, for example: disallow assignment in conditional expressions, disallow the use of debugger.
Enforcing best practices, for example: disallow declaring the same variable more then once, disallow use of arguments.callee.
Finding potential security issues, such as the use of eval() or unsafe regular expressions.
Detecting possible performance problems.
Enforcing a consistent style guide.
For a more complete set of rules checkout the ESLint rules documentation page. You should also read the configuration documents if you want to setup ESLint for your project.

若不想在生产环境中发现问题，最好在开发阶段就使用静态分析工具。

如[ESLint][26]之类的工具可以帮助解决大量的问题,如:
　　

 - 可能出现的错误,例如:不允许条件表达式赋值,不允许使用debugger。
 - 执行最佳实践,例如:不允许多次声明相同的变量,不允许使用arguments.callee。
 - 发现潜在的安全问题,比如使用eval()或不安全的正则表达式。
 - 检测可能的性能问题。
 - 执行一致的代码风格指南。

更完整的规则校验，你可以参考[ESLint规则文档][27]。如果你想为项目设置ESLint，你还应该阅读[配置文档][28]。


In case you were wondering where you can find a sample configuration file for ESLint, the Esprima project has one.

There are other similar linting tools out there such as JSLint or JSHint.

In case you want to parse the AST (abstract source tree) and create a static analysis tool by yourself, consider Esprima or Acorn.

如果你想知道你在哪里可以找到一个配置文件的实例,[Esprima的配置文件][29]是其中一个选择。

还有其他类似的lint工具，比如[JSLint][30]或[JSHint][31]等。

如果您想解析AST(抽象语法树),并创建一个静态分析工具,可以考虑[Esprima][32]或[Acorn][33]。

9 Zero monitoring or profiling
Not monitoring or profiling a Node applications leaves you in the dark. You are not aware of vital things such as event loop delay, CPU load, system load or memory usage.

There are proprietary services that care of these things for you, such as the ones from New Relic, StrongLoop or Concurix, AppDynamics.

You can also achieve that by yourself with open source modules such as look or by gluing different NPM modules. Whatever you choose make sure you are always aware of the status of your application at all times, unless you want to receive weird phone calls at night.

## 9 缺少监控或分析

若nodejs应用缺少监控或分析。很多重要的事情如event loop延迟、CPU负载、系统负载或内存使用你将一无所知。

有专门的服务可以处理这些事情,比如[New Relic][34]，[StrongLoop][35]或[Concurix][36] [AppDynamics][37]。

通过结合一些模块的功能，你也可以用开源模块实现。无论你选择什么，确保你总是可以监控到应用程序的状态,除非你想晚上接到奇怪的电话。

## 10 Debugging with console.log

## 10 用console.log调试

When something goes bad it's easy to just insert console.log in some places and debug. After you figure out the problem you remove the console.log debugging leftovers and go on.

The problem is that the next developer (or even you) might come along and repeat the process. That's why module like debug exist. Instead of inserting and deleting console.log you can replace it with the debug function and just leave it there.

Once the next guy tries to figure out the problem they just start the application using the DEBUG environment variable.

This tiny module has its benefits:

Unless you start the app using the DEBUG environment variable nothing is displayed to the console.
You can selectively debug portions of your code (even with wildcards).
The output is beautifully colored into your terminal.
Let's take a look at their official example:


　　当出现问题时，可以简单的使用console.log，将其插入到某些地方进行调试。找出问题后删除console.log，debug结束，开发继续。

问题是,下一个开发人员(或者甚至你)可能会出现并重复这个过程。这就是为什么会有debug这种模块。不要继续使用console.log,用debug函数，然后把这些代码放在那就可以了。

之后其他人试图找出问题时，他们只需要启动应用程序，使用DEBUG作为环境变量即可。

这个小模块有以下好处:
　　

 - 不会显示到控制台，除非以DEBUG作为环境变量启动node应用。
 - 您可以选择性地调试部分代码(甚至使用通配符)。
 - 终端输出好看的彩色文字。

让我们看看他们的官方的例子:

     // app.js
      var debug = require('debug')('http')
        , http = require('http')
        , name = 'My App';
    
      // fake app
    
      debug('booting %s', name);
    
      http.createServer(function(req, res){
        debug(req.method + ' ' + req.url);
        res.end('hello\n');
      }).listen(3000, function(){
        debug('listening');
      });
    
      // fake worker of some kind
    
      require('./worker');
    
    <!--code lang=javascript linenums=true-->
    
      // worker.js
      var debug = require('debug')('worker');
    
      setInterval(function(){
        debug('doing some work');
      }, 1000);
      
If we run the example with node app.js nothing happens, but if we include the DEBUG flag voila:

如果我们使用 node app.js运行这个程序，不会发生任何事。但如果我们添加一个**DEBUG**的标识后

![此处输入图片的描述][38]

Besides your applications, you can also use it for tiny modules published to NPM. Unlike a more complex logger it only does the debugging job and it does it well.

除了应用程序,你还可以使用它为发布到NPM的小模块进行调试。与一个更复杂的logger相比，debugg只做调试工作而且做的很好。


  [1]: http://www.modulecounts.com/
  [2]: http://www.ruanyifeng.com/blog/2014/10/event-loop.html
  [3]: http://npm.im/chalk
  [4]: https://cldup.com/vBlM92zYwH.png
  [5]: https://www.npmjs.com/package/async
  [6]: http://strongloop.com/strongblog/promises-in-node-js-with-q-an-alternative-to-callbacks/
  [7]: http://strongloop.com/strongblog/how-to-generators-node-js-yield-use-cases/
  [8]: https://github.com/alessioalex/airpair-nodejs-mistakes
  [9]: http://substack.net/how_I_write_modules
  [10]: https://github.com/substack
  [11]: https://github.com/trentm/node-bunyan/
  [12]: https://cldup.com/Pmrp9ePGff.png
  [13]: https://github.com/mochajs/mocha
  [14]: https://github.com/jasmine/jasmine
  [15]: https://github.com/substack/tape
  [16]: https://github.com/chaijs/chai
  [17]: https://github.com/shouldjs/should.js
  [18]: http://sinonjs.org/
  [19]: https://github.com/gotwarlost/istanbul
  [20]: https://github.com/alex-seville/blanket
  [21]: https://github.com/strongloop/express/tree/master/test
  [22]: https://github.com/strongloop/loopback/tree/master/test
  [23]: https://github.com/TryGhost/Ghost/tree/master/core/test
  [24]: https://github.com/hapijs/hapi/tree/master/test
  [25]: https://github.com/baudehlo/Haraka/tree/master/tests
  [26]: http://eslint.org/
  [27]: http://eslint.org/docs/rules/
  [28]: http://eslint.org/docs/configuring/
  [29]: https://github.com/ariya/esprima/blob/master/.eslintrc
  [30]: http://www.jslint.com/
  [31]: http://jshint.com/
  [32]: http://esprima.org/
  [33]: https://github.com/marijnh/acorn
  [34]: http://newrelic.com/nodejs
  [35]: http://strongloop.com/node-js/performance-monitoring/
  [36]: http://concurix.com/
  [37]: https://docs.appdynamics.com/display/PRO39/APM+Overview+-+Node.js
  [38]: https://cldup.com/J-r35MbRtX.png

  原文：https://www.airpair.com/node.js/posts/top-10-mistakes-node-developers-make