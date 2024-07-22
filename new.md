## 单词
```
Reconciliation /ˌrekənsɪliˈeɪʃn/ 调和
derived /dɪˈraɪv/ 得到
getDerivedStateFromProps
Proxy /ˈprɒksi/ 代理服务器
Snapshot /ˈsnæpʃɒt/ 快照
suspense /səˈspens/
profiler /ˈprəʊfaɪlə(r)/ 资料搜集
Repaint
visibility /ˌvɪzəˈbɪləti/ 能见度
opacity /əʊˈpæsəti/ 不透明性
imperative /ɪmˈperətɪv/
useImperativeHandle
Portal /ˈpɔːtl/
```

## React
### 生命周期
- 挂载阶段：constructor(), static getDerivedStateFromProps(), render(), componentDidMount()
- 更新阶段：static getDerivedStateFromProps(nextProps, prevState), shouldComponentUpdate(nextProps, nextState), render(), getSnapshotBeforeUpdate(prevProps, prevState), componentDidUpdate(prevProps, prevState, snapshot)
- 卸载阶段：componentWillUnmount()
- 错误处理：componentDidCatch()

### 字符组件通信
#### 通过 Props 传递数据和函数
- 父组件可以通过 props 向子组件传递数据和回调函数。子组件可以使用这些数据和函数来更新状态或执行特定操作。
#### 通过 Ref 访问子组件的方法（仅类组件）
#### 通过 React.forwardRef 和 useImperativeHandle 访问函数组件的方法
```
import React, { forwardRef, useImperativeHandle, useRef } from 'react';

// 子组件（函数组件）
const Child = forwardRef((props, ref) => {
  const localRef = useRef();

  useImperativeHandle(ref, () => ({
    sayHello() {
      alert('Hello from Child');
    }
  }));

  return <div>Child Component</div>;
});

// 父组件
function Parent() {
  const childRef = useRef();

  const callChildMethod = () => {
    if (childRef.current) {
      childRef.current.sayHello();
    }
  };

  return (
    <div>
      <Child ref={childRef} />
      <button onClick={callChildMethod}>Call Child Method</button>
    </div>
  );
}

export default Parent;
```
### 如何在最外层添加modal组件
> React Portal 允许将子节点渲染到 DOM 的不同部分，而不是其父组件的 DOM 层级。这样可以将 Modal 渲染到应用的最外层，即 body 元素中。


### 优化
1. 避免不必要的重新渲染
	- 使用 PureComponent 或 React.memo
		- PureComponent：适用于类组件，自动实现了 shouldComponentUpdate，并进行浅比较以避免不必要的渲染。
		- React.memo：适用于函数组件，缓存组件的渲染结果，只有在 props 发生变化时才重新渲染
	- 实现 shouldComponentUpdate
		- 手动实现 shouldComponentUpdate 方法，控制组件在接收到新的 props 或 state 时是否重新渲染。
2. 使用不可变数据结构
	- 使用不可变数据结构（如 immer 或 immutable.js），确保数据的每次修改都会返回新的引用，从而使浅比较更加可靠。
3. 分离关注点
	- 使用容器组件和展示组件
	- 将组件分为容器组件（处理数据和逻辑）和展示组件（处理 UI），提高组件的可重用性和可维护性。
4. 使用 useCallback 和 useMemo
	- useCallback：缓存函数，避免在每次渲染时创建新函数。
	- useMemo：缓存计算结果，避免在每次渲染时重复计算
5. 虚拟化长列表
	- 使用 react-window 或 react-virtualized 等库来虚拟化长列表，只渲染可见部分，提高渲染性能。
6. 代码分割和懒加载
	- 使用 React.lazy 和 Suspense 实现代码分割和懒加载，减少初始加载时间。
7. 避免匿名函数和内联对象
	- 在渲染方法中避免使用匿名函数和内联对象，因为它们会在每次渲染时创建新的引用。
8. 避免重复状态更新
	- 在一次操作中合并多个状态更新，避免多次渲染
9. 使用 Profiler
	- 使用 React Profiler 来分析和识别性能瓶颈。

### Diff 算法
> 一种用于比较两个数据结构之间差异的算法，在 React 中，它用来比较虚拟 DOM 的差异并计算出需要更新真实 DOM 的部分。React 的 Diff 算法是基于以下几个主要原则来高效地执行的：

1. 树的对比
	> 在 React 的虚拟 DOM 中，Diff 算法的主要任务是对比两个虚拟 DOM 树，以找出它们之间的差异。算法基于以下原则来优化对比过程：

	1. 节点类型对比
		- 相同类型的节点：

			如果新旧虚拟 DOM 节点的类型相同（例如，都是 div、span、组件等），则认为它们是同一个节点，需要进一步对比其子节点和属性。
			对于同类型的节点，React 会尝试重用旧节点的 DOM 元素，并只更新其属性和子节点。
		- 不同类型的节点：
			如果新旧虚拟 DOM 节点的类型不同，则 React 会卸载旧节点的 DOM 元素，并创建新的 DOM 元素。这意味着旧节点的整个子树将被替换。
	2. 元素属性对比
		- 简单属性：
			对于简单属性（如 id、className、style 等），React 会直接比较属性值。如果属性值发生变化，React 会更新真实 DOM 中的对应属性。
		- 复杂属性：
			对于复杂属性（如对象、数组），React 会递归比较这些属性的变化，确保更新正确。
	3. 子树对比
		- 同级比较：
			React 会对比虚拟 DOM 树中的子节点，找到它们的差异。React 会按照虚拟 DOM 树的顺序进行比较，并尝试最小化实现 DOM 的变更。
		- Key 属性：
			在列表中，React 使用 key 属性来标识元素。key 是唯一的标识符，用于帮助 React 区分列表中的元素。使用 key 可以帮助 React 更准确地识别哪些元素发生了变化，从而更有效地更新 DOM。
2. 实现细节
	> React 的 Diff 算法实现细节包括：
	1. 深度优先遍历
		- React 使用深度优先遍历算法来比较两个虚拟 DOM 树。遍历过程中，React 会比较当前节点的类型、属性和子节点。
	2. 最小化 DOM 操作
		- React 的目标是最小化对真实 DOM 的操作，以提高性能。通过合并和优化差异更新，React 避免了频繁的 DOM 操作和重绘。
	3. Fiber 架构
		- React 16 引入了 Fiber 架构，以优化调和过程和支持更细粒度的控制。Fiber 架构通过增量渲染和优先级调度，进一步提升了 React 的性能和响应能力。

## React Router
1. 路由声明和配置
	- React Router 允许开发者在应用中声明路由，通过配置 URL 路径和对应的组件来实现不同路径的页面展示。
2. 路由匹配
	- 当 URL 变化时，React Router 会遍历定义的路由配置，尝试找到与当前 URL 匹配的路径。匹配机制包括以下几点：

		- 完全匹配：路径完全匹配时，Route 组件会渲染对应的组件。
		- 部分匹配：通过设置 exact 属性，控制是否需要完全匹配。
		- 动态参数：使用 :paramName 形式定义动态参数
3. 路由组件
	- Router
		- BrowserRouter：使用 HTML5 History API 实现 URL 导航。
		- HashRouter：使用 URL 哈希部分实现 URL 导航。
		- MemoryRouter：在内存中管理路由，适用于非浏览器环境。
	- Route
		- 定义路径和对应的组件。
		- 可以通过 component, render, children 三种方式渲染组件。
	- Switch
		- 渲染第一个匹配的路由，确保只有一个路由组件被渲染。
4. 路由状态管理
    - React Router 通过 history 对象管理路由状态，包括当前路径、导航历史等。history 对象提供了以下方法：
      ```
      push：导航到新路径。
      replace：替换当前路径。
      go：导航到历史记录中的某个位置。
      goBack：导航到上一页。
      goForward：导航到下一页。
      ```
5. URL 变化监听
  - React Router 使用 history 对象监听 URL 变化，并在 URL 变化时更新视图。具体步骤如下：
    - 监听 URL 变化：使用 history.listen 方法监听 URL 变化。
    - 匹配路由：URL 变化时，React Router 重新匹配路由。
    - 更新视图：匹配成功后，React 重新渲染对应的组件。
6. 动态路由和嵌套路由
  - React Router 支持动态路由和嵌套路由，通过这种方式，可以实现复杂的页面结构。

  - 动态路由
    - 动态路由通过在路径中使用参数实现。
      ```
      <Route path="/user/:userId" component={UserProfile} />
      ```
    - 在 UserProfile 组件中，可以通过 match.params.userId 获取动态参数。
  - 嵌套路由
    - 嵌套路由允许在一个组件中定义子路由，实现多层级页面结构。
      ```
      const App = () => (
        <Router>
          <Route path="/dashboard" component={Dashboard}>
            <Route path="/dashboard/profile" component={Profile} />
            <Route path="/dashboard/settings" component={Settings} />
          </Route>
        </Router>
      );
      ```
7. 路由守卫
  - React Router 支持在路由进入之前进行权限检查或其他操作，通过自定义组件实现路由守卫。
    ```
    const PrivateRoute = ({ component: Component, ...rest }) => (
      <Route
        {...rest}
        render={props =>
          isAuthenticated() ? (
            <Component {...props} />
          ) : (
            <Redirect to="/login" />
          )
        }
      />
    );

    const App = () => (
      <Router>
        <Switch>
          <PrivateRoute path="/dashboard" component={Dashboard} />
          <Route path="/login" component={Login} />
        </Switch>
      </Router>
    );
    ```
8. URL 参数和查询字符串
  - React Router 支持在组件中访问 URL 参数和查询字符串。
  - URL 参数
    - 通过 match.params 访问 URL 参数。
  - 查询字符串
    - 通过 location.search 访问查询字符串，使用 URLSearchParams 解析。
### 自己编写一个React-Router
1. Router：提供路由上下文，监听 URL 变化并支持导航。
    - 上下文提供：创建一个上下文 (RouterContext) 用于传递当前的路由信息（如当前路径和导航函数）。
    - 监听 URL 变化：在组件挂载时，添加事件监听器以监控浏览器的 URL 变化（通过 popstate 事件）。当 URL 变化时，更新当前路径。
    - 导航功能：提供一个 navigate 函数，通过调用 history.pushState 修改浏览器的历史记录，并更新当前路径。
2. 创建 Route 组件
    - 功能：根据当前路径渲染匹配的组件。
    - 匹配路径：通过访问 RouterContext 中的当前路径，检查是否与 Route 组件的 path 属性匹配。
    - 渲染组件：如果路径匹配，则渲染对应的组件。
3. Link：创建导航链接，允许用户在应用中进行导航。
  - 功能：创建可以点击的链接，允许用户在应用中进行导航，而不会触发页面刷新。
    - 点击处理：当用户点击链接时，阻止默认行为（页面刷新），调用 navigate 函数进行导航。
    - 导航：通过更新浏览器的历史记录并改变路径来实现导航。
4. Switch：渲染第一个匹配的路由组件。
  - 功能：只渲染第一个匹配的 Route 组件，用于实现路由选择。
    - 遍历子组件：遍历 Switch 组件的子组件，找到第一个匹配当前路径的 Route。
    - 渲染匹配的路由：如果找到匹配的 Route，则渲染其对应的组件。
### Redux
#### Redux 的数据流遵循以下顺序：
1. 用户触发事件：用户在应用中执行操作，如点击按钮。
2. Dispatch Action：触发一个 action，描述用户的操作。
3. Middleware（可选）：处理异步操作和其他副作用。
4. Reducer：接收 action 和当前状态，计算并返回新的状态。
5. Store 更新：更新 store 的状态。
6. 视图更新：React 组件通过 connect 订阅 store 的变化，自动重新渲染。
#### 实现原理
1. 创建 Store
    - 功能：Store 负责保存应用的状态，并提供获取状态、派发动作和订阅状态变化的功能。
    - 实现：定义一个 Store 类，包含以下方法：
      - getState()：返回当前的状态。
      - dispatch(action)：接收一个动作（action），通过 reducer 计算新的状态，并更新状态。
      - subscribe(listener)：允许订阅状态变化，当状态发生变化时调用传入的监听函数。

2. 定义 Reducer
    1. 功能：Reducer 是一个纯函数，接收当前的状态和一个动作，根据动作返回新的状态。
    2. 实现：创建一个 reducer 函数，函数接受两个参数：
        - 当前状态
        - 动作对象
    3. 根据动作的类型（如 INCREMENT 或 DECREMENT），更新状态并返回新的状态。如果动作类型不匹配，则返回当前状态。
3. 创建 Action
    1. 功能：动作是描述状态变化的普通 JavaScript 对象。每个动作至少包含一个 type 属性，表示动作的类型。可以包含其他属性（payload）来传递数据。
    2. 实现：定义几个函数，用于创建不同类型的动作，例如 increment() 和 decrement() 函数，分别返回对应的动作对象。
4. 使用 Store 和 Reducer
    - 创建 Store 实例：使用 createStore 函数创建一个 Store 实例，并传入 reducer 函数来管理状态。
    - 订阅状态变化：使用 Store 的 subscribe 方法注册一个监听函数，这样每当状态发生变化时，监听函数会被调用。
    - 派发动作：使用 Store 的 dispatch 方法发送动作，触发状态更新。
5. （可选）添加 Middleware
    - 功能：中间件用于扩展 Redux 的功能，例如处理异步操作或日志记录。
    - 实现：定义中间件的处理函数，在中间件中可以访问 dispatch 和 getState，并在动作派发之前或之后执行自定义逻辑。使用 applyMiddleware 函数将中间件应用到 Store。

## 重绘（Repaint）与回流（Reflow）
### 重绘（Repaint）
- 定义：重绘是指元素的外观发生变化，但不会影响布局。例如，改变元素的颜色、背景、opacity、可见性（如 visibility）、边框样式、css动画transform等。
- 触发条件：只需要改变元素的外观属性。
- 影响：重绘比回流消耗的性能相对较少，因为它不需要重新计算布局，只需要重新绘制受影响的元素。

### 回流（Reflow）
- 定义：回流是指元素的尺寸、布局或整个页面布局发生变化，需要重新计算元素的位置和几何形状。回流是更复杂、更耗时的操作。
- 触发条件：任何影响元素尺寸、位置、显示隐藏、增加或删除元素的操作都会触发回流。例如，改变元素的宽度、高度、边距、填充、边框大小，或者插入、删除 DOM 元素等。
- 影响：回流不仅会导致受影响的元素重新计算布局，还可能导致它的子元素、父元素甚至整个文档的重新布局，因此消耗性能较大。

### 如何优化重绘和回流
1. 批量操作 DOM：
	- 避免频繁的 DOM 操作，将多次 DOM 操作合并成一次。
	- 使用文档片段（DocumentFragment）批量插入多个元素。
2. 使用 CSS3 动画和变换：
	- 使用 transform 和 opacity 进行动画，因为它们通常只会触发重绘，而不会触发回流。
	- 避免使用会触发回流的属性（如 width, height, margin, padding 等）进行动画。
3. 减少布局计算：
	- 避免频繁访问会触发布局计算的属性和方法，如 offsetWidth, offsetHeight, clientWidth, clientHeight, getComputedStyle() 等。
	- 如果需要多次访问这些属性，可以将结果缓存起来，而不是每次都重新计算。
4. 避免触发同步布局：
	- 在一个操作中读取和写入 DOM，会导致强制同步布局，应该先读取所有需要的值，然后再写入。
5. 最小化 CSS 选择器的复杂性：
	- 使用高效的 CSS 选择器，避免使用过于复杂的选择器。
	- 使用类选择器和 ID 选择器，而不是后代选择器或通配符选择器。

## 动画多的项目解决方案
1. 使用 CSS 动画
	- 性能高：CSS 动画通常在浏览器的合成层上运行，避免了回流和重绘。
	- 简洁易用：使用 CSS 实现简单的过渡和动画非常直观。
2. 使用 CSS-in-JS 库
	- 在使用 CSS-in-JS 库（如 styled-components 或 emotion）时，也可以利用其支持的 CSS 动画特性。
3. 使用 JavaScript 动画库
	- react-spring
		- 轻量且功能强大的动画库，适用于 React 项目。
		- 提供了简单的 API 来创建平滑的物理动画。
	- framer-motion
		- 功能强大的动画库，适用于复杂动画需求。
		- 提供了详细的控制和丰富的动画效果。
4. 使用动画框架 GSAP
	- GreenSock Animation Platform 是一个功能强大且广泛使用的动画库。
	- 可以与 React 结合使用，处理复杂的时间轴动画和交互。
5. 优化动画性能
	- 使用 will-change
		- 在需要动画的元素上添加 will-change CSS 属性，可以提前告知浏览器哪些属性会改变，优化性能。
	- 减少动画数量
		- 尽量减少同时运行的动画数量，避免性能瓶颈。
	- 使用硬件加速
		- 使用 transform 和 opacity 进行动画，这些属性可以利用 GPU 加速，提升性能。
6. 使用合适的工具进行调试
	- React Profiler
		- 使用 React Profiler 分析和优化组件的渲染性能。
	- Performance DevTools
		- 使用浏览器的 Performance 工具分析动画的性能瓶颈，调整动画属性和时序，确保动画流畅运行。
7. 考虑用户体验
	- 确保动画不会影响用户的主要操作路径。
	- 提供简洁而不夸张的动画效果，增强用户体验，而不是分散注意力。

## JS
### 问题：解释 var、let 和 const 的区别。
#### var：
- 函数作用域（function scope）。
- 变量提升（hoisting），即在声明之前可以使用。
- 可以重新赋值和重新声明。
#### let：
- 块作用域（block scope）。
- 不允许在声明之前使用（暂时性死区，TDZ）。
- 可以重新赋值，但不能重新声明。
#### const：
- 块作用域（block scope）。
- 必须在声明时初始化。
- 不允许重新赋值，也不能重新声明（对象属性可以修改）

### 什么是闭包？请举例说明它的应用场景。
> 函数嵌套函数，使得内部函数可以访问外部函数的变量，即使外部函数已经返回。
#### 应用场景
- 数据封装：创建具有私有状态的对象。
- 工厂函数：生成具有私有状态的函数实例。

### 类型数据
#### 基础类型
- Number: 用于表示数值，可以是整数或浮点数。例如，42、3.14。
- String: 用于表示文本数据。字符串可以用单引号、双引号或反引号括起来。例如，'hello'、"world"、`template string`。
- Boolean: 只有两个值：true 和 false，用于表示逻辑上的真或假。
- Undefined: 表示未定义的值，当声明变量但未赋值时，变量的值就是 undefined。
- Null: 表示空值或不存在的值。null 是一个空对象指针。
- Symbol: ES6 引入的一种新的基本数据类型，表示唯一的标识符。使用 Symbol() 函数来创建。
- BigInt: ES2020 引入的数据类型，用于表示大整数。可以使用 BigInt 函数或在数字后面加 n 来创建，例如 1234567890123456789012345678901234567890n。
#### 引用类型
- Object: 用于表示对象，复杂数据结构的集合
- Array: 用于表示有序集合的列表。
- Function: 用于表示函数。
- Date: 用于表示日期和时间。
- RegExp: 用于表示正则表达式。
- Map: ES6 引入的键值对集合，键可以是任意类型。
- Set: ES6 引入的值集合，值是唯一的。
- WeakMap: 键是对象的 Map，对象的键是弱引用，可以被垃圾回收。
- WeakSet: 值是对象的 Set，对象的值是弱引用，可以被垃圾回收。
#### 判断类型的方法
- typeof：适用于基本数据类型的判断，但对于对象的判断不够精确。
- instanceof：适合判断对象是否为特定构造函数的实例，但不能判断基本数据类型。
- Object.prototype.toString：提供了最准确的对象类型判断，适合复杂数据类型，但使用起来较为复杂。
- Array.isArray：专门用于判断数组，准确性高，但仅限于数组类型。
- constructor 属性：可以确定对象的构造函数，但对于对象的具体类型可能不准确。
- Symbol.toStringTag：提供了自定义对象类型标识的功能，灵活但不适用于内置类型。
### new一个对象执行了哪些操作
```
创建对象：创建一个新的空对象，并将其 [[Prototype]] 链接到构造函数的 prototype 属性。
绑定 this：将构造函数内部的 this 绑定到新创建的对象上。
执行构造函数：在新对象的上下文中执行构造函数，初始化对象的属性和方法。
返回对象：返回新创建的对象，除非构造函数显式返回了一个不同的对象。
```
### 当js执行一段代码时他的执行流程是什么
### 内存回收
#### 内存回收的基本原理
> JavaScript 中，内存分为两种类型：栈内存和堆内存。
- 栈内存（Stack Memory）：用于存储基本类型的变量和函数调用。在函数调用时，栈内存会自动管理这些数据，当函数返回时，栈内存会自动释放这些数据。
- 堆内存（Heap Memory）：用于存储对象和引用类型。堆内存由垃圾回收器自动管理，当对象不再被引用时，垃圾回收器会自动释放这些对象占用的内存。
#### 垃圾回收的主要算法
```这是最常用的垃圾回收算法，能够有效处理循环引用的问题。```
- 标记-清除（Mark-and-Sweep）：
  - 标记阶段：垃圾回收器从根对象（如全局对象和当前执行上下文中的局部变量）开始遍历，并标记所有可以从根对象直接或间接引用的对象。
  - 清除阶段：垃圾回收器遍历堆中的所有对象，释放那些没有被标记的对象占用的内存。

- 引用计数（Reference Counting）：
  - 每个对象都有一个引用计数器，记录有多少个引用指向该对象。
  - 当一个新的引用指向该对象时，计数器加一；当一个引用不再指向该对象时，计数器减一。
  - 当计数器变为零时，该对象占用的内存可以被回收。
  - 引用计数的主要缺点是无法处理循环引用的问题。
#### 垃圾回收的优化策略
> 现代 JavaScript 引擎采用了多种优化策略来提高垃圾回收的效率和减少对程序运行的影响：
- 分代垃圾回收（Generational Garbage Collection）：
  - 将内存划分为几代（通常为新生代和老年代）。
  - 新生代存储新创建的对象，这些对象往往生命周期较短，垃圾回收器会频繁回收新生代内存。
  - 老年代存储生命周期较长的对象，垃圾回收器会较少回收老年代内存。
- 增量垃圾回收（Incremental Garbage Collection）：
  - 将垃圾回收过程分解为多个小步骤，穿插在程序的执行过程中，以减少一次性回收大量内存时的暂停时间。
- 并发垃圾回收（Concurrent Garbage Collection）：
  - 垃圾回收器与应用程序同时运行，进一步减少因垃圾回收造成的暂停时间。
- 紧凑化（Compaction）：
  - 回收未使用的内存后，重新整理内存布局，以减少内存碎片，提高内存分配效率。
#### 手动回收
1. 解除对象引用
    - 当一个对象不再需要时，可以通过将对该对象的引用设置为 null 或 undefined 来解除引用。
2. 清空数组和对象
    - 对于数组和对象，可以通过设置其长度为 0 或清空其属性来释放内存
3. 移除 DOM 元素
    - 在浏览器环境中，移除不再需要的 DOM 元素可以显著减少内存使用。可以使用 removeChild 或 remove 方法来移除元素
4. 取消事件监听器
    - 事件监听器会引用其绑定的元素，可能导致内存泄漏。确保在不再需要时移除事件监听器
5. 处理定时器
    - 未清除的定时器也会导致内存泄漏。确保在不再需要时清除定时器
6. 解除闭包中的引用
    - 闭包会保持对其上下文中的变量的引用，可能导致内存泄漏。确保在不再需要时解除闭包中的引用
7. 使用 WeakMap 和 WeakSet
    - 对于某些情况，可以使用 WeakMap 和 WeakSet 来存储对象引用，这些引用不会阻止垃圾回收

### 事件循环（Event Loop）概述
```
执行同步任务，直到调用栈为空。
执行所有的微任务。
执行一个宏任务（比如从 setTimeout 中获取的回调）。
重复上述步骤。
示例代码解释
```
### 函数
#### 定义函数有哪些方法，他们之间的区别是什么
| 函数类型                     | 是否提升       | `this` 绑定              | `arguments`对象 | 是否可以使用 `new` | 用途和场景                                |
| ---------------------------- | -------------- | ----------------------- | -------------- | ------------------ | ---------------------------------------- |
| 函数声明 (Function Declaration) | 是             | 调用时确定              | 有              | 可以               | 常规函数定义，代码组织结构清晰           |
| 函数表达式 (Function Expression) | 否             | 调用时确定              | 有              | 可以               | 动态定义函数，常用于回调和闭包           |
| 箭头函数 (Arrow Function)    | 否             | 从外层作用域继承        | 无              | 不可以             | 简洁语法，适合短小函数和嵌套函数         |
| 立即执行函数表达式 (IIFE)    | 否             | 调用时确定              | 有              | 不适用             | 创建独立作用域，避免全局污染             |
| 方法定义 (Method Definition) | 否             | 调用该方法的对象        | 有              | 不可以             | 对象方法，简洁定义                       |
| 类 (Class)                   | 否             | 类的实例                | 有              | 可以               | 类和面向对象编程                         |
| 生成器函数 (Generator Function) | 否             | 调用时确定              | 有              | 不可以             | 生成迭代器，逐步执行函数体               |

### import requset
### 如何开启多线程
## ES
### 有哪些新特性
1. 块级作用域
2. 箭头函数
3. 模板字符串
4. 解构赋值
5. 默认参数
6. 展开运算符
7. 模块化
8. Class
9. 生成器函数
10. Promise
11. Symbol
12. for...of
## Webpack/Vite
### 编写插件
## 网络
  - http2.0
  - websocket
## 算法
## Flutter
## css
- flux
- 布局方案
- 适配方案
- 媒体查询
- 响应式