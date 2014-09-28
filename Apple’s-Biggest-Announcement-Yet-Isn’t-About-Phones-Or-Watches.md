# Apple’s Biggest Announcement Yet Isn’t About Phones Or Watches
# 直到现在苹果最大的发布不是手机或者手表

In case you happen to have missed it, an Apple announcement has claimed the [number one spot on Hacker News](https://news.ycombinator.com/item?id=8328760) this morning. As tired as we all are of Apple and the endless speculating and PR barrage, this one is different from most all of the others.

或许你错过了，就在今天早晨的苹果发布会上

It’s different because it’s not really an announcement at all. It’s just the documentation for an API that allows developers to use JavaScript for OS X Automation, and it has serious implications for developers everywhere.
![pseudo elements](http://developer.telerik.com/wp-content/uploads/2014/09/542027463_8ede265d3f_o.png)
不同的是这不仅仅是一场发布会。而是一个API文档，允许开发者使用在OS X Automation上使用JavaScript，并且为开发者提供了完备的接口。

![osx interface](http://developer.telerik.com/wp-content/uploads/2014/09/542027463_8ede265d3f_o.png)
## What Is Task Automation?
## 任务自动化是什么？
For those that aren’t familiar, the OS X operating system has long supported the ability to script and automate tasks using something called AppleScript. Why would you want to do this? Because it allows the user to automate repetitive tasks that otherwise require user interaction.

对于那些不熟悉的，OS X操作系统很早就支持

As a simple example, assume that you frequently email files to the same group of people. You may click to compose a new email, click the “attach” icon, browse for the document, select it and then click send. You could write an AppleScript to automate that task so all you have to do is right-click on the file you want to send, and select “send to group”. Virtually any interaction you can have with OS X can be automated, which means you can get really advanced. [One user](http://computers.tutsplus.com/tutorials/the-ultimate-beginners-guide-to-applescript--mac-3436) has a script which reads his calendar for hours he’s billed, creates an invoice in Excel and sends that invoice to the client.

Historically, these tasks were composed with AppleScript (or Automator), which is a proprietary scripting language from Apple that aimed to simplify the scripting process through the use of natural language.
For instance, if you wanted to show a simple “Hello World” dialog, you could write the following AppleScript

tell application "Finder"

    display dialog "Hello World"

end tell

With OS X Yosemite, Apple is allowing developers to do the same thing using JavaScript.

## So What?

On the surface, this seems like a small and insignificant addition. Who cares about task automation? [One commenter noted](https://news.ycombinator.com/item?id=8329245) that being able to do the same things with JavaScript that you can do with AppleScript is a “matter of semantics”. You can see that Hacker News thread devolve into a discussion around whether or not JavaScript is better than AppleScript. The downsides of natural language interfaces not withstanding, we’re all missing a much bigger and more important point here.

JavaScript has begun to make its way onto more and more devices as part of the core operating system. Node brings JavaScript right to the server, and huge companies like Walmart are [building their entire infrastructure on it](http://nodejs.org/video/). Microsoft offers it as a first class citizen for writing native Windows 8 apps. Google Chrome offers Chrome Packaged Apps, which allows access to a limited set of native APIs from JavaScript and PhoneGap pioneered the idea of proxying JavaScript commands to exposed native APIs.

The fact that Apple added support for JavaScript to OS X Yosemite Task Automation is so significant because it means that JavaScript is now a first-class citizen.

## JavaScript As A First-Class Citizen

Not only does Apple provide an API for interacting with the operating system and install apps, but they also provide an Objective-C bridge to work directly with native libraries such as Cocoa. This is HUGE.  Just look at what Apple themselves have to say about it…

JavaScript for Automation has a built-in Objective-C bridge that offers powerful utility such as accessing the file system and building Cocoa applications.
— [Mac Developer Library – Pre-Release](https://developer.apple.com/library/prerelease/mac/releasenotes/InterapplicationCommunication/RN-JavaScriptForAutomation/index.html)


They just said you could build Cocoa applications with JavaScript. That just happened.
 
What’s even more incredible, is that this is NOT the first time Apple has given JavaScript a promotion in its ecosystem. With iOS 7, they somewhat quietly [announced the JavaScript Bridge](https://developer.apple.com/library/mac/documentation/AppleApplications/Conceptual/SafariJSProgTopics/Tasks/ObjCFromJavaScript.html). This bridge allows developers to call Objective-C methods from JavaScript. While Apple positions this as a way for developers to call plug-ins from a webview, it’s far more than that.

Items like the JavaScript Bridge are what enabled projects like [NativeScript](http://developer.telerik.com/featured/nativescript-a-technical-overview/). NativeScript is a new framework from Telerik which allows developers to write native applications in JavaScript. Not to be confused with writing a WebView which runs in a native shell, NativeScript allows JavaScript to compose the UI and execute Objective-C at the lowest possible level. Don’t confuse this with cross-compiling. This is JavaScript running directly on the rails.
![javascript on mobile](http://developer.telerik.com/wp-content/uploads/2014/09/nativescript_head_banner_2.png)

That’s why the fact that Apple is now offering JavaScript for task automation is so compelling. It’s not that developers have been dying to write more task automations, it’s that we have all long been searching for a universal language for building applications. The fragmentation in mobile has agitated this to nearly a tipping point. Nobody wants to install different IDEs, learn different SDKs, and maintain separate code bases. It’s simply not sustainable. Cross-compilation is appealing for this reason, but results in enormously bloated apps and a level of complexity between the developer and the operating system that they cannot control. If there is one thing developers hate, it’s a black box.

What’s strange is Apple doesn’t necessarily need to attract a new kind of developer. It’s arguable that it already has platform dominance and developer mindshare when it comes to the mobile space. Adding JavaScript access to native methods would have been akin to Microsoft doing the same thing with .NET when it so dominated the enterprise. What’s in it for Apple?

In any event, as a JavaScript developer I am very excited this morning. I have zero plans to go automate anything on my Mac now, or in the near future. In fact, I have never used task automation. What really amps me up is the fact that Apple has begun to sneak JavaScript in as a first-class citizen, and that opens all kinds of doors for developers everywhere.

We’ve just moved one step closer to that elusive unicorn – write once, run anywhere.

[Automator image by Miguel C from Flickr CC](https://www.flickr.com/photos/shht/)

原文：[Apple’s Biggest Announcement Yet Isn’t About Phones Or Watches](http://developer.telerik.com/featured/apples-biggest-announcement-yet-isnt-phones-watches/)

