# Cross-Platform Development With NW.js

# 使用 NW.js 跨平台开发

More applications are taking advantage of web technologies. For example, Brackets, Peppermint, and Pinegrow are programmer editors made from HTML, JavaScript, and CSS. This allows for using familiar tools, but also makes applications more cross-platform in nature. In this tutorial, I will show you how to use NW.js for making a simple programmer’s editor that you can use in Windows, Mac OS X, and Linux.

越来越多的应用开始借助于 Web 技术。比如，[Brackets](http://brackets.io/)、[Peppermint](http://osxpeppermint.com/) 和 [Pinegrow](http://pinegrow.com/) 都是基于 HTML 、JavaScript 和  CSS 实现的程序编辑器。这样不但可以使用熟悉的工具，应用还是天然跨平台的。在本教程中，我们为你展示如何使用 NW.js 开发一个程序编辑器，可以跨 Windows Mac OS X 和  Linux 使用。

## Introduction and Downloading NW.js

## NW.js 介绍及下载

NW.js, originally known as Node Webkit, is the packaging of Node.js and WebKit HTML render in a package for running local applications. The NW.js version is using io.js, a more current version of the V8 JavaScript engine with more ES6 support. Since io.js is 100% compatible with the latest Node.js, all libraries and programs using Node.js are usable with io.js as well.

[NW.js](http://nwjs.io/) 就是原来的 Node Webkit，即融合了 Node.js 和 Webkit HTML 渲染器来运行本地应用。新版的 NW.js 基于 [io.js](https://iojs.org/) ，后者比起 Node.js 采用了更新版的 JavaScript 引擎 V8，对 ES6 支持得更好。既然 io.js 对最新的 Node.js 是百分百兼容的，因此所有是使用 Node.js 的类库和程序也可以使用在 io.js 中。

To get started, download all three OS versions from NW.js or just the versions you want to run the project. I will be developing on my MacBook Air, but you can use any system you want. The project is Fun Editor: an easy to use, single document code editor. It is built with the Linux motto: A single task done well!

行动起来，下载三个不同 OS 平台的 [NW.js](http://nwjs.io/)，也可以下载你想运行应用平台对应的版本。我将在我的 MacBook Air 上进行开发，但是你可以使用你想想用的系统。项目是 Fun Editor：一个易用的单文件代码编辑器。传承 Linux 的思想：每次只做好一件事！

## Getting the Parts

## 安装组件

To get started, node or io.js has to be installed on your system. Once you install node or io.js, the npm command will be on your system. This command is for installing different JavaScript libraries.

要开始，系统必须安装 [node](http://nodejs.org/) 或者 [io.js](https://iojs.org/)。一旦装好了 node 或者 io.js，你的系统中就包含了 `npm` 命令。这个命令可以用来安装其他 JavaScript 类库。

The first library is Bower. Type the following in a command line prompt:

第一个类库就是 [Bower](http://bower.io/)。在命令行中输入如下命令：

    npm install -g bower

On some systems, you might need to use sudo before the npm command to run it in super user mode.

在某些系统你，你可能需要使用  `sudo` 命令，以超级用户的身份来运行 `npm` 命令。

The bower command is a package manager for many web technologies. It gives an easy way to add web related items to your project.

`bower` 命令作为绝大多数 Web 类库的包管理器，为 Web 项目安装常用类库提供了一种简单的方式。

To do custom actions to the DOM, this editor will make use of Zepto.js JavaScript library instead of jQuery. Since Fun Editor will use the library for DOM work only, Zepto fits the bill at a fraction of the size.

为了进行 DOM 操作，编辑器采用了[ Zepto.js](http://zeptojs.com/)  来代替 jQuery。既然 Fun Editor 就只是使用类库来操作 DOM，小巧的 Zepto 优势就体现出来了。

Create a new directory for the project. In the directory with the command line, type the following:

为项目创建一个新的目录，在命令行的新目录中，输入如下命令：

    bower install less
    bower install zepto

There will now be a new directory called bower_components. In this directory, bower installed the less and zepto libraries. That was much easier than finding the web site and downloading.

在目录中多了一个 `bower_components` 的子目录，在这个目录中，bower 安装了 `less` 和 `zepto` 这两个类库。这比起找到它们网站在下载容易多了。

The Ace JavaScript library is the base for this editor. It is a flexible and easy to use editor in JavaScript designed for embedding into web sites. To install, type the following in the terminal in our project directory:

[Ace](http://ace.c9.io/#nav=about) JavaScript 类库是这个编辑器应用的基础。它是一个灵活易用的编辑器类库，基于 JavaScript，为 Web 站点设计而开发。可以在命令行中输入如下命令安装：

    git clone git://github.com/ajaxorg/ace.git

There is now a new directory ace. All of the sources for the library are in this directory. The library needs to be compiled to the compact format to speed up loading. In the command line, type:

现在又多了一个新目录 `ace`。这个类库所需要的资源都在这个目录中。需要把这个类库编译成合并的模式来加快它的加载速度。在命令行中，输入：

    cd ace
    node install
    node Makefile.dryice.js full -m --target ./build

These commands put you into the ace directory, install all needed libraries for the ace editor, and create the minimized library files in the build sub-directory.

运行这些命令，进入 `ace` 目录，安装所有 ace 编辑器依赖的类库，然后在子目录 `build` 中生成压缩有的 ace 类库文件。

This project will use the node-watch library to know if the file in the editor has changed. This library activates a callback function whenever the specified file changes. To install this library in to our project, the project directory needs to be a node package directory. In the command line in the project directory, type the following:

这个项目将使用  `node-watch` 类库来监听文件变化。文件变化后这个类库会激活一个回调函数。为了把这个模块安装到项目中，项目首先必须是一个 node 包目录。因此在命令行的根目录中，输入：

    npm init

The npm program will ask several questions about the project. Once you answer the questions, a package.json file is in the project directory. The npm command will store the names of all libraries installed in this file. That way, giving the project file to someone else enables them to create the same working environment.

`npm` 程序会询问几个关于项目的问题。你一一作答之后，就会在目录中生成一个 `package.json` 文件。`npm` 命令会把所有用到的类库名和版本保存在文件中。这样有个好处，项目到别人手里可以很快搭建起一样的工作环境。

Now to install the node-watch library, type the following:

输入如下命令，安装 `node-watch` 类库：

    npm install node-watch --save

Once finished, there is a node_modules directory with the library inside. The –save flag told npm to save the library into the project and not globally.

安装完成，增加了一个 `node_modules` 目录，node-watch 就放在里面。`—save` 标识是为了告诉 `npm` 将项目队 node-watch 的依赖写入到 package.json 中。

The last piece of software to install is Emmet. You cannot have a code editor without Emmet. Get the Emmet code from Emmet on GitHub and save it to the file emmet.js in the js directory.

最后一定要安装的就是 [Emmet](http://emmet.io/)。没有它就没有这个代码编辑器。从 [GitHub 上获取 Emmet 的源码](https://raw.githubusercontent.com/nightwing/emmet-core/master/emmet.js)，保存到 `js` 目录的 `emmet.js` 文件中。

Now that all the pieces are in place in the project folder, everything is ready to put it together!

好了，现在所有组件都已经就位，接下来就是把它们组合到一起！

## Putting It All Together

## 组装

The first item needed for a NW.js project is its project file. Unfortunately, the node project file uses the same file name. Since the node project file is not needed during development, it can be moved aside until needed again. In the command line in the project directory, type the following:

对于 NW.js 项目来说第一要素来说就是 project 文件。但不幸的是与 node 的项目文件是重名的。既然在开发过程中 node 项目文件不是必须的，可以把它复制到别的地方去，直到需要在改回来。在命令行中运行：

    mv package.json node.package.json
    touch package.json

These commands moved the original project.json file to node.project.json and created an empty file called project.json. In the project.json file, type the following:

命令把原始的 `package.json` 文件变成 `node.package.json` 文件，并创建了一个内容为空的 `package.json`。在这个文件中，输入：

    {
        "description": "A very small code editor.",
        "main": "main.html",
        "name": "Fun Editor",
        "version": "1.0",
        "window": {
            "height": 600,
            "width": 650,
            "show": false,
            "title": "Fun Editor",
            "toolbar": false,
            "icon": "icon.png"
        }
    }

This file tells NW.js how to launch the program. The different fields are:

这个文件会告诉 NW.js 如何启动这个程序。每个字段的作用如下：

### description

This should be a short description of the program and what it does.

简短描述应用是什么。

### main

This is the main HTML file to open. This file should contain all the HTML for the main page of the program.

入口 HTML 文件。里面包含了应用入口页面所需要的全部 HTML。

### name

This is the name of the program.

应用名。

### version

This should be a version number for the program.

应用的版本号。

### window

This is a json array of values to describe the User Interface of the program. This array has the following items:

一个 json 对象用来描述这个程序的用户界面。包含以下属性：

#### height

This is a number of pixels for the height of the program window when started.

程序启动时窗口的高度。

#### width

This is the number of pixels for the width of the program window when started.

程序启动时窗口的宽度。

#### show

This is a Boolean telling NW.js whether or not to show the main windows upon loading. I have it set to true. If you set it to false, make sure there is some way to activate it later.

Boolean 值，设置 NW.js 在加载时是否显示主窗口。我设置的是 true，如果要设置为 false，必须确保之后在某个地方激活窗口。

#### title

This is the default title for the program. It will be used when loaded, until some code in the program changes it.

默认程序名。在程序加载好之后显示。除非应用中有代码修改了它。

#### toolbar

This Boolean tells NW.js whether or not to have a toolbar. Since FunEditor is a minimalistic editor, this is set to false. But, if you ever need to use the Dev Tools to debug your editor, you can set this to true and an icon for the Dev Tools will be there.

Boolean 值，设置 NW.js 是否包含一个工具栏。因为 FunEditor 是一个极简的编辑器，所以这里设置为 false。但是，如果需要使用 Dev Tools 来调试编辑器，可以将其设置为 true，这样工具栏上就有一个用来打开 Dev Tools 的图标。

#### icon

This is a relative path of the icon to use for the program. The project directory should be the base for the reference to this file.

相对路径指向作为应用图标的图片文件，相对于项目的根目录。

Since a main.html file is in the configuration, it is the next file made. In the project directory, create the main.html file and put this code in it:

在配置中有一个 `main.html` 文件，自然就是下一个要加入的文件。在项目根目录，创建 `main.html` ，添加如下代码：

    <!DOCTYPE html>
    <html>

        <head>
            <title>Fun Editor</title>
            <script type="text/javascript" src="bower_components/zepto/zepto.min.js"></script>
            <script type="text/javascript" src="js/emmet.js"></script>
            <script type="text/javascript" src="ace/build/src-min-noconflict/ace.js"></script>
            <script type="text/javascript" src="ace/build/src-min-noconflict/ext-emmet.js"></script>
            <script type="text/javascript" src="ace/build/src-min-noconflict/ext-language_tools.js"></script>
            <script type="text/javascript" src="ace/build/src-min-noconflict/ext-spellcheck.js"></script>
            <script type="text/javascript" src="ace/build/src-min-noconflict/keybinding-vim.js"></script>
            <script type="text/javascript" src="js/FunEditor.js"></script>
            <link rel="stylesheet/less" type="text/css" href="less/default.less">
            <script type="text/javascript" src="bower_components/less/dist/less.min.js"></script>
        </head>

        <body>
            <div id="editor"></div>

            <div class="info">
                <span id="editMode" class="statuslineitem">Normal</span>
                <span class="title statuslineitem">
                <span class="arrow-right-editMode statuslineitem"></span>
                <span id="title">No file</span>
                </span>
                <span class="mode statuslineitem">
                    <span class="arrow-right-title statuslineitem"></span>
                <span id="mode">JavaScript</span>
                </span>
                <span class="linenum statuslineitem">
                    <span class="arrow-right-mode statuslineitem"></span>
                <span id="linenum">1</span>
                </span>
                <span class="colnum statuslineitem">
                    <span class="arrow-right-linenum statuslineitem"></span>
                <span id="colnum">1</span>
                </span>
                <span class="arrow-right-colnum statuslineitem"></span>
            </div>

            <input style="display:none;" id="openFile" type="file" />
            <input style="display:none;" id="saveFile" type="file" />
        </body>
    </html>

That is the main program. It is a simple HTML page that is mostly a list of JavaScript files to load, one Less file for styling everything, a div with an id of editor for the editor, and a div with an id of info and some spans for doing the status line. I took the styling for the status line from my tutorial on Getting Spiffy With Powerline. It doesn’t use Powerline—it just looks like it. At the bottom are two inputs that do not get displayed. These are for the NW.js open file dialog and save file dialog.

这是应用的主入口，就是一个简单的页面，引入了多个 JavaScript 文件，一个定义所有样式的 Less 文件，一个 `id` 为 `editor` 的 div 用作编辑器，一个 `id` 为 `info` 的 div ，包含数个 span 元素作为状态栏。状态栏的样式是从我之前的一个教程 [Getting Spiffy With Powerline](https://computers.tutsplus.com/tutorials/getting-spiffy-with-powerline--cms-20740) 移植过来的。只是长得像 Powerline，但并没有使用它。在底部还有两个隐藏的输入框，给 NW.js 用来打开去读文件和保存文件的对话框。

Good coding practices would say to load the JavaScript at the end of the page. Since the window for the page is not shown until the program tells it to show, it does not matter.

最佳实践告诉我们 JavaScript 必须放在页面的底部，但既然页面窗口的显示是由程序来控制的，关系就不大了。

To make the HTML look right, styling is the next item to create. Create a new directory called less in the project directory. Then create the file default.less and add this to it:

要让 HTML 可以看，样式是下一个需要添加的。在项目目录新建一个 `less` 文件夹，添加 `default.less` 文件，添加如下样式：

    @StatusLineEditMode:    rgb(98, 105, 255);
    @StatusLineTitle:       rgb(98, 247, 255);
    @StatusLineMode:        rgb(98, 255, 149);
    @StatusLineLineNum:     rgb(224, 217, 87);
    @StatusLineColNum:      rgb(87, 212, 224);
    @StatusLineBackground:  white;

    body {
      margin: 0;
      padding: 0;
      margin-top: 0;
      overflow: hidden;
    }

    #editor {
      position: absolute;
      top: 0px;
      bottom: 25px;
      left: 0;
      right: 0;
      margin: 0px;
      padding: 0px;
    }

    .info {
      position: absolute;
      display: inline;
      bottom: 0px;
      font-family: monospace;
      white-space: nowrap;
      bottom: 0;
      background: @StatusLineBackground;
      padding: 0px;
      height: 25px;
      left: 0;
      width: 100%;
    }

    .statuslineitem {
        height: 25px;
        line-height: 25px;
        text-align: center;
        vertical-align: middle;
        margin: 0px;
        padding-top: 0px;
        padding-bottom: 0px;
        padding-right: 3px;
        padding-left: 0px;
        border: 0px;
        float: left;
    }

    .arrow-right-editMode {
        width: 0;
        height: 0;
        border-top: 13px solid transparent;
        border-bottom: 13px solid transparent;

        border-left: 13px solid @StatusLineEditMode;
        margin: 0px;
        padding: 0px;
        margin-left: 0px;
    }

    .arrow-right-title {
        width: 0;
        height: 0;
        border-top: 13px solid transparent;
        border-bottom: 13px solid transparent;

        border-left: 13px solid @StatusLineTitle;
        margin: 0px;
        padding: 0px;
        margin-left: 0px;
    }

    .arrow-right-mode {
        width: 0;
        height: 0;
        border-top: 13px solid transparent;
        border-bottom: 13px solid transparent;

        border-left: 13px solid @StatusLineMode;
        margin: 0px;
        padding: 0px;
        margin-left: 0px;
    }

    .arrow-right-linenum {
        width: 0;
        height: 0;
        border-top: 13px solid transparent;
        border-bottom: 13px solid transparent;

        border-left: 13px solid @StatusLineLineNum;
        margin: 0px;
        padding: 0px;
        margin-left: 0px;
    }

    .arrow-right-colnum {
        width: 0;
        height: 0;
        border-top: 13px solid transparent;
        border-bottom: 13px solid transparent;

        border-left: 13px solid @StatusLineColNum;
        margin: 0px;
        padding: 0px;
    }

    #editMode {
        background: @StatusLineEditMode;
        padding-left: 5px;
    }

    #title {
        background: @StatusLineTitle;
        padding-left: 5px;
    }

    #mode {
        background: @StatusLineMode;
        padding-left: 5px;
    }

    #linenum {
        background: @StatusLineLineNum;
        padding-left: 5px;
    }

    #colnum {
        background: @StatusLineColNum;
        padding-left: 5px;
    }

    #editMode {
        background: @StatusLineEditMode;
        padding-left: 5px;
    }

    .title {
        background: @StatusLineTitle;
    }

    .mode {
        background: @StatusLineMode;
    }

    .linenum {
        background: @StatusLineLineNum;
    }

    .colnum {
        background: @StatusLineColNum;
    }

This styling information is to make the editor div position absolute and take up all the browser window area except for 25 px at the bottom. That area is for the status line.

这段样式让 `editor` div 绝对定位，占满整个浏览器窗口，处理在底部留出 25px 像素的间隙给状态栏。

At the top of the file, you will notice several Less variable definitions. Since the same colors get used in more than one place (i.e. the span for line number and its “arrow” span afterwards), I made them into a Less variable. That way, I only have to change one item to change all of them. Stylus or SASS could be used as well, but Less allows for dynamically changing them in the code. Great for theme changing down the road.

在这个文件的最上面，发现定义了几个 Less 变量。既然同样的颜色值会用在不同的地方（例如显示行号的 span，包括后面显示“箭头”的 span），所以我把他们定义成了一个 Less 变量。这样写便于就修改一个地方，每个地方都生效。Stylus 或者 SASS 都可以，只是 Less 允许在代码中动态地对值进行修改，便于随后定制主题的修改。

Now for the main program file. In the js directory, create a file called FunEditor.js and place the following code:

现在轮到了主程序文件。在 `js` 目录下，新建 `FunEditor.js` 添加如下代码：

    //
    // Program:         Fun Editor
    //
    // Description:     This is a basic editor built using NW.js.
    //          This is more of a project to learn how to use
    //          NW.js, but I am finding that I really like
    //          the editor!
    //
    // Author:      Richard Guay (raguay@customct.com)
    // License:         MIT
    //

    //
    // Class:       FunEditor
    //
    // Description:     This class contains the information and functionality
    //          of the Fun Editor. There should be only one instance
    //          of this class per editor.
    //
    // Class Variables:
    //                  editor      Keeps the Editor object
    //                  menu        keeps the menu object for the pop-up
    //                              menu.
    //                  menuEdit        Edit menu for the popup menu
    //                  menuEditMain    The Edit menu for the main menu
    //                  menuFile        File menu for the popup menu
    //                  menuFileMain    File menu for the main menu
    //                  nativeMenuBar   The menu bar for OSX Main menu
    //                  hasWriteAccess  boolean for if the file is
    //                                  writable or not.
    //                  origFileName    Last file name opened.
    //                  watch       The node-watch library object.
    //                  osenv       The osenv library object.
    //                  gui             The NW.js gui library
    //                              object.
    //                  fs          The fs library object.
    //                  clipboard       The clipboard library object.
    //
    function FunEditor() {}

    FunEditor.prototype.filesOpened = 0;
    FunEditor.prototype.saving = false;
    FunEditor.prototype.editor = null;
    FunEditor.prototype.menu = null;
    FunEditor.prototype.menuEdit = null;
    FunEditor.prototype.menuFile = null;
    FunEditor.prototype.menuEditMain = null;
    FunEditor.prototype.menuFileMain = null;
    FunEditor.prototype.nativeMenuBar = null;
    FunEditor.prototype.fileEntry = null;
    FunEditor.prototype.hasWriteAccess = false;
    FunEditor.prototype.origFileName = "";
    FunEditor.prototype.theme = {};
    FunEditor.prototype.lastCursor = { line: 0, col: 0 };
    FunEditor.prototype.watch = require("node-watch");
    FunEditor.prototype.gui = require("nw.gui");
    FunEditor.prototype.fs = require("fs");
    FunEditor.prototype.osenv = require("osenv");
    FunEditor.prototype.os = require("os");

    var FE = new FunEditor();

    FunEditor.prototype.clipboard = FE.gui.Clipboard.get();
    FunEditor.prototype.win = FE.gui.Window.get();

    //
    // Function:    handleDocumentChange
    //
    // Description:     This function is called whenever the document
    //          is changed. This function will get the title set,
    //          remove the old document name from the window
    //          list, set the syntax highlighting based on the
    //          file extension, and update the status line.
    //
    // Inputs:
    //          title   Title of the new document
    //
    FunEditor.prototype.handleDocumentChange = function(title) {
         //
         // Setup the default syntax highlighting mode.
         //
         var mode = "ace/mode/javascript";
         var modeName = "JavaScript";

         //
         // Set the new file name. If the title is blank, reflect that for
         // setting a new one later.
         //
         this.fileEntry = title;

         //
         // Set the syntax highlighting based on the file ending.
         //
         if (title) {
            //
            // Set up file watching with node-watch. The file being edited is
            // watched for changes. If the file changes, then reload the file
            // into the editor.
            //
            this.watch(this.fileEntry, function() {
                //
                // The file changed. Load it into the editor. Needs implemented:
                // ask the user if they want the file to be reloaded.
                //
                if(! FE.saving ) {
                        FE.readFileIntoEditor(FE.fileEntry);
                }
            });

            //
            // If there is a title, then setup everything by that title.
            // The title will be the file name.
            //
            if(this.os.platform()  == "win32") {
                title = title.match(/[^\\]+$/)[0];
            } else {
                title = title.match(/[^/]+$/)[0];
            }
            if (this.origFileName.indexOf(title) == -1) {
                //
                // Remove whatever the old file was loaded and put in
                // the new file.
                //
                this.origFileName = title;
            }

            //
            // Check for OS permissions for writing. NOTE: Not implemented.
            //
            this.hasWriteAccess = true;

            //
            // Set the document's title to the file name.
            //
            document.getElementById("title").innerHTML = title;
            document.title = title;

            //
            // Set the syntax highlighting mode based on extension of the file.
            //
            if (title.match(/\.js$/)) {
                mode = "ace/mode/javascript";
                modeName = "JavaScript";
            } else if (title.match(/\.html$/)) {
                mode = "ace/mode/html";
                modeName = "HTML";
            } else if (title.match(/\.css$/)) {
                mode = "ace/mode/css";
                modeName = "CSS";
            } else if (title.match(/\.less$/)) {
                mode = "ace/mode/less";
                modeName = "LESS";
            }  else if (title.match(/\.md$/)) {
                mode = "ace/mode/markdown";
                modeName = "Markdown";
            } else if (title.match(/\.ft$/)) {
                mode = "ace/mode/markdown";
                modeName = "FoldingText";
            } else if (title.match(/\.markdown$/)) {
                mode = "ace/mode/markdown";
                modeName = "Markdown";
            } else if (title.match(/\.php$/)) {
                mode = "ace/mode/php";
                modeName = "PHP";
            }
        } else {
            //
            // Setting an empty document. Leave syntax highlighting as the last
            // file.
            //
            document.getElementById("title").innerHTML = "[no document loaded]";
            this.origFileName = "";
        }

        //
        // Tell the Editor and setup the status bar with the syntax highlight mode.
        //
        this.editor.getSession().setMode(mode);
        document.getElementById("mode").innerHTML = modeName;
    };

    //
    // Function:        setCursorLast
    //
    // Description:     Set the cursor to the last stored state.
    //
    FunEditor.prototype.setCursorLast = function() {
        this.editor.moveCursorTo(this.lastCursor.line, this.lastCursor.col);
    }

    //
    // Function:        newFile
    //
    // Description:     This function is called to set the global
    //          variables properly for a new empty file.
    //
    // Inputs:
    //
    FunEditor.prototype.newFile = function() {
        this.fileEntry = null;
        this.hasWriteAccess = false;
        this.handleDocumentChange(null);
        this.editor.setValue("");
    };

    //
    // Function:    readFileIntoEditor
    //
    // Description:     This function handles the reading of the file
    //          contents into the editor. If reading fails, a
    //          log entry is created.
    //
    // Inputs:
    //          theFileEntry    The path and file name
    //
    FunEditor.prototype.readFileIntoEditor = function(theFileEntry) {
        this.fs.readFile(theFileEntry, function(err, data) {
            if (err) {
                console.log("Error Reading file.");
            }

            //
            // Set the file properties.
            //
            FE.handleDocumentChange(theFileEntry);

            //
            // Set the file contents.
            //
            FE.editor.setValue(String(data));

            //
            // Remove the selection.
            //
            FE.editor.session.selection.clearSelection();

            //
            // Put the cursor to the last know position.
            //
            FE.setCursorLast();
        });
    };

    //
    // Function:    writeEditorToFile
    //
    // Description:     This function takes what is in the editor
    //          and writes it out to the file.
    //
    // Inputs:
    //          theFileEntry    File to write the contents to.
    //
    FunEditor.prototype.writeEditorToFile = function(theFileEntry) {
        var str = this.editor.getValue();
        this.fs.writeFile(theFileEntry, str, function(err) {
            if (err) {
                console.log("Error Writing file.");
                return;
            }
        });
    };

    //
    // Function:    copyFunction
    //
    // Description:     This function takes the current selection and copies it
    //          to the clipboard.
    //
    FunEditor.prototype.copyFunction = function() {
        this.clipboard.set(this.editor.getCopyText());
    };

    //
    // Function:    cutFunction
    //
    // Description:     This function cuts out the current selection and copies it
    //          to the clipboard.
    //
    FunEditor.prototype.cutFunction = function() {
        this.clipboard.set(this.editor.getCopyText());
        this.editor.session.replace(this.editor.selection.getRange(),"");
    };

    //
    // Function:    pasteFunction
    //
    // Description:     This function takes the clipboard and pastes it to the
    //                  current location.
    //
    FunEditor.prototype.pasteFunction = function() {
        this.editor.session.replace(this.editor.selection.getRange(), this.clipboard.get());
    };

    //
    // Function:    openFile
    //
    // Description:     This function opens the select a file dialog for opening
    //          into the editor.
    //
    FunEditor.prototype.openFile = function() {
        $("#openFile").trigger("click");
    };

    //
    // Function:    saveFile
    //
    // Description:     This function saves to the currently open file or
    //          opens the save file dialog.
    //
    FunEditor.prototype.saveFile = function() {
        this.saving = true;
        if (this.fileEntry && this.hasWriteAccess) {
            this.writeEditorToFile(this.fileEntry);
        } else {
            $("#saveFile").trigger("click");
        }
        this.saving = false;
    };

    //
    // Function:    initMenus
    //
    // Description: This function setups the right click menu and system used
    //          in the editor.
    //
    // Inputs:
    //
    FunEditor.prototype.initMenus = function() {
        this.menu = new this.gui.Menu();
        this.menuFile = new this.gui.Menu();
        this.menuEdit = new this.gui.Menu();
        this.menuFile.append(new this.gui.MenuItem({
            label: "New",
            click: function() {
                FE.newFile();
            }
        }));
        this.menuFile.append(new this.gui.MenuItem({
            label: "Open",
            click: function() {
                FE.openFile();
            }
        }));
        this.menuFile.append(new this.gui.MenuItem({
            label: "Save",
            click: function() {
                FE.saveFile();
            }
        }));

        this.menuEdit.append(new this.gui.MenuItem({
            label: "Copy",
            click: function() {
                FE.copyFunction();
            }
        }));
         this.menuEdit.append(new this.gui.MenuItem({
              label: "Cut",
              click: function() {
                FE.cutFunction();
              }
         }));
         this.menuEdit.append(new this.gui.MenuItem({
              label: "Paste",
              click: function() {
                FE.pasteFunction();
              }
         }));

         this.menuFileMain = new this.gui.Menu();
         this.menuEditMain = new this.gui.Menu();
         this.menuFileMain.append(new this.gui.MenuItem({
              label: "New",
              click: function() {
                FE.newFile();
              }
         }));
         this.menuFileMain.append(new this.gui.MenuItem({
              label: "Open",
              click: function() {
                FE.openFile();
              }
         }));
         this.menuFileMain.append(new this.gui.MenuItem({
            label: "Save",
            click: function() {
                FE.saveFile();
            }
         }));

         this.menuEditMain.append(new this.gui.MenuItem({
            label: "Copy",
            click: function() {
                FE.copyFunction();
            }
         }));
         this.menuEditMain.append(new this.gui.MenuItem({
            label: "Cut",
            click: function() {
                FE.cutFunction();
            }
         }));
         this.menuEditMain.append(new this.gui.MenuItem({
            label: "Paste",
            click: function() {
                FE.pasteFunction();
            }
         }));

         this.menu.append(new this.gui.MenuItem({
            label: "File",
            submenu: FE.menuFile
         }));

         this.menu.append(new this.gui.MenuItem({
            label: "Edit",
            submenu: FE.menuEdit
         }));

         //
         // Create the main Mac menu also.
         //
         this.nativeMenuBar = new this.gui.Menu({
            type: "menubar"
         });

         if(this.os.platform()  == "darwin") {
             this.nativeMenuBar.createMacBuiltin("Fun Editor", {
                hideEdit: true,
                hideWindow: true
             });
        }

         this.nativeMenuBar.append(new this.gui.MenuItem({
            label: "File",
            submenu: FE.menuFileMain
         }));

         this.nativeMenuBar.append(new this.gui.MenuItem({
            label: "Edit",
            submenu: FE.menuEditMain
         }));
         this.win.menu = this.nativeMenuBar;

        //
        // Add the menu to the contextmenu event listener.
        //
        document.getElementById("editor").addEventListener("contextmenu",
            function(ev) {
                ev.preventDefault();
                FE.menu.popup(ev.x, ev.y);
                return false;
            }
        );
    };

    //
    // Function:        onChosenFileToOpen
    //
    // Description:     This function is called whenever a open
    //          file dialog is closed with a file selection.
    //          This is an automatically made function in
    //          NW.js that needs to be set by your app.
    //
    // Inputs:
    //          theFileEntry        The path to the file selected.
    //
    onChosenFileToOpen = function(theFileEntry) {
        FE.readFileIntoEditor(theFileEntry);
    };

    //
    // Function:        onChosenFileToSave
    //
    // Description:     When a file is selected to save into, this
    //          function is called. It is originally set by
    //          NW.js.
    //
    // Inputs:
    //          theFileEntry        The path to the file selected.
    //
    onChosenFileToSave = function(theFileEntry) {
         FE.writeEditorToFile(theFileEntry);
    };

    //
    // Function:        onload
    //
    // Description:     This function is setup by NW.js to be
    //                  called when the page representing the application
    //                  is loaded. The application overrides this by
    //                  assigning it's own function.
    //
    //                  Here, we initialize everything needed for the
    //                  Editor. It also loads the initial document for
    //                  the editor, any plugins, and theme.
    //
    // Inputs:
    //
    onload = function() {

         //
         // Initialize the context menu.
         //
         FE.initMenus();

         //
         // Set the change function for saveFile and openFile.
         //
         $("#saveFile").change(function(evt) {
            onChosenFileToSave($(this).val());
        });
        $("#openFile").change(function(evt) {
            onChosenFileToOpen($(this).val());
        });

        FE.editor = ace.edit("editor");
        FE.editor.$blockScrolling = Infinity;
        FE.editor.setTheme("ace/theme/solarized_dark");
        FE.editor.getSession().setMode("ace/mode/javascript");
        FE.editor.setKeyboardHandler("ace/keyboard/vim");
        FE.editor.setOption("enableEmmet", true);
        FE.editor.setOption("selectionStyle","text");
        FE.editor.setOption("highlightActiveLine",true);
        FE.editor.setOption("cursorStyle","slim");
        FE.editor.setOption("autoScrollEditorIntoView",true);
        FE.editor.setOption("tabSize",4);
        FE.editor.setOption("enableSnippets",true);
        FE.editor.setOption("spellcheck",true);
        FE.editor.setOption("wrap",true);
        FE.editor.setOption("enableBasicAutocompletion",true);
        FE.editor.setOption("enableLiveAutocompletion",false);
        FE.editor.commands.addCommand({
            name: "myCopy",
            bindKey: {win: "Ctrl-C",  mac: "Command-C"},
            exec: function(editor) {
                FE.copyFunction();
            },
            readOnly: false
        });
        FE.editor.commands.addCommand({
            name: "myPaste",
            bindKey: {win: "Ctrl-V",  mac: "Command-V"},
            exec: function(editor) {
                FE.pasteFunction();
            },
            readOnly: false
        });
        FE.editor.commands.addCommand({
            name: "myCut",
            bindKey: {win: "Ctrl-X",  mac: "Command-X"},
            exec: function(editor) {
                FE.cutFunction();
            },
            readOnly: false
        });
        FE.editor.commands.addCommand({
            name: "mySave",
            bindKey: {win: "Ctrl-S",  mac: "Command-S"},
            exec: function(editor) {
                FE.saveFile();
            },
            readOnly: false
        });

        //
        // Tie into the Vim mode save function. FE one took some digging to find!
        //
        FE.editor.state.cm.save = function() {
            FE.saveFile();
        }

        //
        // Setup on events listeners. The first one is listen for cursor
        // movements to update the position in the file in the status line.
        // Next, setup the listener for Vim mode changing to update the
        // status line. Lastly, run function on window closing to remove
        // the current file from the open file list.
        //
        FE.editor.on("changeStatus", function() {
            //
            // Get the current cursor to set the row and column.
            //
            var cursor = FE.editor.selection.lead;
            document.getElementById("linenum").innerHTML = cursor.row + 1;
            document.getElementById("colnum").innerHTML = cursor.column + 1;

                //
                    // Save a copy of the cursor location.
                    //
                    FE.lastCursor.line = cursor.row;
                    FE.lastCursor.col = cursor.column;

            //
            // Get the text mode to set the Normal, Visual, or Insert vim
            // modes in the status line.
            //
            var mode = FE.editor.keyBinding.getStatusText(editor);
            if (mode == "") {
                document.getElementById("editMode").innerHTML = "Normal";
            } else if (mode == "VISUAL") {
                document.getElementById("editMode").innerHTML = "Visual";
            } else if (mode == "INSERT") {
                document.getElementById("editMode").innerHTML = "Insert";
            }
        });

        //
        // Capture the Ace editor's copy and paste signals to get
        // or put to the system clipboard.
        //
        FE.editor.on("copy",function(text) {
            FE.clipboard.set(text);
        });
        FE.editor.on("paste", function(e) {
            e.text = FE.clipboard.get();
        });

        //
        // Capture the window close and make
        // sure the file has been saved.
        //
         FE.win.on("close", function() {
            //
            // Make sure the contents are saved.
            //
            if (this.fileEntry && this.hasWriteAccess) {
                FE.saveFile();
             }

            //
            // Close the program.
            //
            this.close(true);
         });

         //
         // Setup for having a new empty file loaded.
         //
         FE.newFile();
         onresize();

         //
         // Show the program and set the focus (focus does not work!).
         //
         FE.win.show();
         FE.win.focus();
    };

    //
    // Function:        onresize
    //
    // Description:     Another NW.js function that is called every time
    //          the application is resized.
    //
    // Inputs:
    //
    onresize = function() {
    };

At the top of the file, you will see the declaration of the FunEditor object and some variables. After the variables, there are three require statements. These statements load the node-watch library for watching a file change, the nw.gui library which allows for interaction with the graphical user interface, the fs library which allows access to the file system, and the os library for working with the operating system. The gui, fs, and os libraries are part of the NW.js program, while the node-watch is a npm downloaded library.

在文件的一开始，定义了 FunEditor 对象以及其他一些变量。变量定义完之后，是三个 `require` 语句。这些语句首先是加载 `node-watch` 类库监听文件的变化，`nw.gui` 类库可以用来与图形的用户界面进行交互，`fs` 类库用来访问文件系统，`os` 类库则是用来与操作系统交互。`gui`、`fs` 和 `os` 都是 NW.js 程序的一部分，而 `node-watch` 则是使用 npm 下载的类库。

With the gui library, the FunEditor.clipboard variable contains the clipboard object for accessing the system clipboard. Likewise, the FunEditor.win variable contains the window object for the main windows created by NW.js for this program.

通过 `gui`，`FunEditor.clipboard` 变量包含了一个 clipboard 对象，可以访问系统剪切板。同理， `FunEditor.win` 变量包含 `window` 对象，即 NW.js 为当前应用创建的主窗口。

After the libraries and global variables are loaded, several helper functions are created. All functions that start with FunEditor are functions for running the editor and are not specific to NW.js. The other functions are required by NW.js.

在这些类库加载完，添加了一些全局变量之后，又增加了一些函数。`FunEditor` 开头的函数是运行编辑器必须的，其他的函数则是 NW.js 所必须的。

I described each function here:

下面描述每个函数的作用：

## FunEditor.handleDocumentChange

## FunEditor.handleDocumentChange

This function loads the editor with the correct syntax highlighting based on the document loaded. It also sets the window title and the document name in the status line. When the window title is set, it checks the operating system being used with FunEditor.os.platform(). If it is a Windows operating system, the search for the last directory separator has to be different since Windows uses \, while OS X and Linux use /.

这个函数基于加载的文档加载具备正确高亮的编辑器。它还会把窗口标题和状态栏设置成文档的名称。当设置窗口标题时，使用 `FunEditor.os.platform()` 检测用户使用什么系统，如果是 Windows，搜索文档所在文件夹的操作符就有点区别，因为 Windows 系统使用的是 `\`，但 OSX 和 Linux 使用的是 `/`。

This function also sets up file watching using FunEditor.watch. When the file given to the function is changed, the callback function reloads the file in to the editor.

这个函数还使用 `FunEditor.watch` 来做文件变更的监听。但给定的文件发生了变化，回调函数就会把文件读到编辑器中。

## FunEditor.setCursorLast

## FunEditor.setCursorLast

This function will set the cursor to the last saved location. The location is saved every time the cursor is moved and recorded in the status line. Whenever the file is reloaded after a changed, this function is used to put the cursor back to the last known location.

该函数把光标上一次记录的位置。每次光标移动都会把它的位置记录在状态栏上。当文档变化重新被加载时，可以用这个函数把放在最后一次记录的位置上。

## FunEditor.newFile

## FunEditor.newFile

This function is for creating a new, blank file.

使用这个函数创建一个新的空文档。

## FunEditor.readFileIntoEditor

## FunEditor.readFileIntoEditor

This function uses the FunEditor.fs library reference variable to read in the file from the file system and place it into the editor. It then clears the selection and puts the cursor at the last known location.

该函数利用 `FunEditor` 上引用的 `fs` 类库从文件系统中读取文件，加入到编辑器中。并会清除选中，将光标加入到最近的一次出现的位置。

## FunEditor.writeEditorToFile

## FunEditor.writeEditorToFile

This function takes the current state of the editor and saves it to the file system using the FunEditor.fs variable.

将编辑器目前的状态通过 `FuncEditor.fs` 保存到文件系统中。

## FunEditor.copyFunction

## FunEditor.copyFunction

This function uses the NW.js library to take the currently selected text and put it in to the system clipboard.

使用 NW.js 把当前选中的文本加入到系统剪切板中。

## FunEditor.cutFunction

## FunEditor.cutFunction

This function is the same as FunEditor.copyFunction, except that it deletes the selected text from the editor.

与 `FunEditor.copyFunction` 一致，只是该函数会删除编辑器中被选中的文本。

## FunEditor.pasteFunction

## FunEditor.pasteFunction

This function takes the clipboard contents using the NW.js library for the clipboard and pastes it to the current cursor location in the editor.

通过 `NW.js` 类库从剪切板拷贝文本，粘贴到编辑器中目前光标的位置中。

## FunEditor.openFile

## FunEditor.openFile

This function opens the system open file dialog by triggering a click on the hidden input element. This is particular to NW.js as well.

通过出发隐藏文件输入框的方式打开系统文件对话框，这个是 `NW.js` 的一部分。

## FunEditor.saveFile

## FunEditor.saveFile

This function saves the editor contents to the file if it was loaded from a file and that file has write access. Otherwise, it opens the system save as dialog for the user to select a file. This also works by triggering a click on the hidden saveFile element.

如果打开的文件有写入权限的话，这个函数会把编辑器的内容写入到打开的文件中。否则，它将会弹出一个文件对话框，让用户选择一个文件。这也是通过触发隐藏的 `saveFile` 元素来实现的。

## FunEditor.initMenus

## FunEditor.initMenus

This function creates the graphical menu elements for an in-context menu and the system menu. This makes use of the FunEditor.gui library.

为右键菜单和系统菜单创建图形化菜单元素。这基于 `FunEditor.gui` 类库。

The main menu for Mac OS X has added functionality. In that section, the FunEditor.os library tells the software platform. If it is Mac OS X, then perform the extra functions needed for the main menu.

Mac OS X 的主菜单还添加了一些更多的功能。在这部分代码中， `FunEditor.os` 类库可以获取到软件运行的平台，如果是 Mac OS X ，为主菜单添额外的功能。

## onChosenFileToOpen

## onChosenFileToOpen

This NW.js defined function gets the file selected when the Open file dialog gets closed. This function will then load that file into the editor using the FunEditor.readFileIntoEditor function.

`NW.js` 定义文件选中对话框关闭时的回调函数，这函数把文件加载到编辑器中，听过 `FunEditor.readFileIntoEditor` 这个函数。

## onChosenFileToSave

## onChosenFileToSave

This NW.js defined function gets the file selected when the Save As file dialog gets closed. This function will then save the editor contents to the given file using the FunEditor.writeEditorToFile function.

当文件对话框关闭获取到选择要存储到的文件时，会调用这个函数。这函数接着通过 `FunEditor.writeEditorToFile` 函数把编辑器的内容存储到指定的文件中。

## Onload

## Onload

This NW.js function is called whenever everything defined in the main.html file gets loaded into NW.js. It's equivalent to the document.onload = function(){}; or the jQuery $(document).ready(funcition(){});.

这个 `NW.js` 函数会在 `main.html` 中所有的资源被加载到 `NW.js` 中后调用。它相当与 `document.onload = function(){};` 或者 jQuery 的 `$(document).ready(function(){})`。

This function gets the editor ready with the desired preferences. The theme is solarized dark, the keyboard to vim layout, active line highlighting, etc. The editor keyboard shortcuts for copy, paste, cut, and save are set with their Windows and Mac defaults. The ace editor takes care of cross-platform issues.

这个函数按照既定的配置初始化编辑器。主题采用 `solarized dark`，键盘绑定为 `vim` 布局，高亮正在编辑的行，等等。编辑器复制、粘贴、剪切以及保存文件的快捷方式都按照 Windows 和 Mac 系统默认的方式进行设置。其他跨平台的问题 ace 编辑器会处理好。

Once the editor is initialized, listeners for the changeStatus, copy, paste, and save editor events are set up. These allow the editor to enforce the use of the system clipboard inside of the ace editor and update the line and column information in the status line. The changeStatus listener updates the vim mode and current cursor location.

当编辑器初始化好之后，绑定`changeStatus`、`copy` 、`paste`、`save` 等编辑器事件。这使得编辑器让 ace editor 使用系统的剪切板，还可以在状态栏中更新行列信息。

I also dug into the save function that Vim mode has (:w). This took some work, but I finally figured it out as well. Ace editor borrows code from Code Mirror to create the Vim keyboard layout. It works, but is not a robust solution.

我还碰到了一个 vim 保存功能（:w）的问题，花了点时间，最终还是搞定了。Ace 从 Code Mirror 上借取了部分代码来实现 Vim 键盘布局，可以工作，但并不是一个鲁棒的方案。

## Onresize

## Onresize

This NW.js defined function gets called whenever the window is resized. For the FunEditor, nothing needs to be done.

当窗口尺寸变化时，就会调用这个 `NW.js` 提供的方法。对于 FunEditor 来说，并不需要做什么。

## Running On Different Platforms

## 在不同的平台上运行

That's all the programming for the editor. Now, to run it on each platform. If you haven’t already downloaded the NW.js for each platform, do it now and follow the instructions for each platform. Take all the files in the project directory and compress them in to a zip archive. When done, change the name of the archive to FunEditor.nw.

这些就是全部编辑器代码。现在，让它在各个平台上运行。如果你还没下载 NW.js 针对每个平台的包，下载之，并根据下面的介绍操作。把项目文件夹的所有文件全部打包成一个`zip`文件，打包后，将文件名修改为 `FunEditor.nw`。

On every platform, you can always go to the command line and run the nw command in the project directory. Unfortunately, the basic install does not put the executable file in the system path. Therefore, depending on the shell you are using, you can create an nw alias to the executable file. While developing, I use this in the project directory to test:

对于每一个平台，你都可以在命令中，在项目文件夹下运行一遍 `nw` 命令。不过，普通的安装过程，并不会在系统路径中添加可执行的文件。因此，针对不同的shell，你可以 alias 一个 nw 的可执行文件。在开发的时候，我在项目目录中使用下面的命名来进行测试：

    nw .

Once I have the FunEditor.nw created, I use:

创建了 `FunEditor.nw` 之后，我使用：

    nw FunEditor.nw

Once verified working, I start the packaging for my other systems.

一旦测试通过，就可以开始为其他平台打包了。

### Mac

### Mac

Once NW.js gets installed on the Mac, you will have the nwjs.app file in your Applications directory. In the directory in which you compressed the application, you can run the program with the following command line:

一旦在 Mac 上安装了 `NW.js`，在 Applications 目录中可以找到 `nwjs.app` 文件。在进行压缩应用的目录中，你可以使用下面的命令运行程序：

    /Applications/nwjs.app/Contents/MacOS/nwjs FunEditor.nw

To make a clickable package, change the FunEditor.nw file to app.nw. Copy the /Applications/nwjs.app to /Applications/FunEditor.app. In Finder, right click on the application and select Show Contents. Place the app.nw file in the Resources directory and change the icon to one that you like. I include one in the download. Remember to keep the nw.icns file name.

为了制作一个可点击运行的包，将 `FunEditor.nw` 更名为 `app.nw`。复制 `Applications/nwjs.app` 到 `/Applications/FunEditor.app`， 在 Finder 中，右键点击应用，选择 `Show Contents`。将 `app.nw` 文件放入到 `Resources` 目录下，并将 icon 替换成随便什么你喜欢的。我在下载包中包含了一个图标，记得保留图标的文件名为 `nw.icns`。

To get the proper name in the menu, you will have to change the info.plist file. Open it up and change the contents to:

为了让菜单显示正确的名字，你要修改 `info.plist` 文件。将它打开，将内容修改为：

    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
        <key>BuildMachineOSBuild</key>
        <string>12F45</string>
        <key>CFBundleDevelopmentRegion</key>
        <string>en</string>
        <key>CFBundleDisplayName</key>
        <string>FunEditor</string>
        <key>CFBundleExecutable</key>
        <string>nwjs</string>
        <key>CFBundleIconFile</key>
        <string>nw.icns</string>
        <key>CFBundleIdentifier</key>
        <string>io.nwjs.nw</string>
        <key>CFBundleInfoDictionaryVersion</key>
        <string>6.0</string>
        <key>CFBundleName</key>
        <string>FunEditor</string>
        <key>CFBundlePackageType</key>
        <string>APPL</string>
        <key>CFBundleShortVersionString</key>
        <string>1.0</string>
        <key>CFBundleVersion</key>
        <string>1.0</string>
        <key>DTSDKBuild</key>
        <string>12F37</string>
        <key>DTSDKName</key>
        <string>macosx10.8</string>
        <key>DTXcode</key>
        <string>0511</string>
        <key>DTXcodeBuild</key>
        <string>5B1008</string>
        <key>LSFileQuarantineEnabled</key>
        <false/>
        <key>LSMinimumSystemVersion</key>
        <string>10.6.0</string>
        <key>NSPrincipalClass</key>
        <string>NSApplication</string>
        <key>NSSupportsAutomaticGraphicsSwitching</key>
        <true/>
        <key>SCMRevision</key>
        <string>df30fb73b312044486237d93cf96f3606862f2a3</string>
    </dict>
    </plist><br>

I took the info.plist file for NW.js, removed everything that is specific for it launching files with the nw extension, changed the version to 1.0, and changed the name to FunEditor.

这是我从 `NW.js` 中拿到的 `info.plist` 文件，移除了所有和启动 `nw` 扩展名文件相关的配置信息，将版本号设置为 `1.0`，将名字换成 `FunEditor`：

![FunEditor on Mac OS X](https://cms-assets.tutsplus.com/uploads/users/71/posts/23281/image/Cross-PlatformDevNodeWebkit-02.jpg)
FunEditor on Mac OS X

在 Mac OS X 上的 FunEditor

Once that is done, you have a clickable application for FunEditor.

搞定后，你就有一个可以点击打开的 FunEditor 了。

### Windows

### Windows

The easiest way to run the editor on Windows is to use a batch file. Create a file FunEditor.bat somewhere in your system path. Type the following into the file:

在 Windows 上运行编辑器最简单的方案就是使用批处理文件。在系统目录中创建一个 `FunEditor.bat` 文件。添加如下内容：

    <path to nw.exe>\nw.exe <path to FunEditor>\FunEdit.nw

![FunEditor on Windows 7](https://cms-assets.tutsplus.com/uploads/users/71/posts/23281/image/Cross-PlatformDevNodeWebkit-03.jpg)
FunEditor on Windows 7

Windows 7 上的 FunEditor

When you double click the batch file, the FunEditor will open.

当你双击批处理文件时，FunEditor 就打开了。

### Linux

### Linux

Once you have the NW.js program downloaded, make sure the directory with it is on your path. Create a script file that will call the NW.js program with your program. Create a file in your path called FunEditor with this:

下载 `NW.js` 程序，确保程序目录包含在你的系统目录中。创建一个脚本文件，通过的程序调用 `NW.js` 程序。在你的系统目中添加一个文件，通过下面的代码调用 FunEditor：

    #!/usr/bin/bash
    nwjs="<path to nwjs directory>/nwjs";
    fe="<path to the FunEditor.nw file>";

    $nwjs $fe

Save and set to executable permissions with:

保存文件，添加可运行的权限：

    chmod a+x FunEditor

If you run that command now, it should launch the editor for Linux!

运行该命令，它就在 Linux 上跑起来了！

![FunEditor on Arch Linux](https://cms-assets.tutsplus.com/uploads/users/71/posts/23281/image/Cross-PlatformDevNodeWebkit-01.jpg)

FunEditor on Arch Linux

在 Arch Linux 上的 FunEditor

On my Arch Linux, FunEditor is fun to use!

在我的 Arch Linux，FunEditor 用起来感觉不错！

## Conclusion

## 总结

With this project, you learned how to take advantage of NW.js to create an editor runnable on Windows, Linux, and Mac OS X. This project has much room for improvements. Have fun making this little editor into your dream editor and use it on every operating system!

通过这个项目，你学习到了如何利用 [NW.js](http://nwjs.io/)，创建一个可运行在 Windows、Linux 和 Mac OS X 的编辑器。这个编辑器还有很多提升的空间。你大可以按照自己的兴趣将它提升成你梦想中的编辑器，在每个平台都使用它写程序！

原文：http://code.tutsplus.com/tutorials/cross-platform-development-with-nwjs--cms-23281
