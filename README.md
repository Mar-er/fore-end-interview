## 目录
#### 一、js、css
1. [原型链相关](#prototype)
2. [es6新特性，使用过那些方法](#es6)
3. [Map与object有什么区别](#map)
4. [判断类型的方法有哪些](#indexOf)
5. [闭包](#closure)
6. [js事件循环、宏任务和微任务，执行顺序有差别，定时器与异步函数的优先级，setTimeout(fn, 0)原理](#eventLoop)
7. [promise、generator、async await](#)
8. [箭头函数与普通函数的区别](#)
9. [== 和 === 区别](#)
10. [密码必须匹配一个大写一个小写字母，至少6位数字 ，正则思路应该 如何 考虑和设计](#)
11. [react router 如何做到单页面刷新（实现），例如你修改路由路径，并不会造成页面重新刷新是如何做到的，关键点是 h5 histroy和hash router](#)
12. [你如何做浏览器缓存 ，例如你给资源文件加了hash码 和版本号，但index.html文件可能还是使用了之前的缓存文件，你应该如何解决，关键字强缓存和弱缓存](#)
13. [JS模块话原理（AMD和CMD，import和export）](#)
14. [CSS的BFC](#)

#### 二、react
1. [react生命周期](#react)
2. [react-router实现原理、钩子、如何自己实现一个](#router)
3. [redux实现原理如何自己实现一个](#redux)
4. [react优化](#react-optimize)
5. [首屏渲染白屏原因如何优化](#first-screen)
6. [hook、hook与函数组件有什么区别](#hook)
5. [component与pureComponent区别，如果使用immer、immutable等不可变数据 pureComponent会进行比较吗](#pure)
6. [封装过那些组件，需要考虑什么问题](#component)
7. [ssr服务端渲染好处及实现原理、是否听说过serverless简单介绍](#ssr)
8. [setState，是同步还是异步，调用后发生了什么](#)
10. [谈谈虚拟dom和diff算法、为什么react说虚拟dom可以提高性能](#)
11. [react refs是干嘛的](#)
12. [说下hoc是什么，你用过吗，如何用](#)
13. [react 有几种构建组件方式，分别是？](#)
14. [react 组件 传递数据有哪些方式？](#react-14)
15. [如何做页面样式切换（主题）](#)
16. [render函数中return如果没有使用()会有什么问题](#)
17. [有用过React的插槽(Portals)吗？怎么用？](#)
18. [webpack配置](#webpack)
19. [redux-saga 应用](#)
20. [webpack loader 原理，写过啥插件](#)

 

#### 三、第三方库
1. [lodash、ramda](#lodash)
1. [immutable、immer](#immutable)

#### 四、扩展
1. [输入网址回车之后会发生什么](#extend)


#### 五、实战项目
1. [做过什么项目，使用过什么技术栈，讲讲项目中遇到的问题。](#project)


#### 六、vue

---

#### 一、js
1. ##### <span id="prototype">原型链相关</span>
1. ##### <span id="es6">es6新特性，使用过那些方法、原型链相关</span>
    const和let、模板字符串、箭头函数、函数的参数默认值、对象和数组解构、

1. ##### <span id="map">Map与object有什么区别</span>
1. ##### <span id="indexOf">判断类型的方法有哪些</span>
1. ##### <span id="closure">闭包</span>
1. ##### <span id="eventLoop">js事件循环</span>


#### 二、react
1. ##### <span id="react">react生命周期</span> [参考链接](https://react.docschina.org/docs/react-component.html)
    ###### 挂载：
    1. constructor()
    2. static getDerivedStateFromProps()
    3. componentWillMount() / UNSAFE_componentWillMount()
    1. render()
    1. componentDidMount()
    ###### 更新：
    1. componentWillReceiveProps() / UNSAFE_componentWillReceiveProps()
    1. static getDerivedStateFromProps()
    1. shouldComponentUpdate()
    1. componentWillUpdate() / UNSAFE_componentWillUpdate()
    1. render()
    1. getSnapshotBeforeUpdate()
    1. componentDidUpdate()
    ###### 卸载：
    componentWillUnmount()
    ###### 错误处理:
    * static getDerivedStateFromError()
    * componentDidCatch()
14. ##### <span id="react-14">react 组件 传递数据有哪些方式？</span>
    * 父传子通过props传递
    * 子传夫通过回调函数传递
    * 兄弟组件 1.订阅发布 2.将状态传给父级然后在通过props传给兄弟


1. ##### <span id="router">react-router实现原理、钩子、如何自己实现一个</span> [参考](https://zhuanlan.zhihu.com/p/44548552#444)
    原理：定义一个routeMap对象，存放路由与组件的键值对。在dom渲染之后，给window绑定hashchange事件（监听页面hash的变化），并且在state属性中添加route属性，代表当前页面路由。当页面跳转时会触发hashchange事件，通过setState更改route值，并在routeMap中进行匹配重新渲染。

1. ##### <span id="redux">redux实现原理如何自己实现一个</span>


1. ##### <span id="react-optimize">react优化</span>[参考](https://zhuanlan.zhihu.com/p/74229420)
    
    从三个方面来优化：
    * 减少渲染节点/降低渲染计算量
        * 不要在渲染函数进行数组排序、数据转换、订阅事件、创建事件处理器等，渲染函数中不应该放置太多副作用。
        * 减少不必要的嵌套
        * 使用虚拟列表库，比如react-window
        * 惰性渲染，比如tab选项卡，只需要渲染当前tab项，其他不用渲染。
        * 选择合适的样式方案，比如css > 大部分css-in-js > inline style
    * 避免重复渲染
        * 优化props 如果一个组件props过于复杂则意味着它违背了“单一职责”原理
        * 不可变事件处理器，不要再组件上书写函数，应该先定义好函数然后传递函数。对于hook可以使用useCallback包裹。
        * 使用不可变数据，比如immer、immutable等。
        * 使用shouldComponentUpdate
        * 使用 recompose 精细化比对
    * 精细化渲染
        * 组件尽可能拆分小一点，遵守“单一职责”不要编写大而全的组件。
        * 不要滥用context

1. ##### <span id="first-screen">首屏渲染白屏原因如何优化</span>
    
    解决方案：
    * webpack分包、使用cdn、按需加载、路由懒加载
    * 骨架屏
    * 服务端渲染

1. ##### <span id="hook">hook、hook与函数组件有什么区别</span>
    ###### Hook 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。
    
    ###### 基础 Hook
    * useState
    * useEffect
    * useContext
    ###### 额外的 Hook
    - useReducer
    - useCallback
    - useMemo
    - useRef
    - useImperativeHandle
    - useLayoutEffect
    - useDebugValue


1. ##### <span id="pure">component与pureComponent区别如果使用immer pureComponent会进行比较吗</span>
    pureComponent 通过浅比较state和props复写shouldComponentUpdate

1. ##### <span id="component">封装过那些组件，需要考虑什么问题</span>


1. ##### <span id="ssr">ssr服务端渲染好处及实现原理、是否听说过serverless简单介绍</span>
    * 好处：减少首屏白屏时间、seo优化
    * 原理：node server 接收客户端请求，得到当前的req url path,然后在已有的路由表内查找到对应的组件，拿到需要请求的数据，将数据作为 props
、context或者store 形式传入组件，然后基于 react 内置的服务端渲染api renderToString() or renderToNodeStream() 把组件渲染为 html字符串或者 stream 流, 在把最终的 html 进行输出前需要将数据注入到浏览器端(注水)，server 输出(response)后浏览器端可以得到数据(脱水)，浏览器开始进行渲染和节点对比，然后执行组件的componentDidMount 完成组件内事件绑定和一些交互，浏览器重用了服务端输出的 html 节点，整个流程结束。

1. ##### <span id="webpack">webpack配置</span>

#### 三、第三方库
1. ##### <span id="lodash">lodash、ramda</span>
1. ##### <span id="immutable">immutable、immer</span>


#### 四、扩展
1. ##### <span id="extend">输入网址回车之后会发生什么</span>

#### 五、实战项目
1. ##### <span id="project">做过什么项目，使用过什么技术栈，讲讲项目中遇到的问题。</span>

1. ##### 前端实现权限控制方案
    * 通过高阶函数封装react-router，在react-router中进行判断
    * 封装button点击的时候判断是否登录跳转页面
    * 自己实现一个类，页面组件继承至这个类（不推荐使用，react不推荐组件继承于自定义的类）
