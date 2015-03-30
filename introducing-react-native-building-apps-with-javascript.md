# Introducing React Native: Building Apps with JavaScript

# 深入浅出 React Native：使用 JavaScript 构建原生应用

【寸志】

A few months ago [Facebook announced React Native](https://code.facebook.com/videos/786462671439502/react-js-conf-2015-keynote-introducing-react-native-/), a framework that lets you build native iOS applications with JavaScript – and the[ official repository](https://github.com/facebook/react-native) just came out of beta today!

People have been using JavaScript and HTML5 to create iOS applications using the [PhoneGap](http://phonegap.com/) wrapper for a number of years, so is React Native really such a big deal?

React Native is a big deal, and people are getting very excited about it for two main reasons:

1. With React Native your application logic is written and runs in JavaScript, whereas your application UI is fully native; therefore you have none of the compromises typically associated with HTML5 UI.

2. React introduces a novel, radical and highly functional approach to constructing user interfaces. In brief, the application UI is simply expressed as a function of the current application state.

The key point with React Native is that it aims to primarily bring the power of the [React](http://facebook.github.io/react/) programming model to mobile app development. It is not aiming to be a cross platform, write-once run-anywhere, tool. It is aiming to be learn-once write-anywhere. An important distinction to make. This tutorial only covers iOS, but once you’ve learned the concepts here you could port that knowledge into creating an Android app very quickly.

If you have only ever written applications in Objective-C or Swift, you might not be particularly excited about the prospect of using JavaScript instead. Although, as a Swift developer, the second point above should pique your interest!

Through Swift, you’ve no doubt been learning new and more functional ways to encode algorithms, and techniques that encourage transformation and immutability. However, the way in which you construct your UI is very much the same as it was when developing with Objective-C: it’s still UIKit-based and imperative.

Through intriguing concepts such as a virtual DOM and reconciliation, React brings functional programming directly to the UI layer.

This tutorial takes you through the process of building an application for searching UK property listings:

![PropertyFinder](http://cdn4.raywenderlich.com/wp-content/uploads/2015/03/PropertyFinder-700x360.png)

If you’ve never written any JavaScript before, fear not; this tutorial leads you through each and every step of the coding. React uses CSS properties for styling which are generally easy to read and understand, but if you need to, you can always refer to the excellent [Mozilla Developer Network reference](https://developer.mozilla.org/en-US/docs/Web/CSS).

Want to learn more? Read on!

## Getting Started 【洪春】

## 起手

The React Native framework is [available via GitHub](https://github.com/facebook/react-native). You can grab the framework by either cloning the repository using git, or you can choose download it as a zip file. Once you have the React Native framework locally, there are a few other prerequisites to take care of before you can start coding.

- React Native uses [Node.js](https://nodejs.org/), a JavaScript runtime, to build your JavaScript code. If you don’t already have Node.js installed, it’s time to get it!

First [install Homebrew](http://brew.sh/) using the instructions on the Homebrew website, then install Node.js by executing the following in a Terminal window:

    brew install node

Next, use homebrew to install [watchman](https://facebook.github.io/watchman/), a file watcher from Facebook:

    brew install watchman

This is used by React Native to figure out when your code changes and rebuild accordingly. It’s like having Xcode do a build each time you save your file.

The React Native code has a number of dependencies that you need to satisfy before you can run it. Open a Terminal window in your React Native folder and execute the following:

    npm install

This uses the Node Package Manager to fetch the project dependencies; it’s similar in function to CocoaPods or Carthage. Once this command has run successfully, you’ll find a node_modules folder has been created with the various external dependencies.

The final step is to start the development server. Within the same Terminal window as the previous step, execute the following:

    npm start

On executing the above, you will see the following:

    $ npm start
     
    > react-native@0.1.0 start /Users/colineberhardt/Projects/react-native
    > ./packager/packager.sh
     
     
     ===============================================================
     |  Running packager on port 8081.       
     |  Keep this packager running while developing on any JS         
     |  projects. Feel free to close this tab and run your own      
     |  packager instance if you prefer.                              
     |                                                              
     |     https://github.com/facebook/react-native                 
     |                                                              
     ===============================================================
 
 
		React packager ready.

That’s it, you’re good to get started! Leave the script running in the terminal window as you continue with the tutorial.

At this point, I’d recommend trying one of the React Native example apps to test your setup. Open the project from the react-native/Examples/Movies folder in Xcode, then build and run it and check that you can launch the Movies application without issue.

> Note: One final thing before you get too deep in the code — you’re going to be writing a lot of JavaScript code in this tutorial, and Xcode is certainly not the best tool for this job! I use [Sublime Text](http://www.sublimetext.com/), which is a cheap and versatile editor, but [atom](https://atom.io/), [brackets](http://brackets.io/) or any other lightweight editor will do the job.

## Hello React Native 【六妹】

## React Native 你好

Before getting started on the property search application, you’re going to create a very simple Hello World app. You’ll be introduced to the various components and concepts as you go along.

Download and unzip the [starter project](http://cdn5.raywenderlich.com/wp-content/uploads/2015/03/PropertyFinderStarter.zip) for this tutorial to the react-native/Examples folder. Once unzipped, open the PropertyFinder project within Xcode. Don’t build and run just yet; you’re going to have to write some JavaScript first!

Open PropertyFinderApp.js in your text editor of choice and add the following to the start of the file:

    'use strict';

This enables Strict Mode, which adds improved error handling and disables some less-than-ideal JavaScript language features. In simple terms, it makes JavaScript better!

> Note: For a more detailed overview of Strict Mode, I’d encourage you to read [Jon Resig’s article](http://ejohn.org/blog/ecmascript-5-strict-mode-json-and-more/) “ECMAScript 5 Strict Mode, JSON, and More”.

Next, add the following line:

    var React = require('react-native');

This loads the react-native module and assigns it to React. React Native uses the same module loading technology as Node.js with the require function, which is roughly equivalent to linking and importing libraries in Swift.

> Note: For more information about JavaScript modules I’d recommend reading[ this article by Addy Osmani](http://addyosmani.com/writing-modular-js/) on writing modular JavaScript.

Just below the require statement, add the following:

    var styles = React.StyleSheet.create({
      text: {
        color: 'black',
        backgroundColor: 'white',
        fontSize: 30,
        margin: 80
      }
    });

This defines a single style that you will shortly apply to your “Hello World” text. If you’ve done any web development before, you’ll probably recognize those property names; React Native uses [Cascading Style Sheets (CSS)](http://www.w3schools.com/css/) to style the application UI.

Now for the app itself! Still working in the same file, add the following code just beneath the style declaration above:

    class PropertyFinderApp extends React.Component {
      render() {
        return React.createElement(React.Text, {style: styles.text}, "Hello World!");
      }
    }

Yes, that’s a JavaScript class!

Classes were introduced in ECMAScript 6 (ES6). Although JavaScript is constantly evolving, web developers are restricted in what they can use due to the need to maintain compatibility with older browsers. React Native runs within JavaScriptCore; as a result, you can use modern language features without worrying about supporting legacy browsers.

> Note: If you are a web developer, I’d thoroughly encourage you to use modern JavaScript, and then convert to older JavaScript using tools such as [Babel](https://babeljs.io/) to maintain support for older and incompatible browsers.

PropertyFinderApp extends React.Component, the basic building block of the React UI. Components contain immutable properties, mutable state variables and expose a method for rendering. Your current application is quite simple and only requires a render method.

React Native components are not UIKit classes; instead they are a lightweight equivalent. The framework takes care of transforming the tree of React components into the required native UI.

Finally, add the following to the end of the file:

    React.AppRegistry.registerComponent('PropertyFinderApp', function() { return PropertyFinderApp });

React.AppRegistry.registerComponent('PropertyFinderApp', function() { return PropertyFinderApp });
AppRegistry defines the entry point to the application and provides the root component.

Save your changes to PropertyFinderApp.js and then return to Xcode. Ensure the PropertyFinder scheme is selected with one of the iPhone simulators, and then build and run your project. After a few seconds you’ll see your “Hello World” app in action:

![react-helloworld](http://cdn4.raywenderlich.com/wp-content/uploads/2015/03/react-helloworld-281x500.png)

That’s a JavaScript application running in the simulator, rendering a native UI, without a browser in sight!

Still don’t trust me? :] Verify it for yourself: within Xcode, select Debug\View Debugging\Capture View Hierarchy and take a look at the native view hierarchy. You will see no UIWebView instances anywhere! Just a proper, real, view! Neat :]!

![A native view hierarchy](http://cdn4.raywenderlich.com/wp-content/uploads/2015/03/ViewDebugging-480x227.png)

Curious as to how it all works? In Xcode open AppDelegate.m and locate application:didFinishLaunchingWithOptions:. This method constructs a RCTRootView, which loads the JavaScript application and renders the resultant view.

When the application starts, the RCTRootView loads the application from the following URL:

    http://localhost:8081/Examples/PropertyFinder/PropertyFinderApp.includeRequire.runModule.bundle

Recall at the beginning of the tutorial when you executed npm start within a terminal window; this starts a packager and server that handles the above request.

Open this URL in Safari; you’ll see the JavaScript code for your app. You should be able to find your “Hello World” code embedded among the React Native framework.

When your app starts, this code is loaded and executed by the JavaScriptCore framework. In the case of your application, it loads the PropertyFinderApp component, then constructs the native UIKit view. You’ll learn a bit more about this later in the tutorial.

## Hello World JSX 【寸志】

## 你好 JSX 的世界

Your current application uses React.createElement to construct the simple UI for your application, which React turns into the native equivalent. While your JavaScript code is perfectly readable in its present form, a more complex UI with nested elements would rapidly become quite a mess.

Make sure the app is still running, then return to your text editor to edit PropertyFinderApp.js. Modify the return statement of your component’s render method as follows:

    return <React.Text style={styles.text}>Hello World (Again)</React.Text>;

return <React.Text style={styles.text}>Hello World (Again)</React.Text>;
This is JSX, or JavaScript syntax extension, which mixes HTML-like syntax directly in your JavaScript code; if you’re already a web developer, this should feel rather familiar. You’ll use JSX throughout this article.

Save your changes to PropertyFinderApp.js and return to the simulator. Press Cmd+R, and you’ll see your application refresh to display the updated message “Hello World (Again)”.

Re-running a React Native application is really as simple as refreshing a web browser! :]

Since you’ll be working with the same set of JavaScript, you can leave the app running and simply refresh the app in this fashion after modifying and saving PropertyFinderApp.js.

> Note: If you are feeling inquisitive, take a look at your ‘bundle’ in the browser to see what the JSX is transformed into.

Okay, enough of this “Hello World” fun; it’s time to build the real application!

## Adding Navigation 【洪春】

## 添加导航

The Property Finder app uses the standard stack-based navigation experience provided by UIKit’s navigation controller. It’s time to add this behavior.

Within PropertyFinderApp.js, rename the PropertyFinderApp class to HelloWorld:

    class HelloWorld extends React.Component {

class HelloWorld extends React.Component {
You’ll keep the “Hello World” text display around a little longer, but it won’t be the root component of your app anymore.

Next add the following class below the HelloWorld component:

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

Within the same file, update the styles declaration to include the container style as shown below:

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

Head back to the simulator and press Cmd+R to see your new UI in action:

![react-helloworldagain](http://cdn5.raywenderlich.com/wp-content/uploads/2015/03/react-helloworldagain-281x500.png)

There’s the navigation controller with its root view, which is currently the “Hello World” text. Excellent — you now have the basic navigation structure for your application in place. It’s time to add the ‘real’ UI!

## Building the Search Page 【六妹】

## 创建搜索页

Add a new file to the project named SearchPage.js and place it in the same folder as PropertyFinderApp.js. Add the following code to this file:

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

This is a destructuring assignment, which lets you extract multiple object properties and assign them to variables using a single statement. As a result, the rest of your code can drop the React prefix; for example, you can refer directly to StyleSheet rather than React.StyleSheet. Destructuring is also useful for manipulating arrays and is [well worth learning more about](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment).

Still working in the same file, SearchPage.js, add the following style:

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

Add the component itself just below the styles you added above:

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

Finally, add the following to the end of the file:

    module.exports = SearchPage;

This exports the SearchPage class, which permits its use in other files.

The next step is to update the application routing in order to make this the initial route.

Open PropertyFinderApp.js and add the following just after the current require import near the top of the file:

    var SearchPage = require('./SearchPage');

Within the render function of the PropertyFinderApp class, update initialRoute to reference the newly added page as shown below:

    component: SearchPage

At this point you can remove the HelloWorld class and its associated style, if you like. You won’t be needing that code any longer.

Return to the simulator, hit Cmd+R and check out the new UI:

![react-searchstarter](http://cdn1.raywenderlich.com/wp-content/uploads/2015/03/react-searchstarter-281x500.png)

This is using the new component, SearchPage, which you added.

## Styling with Flexbox 【寸志】

## 使用 Flexbox 定义外观

So far, you’ve seen basic CSS properties that deal with margins, paddings and color. However, you might not be familiar with flexbox, a more recent addition to the CSS specification that is very useful for application UI layout.

React Native uses the [css-layout](https://github.com/facebook/css-layout) library, a JavaScript implementation of the flexbox standard which is transpiled into C (for iOS) and Java (for Android).

It’s great that Facebook has created this as a separate project that targets multiple languages, since this allows for novel applications, such as applying [flexbox layout to SVG](http://blog.scottlogic.com/2015/02/02/svg-layout-flexbox.html) (yes, that was written by me … and no, I don’t sleep much!).

In your app, the container has the default flow direction of column, which means its children are arranged in a vertical stack, like so:

![FlexStack](http://cdn3.raywenderlich.com/wp-content/uploads/2015/03/FlexStack.png)

This is termed the main axis, and can run either vertically or horizontally.

The vertical position of each child is determined from a combination of its margin, height and padding. The container also sets the alignItems property to center, which determines the placement of children on the cross axis. In this case, it results in center-aligned text.

It’s time to add the input field and buttons. Open SearchPage.js and insert the following just after the closing tag of the second Text element:

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

Next, add the accompanying styles to your styles definition:

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

These styles are used by the text input and buttons which you just added.

Return to the simulator and press Cmd+R to see the updated UI:

![react-searchpageinput](http://cdn2.raywenderlich.com/wp-content/uploads/2015/03/react-searchpageinput-281x500.png)

The text field and ‘Go’ button are on the same row, so you’ve wrapped them in a container that has a flexDirection: 'row' style. Rather than explicitly specifying the widths of the input field and button, you give each a flex value instead. The text field is styled with flex: 4, while the button has flex: 1; this results in their widths having a 4:1 ratio.

You might have noticed that your buttons…aren’t actually buttons! :] With UIKit, buttons are little more than labels that can be tapped, and as a result the React Native team decided it was easier to construct buttons directly in JavaScript. The buttons in your app use TouchableHighlight, a React Native component that becomes transparent and reveals the underlay colour when tapped.

The final step to complete the search screen of the application is to add the house graphic. Add the following beneath the TouchableHighlight component for the location button:

<Image source={require('image!house')} style={styles.image}/>
Now, add the image’s corresponding style to the end of the style list, remembering to add a trailing comma to the previous style:

    image: {
      width: 217,
      height: 138
    }

The require('image!house') statement is used to reference an image located within your application’s asset catalogue. Within Xcode, if you open up Images.xcassets you will find the ‘house’ icon that the above code refers to.

Returning to the simulator, hit Cmd+R and admire your new UI:

![react-searchpagehouse](http://cdn1.raywenderlich.com/wp-content/uploads/2015/03/react-searchpagehouse-281x500.png)

> Note: If you don’t see the house image at this point and see an image that “image!house” cannot be found instead, try restarting the packager (i.e. the “npm start” command you have in the terminal).

Your current app looks good, but it’s somewhat lacking in functionality. Your task now is to add some state to your app and perform some actions.

## Adding Component State 【洪春】

## 添加组件状态

Each React component has its own state object, which is used as a key-value store. Before a component is rendered you must set the initial state.

Within SearchPage.js, add the following code to the SearchPage class, just before render():

    constructor(props) {
      super(props);
      this.state = {
        searchString: 'london'
      };
    }

Your component now has a state variable, with searchString set to an initial value of london.

Time to make use of this component state. Within render, change the TextInput element to the following:

    <TextInput
      style={styles.searchInput}
      value={this.state.searchString}
      placeholder='Search via name or postcode'/>

This sets the TextInput value property — that is, the text displayed to the user — to the current value of the searchString state variable. This takes care of setting the initial state, but what happens when the user edits this text?

The first step is to create a method that acts as an event handler. Within the SearchPage class add the following method:

    onSearchTextChanged(event) {
      console.log('onSearchTextChanged');
      this.setState({ searchString: event.nativeEvent.text });
      console.log(this.state.searchString);
    }

This takes the value from the event’s text property and uses it to update the component’s state. It also adds some logging code that will make sense shortly.

To wire up this method so it gets called when the text changes, return to the TextInput field within the render method and add an onChange property so the tag looks like the following:

    <TextInput
      style={styles.searchInput}
      value={this.state.searchString}
      onChange={this.onSearchTextChanged.bind(this)}
      placeholder='Search via name or postcode'/>

Whenever the user changes the text, you invoke the function supplied to onChange; in this case, it’s onSearchTextChanged.

> Note: You might be wondering what the bind(this) statement is for. JavaScript treats the this keyword a little differently than most other languages; its counterpart in Swift is self. The use of bind in this context ensures that inside the onSearchTextChanged method, this is a reference to the component instance. For more information, see the [MDN page on this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this).

There’s one final step before you refresh your app again: add the following logging statement to the top of render(), just before return:

    console.log('SearchPage.render');

You are about to learn something quite intriguing from these log statements! :]

Return to your simulator, then press Cmd+R. You should now see that the text input has an initial value of ‘london’ and that editing the text causes some statements to be logged to the Xcode console:

react-renderconsole

Looking at the screenshot above, the order of the logging statement seems a little odd:

This is the initial call to render() to set up the view.
You invoke onSearchTextChanged() when the text changes.
You then update the component state to reflect the new input text, which triggers another render.
onSearchTextChanged() then wraps things up by logging the new search string.
Whenever the app updates the state of any React component, this triggers an entire UI re-rendering that in turn calls render of all of your components. This is a great idea, as it entirely de-couples the rendering logic from the state changes that affect the UI.

With most other UI frameworks, it is either your responsibility to manually update the UI based on state changes, or use of some kind of binding framework which creates an implicit link between the application state and its UI representation; see, for example, my article on implementing the [MVVM pattern with ReactiveCocoa](http://www.raywenderlich.com/74106/mvvm-tutorial-with-reactivecocoa-part-1).

With React, you no longer have to worry about which parts of the UI might be affected by a state change; your entire UI is simply expressed as a function of your application state.

At this point you’ve probably spotted a fundamental flaw in this concept. Yes, that’s right — performance!

Surely you can’t just throw away your entire UI and re-build it every time something changes? This is where React gets really smart. Each time the UI renders itself, it takes the view tree returned by your render methods, and reconciles — or diffs — it with the current UIKit view. The output of this reconciliation process is a simple list of updates that React needs to apply to the current view. That means only the things that have actually changed will re-render.

It’s amazing to see the novel concepts that make ReactJS so unique — the virtual-DOM (Document Object Model, the visual-tree of a web document) and reconciliation — applied to an iOS app.

You can wrap your head around all that later; you still have some work to do in the app. Remove the logging code you just added above, since it’s no longer necessary and just adds cruft to the code.

## Initiating a Search 【六妹】

## 初始化一个搜索

In order to implement the search functionality you need handle the ‘Go’ button press, create a suitable API request, and provide a visual indication to the user that a query is in progress.

Within SearchPage.js, update the initial state within the constructor:

    this.state = {
      searchString: 'london',
      isLoading: false
    };

The new isLoading property will keep track of whether a query is in progress.

Add the following logic to the start of render:

    var spinner = this.state.isLoading ?
      ( <ActivityIndicatorIOS
          hidden='true'
          size='large'/> ) :
      ( <View/>);

This is a ternary if statement that either adds an activity indicator or an empty view, depending on the component’s isLoading state. Because the entire component is rendered each time, you are free to mix JSX and JavaScript logic.

Within the JSX that defines the search UI in return, add the following line below the Image:

    {spinner}

Now, add the following property within the TouchableHighlight tag that renders the ‘Go’ text:

    onPress={this.onSearchPressed.bind(this)}

Next, add the following methods to the SearchPage class:

    _executeQuery(query) {
      console.log(query);
      this.setState({ isLoading: true });
    }
     
    onSearchPressed() {
      var query = urlForQueryAndPage('place_name', this.state.searchString, 1);
      this._executeQuery(query);
    }

_executeQuery() will eventually run the query, but for now it simply logs a message to the console and sets isLoading appropriately so the UI can show the new state.

> Note: JavaScript classes do not have access modifiers, so they have no concept of ‘private’. As a result you often see developers prefixing methods with an underscore to indicate that they should be considered private.

You’ll invoke onSearchPressed() and initiate the query when the ‘Go’ button is pressed.

Finally, add the following utility function just above the SearchPage class declaration:

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

Head back to the simulator, press Cmd+R to reload the application and tap the ‘Go’ button. You’ll see the activity indicator spin; take a look at the Xcode console to see what it’s telling you:

![SearchAcitivityIndicator](http://cdn5.raywenderlich.com/wp-content/uploads/2015/03/SearchAcitivityIndicator-700x169.png)

The activity indicator renders and the URL for the required query appears in the log. Copy and paste that URL into your browser to see the result. You’ll see a massive JSON object. Don’t worry — you don’t need to understand that! You’ll add code to parse that now.

> Note: This app makes use of the [Nestoria API for searching property listings](http://www.nestoria.co.uk/help/api). The JSON response coming back from the API is pretty straightforward, but you can have a look at the documentation for all the details on the expected request URL and response formats.
The next step is to make the request from within your application.

## Performing an API Request 【寸志】

## 执行 API 请求

Still within SearchPage.js, update the initial state in the class constructor to add a message variable:

    this.state = {
      searchString: 'london',
      isLoading: false,
      message: ''
    };

Within render, add the following to the bottom of your UI:

    <Text style={styles.description}>{this.state.message}</Text>

You’ll use this to display a range of messages to the user.

Within the SearchPage class, add the following code to the end of _executeQuery():

    fetch(query)
      .then(response => response.json())
      .then(json => this._handleResponse(json.response))
      .catch(error => 
         this.setState({
          isLoading: false,
          message: 'Something bad happened ' + error
       }));

This makes use of the fetch function, which is [part of the Web API](https://fetch.spec.whatwg.org/), and provides a vastly improved API versus XMLHttpRequest. The asynchronous response is returned [as a promise](http://www.html5rocks.com/en/tutorials/es6/promises/), with the success path parsing the JSON and supplying it to a method which you are going to add next.

The final step is to add the following function to SearchPage:

    _handleResponse(response) {
      this.setState({ isLoading: false , message: '' });
      if (response.application_response_code.substr(0, 1) === '1') {
        console.log('Properties found: ' + response.listings.length);
      } else {
        this.setState({ message: 'Location not recognized; please try again.'});
      }
    }

This clears isLoading and logs the number of properties found if the query was successful.

> Note: Nestoria has [a number of non-1** response codes](http://www.nestoria.co.uk/help/api-return-codes) that are potentially useful. For example, 202 and 200 return a list of best-guess locations. When you’ve finished building your app, why not try handling these and present a list of options to the user?

Save your work, then in the simulator press Cmd+R and try searching for ‘london’; you should see a log message saying that 20 properties were found. Next try a non-existent location, such as ‘narnia’ (*sniff*), and you’ll be greeted by the following message:

![react-narnia](http://cdn4.raywenderlich.com/wp-content/uploads/2015/03/react-narnia-281x500.png)

It’s time to see what those 20 properties in real places such as London look like!

## Displaying the Results 【洪春】

## 结果显示

Create a new file SearchResults.js, and add the following:

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

Next add the component itself:

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

When constructing the data source, you provide a function that compares the identity of a pair of rows. The ListView uses this during the reconciliation process, in order to determine the changes in the list data. In this instance, the properties returned by the Nestoria API have a guid property, which is a suitable check for this purpose.

Now add the module export to the end of the file:

    module.exports = SearchResults;

Add the following to SearchPage.js near the top of the file, underneath the require call for React:

    var SearchResults = require('./SearchResults');

This allows us to use the newly added SearchResults class from within the SearchPage class.

Modify the current _handleResponse method by replacing the console.log statement with the following:

    this.props.navigator.push({
      title: 'Results',
      component: SearchResults,
      passProps: {listings: response.listings}
    });

The above code navigates to your newly added SearchResults component and passes in the listings from the API request. Using the push method ensures the search results are pushed onto the navigation stack, which means you’ll get a ‘Back’ button to return to the root.

Head back to the simulator, press Cmd+R and try a quick search. You’ll be greeted by a list of properties:

![react-searchresults1](http://cdn3.raywenderlich.com/wp-content/uploads/2015/03/react-searchresults1-281x500.png)

It’s great to see the property listings, but that list is a little drab. Time to liven things up a bit.

## A Touch of Style 【六妹】

## 可点击样式

This React Native code should be starting to look familiar by now, so this tutorial is going to pick up the pace.

Add the following style definition just after the destructuring assignment in SearchResults.js:

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

Next replace renderRow() with the following:

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

Also, note the use of an arrow function in the onPress property of the TouchableHighlight component; this is used to capture the guid for the row.

The final step is to add this method to the class to handle the press:

    rowPressed(propertyGuid) {
      var property = this.props.listings.filter(prop => prop.guid === propertyGuid)[0];
    }

This method locates the property that was tapped by the user. It doesn’t do anything with it yet, but you’ll fix that shortly. But right now, it’s time to admire your handiwork.

Head back to the simulator, press Cmd+R and check out your results:

![react-searchresults2](http://cdn2.raywenderlich.com/wp-content/uploads/2015/03/react-searchresults2-281x500.png)

That looks a lot better — although it’s a wonder anyone can afford to live in London!

Time to add the final view to the application.

## Property Details View 【寸志】

## 详情视图

Add a new file PropertyView.js to the project, then add the following to the top of the file:

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

Next add the following styles:

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

The rest of render is quite straightforward; it’s simply a function of the immutable state of this component.

Finally add the following export to the end of the file:

    module.exports = PropertyView;

Head back to SearchResults.js and add the require statement to the top of the file, just underneath the React require line:

    var PropertyView = require('./PropertyView');
    Next update rowPressed() to navigate to your newly added PropertyView:
    
    rowPressed(propertyGuid) {
      var property = this.props.listings.filter(prop => prop.guid === propertyGuid)[0];
     
      this.props.navigator.push({
        title: "Property",
        component: PropertyView,
        passProps: {property: property}
      });
    }

You know the drill: head back to the Simulator, press Cmd+R, and go all the way to the property details by running a search and tapping on a row:

![react-property](http://cdn3.raywenderlich.com/wp-content/uploads/2015/03/react-property-281x500.png)

Affordable living at it’s finest — that’s a fancy looking pad!

Your app is almost complete; the final step is to allow users to search for nearby properties.

## Geolocation Search 【洪春】

## 地理位置搜索

Within Xcode, open Info.plist and add a new key, by right clicking inside the editor and selecting Add Row. Use NSLocationWhenInUseUsageDescription as the key name and use the following value:

    PropertyFinder would like to use your location to find nearby properties

Here’s how your plist file will look once you’ve added the new key:

![Info.plist after adding key](http://cdn3.raywenderlich.com/wp-content/uploads/2015/03/Screen-Shot-2015-03-20-at-21.49.06-480x162.png)

This key details the prompt that you’ll present to the to the user to request access to their current location.

Open SearchPage.js, locate the TouchableHighlight that renders the ‘Location’ button and add the following property value:

    onPress={this.onLocationPressed.bind(this)}

When you tap the button, you ‘ll invoke onLocationPressed — you’re going to add that next.

Add the following within the body of the SearchPage class:

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

If the current position is successfully obtained, you invoke the first arrow function; this sends a query to Nestoria. If something goes wrong, you’ll display a basic error message instead.

Since you’ve made a change to the plist, you’ll need to relaunch the app to see your changes. No Cmd+R this time — sorry. Stop the app in Xcode, and build and run your project.

Before you use the location-based search, you need to specify a location that is covered by the Nestoria database. From the simulator menu, select Debug\Location\Custom Location … and enter a latitude of 55.02 and a longitude of -1.42, the coordinates of a rather nice seaside town in the North of England that I like to call home!

![WhitleyBaySearch](http://cdn1.raywenderlich.com/wp-content/uploads/2015/03/WhitleyBaySearch-647x500.png)

> Note from Ray: Location searching worked for some of us, but not for others (reporting an access denied error even though we gave access) – we’re not sure why at the moment, perhaps an issue with React Native? If anyone has the same issue and figures it out, please let us know.

It’s not quite as swank as London — but it’s a lot more affordable! :]

## Where To Go From Here? 【六妹】

## 下一步行动？

Congratulations on completing your first React Native application! You can [download the final project for this tutorial](http://cdn1.raywenderlich.com/wp-content/uploads/2015/03/PropertyFinder-Final1.zip) to try out on your own.

If you come from the web world, you can see how easy it is to define your interface and navigation with JavaScript and React to get a fully native UI from your code. If you work mainly on native apps, I hope you’ve gained a feel for the benefits of React Native: fast app iteration, modern JavaScript and clear style rules with CSS.

Perhaps you might write your next app using this framework? Or then again, perhaps you will stick with Swift or Objective-C? Whichever path you take, I hope you have learned something new and interesting in this article, and can carry some of the principles forward into your next project.

If you have any questions or comments on this tutorial, feel free to join the discussion in the forums below!

原文：http://www.raywenderlich.com/99473/introducing-react-native-building-apps-javascript