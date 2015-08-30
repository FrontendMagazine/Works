# Architecture of ECMAScript 6 Modules

## Terminology

## 技术背景

For those unfamiliar with the current ES6 module proposal, here is some terminology you should understand:

如果你还未熟悉当前的 ES6 模块规范，需要明白：

1	module: a unit of source code with optional imports and exports.
2	export: a module can export a value with a name.
3	imports: a module can import a value exported by another module by its name.
4	module instance object: an instance of the Module constructor that represents a module. Its property names and values come from the module's exports.
5	Loader: an object that defines how modules are fetched, translated, and compiled into a module instance object. Each JavaScript environment (the browser, node.js) defines a default Loader that defines the semantics for that environment.

1. module：一个代码单元，有若干的 import 和 export
2. export：一个 module 可以通过具名的方式输出一个值
3. imports：一个 module 可以通过名字导入一个由其他模块输入的值
4. module 实例对象：一个 Module 构造器的实例化对象，作为模块的代表。这个对象的属性名来自于 module 的 exports
5. Loader：也是一个对象，定义该如何获取 module，将其转换、编译成一个 module 实例对象。每个 JavaScript 环境（浏览器、Node.js）都设计了一个默认的 Loader，为各自的环境定义 module 的语义。

## Imports and Exports

## Import 和 Export

Let's start with the basic API of ES6 modules:

一开始先看看 ES6 的 moudle API：

    // libs/string.js

    var underscoreRegex1 = /([a-z\d])([A-Z]+)/g,
        underscoreRegex2 = /\-|\s+/g;

    export function underscore(string) {
      return string.replace(underscoreRegex1, '$1_$2')
                   .replace(underscoreRegex2, '_')
                   .toLowerCase();
    }

    export function capitalize(string) {
      return string.charAt(0).toUpperCase() + string.substr(1);
    }

    // app.js

    import { capitalize } from "libs/string";

    var app = {
      name: capitalize(document.title)
    };

    export app;

This illustrates the basic syntax of ES6 modules. A module can export named values, and other modules can import those values.

上面展示了 ES6 module 最基础的语法。一个模块可以输出具名的值，其他模块则可以导入这些值。

## Avoiding Scope Pollution

## 避免作用域污染

When working with a module with a large number of exports, you may want to avoid adding each of them as top-level names of another module that wants to import it.

如果一个模块有很多的 export，你应该会希不要在 import 它的模块里产生过多的顶级作用域变量。

For example, consider an API like Node.js fs module. This module has a large number of exports, like rename, chown, chmod, stat and others. With the ES6 module API, it is possible to bring in the module as a single top-level name that contains all of the module's exports.

例如，比如像 Node.js `fs` 这样的模块，有很多的 export，比如 `rename`、`chown`、`chmod` 和 `stat` 等等。通过 ES6 module 的 API，也可以把这个模块所有的 export 包含到一个顶级的具名变量中。

    import "fs" as fs;

    fs.rename(oldPath, newPath, function(err) {
      // continue
    });

## Concatenation

## 打包

In the example above, the modules were loaded based on their location on the file system. This is how the default Loader for the browser will work.

在上面的例子中，可以通过模块在文件系统中的位置将其加载到浏览器中，这也是浏览器端的 Loader 默认的工作方式。

For production applications, you will want to concatenate the files on the file system into a single file. ES6 modules handle this case by providing a literal way to define a module:

在生产环境中，我需要把多个模块文件打包成一个。ES6 考虑到了这种情况，提供了字面量的方式来定义一个模块。

    module "libs/string" {
      var underscoreRegex1 = /([a-z\d])([A-Z]+)/g,
          underscoreRegex2 = /\-|\s+/g;

      export function underscore(string) {
        return string.replace(underscoreRegex1, '$1_$2')
                     .replace(underscoreRegex2, '_')
                     .toLowerCase();
      }

      export function capitalize(string) {
        return string.charAt(0).toUpperCase() + string.substr(1);
      }
    }

    module "app" {
      import { capitalize } from "libs/string";

      var app = {
        name: capitalize(document.title)
      };

      export app;
    }

Modules defined using this syntax will be available to other modules, and will not needed to be fetched through the Loader.

使用这种语法定义的模块可以在别的模块中使用，不再需要通过 Loader 从服务端加载。

## Modules in Non-Default Locations

## 位于其他地方的模块

In web applications, while many modules may be concatenated into a single file for production use, some modules, like jQuery, may be loaded off of a CDN.

在网页应用中，除了大多数被打包成一个文件的模块以外，其他模块，比如jQuery，通常是用 CDN 上加载的。

It is possible to override the default Loader hooks to specify where to load a module from, but ES6 modules provide a simple API for mapping modules to their physical location.

其实可以重载自带 Loader 的钩子，指定从什么地方下载模块。但 ES6 模块提供了一个简单的 API，将模块映射到物理位置。

    System.ondemand({
      "https://ajax.googleapis.com/jquery/2.4/jquery.module.js": "jquery",
      "backbone.js": ["backbone/events", "backbone/model"]
    });

The first line in the example specifies that the jquery module can be found at https://ajax.googleapis.com/jquery/2.4/jquery.module.js.

第一行指定到 `https://ajax.googleapis.com/jquery/2.4/jquery.module.js` 获取 `jquery` 模块。

The second line specifies that backbone/events and backbone/model can both be found at backbone.js.

第二行指定 `backbone/events` 和 `backone/model` 可以从 `backbone.js` 这个文件中获取。

You can call System.ondemand as many times as you want, so libraries can provide a snippet of code for people to use in order to import their libraries.

`System.ondemand` 可以多次调用。因此类库可以给使用者提供一段代码用于加载类库。

## The Compilation Pipeline

## 编译流程

The next several sections deal with various use-cases involving the compilation pipeline.

接下来的几个小节，将介绍如何通过编译流程解决多个实用场景。

Here is a high-level overview of the process.

下面时编译流程的概览图：

![The Compilation Pipeline](http://cl.ly/image/0d2P3j2t2b0s/Pasted_Image_3_10_13_5_16_PM.png)

The dotted line between fetch and translate reflects the fact that process of retrieving the source is asynchronous.

在 `fetch` 和 `translate` 之间的虚线箭头，表示通过异步的过程获取源码。

## Stricter Mode (Linting)

## 更严格的模式（代码检测）

Linting tools are a crucial part of a JavaScript developer's workflow, but they are currently used primarily via a compilation toolchain that presents errors in the terminal.

Lint 工具是 JavaScript 开发者工作流中至关重要的一环，但目前他们主要的是用方式就是通过编译工具来测试将错误输出到终端中。

Using the Module Loader's translate hook, it is possible to add additional static checks that are presented to the user as SyntaxErrors.

使用 Module Loader 的 translate hook，可以添加静态检查，给到用户 `SyntaxErrors` 。

    import { JSHINT } from "jshint";
    import { options } from "app/jshintrc"

    System.translate = function(source, options) {
      var errors = JSHINT(source, options), messages = [options.actualAddress];

      if (errors) {
        errors.forEach(function(error) {
          var message = '';
          message += error.line + ':' + error.character + ', ';
          message += error.reason;
          messages.push(message);
        });

        throw new SyntaxError(messages.join("\n"));
      }

      return source;
    };

If the linter returns errors, the translate hook raises a SyntaxError and the Loader pipeline will stop, throwing the exception as if it was a true SyntaxError.

如果 linter 返回了错误，tranlate hook 会产生一个 SyntaxError，Loader 停止编译过程，抛出一个异常，就好像是产生了一个真的 SyntaxError 一样。

![SyntaxError](http://cl.ly/image/3l2W3L331M1n/Pasted_Image_3_10_13_5_24_PM.png)

## Importing Compile-to-JavaScript Modules (CoffeeScript)

## Import 可编译为 JavaScript 的模块（例如 CoffeeScript）

Increasingly, modules are written using languages that compile to JavaScript.
The translate hook provides a way to translate source code to JavaScript before it is loaded as a module.

现在越来越多的模块使用编译为 JavaScript 的语言编写。利用 translate hook 可以在模块被加载之前将源代码编译为 JavaScript。

    System.translate = function(source, options) {
      if (!options.path.match(/\.coffee$/)) { return; }

      return CoffeeScript.translate(source);
    };

In this example, any modules ending in .coffee will be translated from CoffeeScript to JavaScript, and the rest of the pipeline will just see the compiled JavaScript.

上面的例子，所有以 `.coffee`  结尾的模块会被当作 CoffeeScript 编译为 JavaScript，在编译流程的其它环节看到的是编译后的 JavaScript。

![Importing Compile-to-JavaScript Modules](http://cl.ly/image/3l42190J2o2O/Pasted_Image_3_10_13_5_27_PM.png)

## Verification and Translation

## 验证和编译

Some other compilers, like TypeScript and restrict mode perform both compile-time verification and source translation.

还有其它一些编译器，比如 TypeScript 和 Restrict Mode 即包含编译时验证和源码转译。

The above techniques could be combined to produce seamless in-browser support for such libraries.

结合上面的技巧可以在浏览器中无缝地支持这些类库。

## Using Existing Libraries as Modules

## 使用已存在的类库作为模块

The existing jQuery library is distributed as a library that "exports" the jQuery name onto the global object.

现存的一些类库，比如 jQuery，将 `jQuery` 变量输出到全局对象上。

It should be possible to import existing libraries without having to modify the original source, like this:

对于这类类库，应该可以在不修改类库的源码的情况下将它们 import 进来，像下面这样：

    import { jQuery } from "jquery";

    jQuery(function($) {
      $(".ui-button").button();
    });

The final hook in the process, link can be used to manually process a source file into a Module object.

编译流程中的最后一步，`link` 可以手动地把源码文件处理成一个 Module 对象。

In this case, we could configure the Loader to extract all properties written to window.

在这样的场景中，我门可以配置 Loader，把本来要写入到 window 上到属性都提取出来。

    function extractExports(loader, original) {
      source =
        `var exports = {};
        (function(window) {
          ${source};
        })(exports);
        exports;`

      return loader.eval(source);
    }

    System.link = function(source, options) {
      if (options.metadata.type === 'legacy') {
        return new Module(extractExports(this, source));
      }

      // returning undefined will result in the normal
      // parsing and registration behavior
    }

In order to make it easy for the link hook to decide whether it should use custom linking logic, the resolve hook can provide metadata for the module that will be passed to the following hooks.

为了让 `link` 决定是否适用自定义的 link 逻辑，`resolve` hook 应当为后面的 hook 提供模块的元信息（metadata）。

In this case, you can keep a list of which modules are "legacy" and populate the metadata with that information in resolve:

你可以在 `reslove` 的时候，通过一个 `legacy` 模块的列表来获取这样的 `metadata`：

    var legacy = ["jquery", "backbone", "underscore"];

    System.resolve = function(path, options) {
      if (legacy.indexOf(path) > -1) {
        return { name: path, metadata: { type: 'legacy' } };
      } else {
        return { name: path, metadata: { type: 'es6' } };
      }
    }

![resolve hook](http://cl.ly/image/111R3D2A3R0S/Pasted_Image_3_10_13_5_33_PM.png)

## Importing AMD Modules from ES6 Modules

## 在 ES6 模块中 import AMD 模块

Similarly, you may want to import an AMD module's exports in an ES6 module.
Consider a simple AMD module for the string formatting example above:

同样，你可能需要在 ES6 模块中 import AMD 模块。例如下面这个简单的 AMD 模块：

    // libs/string.js

    define(['exports'], function(exports) {
      var underscoreRegex1 = /([a-z\d])([A-Z]+)/g,
          underscoreRegex2 = /\-|\s+/g;

      exports.underscore = function(string) {
        return string.replace(underscoreRegex1, '$1_$2')
                     .replace(underscoreRegex2, '_')
                     .toLowerCase();
      }

      exports.capitalize = function(string) {
        return string.charAt(0).toUpperCase() + string.substr(1);
      }
    });

    To assimilate this module, you could use a similar technique to the one we used above for jQuery:

为了兼容这个模块，你可以使用类似的针对 jQuery 的技巧：

    var amd = ["string-utils"];

    // Resolve 
    System.resolve = function(path, options) {
      if (amd.indexOf(path) > -1) {
        return { name: path, metadata: { type: 'amd' } };
      } else {
        return { name: path, metadata: { type: 'es6' } };
      }
    };

    function extractAMDExports(loader, source) {
      var loader = new Loader();
      loader.eval(`
        var module;
        var define = function(deps, callback) {
          module = { deps: deps, callback: callback };
        };
        ${source};
        module;
      `);

      // Assume synchronously available dependencies. See below
      // for a discussion of async dependencies.
      var exports = {};
      var deps = module.deps.map(function(name) {
        // AMD uses a special dependency named `exports` to
        // collect exports.
        if (name === 'exports') { return exports; }
        else { return loader.get(name); }
      });

      callback(deps);
      return exports;
    }

    System.link = function(source, options) {
      if (options.metadata.type === 'amd') {
        return new Module(extractAMDExports(this, source));
      }
    }

To be clear, the particular implementation here is simple, and a real approach to AMD assimilation would be more complicated. This should provide some idea of what such an approach would look like.

有一点需要说清楚，这里特定的实现很简单，但是真实的 AMD 兼容的方案要比这复杂得多。这个简单的例子只是说明类似的方案是什么样子。

![](http://cl.ly/image/2U1B1D0U1P3h/Pasted_Image_3_10_13_5_35_PM.png)

## Importing Node Modules from ES6 Modules

## 在 ES6 模块中 import Node 模块

The approach to importing node modules from ES6 modules is similar. Consider a node version of the above module:

在 ES6 模块中引入 node 模块的方案也没什么特别的，比如上面模块的 node 版本：

    var underscoreRegex1 = /([a-z\d])([A-Z]+)/g,
        underscoreRegex2 = /\-|\s+/g;

    exports.underscore = function(string) {
      return string.replace(underscoreRegex1, '$1_$2')
                   .replace(underscoreRegex2, '_')
                   .toLowerCase();
    }

    exports.capitalize = function(string) {
      return string.charAt(0).toUpperCase() + string.substr(1);
    }
    You'd override the hooks in a similar way:
    var node = ["string-utils"];

    // Resolve 
    System.resolve = function(path, options) {
      if (node.indexOf(path) > -1) {
        return { name: path, metadata: { type: 'node' } };
      } else {
        return { name: path, metadata: { type: 'es6' } };
      }
    };

    function extractNodeExports(loader, source) {
      var loader = new Loader();
      return loader.eval(`
        var exports = {};
        ${source};
        exports;
      `);
    }

    System.link = function(source, options) {
      if (options.metadata.type === 'node') {
        return new Module(extractNodeExports(this, source));
      }
    }

## Importing From Multiple Non-ES6 Modules

## import 各种非 ES6 模块

To import from all three of these external module systems together, you would write a resolve hook that would store off the type of module in the context, and then use that information to evaluate the source appropriately in the link hook.

为了兼容不同的模块系统，我们需要编写 resolove hook，在执行过程中获取模块的类型，然后在 link hook 中利用这些信息来处理模块源码。

To make this process easier, a JavaScript library like require.js, built for the ES6 loader, could provide conveniences for registering the type of external modules and assimilation code for link.

为了简化这个过程，需要一个像 require.js 这样的 JavaScript 类库，为 ES6 Loader 而生，可以提供注册其他类型模块的便利，并在 link 阶段对代码进行融合。

## Import a "Single Export" From a Non-ES6 Module

## Import “单个 export” 的非 ES6 模块

Some external module systems support modules that have a single export, rather than a number of named exports.

其他某些模块系统支持有单个 export 的模块，而不是多个具名的 export。

The techniques described above could be used to register that single export under a conventionally known name.

上面使用的技巧也可以用来把单个 export 的模块注册到已知的预先约定的名称下。

Consider the following "single export" module using node-style modules:

看看下面这个 node 模块风格的“单个 export”模块：

    // string-utils/capitalize.js

    module.exports = function(string) {
      return string.charAt(0).toUpperCase() + string.substr(1);
    }

In order to support using this module in an ES6 module, a loader can create a conventional name for the export that ES6 modules can import.

为了让这个模块可以在  ES6 模块中使用，要求 Loader 给 export 起一个约定的名字，便于 ES6 模块引入。

In this example, we will name the export exports for consistency with existing node practice. Once we have done this, ES6 modules will be able to import the module:

在本例中，我们将 `export` 命名为 `exports`，与现存的 node
实践保持一致。通过这样的处理，ES6 模块就可以引入这个模块了：

    // app.js

    import { exports: capitalize } from "string-utils/capitalize";

    console.log(capitalize("hello")) // "Hello"

Here, we are renaming the conventionally named exports to capitalize.

在上面的代码中，我们把 `exports` 改名为了 `capitalize`。

In order to achieve this, we will augment the earlier node assimilation code to handle module.exports = semantics.

为了实现上面的预定，我们需要在 link 代码的地方添加对 `module.exports = ` 的处理。

    function extractNodeExports(loader, source) {
      var loader = new Loader();
      var exports = loader.eval(`
        var module = {};
        var exports = {};
        ${source};
        { single: module.exports, named: exports };
      `);

      if (exports.single !== undefined) {
        return { exports: exports.single }
      } else {
        return exports.named;
      }
    }

    System.link = function(source, options) {
      if (options.metadata.type === 'node') {
        return new Module(extractNodeExports(this, source));
      }
    }

A similar approach could be used to allow assimilated AMD modules to have a "single export".

类似的方案也可以用来处理 AMD 模块只有“单个 export” 的情况。

## Importing an ES6 Module From a Node Module

## 在 Node 模块中 import ES6 Module

When using a node module, we would want to be able to import any other module, regardless of the source.

当使用 node 模块时，我们可能希望这些 node 模块也可以 import 其他模块，但不用在意 node 模块的源码时什么样子。

One major benefit of the above approaches to importing non-ES6 modules is that it means that the standard System.get will be able to load them.

之前用来 import 非 ES6 模块的方案有一个主要的优点就是，可以使用标准的 `System.get` 来 import 这些模块。

This means that it's easy to support require in a node module: just alias it to System.get.

也就是说其实我们可以很容易的支持在 node 模块中的 require，只需要给 `System.get` 起一个别名即可：

    function extractNodeExports(loader, source) {
      var loader = new Loader();
      var exports = loader.eval(`
        var module = {};
        var exports = {};
        var require = System.get;
        ${source};
        { single: module.exports, named: exports };
      `);

      if (exports.single !== undefined) {
        return { exports: exports.single }
      } else {
        return exports.named;
      }
    }

## Importing an AMD Module With Asynchronous Dependencies

## Import 包含异步以来的 AMD 模块

In the above examples, we assumed that all dependencies in external modules are available synchronously, so we could use System.get in the link hook.
AMD modules can have asynchronous dependencies that can be determined without having to execute the module.

在上面的例子中，我们其实做了这样的假设，其他模块的所有以来都是可以同步获取的，因此我们可以在 link hook 中使用 `Sysem.get`。然而 AMD 模块可以拥有异步的依赖，不过这些依赖可以在不执行模块的前提下获取。

For this use-case, you can return (from link) a list of dependencies and a callback to call once the Loader has loaded the dependencies. The callback will receive the list of dependencies as parameters and must return a Module instance.

针对这种情况，你可以在 link 函数中返回一列依赖以及一个回调函数，在依赖加载好之后 Loader 会执行这个回调函数，将依赖作为参数传入，接受一个 Module 实例作为返回值。

    var amd = ['string-utils'];

    System.resolve = function(path, options) {
      if (amd.indexOf(path) !== -1) {
        options.metadata = { type: 'amd' };
      } else {
        options.metadata = { type: 'es6' };
      }
    };

    System.link = function(source, options) {
      if (options.metadata.type !== 'amd') { return; }

      var loader = new Loader();
      var [ imports, factory ] = loader.eval(`
        var dependencies, factory;
        function define(dependencies, factory) {
          imports = dependencies;
          factory = factory;
        }
        ${source};
        [ imports, factory ];
      `);

      var exportsPosition = imports.indexOf('exports');
      imports.splice(exportsPosition, 1);

      function execute(...args) {
        var exports = {};
        args.splice(exportsPosition, 0, [exports]);
        factory(...args);
        return new Module(exports);
      }

      return { imports: imports, execute: execute };
    };

Returning the imports and a callback from link allows the link hook to participate in the same two-phase loading process of ES6 modules, but using the AMD definition to separate the phases instead of ES6 syntax.



![](http://cl.ly/image/2x3X2p1E222X/Pasted_Image_3_10_13_5_41_PM.png)

## Importing a Node Module By Processing requires

Because node modules use a dynamic expression for imports, there is no perfectly reliable way to ensure that all dependencies are loaded before evaluating the module.

The approach used by Browserify is to statically analyze the file first for require statements and use them as the dependencies. The AMD CommonJS wrapper uses a similar approach.

The link hook could be used to analyze Node-style packages for require lines, and return them as imports.

By the time the execute callback was called, all modules would be synchronously available, and aliasing require to System.get would continue to work.

    import { processImports } from "browserify";

    System.link = function(source, options) {
      var imports = processImports(source);

      function execute() {
        return new Module(extractNodeExports(source));
      }

      return { imports: imports, execute: execute};
    };

Of course, this means only works as long as no requires are used with dynamic expressions, in a conditional, or in a try/catch, but those are already limitations of systems like Browserify.

<img src="http://cl.ly/image/3Q38113B2I22/Pasted_Image_3_10_13_5_41_PM.png">

## Interoperability in General

Let's review the overall strategy used for assimilating non-ES6 module definitions:

	•	Non-ES6 modules can be loaded through the Loader by overriding the resolve and link hooks.
	•	Non-ES6 modules can asynchronously load other modules by return imports from link and synchronously through System.get.

This means that all module systems can freely interoperate, using the Loader as an intermediary.

For example, if an AMD module (say, 'app'), depended on a Node-style module (say, 'string-utils'):

	1	When loading app, the link hook would return { imports: ['string-utils'], execute: execute }.
	2	This would cause the Loader to attempt to load 'string-utils', before it would call back the provided execute callback.
	3	The Loader would fetch string-utils and evaluate it using the Node-style link hook.
	4	Once this is done, the provided execute callback would run, receiving the string-utils Module as a parameter.
	5	The execute callback would then return a Module.

This is just an illustrative example; any combination of module systems could freely interoperate through the Loader.

## A Note on "Single Export" Interoperability

Many of the existing module systems support mechanisms for exporting a single value instead of a number of named values from a module.

At the current time, ES6 modules do not provide explicit support for this feature, but it can be emulated using the Loader. One specific strategy would be to export the single value as a well-known name (for example, exports).

Let's take a look at how a Loader could support a Node-style module using require to import the "single export" of another Node-style module.

This same approach would support interoperability between module systems that support importing and exporting of single values.

We'll need to enhance the previous solution we provided for this scenario:

    var isSingle = new Symbol();

    function extractNodeExports(loader, source) {
      var loader = new Loader();
      var exports = loader.eval(`
        var module = {};
        var exports = {};
        var require = System.get;
        ${source};
        { single: module.exports, named: exports };
      `);

      if (exports.single !== undefined) {
        return { exports: exports.single, [isSingle]: true };
      } else {
        return exports.named;
      }
    }

    System.link = function(source, options) {
      if (options.metadata.type === 'node') {
        return new Module(extractNodeExports(this, source));
      }
    }

Here, we create a new unique Symbol that we will use to tag a module as containing a single export. This will avoid conflicts with Node-style modules that export the name exports explicitly.

Next, we will need to enhance the code that we have been using for Node-style require. Until now, we have simply aliased it to System.get. Now, we will check for the isSingle symbol and give it special treatment in that case.

    // this assumes that the `isSingle` Symbol is in scope
    var require = function(name) {
      var module = System.get(name);
      if (module[isSingle]) {
        return module.exports;
      } else {
        return module;
      }
    }

This same approach, using a shared isSingle symbol, could be used to support interoperability between AMD and Node single exports.

As described earlier, ES6 modules would use import { exports: underscore } from 'string-utils/underscore'.

## Configuration of Existing Loaders

## 与现存 Loader 相似的配置

The requirejs loader has a number of useful configuration options that its users can use to control the loader.

This section covers a sampling of those options and how they map onto the semantics of the ES6 Loader. In general, the compilation pipeline provides hooks that can be used to implement these configuration options.

### Base URL

### Base URL

The requirejs loader allows the user to configure a base URL for resolving relative paths.

In the default browser loader, the base URL will default to the page's base URL. The default System.resolve will prefix that base URL and append .js to the end of the module name (if not already present).

The browser's default Loader (window.System) will also include a baseURL configuration option that controls the base URL for its implementation of resolve.

JavaScript code could also configure the Loader's resolve hook to provide any policy they like:

    var resolve = System.resolve;

    System.resolve = function(name, ...args) {
      if (name.match(/fun/)) {
        return `/assets/javascripts/${name}.js"
      }
      return resolve(name, ...args);
    };

### URL Arguments

### URL 参数

Similarly, the requirejs loader allows the specification of additional URL arguments. This could also be handled by overriding the resolve hook.

    var resolve = System.resolve;

    System.resolve = function(...args) {
      return resolve(name, ...args) + "?bust=" + (new Date().getTime());
    };

### Timeouts

### 超时

The requirejs loader allows the specification of a timeout before rejecting the request.

With the ES6 Loader, the fetch hook can be overridden to reject the fetch after some time has passed.

    var fetch = System.fetch;

    System.fetch = function(url, options) {
      setTimeout(function() {
        options.reject("Timeout");
      }, 5000);

      fetch(url, options);
    };

### Support for Legacy Modules

### 支持无规格的模块

The requirejs loader provides a mechanism for declaring how a legacy module should be interpreted:

    requirejs.config({
      shim: {
        backbone: {
          deps: ['underscore', 'jquery']
          exports: 'Backbone'
        },
      }
    });

The example above under Using Existing Libraries as Modules shows one approach to this problem. That approach should work generically, without having to list a specific export name.

The link hook provides a way to define dependencies for legacy modules.

    var config = {
      backbone: {
        deps: ['underscore', 'jquery'],
        exports: ['Backbone']
      }
    }

    function executeCallback(source, exportNames) {
      System.eval(source);
      var exports = {};
      exportNames.forEach(function(name) {
        exports[name] = System.global[name]
      });
      return new Module(exports);
    }

    System.link = function(source, options) {
      if (!config[options.normalized]) { return; }

      var { deps, exports: exportNames } = config[options.normalized];

      if (moduleConfig) {
        return {
          imports: moduleConfig.deps,
          execute: executeCallback(source, exportNames);
        }
      }
    };

## Referencing Modules in HTML

## 在 HTML 中引用模块

In Ember.js, Angular.js, and other contemporary frameworks, JavaScript objects are referenced in HTML templates:

在 Ember.js 、Angular.js 等目前流行的框架中，会在 HTML 中引用 JavaScript 对象：

    <!-- ember.js -->
    {{#view App.FancyButton}}
    <p>Fancy Button Contents</p>
    {{/view}}

Here, the app is asking Ember.js to render some HTML defined in an App.FancyButton constructor. Note that Ember encourages the use of a global namespace for coordination between JavaScript and HTML templates.

    <!-- angular -->
    <button fancy-button>
      <p>Fancy Button Contents</p>
    </button>

Here, the app is asking Angular.js to replace the <button> with some content defined in a globally registered fancy-button directive.

Both Angular and Ember both use globally registered names to define controller objects to attach to parts of the HTML controlled by the framework.

    <!-- ember -->
    {{control "fancy-button"}}

Here, the app is asking Ember.js to render some HTML defined in an App.FancyButtonView and use an instance of the App.FancyButtonController as its controller. Again, Ember is relying on a globally rooted namespace for coordination.

    <!-- angular -->
    <div ng-controller="TodoCtrl">
      <span>{{remaining()}} of {{todos.length}} remaining</span>
    </div>

Here, the app is asking Angular to use a globally rooted object called TodoCtrl as the controller for this part of the HTML. In Angular, this controller is used to control the scope for data-bound content nested inside of its element.

To handle the kind of situation where a module is referenced by a String and needs to be looked up dynamically, ES6 modules provide an API for looking up a module at runtime.

    System.get('controllers/fancy-button');

Systems like Ember or Angular could use this API to allow their users to reference a module's exports in HTML.

In the first Ember example, instead of referencing a globally rooted constructor, the HTML would reference a module name:

    <!-- ember.js -->
    {{#view views/fancy-button}}
    <p>Fancy Button Contents</p>
    {{/view}}

And the module would look like:

    // views/fancy-button.js
    import { View } from "ember";

    export let view = View.extend({
      // contents
    });

The second Angular example could be rewritten as:

    <!-- angular -->
    <div ng-controller="controllers/todo">
      <span>{{remaining()}} of {{todos.length}} remaining</span>
    </div>

And the JavaScript:

    // controllers/todo.js

    export function Controller($scope) {
      // contents
    }

The general pattern is to switch from globally rooted namespaces to named, registered modules. System.get provides a way to dynamically look up already loaded modules.

## Creating Modules from HTML

## 在 HTML 中创建模块

The new Web Components specification provides a way to create a JavaScript constructor through HTML:

新的 Web Component 规范提供了一种通过 HTML 创建 JavaScript 构造函数的方式：

    <element extends="button" name="x-fancybutton" constructor="FancyButton">
      <script>
        FancyButton.prototype.razzle = function () {
        };
        FancyButton.prototype.dazzle = function () {
        };
      </script>
    </element>

    // app.js

    var b = new FancyButton();
    b.textContent = "Show time";
    document.body.appendChild(b);
    b.addEventListener("click", function (event) {
        event.target.dazzle();
    });
    b.razzle();

Here, the <element> tag is creating a globally rooted name for the constructor.
The specifics will probably vary in practice, but something like this could work:

`<element>` 标记在全局创建了一个构造函数，这个规范在实现细节可能有所不同，但是可以像下面这样工作：

    <element extends="button" name="x-fancybutton" module="web/x-fancybutton">
      <script>
      // automatically imports Element from web/x-fancybutton
      Element.prototype.razzle = function () {
      };
      Element.prototype.dazzle = function () {
      };
      </script>
    </element>
    // app.js

import { Element: FancyButton } from "web/x-fancybutton"

    var b = new FancyButton();
    b.textContent = "Show time";
    document.body.appendChild(b);
    b.addEventListener("click", function (event) {
        event.target.dazzle();
    });
    b.razzle();

Originally posted by wycats as a gist on GitHub, but I felt it deserved to get more public attention. Resources describing ES6 modules are scarce, and rarely this detailed. Head over to the gist for an interesting discussion on the state of ES6 modules.

这些内容最初是 wycats 发布在 GitHub 上的 gist，但我认为它值得大家给予更多的关注。描述 ES6 模块化的资源很少，尤其是这样的细节。可以到这个 gist 下看看，有很多有趣的关于 ES6 模块的讨论。

Related links:

相关链接：

	•	ECMAScript 6 modules: the future is now
	•	ECMAScript 6 modules in future browsers
	•	Traceur Compiler
	•	grunt-traceur
	•	ES6 Compatibility Table
	•	ES6 Code Samples in my upcoming JavaScript Application Design book

If you want to toy around with the syntax, ModuleLoader/es6-module-loader is the most up-to-date polyfill I could find. Believe me, I looked.

如果我你再找一个支持这些语法的玩具，我所知道的 ModuleLoader/es6-module-loader 是最跟的上规范的 polyfill。相信我，我看过了。

