---
title: webpack react react-router-v4 code-splitting
date: 2017-9-20 11:04:24
tags:
---
### 背景介绍
最近准备做一个公司项目，在搭建框架时为了技术选型有研究了下React Router v4，看了网上对v4版本的评价可谓是哀鸿一片啊。React Router v3的设计思想更容易前端工程师接受类似于配置文件，v3的配置是将path映射为渲染模块，而去这种映射关系是静态的。而v4时动态映射，v4中Route也是一个React组件，可以和其他组件一样被渲染，在运行时决定某个Route渲染还是不渲染。v3到v4的升级更像时一个全新的库，API发生了巨大变化。于是就想这选用v3版本搭一套，有时间在用v4整一套。等我项目框架快要搭好的时候，结果项目评审没过，产品要回去修改在重新评审，哇哈哈！那我就去研究研究React Router v4吧！使用v4的过程中觉得其他地方都还好，查查API都能解决，唯独麻烦的是Code-splitting这块变化比较大。webpack的文档中没有特定针对react-router下code-splitting的文档。react-router v4下的 code-splitting文档写的使用bundle-loader的方法，也不够优雅。于是乎研究下，写下这篇文章以后自己回顾时方便也希望能帮助到其他人。
### Code-splitting
对于一些大型的单页应用，代码量大，如果我们还是把所有js文件打包在一个js文件中，js文件很大还不能并行加载，就会导致用户首屏页面加载很慢。于是乎把公用的代码抽出来作为一个js文件，不同页面或者不同业务下的页面分成多个代码块（chunk），用户只需要下载当前页面的js，进入其他页面再去加载对应的js代码，这样就能很好的提升用户体验和不必要的流量损失。这就需要用到代码分割（code-splitting）
webpack文档中提供了三种分割代码的方法：  
1、利用 webpack 中的 entry 配置项来进行手动分割  
2、利用 CommonsChunkPlugin 插件来提取重复 chunk  
3、动态引入（Dynamic Imports）
这里只讲第三种方法。
### React Router v3的按需加载
在v4之前的版本中，想要实现按需加载Component，需要使用getCompoent方法，但是在v4中getComponent方法已经被移除。先来介绍下v3下按需加载的方法
#### 1、使用require.ensure
代码如下：
```js
//配置route
<Route path="home" getComponent={(location, cb) => {
    require.ensure([], require => {
        cb(null, require('../Component/home').default)
    },'home')
}} />
```
这种方法比较老，貌似webpack 3之后版本不支持（待验证）
#### 2、使用import+webpackChunkName
这种方法需要使用ES6和ES7语法，但是写起来很简便
```js
<Route path="/home" getComponent={async (nextState, cb) => {
    const Home = await import(/* webpackChunkName: "home" */ '../containers/Home');
    cb(null, Home.default)
}}/>
<Route path="/list" getComponent={async (nextState, cb) => {
    const List = await import(/* webpackChunkName: "list" */ '../containers/List');
    cb(null, List.default)
}} />
```
### React Router v4下的按需加载
#### 使用 bundle-loader 的方案
在 React Router v4 官方给出的[文档](https://reacttraining.com/react-router/web/guides/code-splitting)中，使用了名为 bundle-loader 的工具来实现这一功能。
其主要实现思路为创建一个名为 <Bundle> 的组件，当应用匹配到了对应的路径时，该组件会动态地引入所需模块并将自身渲染出来。
示例代码如下
```js
import loadSomething from 'bundle-loader?lazy!./Something'
<Bundle load={loadSomething}>
  {(mod) => (
    // do something w/ the module
  )}
</Bundle>
```
这种方式看着就不太爽，代码太丑了。而去在几个月前我试过这种方式，貌似有些问题一直没成功。所以我就直接找其他方法了。
#### 使用import()
一个常规的 React Router 项目结构如下：
```js
import Home from './containers/Home'
import List from './containers/List'
import Demo from './containers/Demo'

export default function Routes () {
    return (
        <Switch>
            <Route exact path="/" component={Home}/>
            <Route path="/home" component={Home}/>
            <Route path="/list" component={List}/>
            <Route path="/Demo" component={Demo}/>
        </Switch>
    )
}
```
首先根据我们的 route 引入相应的组件，然后将其用于定义相应的 <Route>。  
但是，不管匹配到了哪一个 route，我们这里都一次性地引入所有的组件。而我们想要的效果是当匹配了一个 route，则只引入与其对应的组件，这就需要实现代码分割了。
##### 创建一个异步组件（Async Component）
```js
import React, { Component } from 'react';

export default function asyncComponent(importComponent) {
    class AsyncComponent extends Component {
        constructor(props) {
            super(props);
            this.state = {
                component: null,
            };
        }
        async componentDidMount() {
            const { default: component } = await importComponent();
            this.setState({
                component: component
            });
        }
        render() {
            const C = this.state.component;
            return C
                ? <C {...this.props} />
                : null;
        }
    }
    return AsyncComponent;
}
```
asyncComponent 接收一个 importComponent 函数作为参数，importComponent() 在被调用时会动态引入给定的组件。  
在 componentDidMount()中，调用传入的 importComponent()，并将动态引入的组件保存在 state 中。
##### 使用异步组件（Async Component
使用 asyncComponent 方法来动态引入组件：
```js
const Home = asyncComponent(() => import(/* webpackChunkName: "home" */ '../containers/Home'));
```
此处import()返回的是一个 Promise，这是一种动态引入模块的方法，即上文 webpack 文档中提到的第三种方法。  

注意这里并没有进行组件的引入，而是传给了 asyncComponent 一个函数，它将在 AsyncHome 组件被创建时进行动态引入。同时，这种传入一个函数作为参数，而非直接传入一个字符串的写法能够让 webpack 意识到此处需要进行代码分割。

routes完整代码如下：
```js
import React, { Component } from 'react'
import { Switch, Route } from 'react-router-dom'
import asyncComponent from './asyncComponent'

const Home = asyncComponent(() => import(/* webpackChunkName: "home" */ '../containers/Home'));
const List = asyncComponent(() => import(/* webpackChunkName: "list" */ '../containers/List'));
const Demo = asyncComponent(() => import(/* webpackChunkName: "list" */ '../containers/Demo'));

export default function Routes () {
    return (
        <Switch>
            <Route exact path="/" component={Home}/>
            <Route path="/home" component={Home}/>
            <Route path="/list" component={List}/>
            <Route path="/Demo" component={Demo}/>
        </Switch>
    )
}
```
