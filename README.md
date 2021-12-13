## 目录

#### **一、BOM与DOM**

#### 二、css

1. [css如何避免样式污染](https://www.zoo.team/article/react-css)
   1. namespaces 利用约定好的命名来隔离css的作用域
   2. css in js 使用styled-components
   3. css modules 使用webpack等构建工具使class作用域为局部
2. 盒子模型
3. [CSS的BFC](#js14)
4. [grid、flex知识点]()
5. [元素隐藏的几种方法]()
   1. visibility: hidden，设置元素隐藏
   2. opacity: 0，设置元素隐藏
   3. display:none，设置元素隐藏 元素不渲染，其他元素渲染
   4. position: absolute，设置元素隐藏
6. [filter相关]
7. align-items和align-content的区别
   1. align-items单行居中
   2. align-content多行居中

#### 三、js

1. [原型链相关，什么是原型链js有继承吗](#js1)
2. [new的实际过程是什么]
3. [es6新特性，使用过那些方法](#js2)
4. [Map、Object、Set有什么区别](#map)
5. [判断类型的方法有哪些](#js4)
6. [闭包](#js5)
7. [js事件循环、宏任务和微任务，执行顺序有差别，定时器与异步函数的优先级，setTimeout(fn, 0)原理](#js6)
8. [promise、generator、async await](#js7)
9. [promise有几种状态]()
   1. 等待pending、成功reslove、失败reject
10. [箭头函数与普通函数的区别](#js8)
11. [箭头函数可以实例化吗]()
12. [== 和 === 区别](#js9)
13. [密码必须匹配一个大写一个小写字母，至少6位数字 ，正则思路应该 如何 考虑和设计](#js10)
14. [react router 如何做到单页面刷新（实现），例如你修改路由路径，并不会造成页面重新刷新是如何做到的，关键点是 h5 histroy和hash router](#js11)
15. [你如何做浏览器缓存 ，例如你给资源文件加了hash码 和版本号，但index.html文件可能还是使用了之前的缓存文件，你应该如何解决，关键字强缓存和弱缓存](#js12)
16. [JS模块话原理（AMD和CMD，import和export）](#js13)
17. [domContentLoad与onload区别](#js15)
18. [判断数据类型的方法](#js16)
19. [ajax实现原理](#js17)
20. [js的执行机制](#js18)
21. [数组去重]()
22. [js作用域]()
23. [http是否有了解，说一下http1.0 http1.1 http2.0]
24. [undefault与null区别]
25. [怎么获取相同的symbol]
26. [js类型]
27. []
    值类型(基本类型)：字符串（String）、数字(Number)、布尔(Boolean)、对空（Null）、未定义（Undefined）、Symbol
    引用数据类型：对象(Object)、数组(Array)、函数(Function)。
28. [HTML中缓存有哪些]
29. [浏览器为什么不能直接显示jsx文件]
30. [session与cookie区别]
31. [柯里化]
32. [tcp与udp的区别]
33. [最简单判断两个值是否相等的方法]
    转为字符串进行对比
34. apply、call、bind区别
35. 设计模式

#### 四、react

1. [react生命周期](#react)
2. [react-router实现原理、钩子、如何自己实现一个](#router)
3. [redux实现原理如何自己实现一个](#redux)
4. [react优化](#react-optimize)
5. [首屏渲染白屏原因如何优化](#first-screen)
6. [hook、hook与函数组件有什么区别](#hook)
7. [component与pureComponent区别，如果使用immer、immutable等不可变数据 pureComponent会进行比较吗](#pure)
8. [setState，是同步还是异步，调用后发生了什么](#)
9. [封装过那些组件，需要考虑什么问题](#component)
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
21. [高阶函数如何传递ref](#react21)
22. [ssr服务端渲染好处及实现原理、是否听说过serverless简单介绍](#ssr)
23. [为什么需要hooks]()
24. [函数组件有什么缺点]()
25. [hooks父组件如何调用子组件]()
    父组件通过useRef获取子组件ref并将ref通过自定义属性传递给子组件，子组件使用useImperativeHandle hooks函数，将ref和函数作为参数进行调用。其中函数返回一个对象，对象中的value为方法，使用forwardRef包裹子组件，在父组件中通过ref.current.方法名调用虚拟
26. dom一定比操作真实dom高效吗，为什么使用需要虚拟dom
27. react constroctor 中的super意义
28. 函数组件与hooks、class组件之间的区别

#### 五、vue

#### 六、项目工程化

#### 七、第三方库

1. [lodash、ramda](#lodash)
2. [immutable、immer](#immutable)

#### 八、扩展

1. [输入网址回车之后会发生什么](#extend)

#### 九、实战项目

1. [做过什么项目，使用过什么技术栈，讲讲项目中遇到的问题。](#project)

---

#### 一、css

1. [css如何避免样式污染](https://www.zoo.team/article/react-css)
   1. namespaces 利用约定好的命名来隔离css的作用域
   2. css in js 使用styled-components
   3. css modules 使用webpack等构建工具使class作用域为局部
2. 盒子模型

#### 二、js

1. ##### `<span id="js1">`原型链相关
2. ##### `<span id="js2">`es6新特性，使用过那些方法、原型链相关

   const和let、模板字符串、箭头函数、函数的参数默认值、对象和数组解构、
3. ##### `<span id="map">`Map、object、Set、Array有什么区别

   map与object


   1. 一个Object 的键只能是字符串或者 Symbols，但一个Map 的键可以是任意值。
   2. Map中的键值是有序的（FIFO 原则），而添加到对象中的键则不是。
   3. Map的键值对个数可以从 size 属性获取，而 Object 的键值对个数只能手动计算。
   4. Object 都有自己的原型，原型链上的键名有可能和你自己在对象上的设置的键名产生冲突。
4. ##### `<span id="indexOf">`判断类型的方法有哪些
5. ##### `<span id="closure">`闭包
6. ##### `<span id="eventLoop">`js事件循环
7. ##### `<span id="js15">`domContentLoad与onload区别

   onload是的在页面所有文件加载完成后执行，domContentLoad是Dom加载完成后执行，不必等待样式脚本和图片加载
8. ##### `<span id="js16">`判断数据类型的方法


   1. typeof：缺点无法判断引用类型数据
   2. instanceof、constructor
   3. Object.prototype.toString.call()
9. ##### `<span id="js17">`ajax实现原理


   1. 创建XMLHttpRequest对象,也就是创建一个异步调用对象.
   2. 创建一个新的HTTP请求,并指定该HTTP请求的方法、URL及验证信息.
   3. 设置响应HTTP请求状态变化的函数.
   4. 发送HTTP请求.
   5. 获取异步调用返回的数据.
   6. 使用JavaScript和DOM实现局部刷新.
10. ##### `<span id="js18">`js的执行机制


    1. 首先判断JS是同步还是异步，同步就进入主线程，异步就进入异步队列等待
    2. 异步任务在异步队列中注册函数，当满足触发条件后，被推入主线程
    3. 同步任务进入主线程后一直执行，直到主线程空闲时，才会去异步队列中查看是否有可执行的异步任务，如果有就推入主进程中
    4.

#### 三、react

1. ##### `<span id="react">`react生命周期 [参考链接](https://react.docschina.org/docs/react-component.html)

   ###### 挂载：


   1. constructor()
   2. static getDerivedStateFromProps()
   3. componentWillMount() / UNSAFE_componentWillMount()
   4. render()
   5. componentDidMount()

   ###### 更新：

   1. componentWillReceiveProps() / UNSAFE_componentWillReceiveProps()
   2. static getDerivedStateFromProps()
   3. shouldComponentUpdate()
   4. componentWillUpdate() / UNSAFE_componentWillUpdate()
   5. render()
   6. getSnapshotBeforeUpdate()
   7. componentDidUpdate()

   ###### 卸载：

   componentWillUnmount()

   ###### 错误处理:

   * static getDerivedStateFromError()
   * componentDidCatch()
2. ##### `<span id="react-14">`react 组件 传递数据有哪些方式？


   * 父传子通过props传递
   * 子传夫通过回调函数传递
   * 兄弟组件 1.订阅发布 2.将状态传给父级然后在通过props传给兄弟
3. ##### `<span id="router">`react-router实现原理、钩子、如何自己实现一个 [参考](https://zhuanlan.zhihu.com/p/44548552#444)

   原理：定义一个routeMap对象，存放路由与组件的键值对。在dom渲染之后，给window绑定hashchange事件（监听页面hash的变化），并且在state属性中添加route属性，代表当前页面路由。当页面跳转时会触发hashchange事件，通过setState更改route值，并在routeMap中进行匹配重新渲染。
4. ##### `<span id="redux">`redux实现原理如何自己实现一个
5. ##### `<span id="react-optimize">`react优化[参考](https://zhuanlan.zhihu.com/p/74229420)

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
6. ##### `<span id="first-screen">`首屏渲染白屏原因如何优化

   解决方案：


   * webpack分包、使用cdn、按需加载、路由懒加载
   * 骨架屏
   * 服务端渲染
7. ##### `<span id="hook">`hook、hook与函数组件有什么区别

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
8. ##### `<span id="pure">`component与pureComponent区别如果使用immer pureComponent会进行比较吗

   pureComponent 通过浅比较state和props复写shouldComponentUpdate
9. ##### `<span id="component">`封装过那些组件，需要考虑什么问题[参考](https://juejin.cn/post/6844903847874265101#heading-5)


   1. 细粒度的考量
   2. 通用性考量
10. ##### `<span id="ssr">`ssr服务端渲染好处及实现原理、是否听说过serverless简单介绍


    * 好处：减少首屏白屏时间、seo优化
    * 原理：node server 接收客户端请求，得到当前的req url path,然后在已有的路由表内查找到对应的组件，拿到需要请求的数据，将数据作为 props
      、context或者store 形式传入组件，然后基于 react 内置的服务端渲染api renderToString() or renderToNodeStream() 把组件渲染为 html字符串或者 stream 流, 在把最终的 html 进行输出前需要将数据注入到浏览器端(注水)，server 输出(response)后浏览器端可以得到数据(脱水)，浏览器开始进行渲染和节点对比，然后执行组件的componentDidMount 完成组件内事件绑定和一些交互，浏览器重用了服务端输出的 html 节点，整个流程结束。
11. ##### `<span id="webpack">`webpack配置
12. ##### `<span id="react21">`高阶函数如何传递ref

在子级中封装getInstance函数将this传给父级从而获取ref [参考](https://www.jianshu.com/p/2609fd3777cd)

#### 四、vue

#### 五、项目工程化

#### 六、第三方库

1. ##### `<span id="lodash">`lodash、ramda
2. ##### `<span id="immutable">`immutable、immer

#### 七、扩展

1. ##### `<span id="extend">`输入网址回车之后会发生什么

#### 八、实战项目

1. ##### `<span id="project">`做过什么项目，使用过什么技术栈，讲讲项目中遇到的问题。
2. ##### 前端实现权限控制方案

   * 通过高阶函数封装react-router，在react-router中进行判断
   * 封装button点击的时候判断是否登录跳转页面
   * 自己实现一个类，页面组件继承至这个类（不推荐使用，react不推荐组件继承于自定义的类）

https://www.zhipin.com/wenti/w_388a771a2bb0c66etnV90t25E1c~.html
