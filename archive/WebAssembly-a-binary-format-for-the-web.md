# WebAssembly: a binary format for the web

# WebAssembly：Web 的二进制格式

## Updates:

## 更新

WebAssembly (short: wasm) is a new binary format for the web, created by Google, Microsoft, Mozilla and others. It will be used for performance critical code and to compile languages other than JavaScript (especially C/C++) to the web platform. It can be seen as a next step for asm.js [3].

WebAssembly（简写：wasm）


## asm.js

## asm.js

Most JavaScript engines have the following compilation pipeline:

> JavaScript source → bytecode → machine code

The idea of asm.js is to code JavaScript in such a way that engines produce machine code that is as efficient as possible. That is, you kind of try to bypass the first compilation step. The results are impressive: if one compiles C++ to asm.js one can reach 70% of native speed in web browsers.

## WebAssembly: the next step for asm.js

## WebAssembly： 更进一步的 asm.js

Initially, WebAssembly is (roughly) a binary format for delivering asm.js code. The two main advantages of such a format are:

	•	Faster loading. Especially with large code bases and mobile devices, parsing becomes a bottleneck: “On mobile, large compiled codes can easily take 20–40s just to parse” (WebAssembly FAQ). First experiments show that WebAssembly can be loaded more than 20 times faster, because the work for parsing is minimal.
	•	Evolving WebAssembly is simpler: The way that asm.js is currently written is constrained by having to run fast as normal JavaScript (in legacy engines) and by having to support ahead of time compilation within JavaScript syntax. 
Legacy engines will be supported via a JavaScript library that translates WebAssembly (binary) to asm.js (source code). First indications are that that is reasonably fast.

## WebAssembly binaries are trees

## WebAssembly 二进制码是树形的

WebAssembly binaries will encode abstract syntax trees. That is, they are not a linear format like stack- or register-based bytecode. Not surprisingly, the format looks much like asm.js: roughly, a statically typed version of JavaScript without objects.

There will be a text format for WebAssembly. It will mirror the semantics of the binaries and contain a tree of statements and expressions.

## WebAssembly does not replace JavaScript

## WebAssembly 不是 JavaScript 的替代品

The previous diagram (which is a much simplified depiction of reality) should make it obvious that WebAssembly is not a replacement for JavaScript, it is a new feature of JavaScript engines that builds on their infrastructures. That means that it will fit well into the web platform:

	•	It will have the same evolution strategy as JavaScript: everything is always backward-compatible, there is no explicit versioning, code uses feature testing to determine how it should run.
	•	It will execute in the same semantic universe as JavaScript and allow synchronous calls to and from JavaScript (including browser APIs).
	•	Security will be handled like in JavaScript, via same-origin and permissions security policies.

The nice thing is that WebAssembly removes the burden from JavaScript to be a good compilation target for other languages. For example, the following two methods were only added to ES6 to support asm.js well:

	•	Math.fround(x) rounds x to a 32 bit float.
	•	Math.imul(x,y) multiplies the two integers x and y.

## How will JavaScript and WebAssembly coexist?

## JavaScript 和 WebAssembly 如何共存？

	•	Performance critical functionality (games, video and image decoders, etc.) will be implemented via WebAssembly, either by hand-coding it or by yet-to-be-invented languages that are slightly higher-level.
	•	External code bases, especially those in C/C++, will be easy to port to the web platform, via WebAssembly.
	•	Other than that, JavaScript will continue to evolve and will probably remain the most popular way of implementing web apps. 
## The future

## 将来

The initial focus is for WebAssembly to be a compilation target for C/C++. Longer term, more features supporting other languages will be added, including the ability to create garbage-collected data (currently, asm.js creates and manages its own heap).
You’ll eventually be able to debug a WebAssembly binary via the source code that was compiled to it. That mechanism will be an evolution of source maps [4] that provide similar services for JavaScript.

## Frequently asked questions

## 常见问题

### What is different this time?

### 为何不同？

Why should WebAssembly succeed where previous attempts (such as Adobe Flash and Google Portable Native Client) have failed? There are three reasons:

	•	First, this is a collaborative effort, no single company goes it alone. At the moment, the following projects are involved: Firefox, Chromium, Edge and WebKit.
	•	Second, the interoperability with the web platform and JavaScript is excellent. Using WebAssembly code from JavaScript will be as simple as importing a module.
	•	Third, this is not about replacing JavaScript engines, it is more about adding a new feature to them. That greatly reduces the amount of work to implement WebAssembly and should help with getting the support of the web development community. 
### Does the web finally have a universal bytecode?

### Web 最终有了统一的二进制码了？

WebAssembly is not bytecode: Bytecode is linear and (usually) stack-, register- or SSA-based, WebAssembly is a binary format for an abstract syntax tree (AST). Compared to bytecode, this has the following advantages:

	•	WebAssembly is relatively easy to add to all current JavaScript engines, because it is high-level and similar to parts of JavaScript. That is, it builds on the infrastructures of engines, instead of replacing them. Engines will continue to have different compilation strategies and/or bytecode, which is good for the web ecosystem, because the differences have fostered experimentation and innovation.
	•	WebAssembly is not versioned. It uses the same evolution strategy as JavaScript (feature detection and polyfills). 
On the other hand, WebAssembly is like bytecode in two ways:

	•	WebAssembly will eventually lead to a “universal” virtual machine (VM). It will probably gain features that JavaScript/asm.js will never have and therefore better accommodate languages that currently don’t compile well to JavaScript. Note, though, that asm.js was the beginning of that universal VM – you don’t need a binary format for that.
	•	WebAssembly eliminates the parser as a bottleneck. 
### Isn’t WebAssembly like Flash?

### WebAssembly 不是 Flash 吧？

Everything mentioned in the previous question applies here, too: Flash is based on bytecode, but WebAssembly is not bytecode and does not have some of its disadvantages. Additionally, WebAssembly is much better integrated into the web platform, its code lives in the universe of JavaScript. So far, the power consumption story of the web platform also seems to be better than Flash’s, but there is still a lot of room for improvement.

### Will WebAssembly make JavaScript faster?

### WebAssembly 能提高 JavaScript 的速度么？

Short answer: yes (load time) and no (execution time).

70% of the speed native C/C++ means that WebAssembly is fast where C/C++ is fast (static code) and slow where C/C++ is slow (e.g. dynamic OOP features).
It does not currently make sense to compile JavaScript to WebAssembly, because it lacks JavaScript-specific features such as objects and arrays (for C++, one manually manages a heap in a typed array). Once WebAssembly gains those features, JavaScript can be compiled to it, but it will use the same techniques as current engines. Therefore, only the load time of applications will improve, because parsing is much faster.

The reduction of the size of executables will be less dramatic. One can already save a lot of space via minification and compression. First experiments (see below) resulted in gzipped asm.js being 1.4 times bigger than gzipped WebAssembly.

### How do I create WebAssembly code?

### 如何创建 WebAssembly 吗？

The main three options are:

	•	Write the code manually and use the textual representation.
	•	Produce binary output programmatically.
	•	Use a compiler to compile an LLVM-based language (initially mainly C/C++) to WebAssembly. The official FAQ “What compilers can I use to build WebAssembly programs?” has more information.

## First experiences in practice

## 第一次实战

For the Unity Game Engine, first tests were made with WebAssembly. Quoting “WebGL: WebAssembly and Feature Roadmap” by Jonas Echterhoff for the Unity Blog:

Experimenting with a prototype WebAssembly format on a build of our AngryBots demo, we saw the size of the generated JavaScript code go from 19.0 MB of asm.js code (gzip-compressed to 4.1 MB) down to 6.3 MB of WebAssembly code (gzip-compressed to 3.0 MB). This means that the amount of data the browser needs to process gets reduced by 3.0x, and the compressed download size gets reduced by 1.4x. Actual results may change based on the project used, but we expect to see very relevant improvements to anyone caring about WebGL deployment in Unity.

## Further reading

## 更多参考

Especially informative is Eric Elliott’s interview with Brendan Eich: “Why we Need WebAssembly”.

原文：http://www.2ality.com/2015/06/web-assembly.html
