### 开搞 React, Redux and Immutable: 一个TDD方法教程 (Part 1)

几星期前我在逛hacker news的时候，都到一篇关于redux的，貌似又是一个可以和React搭配的很好的东东。我已经得了JS Fatigue，所以我留了个心，直到我注意到以下特性

- 他强制了FP还确保了应用的行为的可预测性
- 他允许了isomorphic应用，就是把逻辑在客户和服务端共享
- 一个时间穿越debugger，这他妈也可以？

看起来一个优雅的管理React应用的方式，再然后谁会对时光穿越说不？于是我入了，我读了实力应用以及@teropa的那篇[http://teropa.info/blog/2015/09/10/full-stack-redux-tutorial.html](A Comprehensive Guide to Test-First Development with Redux, React, and Immutable)。

Github地址在[here](https://github.com/phacks/redux-todomvc)

### 应用

为了这个教学原因，我们就做一个经典的TodoMVC应用。需求这样

- 每一个todo可以背设置成active活着completed
- todo可以背增加，编辑活着删除
- 可以用status来过滤
- 需要一个计数器
- 可以同时删除所有和一部分

### Redux & immutable FP来救场！

几个月前，我在开发一个包含dashboard的应用。当这个应用变大了，我们发现越来越多的恶心的bug。“例如你走到这个page点这个按钮再回去再搞基把搞就出现的”这些问题要么是我们的代码的副作用或者逻辑：一个action可以有一个不被期待的作用在某一处。

这就是Redux牛逼的地方：整个应用的状态被包含在一个状态树的数据结构，这就代表着任何时候，显示的UI就是我们的状态树的结果。我们每一个步骤，作出相应的变化，输出更新的状态树，然后就被渲染给用户。没有副作用，没有更多的关联比如某个变量不经意被改变了。这就让我们需要关心的东西变清晰了，一个更好的应用可以让我们更好的Debug。

immutable是一个脸书开发的工具库，可以创建和调教不可变数据结构。尽管他没有被强制这样使用，它可以加强我们的函数式做法，通过禁止对象修改的方法。有了它当我们想要更新一个对象，我们就创建一个另外一个修改过的对象。
```js
var map1 = Immutable.Map({a:1, b:2, c:3});
var map2 = map1.set('b', 2);
assert(map1 === map2); // no change
var map3 = map1.set('b', 50);
assert(map1 !== map3); // change
```

当我们更新了map1的值，map1对象保持一样，

Immutable将会背运用到来储存我们的值

### 设置项目

```
mkdir redux-todomvc
cd redux-todomvc
npm init -y

```
项目结构大概这样

```
├── dist
│   ├── bundle.js
│   └── index.html
├── node_modules
├── package.json
├── src
├── test
└── webpack.config.js

```
首先我们先写一个HTML页面

`dist/index.html`

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>React TodoMVC</title>
</head>
<body>
  <div id="app"></div>
  <script src="bundle.js"></script>
</body>
</html>
```

译序我们来写一个脚本来确定一切都O了

`src/index.js`

`console.log('Hello World!')`

我们将要建立这个bundle.js用Webpack。

```
npm install --save-dev webpack webpack-dev-server
```

我们要用Babel来转译ES5的语法到CommonJS

```
npm install --save-dev babel-core babel-loader babel-preset-es2015

```

我们将要用jsx来写我们的react组件，所以我们来安装吧！
```
npm install --save-dev babel-preset-react

```

我们要来配置了！

`package.json`

```
"babel": {
  "presets": ["es2015", "react"]
}

```

为了开动hot loading，有一些修改需要改

`webpack.config.js`

```
var webpack = require('webpack'); // Requiring the webpack lib

module.exports = {
  entry: [
    'webpack-dev-server/client?http://localhost:8080', // Setting the URL for the hot reload
    'webpack/hot/only-dev-server', // Reload only the dev server
    './src/index.js'
  ],
  module: {
    loaders: [{
      test: /\.jsx?$/,
      exclude: /node_modules/,
      loader: 'react-hot!babel' // Include the react-hot loader
    }]
  },
  resolve: {
    extensions: ['', '.js', '.jsx']
  },
  output: {
    path: __dirname + '/dist',
    publicPath: '/',
    filename: 'bundle.js'
  },
  devServer: {
    contentBase: './dist',
    hot: true // Activate hot loading
  },
  plugins: [
    new webpack.HotModuleReplacementPlugin() // Wire in the hot loading plugin
  ]
};

```

设置单元测试框架
我们将会用Mocha和chai作为我们的测试框架。他们被广泛应用。这些输出（一个diff对比期待和实际）很适合我们做TDD。Chai-Immutable是一个chai插件来处理不可变的数据结构

```
npm install --save immutable
npm install --save-dev mocha chai chai-immutable
```

在我们这个情况我们不需要依赖一个browser-based的测试runner，例如Karma，相反我们用jsdom，这个东东会给我们设置一个DOM mock以纯js的方式，还允许我们跑测试快快的。

```
npm install -save-dev jsdom
```
我们还需要写一个bootstrapping脚本给我们的测试来搞定这些
- Mock document和window对象，一般由浏览器提供
- 告诉Chai我们要以`chai-immutable`的方式用immutable数据结构

`test/setup.js`

```
import jsdom from 'jsdom';
import chai from 'chai';
import chaiImmutable from 'chai-immutable';

const doc = jsdom.jsdom('<!doctype html><html><body></body></html>');
const win = doc.defaultView;

global.document = doc;
global.window = win;

Object.keys(window).forEach((key) => {
  if (!(key in global)) {
    global[key] = window[key];
  }
});

chai.use(chaiImmutable);

```

让我们更新一下`npm test`
`package.json`

```
"scripts": {
  "test": "mocha --compilers js:babel-core/register --require ./test/setup.js 'test/**/*.@(js|jsx)'",
  "test:watch": "npm run test -- --watch --watch-extensions jsx"
},
```

更新： 似乎`npm run test:watch`在windows上用不了，如果是的话请看[这个](https://github.com/phacks/redux-todomvc/issues/1)

现在如果我们运行`npm run test:watch`，所有的.js .jsx文件都会被当作mocha测试每次我们更新他们的时候都会被抛一边。

设置完毕，我们可以跑`webpack--dev`在一个终端，另外一个`npm run test:watch`来到`localhost:8080`看看helloworld！

### 建造一个状态树

像我之前说的一样，状态树数据结构拿着我们整个应用的状态，这个结构需要足够好，在我们开发之前就搞得妥妥的。因为它将会影响到很多代码结构和交互。

作为一个例子，我们的应用是由很多的items在一个todolist里组成的。
### 图一
每一个都有text，为了更好的操作，有一个id，更重要的是，每一个item里有两种状态，积极的和完成的：最后，一个item可以在一个被编辑的状态，（当用户想要编辑文本），这样我们就可以很好的跟踪它们了。
### 图一
我们也可以通过他们的状态来过滤这些item，这样我们就可以加一个`filter`入口来获得最终的状态树。

### 图一

### 写UI
### 图一

首先我们将会把这个应用拆分成component

- TodoHeader component
- todolist
- TodoItem
- TextInput
- TodoTools
- Footer

我们还将创建一个`TodoApp`组件来包住她们。

### 开搞我们第一个Component
注意： [这里](https://github.com/phacks/redux-todomvc/commit/d1d2a56a8d2b4f898ed8fdf20f55e7f7f11ad6ad)是我们的第一个commit。

我们可以看到，我们将会把我们所有的components放到一个TodoApp里面，

`src/index.jsx`

```js
import React from 'react';
import ReactDOM from 'react-dom';
import {List, Map} from 'immutable';

import TodoApp from './components/TodoApp';

const todos = List.of(
  Map({id: 1, text: 'React', status: 'active', editing: false}),
  Map({id: 2, text: 'Redux', status: 'active', editing: false}),
  Map({id: 3, text: 'Immutable', status: 'completed', editing: false})
);

ReactDOM.render(
  <TodoApp todos={todos} />,
  document.getElementById('app')
);

```

因为我们用了jsx语法在我们的index.js文件，我们需要改变他的扩展为.jsx，以及在webpack配置文件里改变。
`webpack.config.js`

```
entry: [
  'webpack-dev-server/client?http://localhost:8080',
  'webpack/hot/only-dev-server',
  './src/index.jsx' // Change the index file extension
],

```

### 写todo list 的UI

现在我们准备写TodoApp组件的第一个版本组件，来战是我们的todo items list。

`src/components/TodoApp.jsx`

```js
import React from 'react';

export default class TodoApp extends React.Component {
  getItems() {
    return this.props.todos || [];
  }
  render() {
    return <div>
      <section className="todoapp">
        <section className="main">
          <ul className="todo-list">
            {this.getItems().map(item =>
              <li className="active" key={item.get('text')}>
                <div className="view">
                  <input type="checkbox"
                         className="toggle" />
                  <label htmlFor="todo">
                    {item.get('text')}
                  </label>
                  <button className="destroy"></button>
                </div>
              </li>
            )}
          </ul>
        </section>
      </section>
    </div>
  }
};

```

两件事充斥了我的大脑。

第一，如果你看看浏览器的结果，她们看起来就像一坨。为了修补这个，我们准备用todomvc-app-css包裹，来搞定。

```
npm install --save todomvc-app-css
npm install style-loader css-loader --save-dev

```

我们需要告诉webpack来加载css。

`webpack.config.js`

```
// ...
module: {
  loaders: [{
    test: /\.jsx?$/,
    exclude: /node_modules/,
    loader: 'react-hot!babel'
  }, {
    test: /\.css$/,
    loader: 'style!css' // We add the css loader
  }]
},
//...

```

然后我们就把style包含爱我们的index.jsx中
`src/index.jsx`

```js
// ...
require('../node_modules/todomvc-app-css/index.css');

ReactDOM.render(
  <TodoApp todos={todos} />,
  document.getElementById('app')
);

```

第二件事，这代码看上去有点麻烦。这就是为什么我们需要创建两个或者更多的components：TodoList和TodoItem，她们会照顾好事情的！

`src/components/TodoApp.jsx`

```js
import React from 'react';
import TodoList from './TodoList'

export default class TodoApp extends React.Component {
  render() {
    return <div>
      <section className="todoapp">
        <TodoList todos={this.props.todos} />
      </section>
    </div>
  }
};

```

TodoList component将会显示一个TodoItem component给他接受的props的每一个item。

`src/components/TodoList.jsx`

```js
import React from 'react';
import TodoItem from './TodoItem';

export default class TodoList extends React.Component {
  render() {
    return <section className="main">
      <ul className="todo-list">
        {this.props.todos.map(item =>
          <TodoItem key={item.get('text')}
                    text={item.get('text')} />
        )}
      </ul>
    </section>
  }
};

```

`src/components/TodoItem.jsx`

```js
import React from 'react';

export default class TodoItem extends React.Component {
  render() {
    return <li className="todo">
      <div className="view">
        <input type="checkbox"
               className="toggle" />
        <label htmlFor="todo">
          {this.props.text}
        </label>
        <button className="destroy"></button>
      </div>
    </li>
  }
};
```

在我们更加深入可能的用户行为以及我们如何将他们集成到应用中之前，我们献给TodoItem加一个input。

`src/components/TodoItem.jsx`

```js
import React from 'react';

import TextInput from './TextInput';

export default class TodoItem extends React.Component {

  render() {
    return <li className="todo">
      <div className="view">
        <input type="checkbox"
               className="toggle" />
        <label htmlFor="todo">
          {this.props.text}
        </label>
        <button className="destroy"></button>
      </div>
      <TextInput /> // We add the TextInput component
    </li>
  }
};
```

这个TextInput component可以被写成以下这样的

`src/components/TextInput.jsx`

```js
import React from 'react';

export default class TextInput extends React.Component {
  render() {
    return <input className="edit"
                  autoFocus={true}
                  type="text" />
  }
};
```

### 纯组件的福利：PureRenderMixin

出了允许函数式编程风格。我们的UI单纯地依靠props允许我们使用`PureRenderMixin`来达到一个性能的提升，就像React文档说的：

> 如果你的React component的render是纯净的（换言之，给他相同的输入他会给你相同的输出），你就可以用Mixin来给性能提升。

加入到我们的子组件很简单，就像React的文档所说的。（我们将会在第二篇看到TodoApp component 有一些其他的功能来防止PureRenderMixin的使用）：

`npm install --save react-addons-pure-render-mixin`

`src/components/TodoList.jsx`


```js
import React from 'react';
import PureRenderMixin from 'react-addons-pure-render-mixin'
import TodoItem from './TodoItem';

export default class TodoList extends React.Component {
  constructor(props) {
    super(props);
    this.shouldComponentUpdate = PureRenderMixin.shouldComponentUpdate.bind(this);
  }
  render() {
    // ...
  }
};
```

`src/components/TodoItem.jsx`

```js
import React from 'react';
import PureRenderMixin from 'react-addons-pure-render-mixin'
import TextInput from './TextInput';

export default class TodoItem extends React.Component {
  constructor(props) {
    super(props);
    this.shouldComponentUpdate = PureRenderMixin.shouldComponentUpdate.bind(this);
  }
  render() {
    // ...
  }
};
```

`src/components/TextInput.jsx`

```js

import React from 'react';
import PureRenderMixin from 'react-addons-pure-render-mixin'

export default class TextInput extends React.Component {
  constructor(props) {
    super(props);
    this.shouldComponentUpdate = PureRenderMixin.shouldComponentUpdate.bind(this);
  }
  render() {
    // ...
  }
};
```

### 在list components里处理用户动作

好了，我们现在在list components里有了UI。然而，

## props的能力

在React里，在我们初始化一个container的时候设置的属性的时候传入props对象。举例说，如果我们初始化`TodoItem`元素：

`<TodoItem text={'Text of the item'} />`

然后我们就可以在TodoItem component里访问`this.props.text`变量：

```js
// in TodoItem.jsx
console.log(this.props.text);
// outputs 'Text of the item'
```

Redux架构用了大量的props。基本的原则是几乎每个元素的state都应该活在props里。换言之，给相同的 props，两个元素的实例应该输出同样的结果。就像我们之前看过的，整个应用的状态应该被包含在状态树：这意味着被从上往下在组件中通过props传递的状态树将会整个并预言着整个应用的长相的变化。

### TodoList Component

在这一劫我们将会来尝尝TDD的方式。

味了帮助我们来测试components，React提供了TestUtils插件，包含很多

- renderIntoDocument 渲染一个组件到一个单独的DOM节点中。
- scryRenderedDOMComponentsWithTag 找到所有在DOM的有类似li input标记的components。
- scryRenderedDOMComponentsWithClass 同上，换成Class
- Simulate 模拟用户行为

这个不包括在React之中 我们需要分开安装：

`npm install --save-dev react-addons-test-utils`

我们第一个test将会确定如果给我们了filter这个属性active，TodoList组件正确的展示内容。

`test/Components/TodoList_spec.jsx`

```js
import React from 'react';
import TestUtils from 'react-addons-test-utils';
import TodoList from '../../src/components/TodoList';
import {expect} from 'chai';
import {List, Map} from 'immutable';

const {renderIntoDocument,
       scryRenderedDOMComponentsWithTag} = TestUtils;

describe('TodoList', () => {
  it('renders a list with only the active items if the filter is active', () => {
    const todos = List.of(
      Map({id: 1, text: 'React', status: 'active'}),
      Map({id: 2, text: 'Redux', status: 'active'}),
      Map({id: 3, text: 'Immutable', status: 'completed'})
    );
    const filter = 'active';
    const component = renderIntoDocument(
      <TodoList filter={filter} todos={todos} />
    );
    const items = scryRenderedDOMComponentsWithTag(component, 'li');

    expect(items.length).to.equal(2);
    expect(items[0].textContent).to.contain('React');
    expect(items[1].textContent).to.contain('Redux');
  });
});

```

然后我们可以看到有个测试挂掉了，这很正常，我们还没做呢

`src/components/TodoList.jsx`

```js
// ...
export default class TodoList extends React.Component {
  // Filters the items according to their status
  getItems() {
    if (this.props.todos) {
      return this.props.todos.filter(
        (item) => item.get('status') === this.props.filter
      );
    }
    return [];
  }
  render() {
    return <section className="main">
      <ul className="todo-list">
        // Only the filtered items are displayed
        {this.getItems().map(item =>
          <TodoItem key={item.get('text')}
                    text={item.get('text')} />
        )}
      </ul>
    </section>
  }
};

```

我们拿到了第一滴血！现在我们不要停下来，继续继续！

`test/components/TodoList_spec.js`

```js
// ...
describe('TodoList', () => {
  // ...
  it('renders a list with only completed items if the filter is completed', () => {
    const todos = List.of(
      Map({id: 1, text: 'React', status: 'active'}),
      Map({id: 2, text: 'Redux', status: 'active'}),
      Map({id: 3, text: 'Immutable', status: 'completed'})
    );
    const filter = 'completed';
    const component = renderIntoDocument(
      <TodoList filter={filter} todos={todos} />
    );
    const items = scryRenderedDOMComponentsWithTag(component, 'li');

    expect(items.length).to.equal(1);
    expect(items[0].textContent).to.contain('Immutable');
  });

  it('renders a list with all the items', () => {
    const todos = List.of(
      Map({id: 1, text: 'React', status: 'active'}),
      Map({id: 2, text: 'Redux', status: 'active'}),
      Map({id: 3, text: 'Immutable', status: 'completed'})
    );
    const filter = 'all';
    const component = renderIntoDocument(
      <TodoList filter={filter} todos={todos} />
    );
    const items = scryRenderedDOMComponentsWithTag(component, 'li');

    expect(items.length).to.equal(3);
    expect(items[0].textContent).to.contain('React');
    expect(items[1].textContent).to.contain('Redux');
    expect(items[2].textContent).to.contain('Immutable');
  });
});

```

第三个测试失败了，因为all的逻辑稍微有点不同。

`src/components/TodoList.jsx`

```js
// ...
export default React.Component {
  // Filters the items according to their status
  getItems() {
    if (this.props.todos) {
      return this.props.todos.filter(
        (item) => this.props.filter === 'all' || item.get('status') === this.props.filter
      );
    }
    return [];
  }
  // ...
});
```

这下我们知道了这些items应该根据filter来被显示在这个应用。真的，我们需要看看这个应用啦！打开浏览器！我们可以看到没有items被显示，因为我们还没设置呢。

`src/index.jsx`

```js
// ...
const todos = List.of(
  Map({id: 1, text: 'React', status: 'active', editing: false}),
  Map({id: 2, text: 'Redux', status: 'active', editing: false}),
  Map({id: 3, text: 'Immutable', status: 'completed', editing: false})
);

const filter = 'all';

require('../node_modules/todomvc-app-css/index.css')

ReactDOM.render(
  <TodoApp todos={todos} filter = {filter}/>,
  document.getElementById('app')
);

```

`src/components/TodoApp.jsx`

```js
// ...
export default class TodoApp extends React.Component {
  render() {
    return <div>
      <section className="todoapp">
        // We pass the filter props down to the TodoList component
        <TodoList todos={this.props.todos} filter={this.props.filter}/>
      </section>
    </div>
  }
};
```

我们的items现在重新出现了，而且被filter了。

### TodoItem Component

现在我们来照顾TodoItem组件。首先，我们希望确保TodoItem组件真的渲染了一个item。我们还需要确保一个还未实现的功能，当一个item完成后，被杠掉了。

`test/components/TodoItem_spec.js`

```js
import React from 'react';
import TestUtils from 'react-addons-test-utils';
import TodoItem from '../../src/components/TodoItem';
import {expect} from 'chai';

const {renderIntoDocument,
       scryRenderedDOMComponentsWithTag} = TestUtils;

describe('TodoItem', () => {
  it('renders an item', () => {
    const text = 'React';
    const component = renderIntoDocument(
      <TodoItem text={text} />
    );
    const todo = scryRenderedDOMComponentsWithTag(component, 'li');

    expect(todo.length).to.equal(1);
    expect(todo[0].textContent).to.contain('React');
  });

  it('strikes through the item if it is completed', () => {
    const text = 'React';
    const component = renderIntoDocument(
      <TodoItem text={text} isCompleted={true}/>
    );
    const todo = scryRenderedDOMComponentsWithTag(component, 'li');

    expect(todo[0].classList.contains('completed')).to.equal(true);
  });
});
```

为了让第二个测试通过，我们需要给这个item一个completed类如果是的话。我们将会用classnames这个包来管理DOM classes。

`npm install --save classnames`

`src/components/TodoItem.jsx`

```js
import React from 'react';
// We need to import the classNames object
import classNames from 'classnames';

import TextInput from './TextInput';

export default class TodoItem extends React.Component {
  render() {
    var itemClass = classNames({
      'todo': true,
      'completed': this.props.isCompleted
    });
    return <li className={itemClass}>
      // ...
    </li>
  }
};
```

当一个item被编辑的时候他应该有一个格外的造型，事实上他被包含在了isEditing属性。
`test/components/TodoItem_spec.js`

```js
// ...
describe('TodoItem', () => {
  //...

  it('should look different when editing', () => {
    const text = 'React';
    const component = renderIntoDocument(
      <TodoItem text={text} isEditing={true}/>
    );
    const todo = scryRenderedDOMComponentsWithTag(component, 'li');

    expect(todo[0].classList.contains('editing')).to.equal(true);
  });
});
```

为了让测试通过，我们只需要更新itemClass对象：

`src/components/TodoItem.jsx`

```js
// ...
export default class TodoItem extends React.Component {
  render() {
    var itemClass = classNames({
      'todo': true,
      'completed': this.props.isCompleted,
      'editing': this.props.isEditing
    });
    return <li className={itemClass}>
      // ...
    </li>
  }
};

```

如果这个item完成的话，左边的checkbox应该被选中：
`test/components/TodoItem_spec.js`

```js
// ...
describe('TodoItem', () => {
  //...

  it('should be checked if the item is completed', () => {
    const text = 'React';
    const text2 = 'Redux';
    const component = renderIntoDocument(
      <TodoItem text={text} isCompleted={true}/>,
      <TodoItem text={text2} isCompleted={false}/>
    );
    const input = scryRenderedDOMComponentsWithTag(component, 'input');
    expect(input[0].checked).to.equal(true);
    expect(input[1].checked).to.equal(false);
  });
});

```

React有一个设置checkbox的方法，就是defaultChecked。

`src/comopnents/TodoItem.jsx`

```js
// ...
export default class TodoItem extends React.Component {
  render() {
    // ...
    return <li className={itemClass}>
      <div className="view">
        <input type="checkbox"
               className="toggle"
               defaultChecked={this.props.isCompleted}/>
        // ...
    </li>
  }
};
```

我们还要从TodoList组件传下来isCompleted, isEditing属性。

`src/components/TodoList.jsx`

```js
// ...
export default class TodoList extends React.Component {
  // ...
  // This function checks whether an item is completed
  isCompleted(item) {
    return item.get('status') === 'completed';
  }
  render() {
    return <section className="main">
      <ul className="todo-list">
        {this.getItems().map(item =>
          <TodoItem key={item.get('text')}
                    text={item.get('text')}
                    // We pass down the info on completion and editing
                    isCompleted={this.isCompleted(item)}
                    isEditing={item.get('editing')} />
        )}
      </ul>
    </section>
  }
};
```
现在，我们需要在我们的组件里反映出我们的应用的state：例如，一个完成了的item将会被杠掉。然而，一个web app还会处理用户的事件，例如点击。在Redux模型，这也是用Props来处理的，而且更具体的，把callback用props来传递。这么做我们可以把UI和我们的逻辑分离：这个组件不需要知道点击或者其他的事情发生的时候，我们该做啥。

味了演示这个原则，我们将会测试如果用户点击删除按钮delteItem会被调用：

`test/components/TodoItem_spec.jsx`

```js
// ...
// The Simulate helper allows us to simulate a user clicking
const {renderIntoDocument,
       scryRenderedDOMComponentsWithTag,
       Simulate} = TestUtils;

describe('TodoItem', () => {
  // ...
  it('invokes callback when the delete button is clicked', () => {
    const text = 'React';
    var deleted = false;
    // We define a mock deleteItem function
    const deleteItem = () => deleted = true;
    const component = renderIntoDocument(
      <TodoItem text={text} deleteItem={deleteItem}/>
    );
    const buttons = scryRenderedDOMComponentsWithTag(component, 'button');
    Simulate.click(buttons[0]);

    // We verify that the deleteItem function has been called
    expect(deleted).to.equal(true);
  });
});

```
为了让测试通过，我们必须讲onclick生命在button上，告诉他deleteItem要被调用。

`src/components/TodoItem.jsx`
```js
// ...
export default class TodoItem extends React.Component {
  render() {
    // ...
    return <li className={itemClass}>
      <div className="view">
        // ...
        // The onClick handler will call the deleteItem function given in the props
        <button className="destroy"
                onClick={() => this.props.deleteItem(this.props.id)}></button>
      </div>
      <TextInput />
    </li>
  }
};
```


还有一点很重要，就是我们实际的逻辑还没做呢，这个交给Redux。

在同样的模型里，我们可以测试和实现以下功能

- 当点击checkbox我们要调用toggleComplete
- 当双击item label的时候调用editItem

`test/components/TodoItem_spec.js`
```js
// ...
describe('TodoItem', () => {
  // ...
  it('invokes callback when checkbox is clicked', () => {
    const text = 'React';
    var isChecked = false;
    const toggleComplete = () => isChecked = true;
    const component = renderIntoDocument(
      <TodoItem text={text} toggleComplete={toggleComplete}/>
    );
    const checkboxes = scryRenderedDOMComponentsWithTag(component, 'input');
    Simulate.click(checkboxes[0]);

    expect(isChecked).to.equal(true);
  });

  it('calls a callback when text is double clicked', () => {
    var text = 'React';
    const editItem = () => text = 'Redux';
    const component = renderIntoDocument(
      <TodoItem text={text} editItem={editItem}/>
    );
    const label = component.refs.text
    Simulate.doubleClick(label);

    expect(text).to.equal('Redux');
  });
});
```

`src/components/TodoItem.jsx`

```js
// ...
render() {
  // ...
  return <li className={itemClass}>
    <div className="view">
      // We add an onClick handler on the checkbox
      <input type="checkbox"
             className="toggle"
             defaultChecked={this.isCompleted()}
             onClick={() => this.props.toggleComplete(this.props.id)}/>
      // We add a ref attribute to the label to facilitate the testing
      // The onDoubleClick handler is unsurprisingly called on double clicks
      <label htmlFor="todo"
             ref="text"
             onDoubleClick={() => this.props.editItem(this.props.id)}>
        {this.props.text}
      </label>
      <button className="destroy"
              onClick={() => this.props.deleteItem(this.props.id)}></button>
    </div>
    <TextInput />
  </li>
  }
```

我们还必须从TodoList Component向下传editItem, deleteItem, toggleComplete函数。

```js
src/components/TodoList.jsx

// ...
export default class TodoList extends React.Component {
  // ...
  render() {
      return <section className="main">
        <ul className="todo-list">
          {this.getItems().map(item =>
            <TodoItem key={item.get('text')}
                      text={item.get('text')}
                      isCompleted={this.isCompleted(item)}
                      isEditing={item.get('editing')}
                      // We pass down the callback functions
                      toggleComplete={this.props.toggleComplete}
                      deleteItem={this.props.deleteItem}
                      editItem={this.props.editItem}/>
          )}
        </ul>
      </section>
    }
};

```

### 设置其他components

现在你应该熟悉我们的套路了。你会发现有一些类似`editItem`, `toggleComplete`还没有实现，我们下次再战，作为Redux行为。

### 概括

这篇我们铺好了路，我们的UI超级模块化，这些dumb components，不知道该做具体啥的玩意该如何帮助我们写一个可以穿越时间的应用？

等我们的下一篇吧～
