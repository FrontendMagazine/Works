# Introducing React Native: Building Apps with JavaScript

# 深入浅出 React Native：使用 JavaScript 构建原生应用

A few months ago [Facebook announced React Native](https://code.facebook.com/videos/786462671439502/react-js-conf-2015-keynote-introducing-react-native-/), a framework that lets you build native iOS applications with JavaScript – and the[ official repository](https://github.com/facebook/react-native) just came out of beta today!

数月前，[Facebook 对外宣布了正在开发的 React Native 框架](https://code.facebook.com/videos/786462671439502/react-js-conf-2015-keynote-introducing-react-native-/)，这个框架允许你使用 JavaScript 开发原生的 iOS 应用——就在今天，[Beta 版的仓库](https://github.com/facebook/react-native)释出了！

People have been using JavaScript and HTML5 to create iOS applications using the [PhoneGap](http://phonegap.com/) wrapper for a number of years, so is React Native really such a big deal?

基于 [PhoneGap](http://phonegap.com/) 使用 JavaScript 和 HTML5 开发 iOS 应用已经有好几年了，那 React Native 有什么牛的？

React Native is a big deal, and people are getting very excited about it for two main reasons:

React Native 真的很牛，让大家兴奋异常的主要原因有两点：

1. With React Native your application logic is written and runs in JavaScript, whereas your application UI is fully native; therefore you have none of the compromises typically associated with HTML5 UI.

2. React introduces a novel, radical and highly functional approach to constructing user interfaces. In brief, the application UI is simply expressed as a function of the current application state.

1. ，可以基于 React Native使用 JavaScript 编写应用逻辑，UI 则可以保持全是原生的。这样的话就没有必要就 HTML5 的 UI 做出常见的妥协；

2. React 引入了一种与众不同的、略显激进但具备高可用性的方案来构建用户界面。长话短说，应用的 UI 简单通过一个基于应用目前状态的函数来表达。

The key point with React Native is that it aims to primarily bring the power of the [React](http://facebook.github.io/react/) programming model to mobile app development. It is not aiming to be a cross platform, write-once run-anywhere, tool. It is aiming to be learn-once write-anywhere. An important distinction to make. This tutorial only covers iOS, but once you’ve learned the concepts here you could port that knowledge into creating an Android app very quickly.

React Native 的关键就是，以把 [React](http://facebook.github.io/react/) 编程模式的能力带到移动开发来作为主要目标。它的目标不是跨平台一次编写到处执行，而是一次学习跨平台开发。这个是一个非常大的区别。这篇教程只介绍 iOS 平台，不过你一旦掌握了相关的概念，就可以应用到 Android 平台，快速构建 Android 应用。

If you have only ever written applications in Objective-C or Swift, you might not be particularly excited about the prospect of using JavaScript instead. Although, as a Swift developer, the second point above should pique your interest!

如果之前只用过 Objective-C 或者 Swift 写应用的话，你很可能不会对使用 JavaScript 来编写应用的愿景感到兴奋。尽管如此，作为一个 Swift 开发者来说，上面提到的第二点应该可以激起你的兴趣！

Through Swift, you’ve no doubt been learning new and more functional ways to encode algorithms, and techniques that encourage transformation and immutability. However, the way in which you construct your UI is very much the same as it was when developing with Objective-C: it’s still UIKit-based and imperative.

你通过 Swift，毫无疑问学习到了新的更多有效的编码方法和技巧，鼓励转换和不变性。然而，构建 UI 的方式还是和使用 Objective-C 的方式一致。仍然以 UIKit 为基础，独断专横。

Through intriguing concepts such as a virtual DOM and reconciliation, React brings functional programming directly to the UI layer.

通过像 virtual DOM 和 reconciliation 这些有趣的概念，React 将函数式编程直接带到了 UI 层。

This tutorial takes you through the process of building an application for searching UK property listings:

这篇教程将带着你一路构建一个 UK 房产搜索应用：

![PropertyFinder](http://cdn4.raywenderlich.com/wp-content/uploads/2015/03/PropertyFinder-700x360.png)

If you’ve never written any JavaScript before, fear not; this tutorial leads you through each and every step of the coding. React uses CSS properties for styling which are generally easy to read and understand, but if you need to, you can always refer to the excellent [Mozilla Developer Network reference](https://developer.mozilla.org/en-US/docs/Web/CSS).

如果你之前一点 JavaScript 都没写过，别担心。这篇教程带着你进行一步一步进行编码。React 使用 CSS 属性来定义样式，一般比较容易读也比较容易理解。但是如果你想了解更多的话，可以去看看 [Mozilla Developer Network reference](https://developer.mozilla.org/en-US/docs/Web/CSS)，很不错的。

Want to learn more? Read on!

想要学习更多，继续往下读！

## Getting Started

## 准备工作

The React Native framework is [available via GitHub](https://github.com/facebook/react-native). You can grab the framework by either cloning the repository using git, or you can choose download it as a zip file. Once you have the React Native framework locally, there are a few other prerequisites to take care of before you can start coding.

React Native 框架[托管在 GitHub 上](https://github.com/facebook/react-native)。你可以通过两种方式获取到它：使用 git 克隆仓库，或者下载一个 zip 压缩包文件。如果你的机器上已经安装了 React Native，在着手编码前还有其他几个因素需要考虑。

- React Native uses [Node.js](https://nodejs.org/), a JavaScript runtime, to build your JavaScript code. If you don’t already have Node.js installed, it’s time to get it!

- React Native 借助 [Node.js](https://nodejs.org/)，即 JavaScript 运行时来创建 JavaScript 代码。如果你已经安装了 Node.js，那就可以上手了。

First [install Homebrew](http://brew.sh/) using the instructions on the Homebrew website, then install Node.js by executing the following in a Terminal window:

首先，使用 Homebrew 官网提供的指引[安装 Homebrew](http://brew.sh/)，然后在终端执行以下命令：

    brew install node

Next, use homebrew to install [watchman](https://facebook.github.io/watchman/), a file watcher from Facebook:

接下来，使用 homebrew 安装 [watchman](https://facebook.github.io/watchman/)，一个来自Facebook 的观察程序：

    brew install watchman

This is used by React Native to figure out when your code changes and rebuild accordingly. It’s like having Xcode do a build each time you save your file.

通过配置 watchman，React 实现了在代码发生变化时，完成相关的重建的功能。就像在使用 Xcode 时，每次保存文件都会进行一次创建。

Next use npm to install the React Native CLI tool:

接下来使用 `npm` 安装 React Native CLI 工具：

	npm install -g react-native-cli
	
This uses the Node Package Manager to fetch the CLI tool and install it globally; npm is similar in function to CocoaPods or Carthage.

这使用 Node 包管理器抓取 CLI 工具，并且全局安装；`npm` 在功能上与 CocoaPods 或者 Carthage 类似。

Navigate to the folder where you would like to develop your React Native application, and use the CLI tool to construct the project:

浏览到你想要创建 React Native 应用的文件夹下，使用 CLI 工具构建项目：

	react-native init PropertyFinder
	
This creates a starter-project containing everything you need to build and run a React Native application.

现在，已经创建了一个初始项目，包含了创建和运行一个 React Native 应用所需的一切。

If you look at the created folders and files you will find a node_modules folder, which contains the React Native framework. You will also find an index.ios.js file, which is the skeletal app created by the CLI tool. Finally, there is an Xcode project file and an iOS folder, containing the small amount of code required to ‘bootstrap’ your application.

如果仔细观察了创建的文件夹和文件，你会发现一个 `node_modules` 文件夹，包含了 React Native 框架。你也会发现一个 `index.ios.js` 文件，这是 CLI 工具创建的一个空壳应用。最后，会出现一个 Xcode 项目文件和一个 iOS 文件夹，包含了少量的代码用来引入 `bootstrap` 到你的项目中。

Open the Xcode project file then build and run. The simulator will start and display the following greeting:

打开 Xcode 项目文件，然后创建并运行。模拟器将会启动并且展示下面的问候语：

![](http://cdn1.raywenderlich.com/wp-content/uploads/2015/03/ReactNative-Starter-281x500.png)

You might also have noticed that a terminal window has popped up, displaying the following:

你可以能发现，一个终端窗口被弹出，输出了下面的信息：

	===============================================================
	 |  Running packager on port 8081.       
	 |  Keep this packager running while developing on any JS         
	 |  projects. Feel free to close this tab and run your own      
	 |  packager instance if you prefer.                              
	 |                                                              
	 |     https://github.com/facebook/react-native                 
	 |                                                              
	 ===============================================================
	 
	Looking for JS files in
	   /Users/colineberhardt/Temp/TestProject
	 
	React packager ready.
	

This is the React Native packager, running under node. You’ll find out what it does shortly.

这就是 React Native 包，在 node 下运行。一会儿你就会知道它是用来干什么的。

Don’t close the Terminal window; just keep it running in the background. If you do close it by mistake, simply stop and re-run the project via Xcode.

不要关闭终端窗口；就然它在后台运行。如果你不小心关了，只需要停下来使用 Xcode 重新运行项目。

> 注意：在进入编码工作之前，还有最后一件事 —— 在这个教程中，你需要编写大量的 JavaScript 代码，Xcode 并非是最好的工具！我使用 [Sublime Text](http://www.sublimetext.com/)，一个价格合理且应用广泛的编辑器。不过，[atom](https://atom.io/)，[brackets](http://brackets.io/) 或者其他轻量的编辑器都能胜任这份工作。

## Hello React Native

## React Native 你好

Before getting started on the property search application, you’re going to create a very simple Hello World app. You’ll be introduced to the various components and concepts as you go along.

在开始“搜房App”之前，先来个简单的Hello World App热热身。在这一节里，你将会使用到一些组件。

Open index.ios.js in your text editor of choice and delete the current contents since you’re going to build your own app from scratch. Add the following to the start of the file:

在你钟爱的编辑其中打开 `index.ios.js` ，删除当前的内容，因为你要从头构建你自己的应用。然后在文件顶部增加下面这样一行：

    'use strict';

This enables Strict Mode, which adds improved error handling and disables some less-than-ideal JavaScript language features. In simple terms, it makes JavaScript better!

这行代码是用于开启**Strict Mode**，Strict mode的错误处理可以有所提高，JavaScript的一些语言缺陷也可以避免。简而言之就是，JavaScript在这种模式下工作地更好！

> Note: For a more detailed overview of Strict Mode, I’d encourage you to read [Jon Resig’s article](http://ejohn.org/blog/ecmascript-5-strict-mode-json-and-more/) “ECMAScript 5 Strict Mode, JSON, and More”.

> 注意：想要研究一下Strict Mode的朋友，我会推荐你阅读[Jon Resig的文章:“ECMAScript 5 Strict Mode, JSON, and More”](http://ejohn.org/blog/ecmascript-5-strict-mode-json-and-more/)

Next, add the following line:

然后，加上这一行：

    var React = require('react-native');

This loads the react-native module and assigns it to React. React Native uses the same module loading technology as Node.js with the require function, which is roughly equivalent to linking and importing libraries in Swift.

这句代码是将react-native模块加载进来，并将它赋值给变量```React```的。React Native使用同Node.js相同的模块加载方式：```require```，这个概念可以等同于Swift中的“链接库”或者“导入库”。

> Note: For more information about JavaScript modules I’d recommend reading[ this article by Addy Osmani](http://addyosmani.com/writing-modular-js/) on writing modular JavaScript.

> 注意：想要了解更多关于JavaScript模块的知识，我推荐阅读[Addy Osmani写的这篇文章](http://addyosmani.com/writing-modular-js/)。

Just below the require statement, add the following:

在```require```语句的下面，加上这一段：

    var styles = React.StyleSheet.create({
      text: {
        color: 'black',
        backgroundColor: 'white',
        fontSize: 30,
        margin: 80
      }
    });

This defines a single style that you will shortly apply to your “Hello World” text. If you’ve done any web development before, you’ll probably recognize those property names; React Native uses [Cascading Style Sheets (CSS)](http://www.w3schools.com/css/) to style the application UI.

以上代码定义了一段应用在"Hello World"文本上的样式。如果你曾接触过We开发，那你很可能已经发现了：React Native使用的是 [CSS](http://www.w3schools.com/css/) 来定义应用界面的样式。

Now for the app itself! Still working in the same file, add the following code just beneath the style declaration above:

现在我们来关注应用本身吧！依然是在相同的文件下，将以下代码添加到样式代码的下面：

    class PropertyFinderApp extends React.Component {
      render() {
        return React.createElement(React.Text, {style: styles.text}, "Hello World!");
      }
    }

Yes, that’s a JavaScript class!

是的，奏是 JavaScript class!

Classes were introduced in ECMAScript 6 (ES6). Although JavaScript is constantly evolving, web developers are restricted in what they can use due to the need to maintain compatibility with older browsers. React Native runs within JavaScriptCore; as a result, you can use modern language features without worrying about supporting legacy browsers.

类 (class) 是在ES6中被引入的，纵然JavaScript一直在进步，但Web开发者受困于兼容浏览器的状况中，不能怎么使用JS的新特性。React Native运行在JavaScriptCore中是，也就是说，你可以使用JS的新特性啦，完全不用担心兼容什么的呢。

> Note: If you are a web developer, I’d thoroughly encourage you to use modern JavaScript, and then convert to older JavaScript using tools such as [Babel](https://babeljs.io/) to maintain support for older and incompatible browsers.

> 注意：如果你是一名Web开发者，我百分百鼓励你要使用现代的JavaScript，然后使用像 [Babel](https://babeljs.io/) 这样的工具生成兼容性的JavaScript，用于支持兼容性不好的老浏览器。

PropertyFinderApp extends React.Component, the basic building block of the React UI. Components contain immutable properties, mutable state variables and expose a method for rendering. Your current application is quite simple and only requires a render method.

```PropertyFinderApp``` 继承了 ```React.Component```（React UI的基础模块）。组件包含着不可变的属性，可变的状态变量以及暴露给渲染用的方法。这会你做的应用比较简单，只用一个渲染方法就可以啦。

React Native components are not UIKit classes; instead they are a lightweight equivalent. The framework takes care of transforming the tree of React components into the required native UI.

React Native 组件并不是 UIKit 类，它们只能说是在某种程度上等同。框架只是将 React 组件树转化成为原生的UI。

Finally, add the following to the end of the file:

最后一步啦，将这一行加在文件末尾：

    React.AppRegistry.registerComponent('PropertyFinder', function() { return PropertyFinderApp });

AppRegistry defines the entry point to the application and provides the root component.

```AppRegistry``` 定义了App的入口，并提供了根组件。

Save your changes to PropertyFinderApp.js and then return to Xcode. Ensure the PropertyFinder scheme is selected with one of the iPhone simulators, and then build and run your project. After a few seconds you’ll see your “Hello World” app in action:

保存 **PropertyFinderApp.js**，回到Xcode中。确保**PropertyFinder**规划（scheme）已经勾选了，并设置了相应的iPhone模拟器，然后生成并运行你的项目。几秒之后，你应该就可以看到 "Hello World" 应用正在运行了：

![react-helloworld](http://cdn4.raywenderlich.com/wp-content/uploads/2015/03/react-helloworld-281x500.png)

That’s a JavaScript application running in the simulator, rendering a native UI, without a browser in sight!

这个JavaScript应用运行在模拟器上，使用的是原生UI，没有任何内嵌的浏览器哦！

Still don’t trust me? :] Verify it for yourself: within Xcode, select Debug\View Debugging\Capture View Hierarchy and take a look at the native view hierarchy. You will see no UIWebView instances anywhere! Just a proper, real, view! Neat :]!

还不相信这是真的？:] 那打开你的Xcode，选择 **Debug\View Debugging\Capture View Hierarchy**，你看 **native view hierarchy** 中都没有 ```UIWebView```，就只有一个原生的view！:]

![A native view hierarchy](http://cdn4.raywenderlich.com/wp-content/uploads/2015/03/ViewDebugging-480x227.png)

Curious as to how it all works? In Xcode open AppDelegate.m and locate application:didFinishLaunchingWithOptions:. This method constructs a RCTRootView, which loads the JavaScript application and renders the resultant view.

你一定很好奇其中的原理吧，那就在Xcode中打开 **AppDelegate.m**，接着找到 ```application:didFinishLaunchingWithOptions```：这个方法构建了 ```RCTRootView``` 用于加载 JavaScript 应用以及渲染最后的视图的。

When the application starts, the RCTRootView loads the application from the following URL:

当应用开始运行的时候，```RCTRootView```将会从以下的URL中加载应用：

    http://localhost:8081/index.ios.bundle

Recall the Terminal window that was opened when you ran this application; this starts a packager and server that handles the above request.

重新调用了你在运行这个App时打开的终端窗口，它开启了一个 packager 和 server 来处理上面的请求。

Open this URL in Safari; you’ll see the JavaScript code for your app. You should be able to find your “Hello World” code embedded among the React Native framework.

在 Safari 中打开那个 URL；你将会看到这个 App 的 JavaScript 代码。你也可以在 React Native 框架中找到你的 "Hello World" 代码。 

When your app starts, this code is loaded and executed by the JavaScriptCore framework. In the case of your application, it loads the PropertyFinderApp component, then constructs the native UIKit view. You’ll learn a bit more about this later in the tutorial.

当你的App开始运行了以后，这段代码将会被加载进来，然后 JavaScriptCore 框架将会执行它。在 Hello World 的例子里，它将会加载 ```PropertyFinderApp``` 组件，然后构建出原生的 UIKit 视图。关于这部分的内容，后文里会再详细解释的。

## Hello World JSX

## 你好 JSX 的世界

Your current application uses React.createElement to construct the simple UI for your application, which React turns into the native equivalent. While your JavaScript code is perfectly readable in its present form, a more complex UI with nested elements would rapidly become quite a mess.

Make sure the app is still running, then return to your text editor to edit PropertyFinderApp.js. Modify the return statement of your component’s render method as follows:

你当前的应用程序会使用 React.createElement 来构建应用 UI ,React会将其转换到原生环境中。在当前情况下，你的JavaScript代码是完全可读的,但一个更复杂的 UI 与嵌套的元素将迅速使代码变成一大坨。
　　
确保应用程序仍在运行,然后回到你的文本编辑器中，编辑 PropertyFinderApp.js 。修改组件 render 方法的返回语句如下:

    return <React.Text style={styles.text}>Hello World (Again)</React.Text>;

return <React.Text style={styles.text}>Hello World (Again)</React.Text>;
This is JSX, or JavaScript syntax extension, which mixes HTML-like syntax directly in your JavaScript code; if you’re already a web developer, this should feel rather familiar. You’ll use JSX throughout this article.

这是 JSX ，或 JavaScript 语法扩展,它直接在你的 JavaScript 代码中混合了类似 HTML 的语法;如果你是一个 web 开发人员,应该对此不陌生。在本篇文章中你将一直使用 JSX 。

Save your changes to PropertyFinderApp.js and return to the simulator. Press Cmd+R, and you’ll see your application refresh to display the updated message “Hello World (Again)”.

Re-running a React Native application is really as simple as refreshing a web browser! :]

把你的改动保存到 PropertyFinderApp.js 中，并返回到模拟器。按下 Cmd + R ,你将看到你的应用程序刷新,并显示更新的消息 "Hello World(again)"。

重新运行一个 React Native 应用程序像刷新 web 浏览器一样简单!:]

Since you’ll be working with the same set of JavaScript, you can leave the app running and simply refresh the app in this fashion after modifying and saving PropertyFinderApp.js.

> Note: If you are feeling inquisitive, take a look at your ‘bundle’ in the browser to see what the JSX is transformed into.

因为你会使用相同的一系列 JavaScript 文件,您可以让应用程序一直运行,只在更改和保存 PropertyFinderApp.js 后刷新即可

> 注意:如果你感到好奇,可以看看你的“包”在浏览器中，JSX被转换成什么。


Okay, enough of this “Hello World” fun; it’s time to build the real application!

这个 "Hello World" 已经够大家玩耍了,是时候构建实际的应用程序了!

## Adding Navigation

## 添加导航

The Property Finder app uses the standard stack-based navigation experience provided by UIKit’s navigation controller. It’s time to add this behavior.

我们的房产查找应用使用标准的栈式导航，基于 UIKit 的 navigation controller。现在正是添加的时候。

Within index.ios.js, rename the PropertyFinderApp class to HelloWorld:

在 `index.ios.js` 文件中，把 `PropertyFinderApp` 重命名为 `HelloWorld`：


    class HelloWorld extends React.Component {

You’ll keep the “Hello World” text display around a little longer, but it won’t be the root component of your app anymore.

“Hello World” 这几个字你还需要让它显示一会儿，但它不再是应用的根组件了。

Next add the following class below the HelloWorld component:

接下来，在 `HelloWorld` 这个组件下面添加如下这个类：

    class PropertyFinderApp extends React.Component {
      render() {
        return (
          <React.NavigatorIOS
            style={styles.container}
            initialRoute={{
              title: 'Property Finder',
              component: HelloWorld,
            }}/>
        );
      }
    }

This constructs a navigation controller, applies a style and sets the initial route to the HelloWorld component. In web development, routing is a technique for defining the navigation structure of an application, where pages — or routes — are mapped to URLs.

构造一个 navigation controller，应用一个样式，并把初始路由设为 `Hello World` 组件。在 Web 开发中，`路由`就是一种定义应用导航的一种技术，即定义页面——或者说是路由——与 URL 的对应关系。

Within the same file, update the styles declaration to include the container style as shown below:

在同一个文件中，更新样式定义，包含如下 container 的样式：

    var styles = React.StyleSheet.create({
      text: {
        color: 'black',
        backgroundColor: 'white',
        fontSize: 30,
        margin: 80
      },
      container: {
        flex: 1
      }
    });

You’ll find out what flex: 1 means a bit later in this tutorial.

在随后的教程中会告诉你 `flex: 1` 是什么意思。

Head back to the simulator and press Cmd+R to see your new UI in action:

回到模拟器，`Cmd+R`，看看新 UI 的样子：

![react-helloworldagain](http://cdn5.raywenderlich.com/wp-content/uploads/2015/03/react-helloworldagain-281x500.png)

There’s the navigation controller with its root view, which is currently the “Hello World” text. Excellent — you now have the basic navigation structure for your application in place. It’s time to add the ‘real’ UI!

这就是包含了 root view 的 navigation controller，目前 root view 就是 "Hello World"。很棒——应用已经有了基础的导航结构，到添加**真实** UI 的时候了。

## Building the Search Page

## 创建搜索页

Add a new file to the project named SearchPage.js and place it in the same folder as PropertyFinderApp.js. Add the following code to this file:

在项目中添加一个新文件，命名为 `SearchPage.js`，然后将其放在 `PropertyFinderApp.js` 所在目录下。在文件中添加下面代码：

    'use strict';
     
    var React = require('react-native');
    var {
      StyleSheet,
      Text,
      TextInput,
      View,
      TouchableHighlight,
      ActivityIndicatorIOS,
      Image,
      Component
    } = React;

You’ve already seen the strict mode and the react-native import before, but the assignment statement that follows it is something new.

你会注意到，位于引入 `react-native` 所在位置的前面有一个严格模式标识，紧接着的声明语句是新知识。


This is a destructuring assignment, which lets you extract multiple object properties and assign them to variables using a single statement. As a result, the rest of your code can drop the React prefix; for example, you can refer directly to StyleSheet rather than React.StyleSheet. Destructuring is also useful for manipulating arrays and is [well worth learning more about](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment).

这是一种解构赋值，准许你获取对象的多个属性并且使用一条语句将它们赋给多个变量。结果是，后面的代码中可以省略掉 React 前缀；比如，你可以直接引用 `StyleSheet` ，而不再需要 `React.StyleSheet`。解构同样适用于操作数组，[更多细节请戳这里](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)。

Still working in the same file, SearchPage.js, add the following style:

继续在 `SearchPage.js` 文件中添加下面的样式：

    var styles = StyleSheet.create({
      description: {
        marginBottom: 20,
        fontSize: 18,
        textAlign: 'center',
        color: '#656565'
      },
      container: {
        padding: 30,
        marginTop: 65,
        alignItems: 'center'
      }
    });

Again, these are standard CSS properties. Setting up styles like this is less visual than using Interface Builder, but it’s better than setting view properties one by one in your viewDidLoad() methods! :]

同样，以上都是标准的 CSS 属性。和 Interface Builder 相比，这样设置样式缺少了可视化，但是比起在 `viewDidLoad()` 中逐个设置视图属性的做法更友好！

Add the component itself just below the styles you added above:

只需要把组件添加到样式声明的前面：

    class SearchPage extends Component {
      render() {
        return (
          <View style={styles.container}>
            <Text style={styles.description}>
              Search for houses to buy!
            </Text>
            <Text style={styles.description}>
              Search by place-name, postcode or search near your location.
            </Text>
          </View>
        );
      }
    }

render is a great demonstration of JSX and the structure it provides. Along with the style, you can very easily visualize the UI constructed by this component: a container with two text labels.

`render` 很好地展示出 JSX 以及它表示的结构。通过这个样式，你可以轻易地描绘出组件 UI 的结构：一个容器，包含两个 `text` 标签。

Finally, add the following to the end of the file:

最后，将下面的代码添加到文件末尾：

    module.exports = SearchPage;

This exports the SearchPage class, which permits its use in other files.

这可以 export `SearchPage` 类，方便在其他文件中使用它。

The next step is to update the application routing in order to make this the initial route.

下一步是更新应用的路由，以初始化路由。

Open PropertyFinderApp.js and add the following just after the current require import near the top of the file:

打开 `PropertyFinderApp.js`，在文件顶部紧接着上一个 require 语句的位置添加下面代码：

    var SearchPage = require('./SearchPage');

Within the render function of the PropertyFinderApp class, update initialRoute to reference the newly added page as shown below:

在 PropertyFinderApp 类的 render 函数内部，通过更新 initialRoute 来引用最新添加的页面，如下：

    component: SearchPage

At this point you can remove the HelloWorld class and its associated style, if you like. You won’t be needing that code any longer.

此时，如果你愿意则可以移除 HelloWorld 类以及与它相关联的样式。你不在需要那段代码了。

Return to the simulator, hit Cmd+R and check out the new UI:

切换到模拟器，按下 Cmd+R 查看新的 UI：

![react-searchstarter](http://cdn1.raywenderlich.com/wp-content/uploads/2015/03/react-searchstarter-281x500.png)

This is using the new component, SearchPage, which you added.

## Styling with Flexbox

## 使用 Flexbox 定义外观

So far, you’ve seen basic CSS properties that deal with margins, paddings and color. However, you might not be familiar with flexbox, a more recent addition to the CSS specification that is very useful for application UI layout.

现在，你已经看到了用基本的CSS属性来控制外间距（margin），内间距（padding）还有颜色（color）。不过，可能你还不太了解要如何使用伸缩盒（flexbox），flexbox是最近新加入CSS规范，用它就能很便利地布局界面。

React Native uses the [css-layout](https://github.com/facebook/css-layout) library, a JavaScript implementation of the flexbox standard which is transpiled into C (for iOS) and Java (for Android).

React Native用[css-layout](https://github.com/facebook/css-layout)（这是一个用JavaScript实现flexbox标准然后编译成C（iOS平台）或者Java（Android平台）的库）。

It’s great that Facebook has created this as a separate project that targets multiple languages, since this allows for novel applications, such as applying [flexbox layout to SVG](http://blog.scottlogic.com/2015/02/02/svg-layout-flexbox.html) (yes, that was written by me … and no, I don’t sleep much!).

Facebook把这个项目单独出来实在太正确了，这样可以编译成多种语言，促进更多新颖的应用的发展，比如[flexbox layout to SVG](http://blog.scottlogic.com/2015/02/02/svg-layout-flexbox.html)。

In your app, the container has the default flow direction of column, which means its children are arranged in a vertical stack, like so:

在你的App中，容器（container）默认地是纵向布局，也就是说在它的子元素将会竖直地排列，像这样：

![FlexStack](http://cdn3.raywenderlich.com/wp-content/uploads/2015/03/FlexStack.png)

This is termed the main axis, and can run either vertically or horizontally.

这被称为```主轴 (main axis)```，它的方向可以是竖直的也可以是水平的。

The vertical position of each child is determined from a combination of its margin, height and padding. The container also sets the alignItems property to center, which determines the placement of children on the cross axis. In this case, it results in center-aligned text.

每一个子元素在竖直方向上的位置是由它的margin，height和padding共同决定的。容器的```alignItems```属性也要设置成```center```，这个属性可以控制子元素在十字轴上的位置。在这里，它实现了居中对齐的文本。

It’s time to add the input field and buttons. Open SearchPage.js and insert the following just after the closing tag of the second Text element:

好啦，现在我们把输入框和按钮加上去吧。打开**SearchPage.js**，将下面的代码插入第二个```Text```元素的后面：

    <View style={styles.flowRight}>
      <TextInput
        style={styles.searchInput}
        placeholder='Search via name or postcode'/>
      <TouchableHighlight style={styles.button}
          underlayColor='#99d9f4'>
        <Text style={styles.buttonText}>Go</Text>
      </TouchableHighlight>
    </View>
    <TouchableHighlight style={styles.button}
        underlayColor='#99d9f4'>
      <Text style={styles.buttonText}>Location</Text>
    </TouchableHighlight>

You’ve added two top-level views here: one to hold a text input and a button, and one with only a button. You’ll read about how these elements are styled in a little bit.

现在你已经加上了两个最高等级的视图（top-level view），一个视图包含了文本输入框和一个按钮，还有一个视图内只有一个按钮。在后文中你会看到，它们的样式是什么样的。

Next, add the accompanying styles to your styles definition:

接着，添加上对应的样式：

    flowRight: {
      flexDirection: 'row',
      alignItems: 'center',
      alignSelf: 'stretch'
    },
    buttonText: {
      fontSize: 18,
      color: 'white',
      alignSelf: 'center'
    },
    button: {
      height: 36,
      flex: 1,
      flexDirection: 'row',
      backgroundColor: '#48BBEC',
      borderColor: '#48BBEC',
      borderWidth: 1,
      borderRadius: 8,
      marginBottom: 10,
      alignSelf: 'stretch',
      justifyContent: 'center'
    },
    searchInput: {
      height: 36,
      padding: 4,
      marginRight: 5,
      flex: 4,
      fontSize: 18,
      borderWidth: 1,
      borderColor: '#48BBEC',
      borderRadius: 8,
      color: '#48BBEC'
    }

Take care with the formatting; each style property should be separated by a comma. That means you’ll need to add a trailing comma after the container selector.

要注意格式问题：每一个样式都是用逗号分隔开的，所以别忘了在```container```选择器后面还要加上一个逗号。

These styles are used by the text input and buttons which you just added.

以上的样式将会应用在你刚刚加上的输入框和按钮上。

Return to the simulator and press Cmd+R to see the updated UI:

现在返回到模拟器，然后按下Cmd+R刷新界面：

![react-searchpageinput](http://cdn2.raywenderlich.com/wp-content/uploads/2015/03/react-searchpageinput-281x500.png)

The text field and ‘Go’ button are on the same row, so you’ve wrapped them in a container that has a flexDirection: 'row' style. Rather than explicitly specifying the widths of the input field and button, you give each a flex value instead. The text field is styled with flex: 4, while the button has flex: 1; this results in their widths having a 4:1 ratio.

文本区域和'Go'按钮在同一行，不需要显式地定义两个组件的宽度，你只需要将它们放在同一个容器中，加上```flexDirection:'row'```样式，再定义好它们的```flex```值。文本区域是```flex:4```，按钮则是```flex:1```，这说明两者的宽度比是4:1。

You might have noticed that your buttons…aren’t actually buttons! :] With UIKit, buttons are little more than labels that can be tapped, and as a result the React Native team decided it was easier to construct buttons directly in JavaScript. The buttons in your app use TouchableHighlight, a React Native component that becomes transparent and reveals the underlay colour when tapped.

大概你也发现了，你的“按钮”其实并不是按钮！:] 使用了UIKit后，按钮更倾向于是可以轻碰（tap）的标签（label），所以React Native团队决定直接在JavaScript中构建按钮了。所以你在App中使用的按钮是```TouchableHighlight```，这是一个React Native组件，当轻碰```TouchableHighlight```时，它会变得透明从而显示出衬底的颜色（也就是按钮下层的组件颜色）。

The final step to complete the search screen of the application is to add the house graphic. The images are available to download as a zip file. Download and unzip the file.

搜索界面的最后一步就是加上一张图片.你可以从[这里下载我们用的图片素材](http://cdn2.raywenderlich.com/wp-content/uploads/2015/03/ReactNative-HouseImage.zip)并解压。

Within Xcode open the Images.xcassets file and tap the plus button to add a new image set. Next drag the required images into the correct ‘slots':

在Xcode中打开**Images.xcassets**文件，点击加号添加一个新的图片集。然后将图片素材拖进正确的“区间”：

![](http://cdn4.raywenderlich.com/wp-content/uploads/2015/03/AddingHouseImage.png)

You’ll have to stop the running application and restart so that these images become available to your React Native app.

你需要重启应用才能让图片生效。

Add the following beneath the TouchableHighlight component for the location button:

将以下代码添加到```TouchableHighlight```组件后面，它将用于“获取位置”按钮：

	<Image source={require('image!house')} style={styles.image}/>

Now, add the image’s corresponding style to the end of the style list, remembering to add a trailing comma to the previous style:

现在再样式表的最后加上图片对应的样式，别忘了给原样式中最后一个加上逗号哦：

    image: {
      width: 217,
      height: 138
    }

The require('image!house') statement is used to reference an image located within your application’s asset catalogue. Within Xcode, if you open up Images.xcassets you will find the ‘house’ icon that the above code refers to.

```require('image!house') ```语句用于确定在你应用的asset目录下的图片资源，在Xcode中，如果你的打开了**Images.xcassets**，你会看到一个“房屋”的图标，正是上面代码中引用到的。

Returning to the simulator, hit Cmd+R and admire your new UI:

返回到模拟器，Cmd+R刷新UI：

![react-searchpagehouse](http://cdn1.raywenderlich.com/wp-content/uploads/2015/03/react-searchpagehouse-281x500.png)

> Note: If you don’t see the house image at this point and see an image that “image!house” cannot be found instead, try restarting the packager (i.e. the “npm start” command you have in the terminal).

> 注意：如果你这会没有看到“房屋”图片，取而代之的是一张“找不到资源”的图片，尝试重启packager（也就是在终端里输入```npm start```命令）。

Your current app looks good, but it’s somewhat lacking in functionality. Your task now is to add some state to your app and perform some actions.

现在你的应用看起来挺不错的啦，不过它还少了点功能。接下来你的任务就是给它加上点状态，让它执行一些操作。

## Adding Component State

## 添加组件状态

Each React component has its own state object, which is used as a key-value store. Before a component is rendered you must set the initial state.

每个 React 组件都带有一个key-value存储的状态对象，你必须在组件渲染之前设置其初始状态。

Within SearchPage.js, add the following code to the SearchPage class, just before render():

在 SearchPage.js 中，我们对 SearchPage 类中，render方法前添加以下的代码。

    constructor(props) {
      super(props);
      this.state = {
        searchString: 'london'
      };
    }

Your component now has a state variable, with searchString set to an initial value of london.

现在你的组件拥有一个状态变量：searchString ，且初始值被设置为 london 。

Time to make use of this component state. Within render, change the TextInput element to the following:

这时候你需要利用起组件中的状态了。在render方法中，用以下的代码替换TextInput元素中的内容：

    <TextInput
      style={styles.searchInput}
      value={this.state.searchString}
      placeholder='Search via name or postcode'/>

This sets the TextInput value property — that is, the text displayed to the user — to the current value of the searchString state variable. This takes care of setting the initial state, but what happens when the user edits this text?

这一步设置了 TextInput 组件 value 属性的值，这个值用于把状态变量 searchString 的当前值作为展示给用户的文字。我们已经考虑初始值的设定了，但如果用户编辑这里的文字会发生什么呢？

The first step is to create a method that acts as an event handler. Within the SearchPage class add the following method:

第一步需要建立一个方法来处理事件。在 SearchPage 类中添加以下的代码：

    onSearchTextChanged(event) {
      console.log('onSearchTextChanged');
      this.setState({ searchString: event.nativeEvent.text });
      console.log(this.state.searchString);
    }

This takes the value from the event’s text property and uses it to update the component’s state. It also adds some logging code that will make sense shortly.

上面的代码从 events 中取出了 text 属性的值，并用于更新组件的状态。这里面也添加了一些有用的调试代码。

To wire up this method so it gets called when the text changes, return to the TextInput field within the render method and add an onChange property so the tag looks like the following:

当文字改变时，需要让这个方法被调用，调用后的文字会通过 render 函数返回到组件中。因此我们需要在标签上添加一个 onChange 属性，添加后的标签如下所示：

    <TextInput
      style={styles.searchInput}
      value={this.state.searchString}
      onChange={this.onSearchTextChanged.bind(this)}
      placeholder='Search via name or postcode'/>

Whenever the user changes the text, you invoke the function supplied to onChange; in this case, it’s onSearchTextChanged.

当用户更改文本时,会调用 onChange 上 的函数;在本例中,则是 onSearchTextChanged 。

> Note: You might be wondering what the bind(this) statement is for. JavaScript treats the this keyword a little differently than most other languages; its counterpart in Swift is self. The use of bind in this context ensures that inside the onSearchTextChanged method, this is a reference to the component instance. For more information, see the [MDN page on this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this).

> 注意:你估计会对 bind(this) 语句有疑问。在 JavaScript 中，this 这个关键字有点不同于大多数其他语言;在 Swift 表示 “自身”。在这种情况中，bind 可以确保在 onSearchTextChanged 方法中, this 可以作为组件实例的引用。有关更多信息,请参见[MDN this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)页面。

There’s one final step before you refresh your app again: add the following logging statement to the top of render(), just before return:

    console.log('SearchPage.render');

在你再次刷新你的应用程序之前，还有一个步骤:在 return 前添加以下语句，打印一条日志来记录 render() 函数的调用:

You are about to learn something quite intriguing from these log statements! :]

Return to your simulator, then press Cmd+R. You should now see that the text input has an initial value of ‘london’ and that editing the text causes some statements to be logged to the Xcode console:

react-renderconsole

你会从这些日志语句中学到一些很有趣的东西!:]

回到你的模拟器,然后按Cmd + R。您现在应该看到文本输入的初始值为 "london" ,编辑一下文本，从而在 Xcode 控制台中产生一些日志:

Looking at the screenshot above, the order of the logging statement seems a little odd:

注意看上面的截图，日志打印的顺序看起来有些奇怪：

This is the initial call to render() to set up the view.
You invoke onSearchTextChanged() when the text changes.
You then update the component state to reflect the new input text, which triggers another render.
onSearchTextChanged() then wraps things up by logging the new search string.
Whenever the app updates the state of any React component, this triggers an entire UI re-rendering that in turn calls render of all of your components. This is a great idea, as it entirely de-couples the rendering logic from the state changes that affect the UI.

第一次调用 render() 函数用于设置视图。当文本变化时， onSearchTextChanged  函数被调用。之后，通过更新组件的状态来反映输入了新的文本,这会触发另一次 render 。 onSearchTextChanged() 函数也会被调用，会将改变的字符串打印出来。每当应用程序更新任何 React 组件,将会触发整个UI层的重新绘制,这会调用你所有组件的 render 方法。这是一个好主意,因为这样做把组件的渲染逻辑，从状态变化影响UI这一过程中完全解耦出来。


With most other UI frameworks, it is either your responsibility to manually update the UI based on state changes, or use of some kind of binding framework which creates an implicit link between the application state and its UI representation; see, for example, my article on implementing the [MVVM pattern with ReactiveCocoa](http://www.raywenderlich.com/74106/mvvm-tutorial-with-reactivecocoa-part-1).

与其他大多数 UI 框架所不同的是,你既不需要在状态改变的时候去手动更新 UI ,或使用某种类型的绑定框架，来创建某种应用程序状态和它的 UI 表现的关联;例如,我的文章中讲的，[通过ReactiveCocoa实现MVVM模式](http://www.raywenderlich.com/74106/mvvm-tutorial-with-reactivecocoa-part-1)。

With React, you no longer have to worry about which parts of the UI might be affected by a state change; your entire UI is simply expressed as a function of your application state.

在 React 中,你不再需要担心 UI 的哪些部分可能受到状态变化的影响;你的整个应用程序的 UI，都可以简单地表示为一个函数的状态。

At this point you’ve probably spotted a fundamental flaw in this concept. Yes, that’s right — performance!

此时,您可能已经发现了这一概念中一个根本性的缺陷。是的,非常准确——性能!

Surely you can’t just throw away your entire UI and re-build it every time something changes? This is where React gets really smart. Each time the UI renders itself, it takes the view tree returned by your render methods, and reconciles — or diffs — it with the current UIKit view. The output of this reconciliation process is a simple list of updates that React needs to apply to the current view. That means only the things that have actually changed will re-render.

你肯定不能在 UI 变化时，完全抛弃掉整个 UI 然后重新绘制吧
？这就是 React 高明的地方了。每当 UI 渲染出来后,render 方法会返回一颗视图渲染树,并与当前的 UIKit 视图进行比较。这个称之为 reconciliation 的过程的输出是一个简单的更新列表, React 会将这个列表应用到当前视图。这意味着，只有实际改变了的部分才会重新绘制。

It’s amazing to see the novel concepts that make ReactJS so unique — the virtual-DOM (Document Object Model, the visual-tree of a web document) and reconciliation — applied to an iOS app.

这个令人拍案叫绝的崭新概念让ReactJS变得独特——virtual-DOM(文档对象模型,一个web文档的视图树)和 reconciliation 这些概念——被应用于iOS应用程序。


You can wrap your head around all that later; you still have some work to do in the app. Remove the logging code you just added above, since it’s no longer necessary and just adds cruft to the code.


稍后你可以整理下思路,之后,在刚才的应用中你仍然有一些工作要做。日志代码增加了代码的繁琐性，已经不需要了，所以删除掉日志代码。

## Initiating a Search

## 初始化搜索功能

In order to implement the search functionality you need handle the ‘Go’ button press, create a suitable API request, and provide a visual indication to the user that a query is in progress.

为了实现搜索功能，你需要处理 “Go” 按钮的点击事件，调用对应的 API，并提供一个视觉效果，告诉用户正在做查询。

Within SearchPage.js, update the initial state within the constructor:

在 `SearchPage.js` 中，在构造函数中把初始状态更新成：

    this.state = {
      searchString: 'london',
      isLoading: false
    };

The new isLoading property will keep track of whether a query is in progress.

新的 `isLoading` 属性将会记录是否有请求正在处理的状态。

Add the following logic to the start of render:

在 `render` 开始的添加如下逻辑：

    var spinner = this.state.isLoading ?
      ( <ActivityIndicatorIOS
          hidden='true'
          size='large'/> ) :
      ( <View/>);

This is a ternary if statement that either adds an activity indicator or an empty view, depending on the component’s isLoading state. Because the entire component is rendered each time, you are free to mix JSX and JavaScript logic.

这是一个三元操作符，与 if 语句类似，即根据组件 `isLoading` 的状态，要么添加一个 indicator，要么添加一个空的 view。因为整个组件会不停地更新，所以你自由地混合 JSX 和 JavaSript 代码。

Within the JSX that defines the search UI in return, add the following line below the Image:

回到用 JSX 定义搜索界面的地方，在图片的下面添加：

    {spinner}

Now, add the following property within the TouchableHighlight tag that renders the ‘Go’ text:

给渲染“Go”的 `TouchableHighlight` 标记添加如下的属性：

    onPress={this.onSearchPressed.bind(this)}

Next, add the following methods to the SearchPage class:

接下来，添加下面这两个方法到 `SearchPage` 类中：

    _executeQuery(query) {
      console.log(query);
      this.setState({ isLoading: true });
    }
     
    onSearchPressed() {
      var query = urlForQueryAndPage('place_name', this.state.searchString, 1);
      this._executeQuery(query);
    }

_executeQuery() will eventually run the query, but for now it simply logs a message to the console and sets isLoading appropriately so the UI can show the new state.

`_executeQuery()` 之后会进行真实的查询，现在的话就是简单输出一条信息到控制台，并且把 `isLoading` 设置为对应的值，这样 UI 就可以显示新的状态了。

> Note: JavaScript classes do not have access modifiers, so they have no concept of ‘private’. As a result you often see developers prefixing methods with an underscore to indicate that they should be considered private.

> 提示：JavaScript 的类并没有访问修饰符，因此没有 “私有”  的该奶奶。因此常常会发现开发者使用一个下划线作为方法的前缀，来说明这些方法是私有方法。

You’ll invoke onSearchPressed() and initiate the query when the ‘Go’ button is pressed.

当 “Go” 按钮被点击时，`onSearchPressed()` 将会被调用，开始查询。

Finally, add the following utility function just above the SearchPage class declaration:

最后，添加下面这个工具函数在定义 `SearchPage` 类的上面：

    function urlForQueryAndPage(key, value, pageNumber) {
      var data = {
          country: 'uk',
          pretty: '1',
          encoding: 'json',
          listing_type: 'buy',
          action: 'search_listings',
          page: pageNumber
      };
      data[key] = value;
     
      var querystring = Object.keys(data)
        .map(key => key + '=' + encodeURIComponent(data[key]))
        .join('&');
     
      return 'http://api.nestoria.co.uk/api?' + querystring;
    };

This function doesn’t depend on SearchPage, so it’s implemented as a free function rather than a method. It first creates the query string based on the parameters in data. Following this, it transforms the data into the required string format, name=value pairs separated by ampersands. The => syntax is an arrow function, another [recent addition to the JavaScript language](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) that provides a succinct syntax for creating anonymous functions.

- 这个函数并不依赖 `SearchPage`，因此被定义成了一个独立的函数，而不是类方法。他首先通过 `data` 来定义查询字符串所需要的参数，接着将 data 转换成需要的字符串格式，`name=value` 对，使用 & 符号分割。语法 => 是一个箭头函数，又一个[对 JavaScript 语言的扩展](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)，提供了这个便捷的语法来创建一个匿名函数。

Head back to the simulator, press Cmd+R to reload the application and tap the ‘Go’ button. You’ll see the activity indicator spin; take a look at the Xcode console to see what it’s telling you:

回到模拟器，Cmd+R，重新加载应用，点击 “Go” 按钮。你可以看到 activity indicator 显示出来，再看看 Xcode 的控制台：

![SearchAcitivityIndicator](http://cdn5.raywenderlich.com/wp-content/uploads/2015/03/SearchAcitivityIndicator-700x169.png)

The activity indicator renders and the URL for the required query appears in the log. Copy and paste that URL into your browser to see the result. You’ll see a massive JSON object. Don’t worry — you don’t need to understand that! You’ll add code to parse that now.

activity indicator 渲染了，并且作为请求的 URL 出现在输出中。把 URL 拷贝到浏览器中访问看看得到的结果。你会看到大量的 JSON 对象。别担心——你不需要理解它们，之后会使用代码来解析之。

> Note: This app makes use of the [Nestoria API for searching property listings](http://www.nestoria.co.uk/help/api). The JSON response coming back from the API is pretty straightforward, but you can have a look at the documentation for all the details on the expected request URL and response formats.

> 提示：应用使用了 [Nestoria 的 API 来做房产的搜索](http://www.nestoria.co.uk/help/api)。API 返回的 JSON 数据非常的直白。但是你也可以看看文档了解更多细节，请求什么 URL 地址，以及返回数据的格式。

The next step is to make the request from within your application.

下一步就是从应用中发出请求。

## Performing an API Request

## 执行 API 请求

Still within SearchPage.js, update the initial state in the class constructor to add a message variable:

还是 `SearchPage.js` 文件中，更新构造器中的初始 `state` 添加一个 `message` 变量：

    this.state = {
      searchString: 'london',
      isLoading: false,
      message: ''
    };

Within render, add the following to the bottom of your UI:

在 `render` 内部，将下面的代码添加到 UI 的底部：

    <Text style={styles.description}>{this.state.message}</Text>

You’ll use this to display a range of messages to the user.

你需要使用这个为用户展示多种信息。

Within the SearchPage class, add the following code to the end of _executeQuery():

在 `SearchPage` 类内部，将以下代码添加到 `_executeQuery()` 底部：

    fetch(query)
      .then(response => response.json())
      .then(json => this._handleResponse(json.response))
      .catch(error => 
         this.setState({
          isLoading: false,
          message: 'Something bad happened ' + error
       }));

This makes use of the fetch function, which is [part of the Web API](https://fetch.spec.whatwg.org/), and provides a vastly improved API versus XMLHttpRequest. The asynchronous response is returned [as a promise](http://www.html5rocks.com/en/tutorials/es6/promises/), with the success path parsing the JSON and supplying it to a method which you are going to add next.

这里使用了 `fetch` 函数，它是 [Web API 的一部分](https://fetch.spec.whatwg.org/)。和 XMLHttpRequest 相比，它提供了更加先进的 API。异步响应会返回[一个 promise](http://www.html5rocks.com/en/tutorials/es6/promises/)，成功的话会转化 JSON 并且为它提供了一个你将要添加的方法。

The final step is to add the following function to SearchPage:

最后一步是将下面的函数添加到 `SearchPage`：

    _handleResponse(response) {
      this.setState({ isLoading: false , message: '' });
      if (response.application_response_code.substr(0, 1) === '1') {
        console.log('Properties found: ' + response.listings.length);
      } else {
        this.setState({ message: 'Location not recognized; please try again.'});
      }
    }

This clears isLoading and logs the number of properties found if the query was successful.

如果查询成功，这个方法会清除掉正在加载标识并且记录下查询到属性的个数。

> Note: Nestoria has [a number of non-1** response codes](http://www.nestoria.co.uk/help/api-return-codes) that are potentially useful. For example, 202 and 200 return a list of best-guess locations. When you’ve finished building your app, why not try handling these and present a list of options to the user?

> 注意：Nestoria 有[很多种返回码](http://www.nestoria.co.uk/help/api-return-codes)具备潜在的用途。比如，202 和 200 会返回最佳位置列表。当你创建完一个应用，为什么不处理一下这些，可以为用户呈现一个可选列表。

Save your work, then in the simulator press Cmd+R and try searching for ‘london’; you should see a log message saying that 20 properties were found. Next try a non-existent location, such as ‘narnia’ (*sniff*), and you’ll be greeted by the following message:

保存项目，然后在模拟器中按下 Cmd+R，尝试搜索 ‘london’；你会在日志信息中看到 `20 properties were found`。然后随便尝试搜索一个不存在的位置，比如‘narnia’，你会得到下面的问候语。

![react-narnia](http://cdn4.raywenderlich.com/wp-content/uploads/2015/03/react-narnia-281x500.png)

It’s time to see what those 20 properties in real places such as London look like!

是时候看一下这20个属性所对应的真实的地方，比如伦敦！

## Displaying the Results

## 结果显示

Create a new file SearchResults.js, and add the following:

创建一个新的文件，命名为 **SearchResults.js**，然后加上下面这段代码：

    'use strict';
     
    var React = require('react-native');
    var {
      StyleSheet,
      Image, 
      View,
      TouchableHighlight,
      ListView,
      Text,
      Component
    } = React;

Yes, that’s right, it’s a require statement that includes the react-native module, and a destructuring assignment. You’ve been paying attention haven’t you? :]

你肯定注意到啦，这里用到了 ```require``` 语句将 ```react-native``` 模块引入其中，还有一个重构赋值语句。

Next add the component itself:

接着就是加入搜索结果的组件：

    class SearchResults extends Component {
     
      constructor(props) {
        super(props);
        var dataSource = new ListView.DataSource(
          {rowHasChanged: (r1, r2) => r1.guid !== r2.guid});
        this.state = {
          dataSource: dataSource.cloneWithRows(this.props.listings)
        };
      }
     
      renderRow(rowData, sectionID, rowID) {
        return (
          <TouchableHighlight
              underlayColor='#dddddd'>
            <View>
              <Text>{rowData.title}</Text>
            </View>
          </TouchableHighlight>
        );
      }
     
      render() {
        return (
          <ListView
            dataSource={this.state.dataSource}
            renderRow={this.renderRow.bind(this)}/>
        );
      }
     
    }

The above code makes use of a more specialized component — ListView — which displays rows of data within a scrolling container, similar to UITableView. You supply data to the ListView via a ListView.DataSource, and a function that supplies the UI for each row.

上述的代码里用到了一个特定的组件 -- ```ListView``` -- 它能将数据一行行地呈现出来，并放置在一个可滚动的容器内，和 ```UITableView``` 很相似。通过 ```ListView.DataSource``` 将 ```ListView``` 的数据引入，还有一个函数来显示每一行UI。

When constructing the data source, you provide a function that compares the identity of a pair of rows. The ListView uses this during the reconciliation process, in order to determine the changes in the list data. In this instance, the properties returned by the Nestoria API have a guid property, which is a suitable check for this purpose.

在构建数据源的同时，你还需要一个函数用来比较每两行之间是否重复。 为了确认列表数据的变化，在 reconciliation 过程中```ListView``` 就会使用到这个函数。在这个实例中，由 Nestoria API 返回的房屋数据都有一个 ```guid``` 属性，它就是用来测试数据变化的。

Now add the module export to the end of the file:

现在将模块导出的代码添加至文件末尾：

    module.exports = SearchResults;

Add the following to SearchPage.js near the top of the file, underneath the require call for React:

将下面这段代码加到 **SearchPage.js** 较前的位置，不过要在 ```require``` 语句的后面哦：

    var SearchResults = require('./SearchResults');

This allows us to use the newly added SearchResults class from within the SearchPage class.

这样我们就能在 ```SearchPage``` 类中使用刚刚加上的 ```SearchResults``` 类。

Modify the current _handleResponse method by replacing the console.log statement with the following:

还要把 ```_handleResponse``` 方法中的 ```console.log``` 语句改成下面这样：

    this.props.navigator.push({
      title: 'Results',
      component: SearchResults,
      passProps: {listings: response.listings}
    });

The above code navigates to your newly added SearchResults component and passes in the listings from the API request. Using the push method ensures the search results are pushed onto the navigation stack, which means you’ll get a ‘Back’ button to return to the root.

```SearchResults``` 组件通过上面的代码传入列表里。在这里用的是 ```push``` 方法确保搜索结果全部推进导航栈中，这样你就可以通过 'Back' 按钮返回到根页面。

Head back to the simulator, press Cmd+R and try a quick search. You’ll be greeted by a list of properties:

回到模拟器，按下 Cmd+R 刷新页面，然后试试看我们的搜索。估计你会得到类似下面这样的结果：

![react-searchresults1](http://cdn3.raywenderlich.com/wp-content/uploads/2015/03/react-searchresults1-281x500.png)

It’s great to see the property listings, but that list is a little drab. Time to liven things up a bit.

耶你的搜索实现了呢，不过这个搜索结果颜值好低。不要担心，接下来给它化化妆。

## A Touch of Style

## 可点击样式

This React Native code should be starting to look familiar by now, so this tutorial is going to pick up the pace.

这些 React Native 的原生代码现在应该理解起来轻车熟路了,所以本教程将会加快速度。

Add the following style definition just after the destructuring assignment in SearchResults.js:

在 SearchResults.js 中，destructuring 声明后面添加以下语句来定义样式：

    var styles = StyleSheet.create({
      thumb: {
        width: 80,
        height: 80,
        marginRight: 10
      },
      textContainer: {
        flex: 1
      },
      separator: {
        height: 1,
        backgroundColor: '#dddddd'
      },
      price: {
        fontSize: 25,
        fontWeight: 'bold',
        color: '#48BBEC'
      },
      title: {
        fontSize: 20,
        color: '#656565'
      },
      rowContainer: {
        flexDirection: 'row',
        padding: 10
      }
    });

This defines all the styles that you are going to use to render each row.

这些定义了每一行的样式。

Next replace renderRow() with the following:

接下来修改 renderRow() 如下:

    renderRow(rowData, sectionID, rowID) {
      var price = rowData.price_formatted.split(' ')[0];
     
      return (
        <TouchableHighlight onPress={() => this.rowPressed(rowData.guid)}
            underlayColor='#dddddd'>
          <View>
            <View style={styles.rowContainer}>
              <Image style={styles.thumb} source={{ uri: rowData.img_url }} />
              <View  style={styles.textContainer}>
                <Text style={styles.price}>£{price}</Text>
                <Text style={styles.title} 
                      numberOfLines={1}>{rowData.title}</Text>
              </View>
            </View>
            <View style={styles.separator}/>
          </View>
        </TouchableHighlight>
      );
    }

This manipulates the returned price, which is in the format ‘300,000 GBP’, to remove the GBP suffix. Then it renders the row UI using techniques that you are by now quite familiar with. This time, the data for the thumbnail image is supplied via a URL, and React Native takes care of decoding this off the main thread.

这个操作修改了返回的价格,将已经格式了化的"300000 GBP"中的GBP后缀删除。然后它通过你已经很熟悉的技术来渲染每一行的 UI 。这一次,通过一个 URL 来提供缩略图的数据, React Native 负责在主线程之外解码这些数据。

Also, note the use of an arrow function in the onPress property of the TouchableHighlight component; this is used to capture the guid for the row.

同时要注意 TouchableHighlight 组件中 onPress属性后使用的箭头函数；它用于捕获每一行的 guid。

The final step is to add this method to the class to handle the press:

最后一步，给类添加一个方法来处理按下操作:

    rowPressed(propertyGuid) {
      var property = this.props.listings.filter(prop => prop.guid === propertyGuid)[0];
    }

This method locates the property that was tapped by the user. It doesn’t do anything with it yet, but you’ll fix that shortly. But right now, it’s time to admire your handiwork.

该方法通过用户触发的属性来定位。目前该方法没有做任何事，你可以稍后处理。现在，是时候欣赏你的大作了。

Head back to the simulator, press Cmd+R and check out your results:

回到模拟器,按下 Cmd + R 查看结果:

![react-searchresults2](http://cdn2.raywenderlich.com/wp-content/uploads/2015/03/react-searchresults2-281x500.png)

That looks a lot better — although it’s a wonder anyone can afford to live in London!

看起来好多了——尽管你会怀疑是否任何人都能承受住在伦敦的代价!

Time to add the final view to the application.

是时候向应用程序添加最后一个视图了。

## Property Details View

## 房产详情视图

Add a new file PropertyView.js to the project, then add the following to the top of the file:

添加一个新的文件 `PropertyView.js` 到项目中，在文件的顶部添加如下代码：

    'use strict';
     
    var React = require('react-native');
    var {
      StyleSheet,
      Image, 
      View,
      Text,
      Component
    } = React;

Surely you can do this in your sleep by now! :]

信手拈来了吧！

Next add the following styles:

接着添加如下样式：

    var styles = StyleSheet.create({
      container: {
        marginTop: 65
      },
      heading: {
        backgroundColor: '#F8F8F8',
      },
      separator: {
        height: 1,
        backgroundColor: '#DDDDDD'
      },
      image: {
        width: 400,
        height: 300
      },
      price: {
        fontSize: 25,
        fontWeight: 'bold',
        margin: 5,
        color: '#48BBEC'
      },
      title: {
        fontSize: 20,
        margin: 5,
        color: '#656565'
      },
      description: {
        fontSize: 18,
        margin: 5,
        color: '#656565'
      }
    });

Then add the component itself:

然后加上组件本身：

    class PropertyView extends Component {
     
      render() {
        var property = this.props.property;
        var stats = property.bedroom_number + ' bed ' + property.property_type;
        if (property.bathroom_number) {
          stats += ', ' + property.bathroom_number + ' ' + (property.bathroom_number > 1
            ? 'bathrooms' : 'bathroom');
        }
     
        var price = property.price_formatted.split(' ')[0];
     
        return (
          <View style={styles.container}>
            <Image style={styles.image} 
                source={{uri: property.img_url}} />
            <View style={styles.heading}>
              <Text style={styles.price}>£{price}</Text>
              <Text style={styles.title}>{property.title}</Text>
              <View style={styles.separator}/>
            </View>
            <Text style={styles.description}>{stats}</Text>
            <Text style={styles.description}>{property.summary}</Text>
          </View>
        );
      }
    }

The first part of render() performs some manipulation on the data. As is often the case, the data returned by the API is of mixed quality and often has missing fields. This code applies some simple logic to make the data a bit more presentable.

`render()` 前面部分对数据进行了处理，与通常的情况一样，API 返回的数据良莠不齐，往往有些字段是缺失的。这段代码通过一些简单的逻辑，让数据更加地规整一些。

The rest of render is quite straightforward; it’s simply a function of the immutable state of this component.

`render` 剩余的部分就非常直接了。它就是一个简单的这个状态不可变状态的函数。

Finally add the following export to the end of the file:

最后在文件的末尾加上如下的 export：

    module.exports = PropertyView;

Head back to SearchResults.js and add the require statement to the top of the file, just underneath the React require line:

返回到 `SearchResults.js` 文件，在顶部，require React 的下面，添加一个新的 `require` 语句。

    var PropertyView = require('./PropertyView');

Next update rowPressed() to navigate to your newly added PropertyView:

接下来更新 `rowPassed() `，添加跳转到新加入的 `PropertyView`：
    
    rowPressed(propertyGuid) {
      var property = this.props.listings.filter(prop => prop.guid === propertyGuid)[0];
     
      this.props.navigator.push({
        title: "Property",
        component: PropertyView,
        passProps: {property: property}
      });
    }

You know the drill: head back to the Simulator, press Cmd+R, and go all the way to the property details by running a search and tapping on a row:

你知道的：回到模拟器，`Cmd + R`，一路通过搜索点击一行到房产详情界面：

![react-property](http://cdn3.raywenderlich.com/wp-content/uploads/2015/03/react-property-281x500.png)

Affordable living at it’s finest — that’s a fancy looking pad!

物廉价美——看上去很不错哦！

Your app is almost complete; the final step is to allow users to search for nearby properties.

应用即将完成，最后一步是允许用户搜索附近的房产。

## Geolocation Search

## 地理位置搜索

Within Xcode, open Info.plist and add a new key, by right clicking inside the editor and selecting Add Row. Use NSLocationWhenInUseUsageDescription as the key name and use the following value:

在 Xcode 中，打开 `Info.plist` 添加一个新的 `key`，在编辑器内部单击鼠标右键并且选择 **Add Row**。使用 `NSLocationWhenInUseUsageDescription` 作为 `key` 名并且使用下面的值：

    PropertyFinder would like to use your location to find nearby properties

Here’s how your plist file will look once you’ve added the new key:

下面是当你添加了新的 `key` 后，所得到的属性列表：

![Info.plist after adding key](http://cdn3.raywenderlich.com/wp-content/uploads/2015/03/Screen-Shot-2015-03-20-at-21.49.06-480x162.png)

This key details the prompt that you’ll present to the to the user to request access to their current location.

你将把这个关键的细节提示呈现给用户，方便他们请求访问当前位置。

Open SearchPage.js, locate the TouchableHighlight that renders the ‘Location’ button and add the following property value:

打开 `SearchPage.js`，找到用于渲染 `Location` 按钮的 `TouchableHighlight`，然后为其添加下面的属性值：

    onPress={this.onLocationPressed.bind(this)}

When you tap the button, you ‘ll invoke onLocationPressed — you’re going to add that next.

当你用手指轻点这个按钮，会调用 `onLocationPressed` —— 接下来会定义这个方法。

Add the following within the body of the SearchPage class:

将下面的代码添加到 `SearchPage` 类中：

    onLocationPressed() {
      navigator.geolocation.getCurrentPosition(
        location => {
          var search = location.coords.latitude + ',' + location.coords.longitude;
          this.setState({ searchString: search });
          var query = urlForQueryAndPage('centre_point', search, 1);
          this._executeQuery(query);
        },
        error => {
          this.setState({
            message: 'There was a problem with obtaining your location: ' + error
          });
        });
    }

The current position is retrieved via navigator.geolocation; this is an interface [defined by the Web API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation/Using_geolocation), so it should be familiar to anyone who has used location services within the browser. The React Native framework provides its own implementation of this API using the native iOS location services.

通过 `navigator.geolocation` 检索当前位置；这是一个 [Web API 所定义的](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation/Using_geolocation)接口，所以对于每个在浏览器中使用 `location` 服务的用户来说这个接口都应该是一致的。React Native 框架借助原生的 iOS `location` 服务提供了自身的 API 实现。

If the current position is successfully obtained, you invoke the first arrow function; this sends a query to Nestoria. If something goes wrong, you’ll display a basic error message instead.

如果当前位置很容易获取到，你将调用第一个箭头函数；这会向 `Nestoria` 发送一个 `query`。如果出现错误则会得到一个基本的出错信息。

Since you’ve made a change to the plist, you’ll need to relaunch the app to see your changes. No Cmd+R this time — sorry. Stop the app in Xcode, and build and run your project.

因为你已经改变了属性列表，你需要重新启动这个应用以看到更改。抱歉，这次不可以 Cmd+R。请中断 Xcode 中的应用，然后创建和运行项目。

Before you use the location-based search, you need to specify a location that is covered by the Nestoria database. From the simulator menu, select Debug\Location\Custom Location … and enter a latitude of 55.02 and a longitude of -1.42, the coordinates of a rather nice seaside town in the North of England that I like to call home!

在使用基于位置的搜索前，你需要指定一个被 Nestoria 数据库覆盖的位置。在模拟器菜单中，选择 `Debug\Location\Custom Location … ` 然后输入 55.02 维度和 -1.42 经度，这个坐标是英格兰北部的一个景色优美的海边小镇，我经常在那给家里打电话。

![WhitleyBaySearch](http://cdn1.raywenderlich.com/wp-content/uploads/2015/03/WhitleyBaySearch-647x500.png)

> Note from Ray: Location searching worked for some of us, but not for others (reporting an access denied error even though we gave access) – we’re not sure why at the moment, perhaps an issue with React Native? If anyone has the same issue and figures it out, please let us know.

> 警示：我们可以正常地使用位置搜索功能，不过可能有部分同学不能使用（在访问时返回 `access denied` 错误）—— 我们尚不确定其原因，可能是 React Native 的问题？如果谁遇到了同样的问题并且已经结果，烦请告诉我们。
It’s not quite as swank as London — but it’s a lot more affordable! :]

这里没有伦敦那样值得炫耀 —— 不过更加经济！

## Where To Go From Here? 

## 下一步行动？

Congratulations on completing your first React Native application! You can [download the final project for this tutorial](http://cdn1.raywenderlich.com/wp-content/uploads/2015/03/PropertyFinder-Final1.zip) to try out on your own.

完成了第一个React Native应用呢，恭喜你！你可以[下载本教程的完整代码](http://cdn1.raywenderlich.com/wp-content/uploads/2015/03/PropertyFinder-Final1.zip)，亲自来试试看。

If you come from the web world, you can see how easy it is to define your interface and navigation with JavaScript and React to get a fully native UI from your code. If you work mainly on native apps, I hope you’ve gained a feel for the benefits of React Native: fast app iteration, modern JavaScript and clear style rules with CSS.

如果已经接触过Web开发了，你会发现使用JavaScript和React来定义与原生UI相连接的接口和导航是多么地容易。而如果你曾经开发过原生App，我相信在使用React Native的过程里你会感受到它种种好处：快速的应用迭代，JavaScript的引入以及清晰地使用CSS定义样式。

Perhaps you might write your next app using this framework? Or then again, perhaps you will stick with Swift or Objective-C? Whichever path you take, I hope you have learned something new and interesting in this article, and can carry some of the principles forward into your next project.

也许下次做App的时候，你可以试试这个框架？或者说，你依然坚持使用Swift或者Objective-C？无论之后你的选择是怎么样的，我都希望读完这篇文章的你有所收获，还能把这些收获融入到你的项目当中是最好的啦。

If you have any questions or comments on this tutorial, feel free to join the discussion in the forums below!

如果你对这篇教程又任何疑问，请在[这篇文章](http://www.raywenderlich.com/99473/introducing-react-native-building-apps-javascript)下方的讨论区留言！

原文：http://www.raywenderlich.com/99473/introducing-react-native-building-apps-javascript