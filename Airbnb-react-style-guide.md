# Airbnb React/JSX Style Guide

*A mostly reasonable approach to React and JSX*
*用更合理的方式书写React和JSX*

## Table of Contents
## 目录
  1. [Basic Rules](#basic-rules)
  1. [基本规则](#basic-rules)
  1. [Class vs `React.createClass` vs stateless](#class-vs-reactcreateclass-vs-stateless)
  1. [Class vs `React.createClass` vs stateless](#class-vs-reactcreateclass-vs-stateless)
  1. [Naming](#naming)
  1. [命名](#naming)
  1. [Declaration](#declaration)
  1. [声明](#declaration)
  1. [Alignment](#alignment)
  1. [对齐](#alignment)
  1. [Quotes](#quotes)
  1. [引号](#quotes)
  1. [Spacing](#spacing)
  1. [空格](#spacing)
  1. [Props](#props)
  1. [属性](#props)
  1. [Parentheses](#parentheses)
  1. [大括号](#parentheses)
  1. [Tags](#tags)
  1. [标签](#tags)
  1. [Methods](#methods)
  1. [方法](#methods)
  1. [Ordering](#ordering)
  1. [排序](#ordering)
  1. [`isMounted`](#ismounted)
  1. [`isMounted`](#ismounted)

## Basic Rules
## 基本规则
  - Only include one React component per file.
    - However, multiple [Stateless, or Pure, Components](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) are allowed per file. eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
  - Always use JSX syntax.
  - Do not use `React.createElement` unless you're initializing the app from a file that is not JSX.

  - 每个文件只包含一个React组件；
    - 但是[无状态, 或者 Pure 组件](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) 允许一个文件包含多个组件。eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
  - 始终使用 JSX 语法;
  - 不要使用 `React.createElement`方法，除非初始化 app 的文件不是 JSX 格式。

## Class vs `React.createClass` vs stateless

  - If you have internal state and/or refs, prefer `class extends React.Component` over `React.createClass` unless you have a very good reason to use mixins. eslint: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md)

  - 如果组件拥有内部的 state 或者 refs 的时，更推荐使用 `class extends React.Component`，除非你有一个非常好的理由要使用 mixin。 eslint: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md)

    ```javascript
    // bad
    const Listing = React.createClass({
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    });

    // good
    class Listing extends React.Component {
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    }
    ```

    And if you don't have state or refs, prefer normal functions (not arrow functions) over classes:
    如果没有组件没有内部 state 或者 refs，那么普通函数 (不要使用箭头函数) 比类的写法更好：
    ```javascript

    // bad
    class Listing extends React.Component {
      render() {
        return <div>{this.props.hello}</div>;
      }
    }

    // bad (since arrow functions do not have a "name" property)
    // bad (因为箭头函数没有“name”属性)
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    );

    // good
    function Listing({ hello }) {
      return <div>{hello}</div>;
    }
    ```

## Naming
## 命名

  - **Extensions**: Use `.jsx` extension for React components.
  - **Filename**: Use PascalCase for filenames. E.g., `ReservationCard.jsx`.
  - **Reference Naming**: Use PascalCase for React components and camelCase for their instances. eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

  - **扩展名**：React 组件使用`.jsx`扩展名；
  - **文件名**：文件名使用帕斯卡命名。 例如： `ReservationCard.jsx`。
  - **引用命名**：React 组件使用帕斯卡命名，引用实例采用骆驼命名。 eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)
    ```javascript
    // bad
    import reservationCard from './ReservationCard';

    // good
    import ReservationCard from './ReservationCard';

    // bad
    const ReservationItem = <ReservationCard />;

    // good
    const reservationItem = <ReservationCard />;
    ```

  - **Component Naming**: Use the filename as the component name. For example, `ReservationCard.jsx` should have a reference name of `ReservationCard`. However, for root components of a directory, use `index.jsx` as the filename and use the directory name as the component name:

  - **组件命名**：组件名称应该和文件名一致， 例如： `ReservationCard.jsx` 应该有一个`ReservationCard`的引用名称。 但是， 如果是在目录中的组件， 应该使用 `index.jsx` 作为文件名 并且使用文件夹名称作为组件名：
    ```javascript
    // bad
    import Footer from './Footer/Footer';

    // bad
    import Footer from './Footer/index';

    // good
    import Footer from './Footer';
    ```

## Declaration
## 声明

  - Do not use `displayName` for naming components. Instead, name the component by reference.
  - 不要使用｀displayName｀属性来命名组件，应该使用类的引用名称。
    ```javascript
    // bad
    export default React.createClass({
      displayName: 'ReservationCard',
      // stuff goes here
    });

    // good
    export default class ReservationCard extends React.Component {
    }
    ```

## Alignment
## 对齐

  - Follow these alignment styles for JSX syntax. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

  - 为 JSX 语法使用下列的对其方式。eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```javascript
    // bad
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // good
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    />

    // if props fit in one line then keep it on the same line
    // 如果组件的属性可以放在一行就保持在当前一行中
    <Foo bar="bar" />

    // 多行属性采用缩进
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    >
      <Quux />
    </Foo>
    ```

## Quotes
## 引号

  - Always use double quotes (`"`) for JSX attributes, but single quotes for all other JS. eslint: [`jsx-quotes`](http://eslint.org/docs/rules/jsx-quotes)
  - JSX 的属性都采用双引号，其他的 JS 都使用单引号。eslint: [`jsx-quotes`](http://eslint.org/docs/rules/jsx-quotes)

  > Why? JSX attributes [can't contain escaped quotes](http://eslint.org/docs/rules/jsx-quotes), so double quotes make conjunctions like `"don't"` easier to type.

  > 为什么这样做？JSX 属性 [不能包含转义的引号](http://eslint.org/docs/rules/jsx-quotes), 所以当输入`"don't"`这类的缩写的时候用双引号会更方便。

  > Regular HTML attributes also typically use double quotes instead of single, so JSX attributes mirror this convention.

  > 标准的 HTML 属性通常也会使用双引号，所以 JSX 属性也会遵守这样的约定。

    ```javascript
    // bad
    <Foo bar='bar' />

    // good
    <Foo bar="bar" />

    // bad
    <Foo style={{ left: "20px" }} />

    // good
    <Foo style={{ left: '20px' }} />
    ```

## Spacing
## 空格
  - Always include a single space in your self-closing tag.
  - 终始在自闭合标签前面添加一个空格。
    ```javascript
    // bad
    <Foo/>

    // very bad
    <Foo                 />

    // bad
    <Foo
     />

    // good
    <Foo />
    ```

## Props
## 属性
  - Always use camelCase for prop names.
  - 属性名称始终使用骆驼命名法。
    ```javascript
    // bad
    <Foo
      UserName="hello"
      phone_number={12345678}
    />

    // good
    <Foo
      userName="hello"
      phoneNumber={12345678}
    />
    ```

  - Omit the value of the prop when it is explicitly `true`. eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)
  - 当属性值等于`true`的时候，省略该属性的赋值。 eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

    ```javascript
    // bad
    <Foo
      hidden={true}
    />

    // good
    <Foo
      hidden
    />
    ```

## Parentheses
## 大括号
  - Wrap JSX tags in parentheses when they span more than one line. eslint: [`react/wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/wrap-multilines.md)
  - 用括号包裹多行 JSX 标签。 eslint: [`react/wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/wrap-multilines.md)

    ```javascript
    // bad
    render() {
      return <MyComponent className="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // good
    render() {
      return (
        <MyComponent className="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );
    }

    // good, when single line
    render() {
      const body = <div>hello</div>;
      return <MyComponent>{body}</MyComponent>;
    }
    ```

## Tags
## 标签

  - Always self-close tags that have no children. eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)
  - 当标签没有子元素时，始终时候自闭合标签。 eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)
    ```javascript
    // bad
    <Foo className="stuff"></Foo>

    // good
    <Foo className="stuff" />
    ```

  - If your component has multi-line properties, close its tag on a new line. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)
  - 如果控件有多行属性，关闭标签要另起一行。 eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```javascript
    // bad
    <Foo
      bar="bar"
      baz="baz" />

    // good
    <Foo
      bar="bar"
      baz="baz"
    />
    ```

## Methods
## 方法
  - Bind event handlers for the render method in the constructor. eslint: [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)
  - 在 render 方法中事件的回调函数，应该在构造函数中进行bind绑定。 eslint: [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)

  > Why? A bind call in the render path creates a brand new function on every single render.

  > 为什么这样做? 在 render 方法中的 bind 调用每次调用 render 的时候都会创建一个全新的函数。

    ```javascript
    // bad
    class extends React.Component {
      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv.bind(this)} />
      }
    }

    // good
    class extends React.Component {
      constructor(props) {
        super(props);

        this.onClickDiv = this.onClickDiv.bind(this);
      }

      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv} />
      }
    }
    ```

  - Do not use underscore prefix for internal methods of a React component.
  - React 组件的内部方法命名不要使用下划线前缀。
    ```javascript
    // bad
    React.createClass({
      _onClickSubmit() {
        // do stuff
      },

      // other stuff
    });

    // good
    class extends React.Component {
      onClickSubmit() {
        // do stuff
      }

      // other stuff
    }
    ```

## Ordering
## 排序
  - Ordering for `class extends React.Component`:
  - `class extends React.Component`的顺序：

  1. `static`静态方法
  1. `constructor`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *点击回调或者事件回调* 比如 `onClickSubmit()` 或者 `onChangeDescription()`
  1. *`render`函数中的 getter 方法* 比如 `getSelectReason()` 或者 `getFooterContent()`
  1. *可选的 render 方法* 比如 `renderNavigation()` 或者 `renderProfilePicture()`
  1. `render`

  - How to define `propTypes`, `defaultProps`, `contextTypes`, etc...
  - 怎样定义 `propTypes`, `defaultProps`, `contextTypes`等

    ```javascript
    import React, { PropTypes } from 'react';

    const propTypes = {
      id: PropTypes.number.isRequired,
      url: PropTypes.string.isRequired,
      text: PropTypes.string,
    };

    const defaultProps = {
      text: 'Hello World',
    };

    class Link extends React.Component {
      static methodsAreOk() {
        return true;
      }

      render() {
        return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>
      }
    }

    Link.propTypes = propTypes;
    Link.defaultProps = defaultProps;

    export default Link;
    ```

  - Ordering for `React.createClass`: eslint: [`react/sort-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/sort-comp.md)
  - `React.createClass`的排序：eslint: [`react/sort-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/sort-comp.md)

  1. `displayName`
  1. `propTypes`
  1. `contextTypes`
  1. `childContextTypes`
  1. `mixins`
  1. `statics`
  1. `defaultProps`
  1. `getDefaultProps`
  1. `getInitialState`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *clickHandlers or eventHandlers* like `onClickSubmit()` or `onChangeDescription()`
  1. *getter methods for `render`* like `getSelectReason()` or `getFooterContent()`
  1. *Optional render methods* like `renderNavigation()` or `renderProfilePicture()`
  1. *点击回调或者事件回调* 比如 `onClickSubmit()` 或者 `onChangeDescription()`
  1. *`render`函数中的 getter 方法* 比如 `getSelectReason()` 或者 `getFooterContent()`
  1. *可选的 render 方法* 比如 `renderNavigation()` 或者 `renderProfilePicture()`
  1. `render`

## `isMounted`

  - Do not use `isMounted`. eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)
  - 不要使用 `isMounted`. eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

  > Why? [`isMounted` is an anti-pattern][anti-pattern], is not available when using ES6 classes, and is on its way to being officially deprecated.
  > 为什么这样做? [`isMounted`是一种反模式][反模式]，当使用 ES6 类风格声明 React 组件时该属性不可用，并且即将被官方弃用。

  [反模式]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html

**[⬆ back to top](#table-of-contents)**
