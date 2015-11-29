# PostHTML 入门教程

看到 [PostHTML](https://github.com/posthtml/posthtml)，自然就会想起 [PostCSS](https://github.com/postcss/postcss)，后者作为新一代的 CSS 处理器，一扫 SASS LESS 的威风，在社区中刮起了一阵旋风。虽然 PostHTML 后起，也不知道能否秀起来，先带着大家揭开 PostHTML 的神秘面纱。

## 牛刀小试

PostHTML 能干什么呢？先给大家上个小例子。如下一个 HTML 文档：

```html
<html>
<body>
    <article class="my-article">
        <h1>Hello "world"...</h1>
        <p>The three wise monkeys [. . .] sometimes called the three mystic
        apes--are a pictorial maxim. Together they embody the proverbial
        principle to ("see no evil, hear no evil, speak no evil"). The
        three monkeys are Mizaru (:see_no_evil:), covering his eyes, who
        sees no evil; Kikazaru (:hear_no_evil:), covering his ears, who
        hears no evil; and Iwazaru (:speak_no_evil:), covering his mouth,
        who speaks no evil.</p>
    </article>
</body>
</html>
```

在段落中包含了一些 emoji 表情的文本表示，比如 `:speak_no_evil:`等。如何将其换成可在网页上直接显示的表情呢？我们可以借助 PostHTML 的插件 [PostHTML-Retext](https://github.com/voischev/posthtml-retext) 来实现：

```javascript
var fs = require('fs'),
    posthtml = require('posthtml'),
    html = fs.readFileSync('path/to/file.html');

posthtml()
    .use(require('posthtml-retext')([
        [require('retext-emoji'), { convert: 'encode' }], // Array if plugin has options
        require('retext-smartypants')
    ]))
    .process(html)
    .then(function(result) {
        fs.writeFileSync('path/to/file.html');
    })
```

运行上面的代码，即可把原来的 HTML 替换成如下的样子：

```html
<html>
<body>
    <article class="my-article">
        <h1>Hello “world”…</h1>
        <p>The three wise monkeys […] sometimes called the three mystic
        apes—are a pictorial maxim. Together they embody the proverbial
        principle to (“see no evil, hear no evil, speak no evil”). The
        three monkeys are Mizaru (🙈), covering his eyes, who
        sees no evil; Kikazaru (🙉), covering his ears, who
        hears no evil; and Iwazaru (🙊), covering his mouth,
        who speaks no evil.</p>
    </article>
</body>
</html>
```

大家现在应该明白 PostHTML 的功能了吧——PostHTML 即为 HTML 的处理器，输入 HTML，经过一系列的修改，输出新的 HTML。

## PostHTML 的特点

客官可能会问，这种替换的功能，我用正则表达式也能做呀，那 PostHTML 的优势是什么呀？

PostHTML 之于 HTML，就像 PostCSS 之于 CSS。

> Uglify 之于 JavaScript，除了插件体系并不成熟以外。

PostHTML 好比一个汽车翻新工厂，而 PostHTML 插件就是一个个流水线车间：

1. PostHTML 将 HTML 文档按照 DOM 模型分解为一个个 node（JavaScript 对象），加上这些 node 的父子关系，形成 PostHTMLTree；
2. PostHTML 插件获得用 JavaScript 表示的 PostHTMLTree 对象，修改、更新或者移除树上的节点，实现特定功能；
3. 最后 PostHTML 再把新的 PostHTMLTree 对象转换成 HTML 文档。

可见，PostHTML 并不提供具体的功能，仅仅实现了 HTML 和 PostHTMLTree 互相转化，且提供通用的 API 和 插件模型，让插件操作 PostHTMLTree。这与 PostCSS 如出一辙。具备如下的有点：

- **JavaScript only**：使用 JavaScript 实现，是每一个前端的梦想；
- **模块化**：你可以按照需求，将插件（功能）组合起来使用；
- **轻量**：按需添加，避免引入大量并不使用的特性；
- **快速扩展**：在需求无法满足的时候，PostHTML 提供了便利的方式来扩展功能；
- **鲁棒性**：按照 DOM 语法，将 HTML 转换为 AST，比起正则匹配来说有更高的准确性、更细的粒度以及更强的控制力；
**可编程**：将 HTML 转换为用 JS 对象表示的 AST，可以很方面的使用 JS 来修改，易于编程。

## 如何使用 PostHTML？

### 安装 PostHTML

```bash
npm install --save-dev posthtml
```

### API

**posthtml([Array(plugin)])**：

调用 `posthtml` 装载插件，获取 `PostHTML` 实例；

**posthtml().use(Array(plugin))**：

也可以通过 `PostHTML` 实例的 `use` 方法来装载插件；

**posthtml().use(…).process(html[, options])**：

插件装载好之后，就可以使用 `process` 来处理 HTML 文档了。

### 实例

下面是来自官方的例子。

```javascript
var posthtml = require('posthtml');

var html = '<myComponent><myTitle>Super Title</myTitle><myText>Awesome Text</myText></myComponent>';

posthtml()
    .use(require('posthtml-custom-elements')())
    .process(html/*, options */)
    .then(function(result) {
        console.log(result.html);
        // <div class="myComponent"><div class="myTitle">Super Title</div><div class="myText">Awesome Text</div></div>
    });
```

通过 `use` 方法装载了 `posthtml-custom-elements` 插件，就可以将自定义组件 `myComponent` 转换为 `<div class="myComponent">`。

#### gulp 插件

posthtml 也可以在 gulp 中使用。安装 gulp 插件 `gulp-posthtml`：

```bash
npm install --save-dev gulp-posthtml
```

使用：

```javascript
gulp.task('html', function() {
    var posthtml = require('gulp-posthtml');
    return gulp.src('src/**/*.html')
        .pipe(posthtml([ require('posthtml-custom-elements')() ]/*, options */))
        .pipe(gulp.dest('build/'));
});
```

## 如何编写 PostHTML 插件？

PostHTML 的插件本质上就是一个函数，就接受一个参数，即 PostHTML 工厂处理好的 `PostHTMLTree`，插件要做的就是对这个 `tree` 进行修改。

PostHTML 提供了两个方法来遍历 `tree`：

- **.match(matcher, callback)**：matcher 可以是对象、字符串或者正则表达式，`match` 方法会遍历整个 tree，寻找与 matcher 匹配的 node，然后将 node 传递给 callback，我们可以在 callback 中修改 node 实现我们想要的功能；
- **.walk(callback)**：比起 match，walk 是一个更加通用的方法，遍历 tree 中的每一个节点，传递给 callback。

下面是一个简单的插件：

```javascript
module.exports = function pluginName(tree) {
    // do something for tree
    tree.match({ tag: 'img' }, function(node) {
        node = Object.assign(node, { attrs: { class: 'img-wrapped' } }});
        return {
            tag: 'span',
            attrs: { class: 'img-wrapper' },
            content: node
        }
    });
};
```

这个插件会对影响 HTML 中的 img 标记产生影响，例如，有段 HTML 为:

```html
<img src='path/to/img'/>
```

使用了这个插件后：

```html
<span class="img-wrapper"><img class="img-wrapped" src='path/to/img'/></span>
```

很简单不是，是时候发挥你的聪明才智了，动手写一个插件来满足自己的需求吧！

> **提示**：在使用 `match` 或者 `walk` 方法遍历 tree 时，如果 callback 没有返回值的话，当前的 node 就相当于本删除了。也就是说，如果你想移除某些节点，那只需在 callback 不返回值或者返回 `undefined` 即可。暂时不能返回 `null`，报错，为此我提交了一个 [PR](https://github.com/posthtml/posthtml/pull/73)。

## 如何与模板引擎结合使用？

PostHTML 作为 HTML 处理器，自然可以与 jade 或者 ejs 之类的模板引擎结合使用。jade 和 ejs 在处理业务逻辑，拼装业务数据方面有自己的优势，
而 PostHTML 可以更多地用来处理这些模板引擎产生的 HTML，比如压缩 HTML 代码等等。

下面这个例子来自 PostHTML 上的一个 [PR](https://github.com/posthtml/posthtml/pull/71/files)：

```javascript
app.engine('jade', function (path, options, callback) {
    // PostHTML plugins
    var plugins = [
        require('posthtml-bem')(),                  
        require('posthtml-textr')({ locale: 'ru'}, [
            require('typographic-ellipses'), 
            require('typographic-single-spaces'),
            require('typographic-quotes')
        ])
    ];

    var html = require('jade').renderFile(path, options); 

    posthtml(plugins)
        .process(html)
        .then(function (result) {
            if (typeof callback === 'function') {
                var res;
                try {
                    res = result.html;
                } catch (ex) {
                    return callback(ex);
                }
                return callback(null, res);
            }
        });
})
app.set('view engine', 'jade');
```

## 后记

PostHTML 初出茅庐，在 Github star 也才 214，但相信不久的将来PostHTML 也会像 PostCSS 一样横扫整个 HTML 处理的世界。我也着手编写一个 PostHTML 插件 [posthtml-web-component](https://github.com/island205/posthtml-web-component)，用来实现服务器端的动态化组件。欢迎关注。








