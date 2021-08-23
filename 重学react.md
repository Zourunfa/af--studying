## React中性能优化的点

1. 函数改变作用域的修改放在constructor里面，这样可以保证这样的操作只会执行一次
2. setState 的底层就是将改变的数据放到异步的队列中，多次改变的操作放在一次中渲染
3. 虚拟DOM，diff算法
4. SCU，可以避免无畏的子组件渲染





## React的生命周期

### Initialization:初始化过程

> 到此步的构造函数已经初始化好props 等数据了

#### componentWillMount（）

**当组件即将被挂载到页面的时刻被执行，此时页面还没被挂载，一般只会在组件第一次挂载时被执行一次**

#### render（）

1. 无论是props还是state发生变化都会执行的函数
2. **当一个组件的父组件的render函数被执行的时候，父组件里面的子组件的render函数也会被重新执行（这样会消耗性能，所以产生了下面的SCU来判断子组件是否需要渲染）**

**在组件被渲染的过程中被执行，可以多次执行，注意除了render函数，所有的生命周期函数都可以不存在，但是自己必须自定义render函数**

#### componentDidMount（）

在组件被挂载到页面之后被执行，一般只会在组件第一次挂载时被执行一次，其实ajax的请求放在componentWillMount()也是没问题的，但是学到后面的时候可能会和这里面需要做的事情有冲突

一般用于发送和接收网络请求的代码，如果把ajax卸载render函数里面，因为render函数会被执行许多次，所以会请求多次，性能会极大被消耗，写在componentWillMount到后面遇到更多的技术是会有问题，所以最好写在这个函数里面，没一点问题





**无论是props还是state发生变化都会执行的函数如下**

### Updation：更新数据的过程

这个过程有如下这些生命周期函数

#### shoudComponentUpdate()

组件在被更新之前会执行此函数，要求你返回一个布尔值，原因是顾明思议（你的组件需要更改吗），如果是返回false，那么组件是不会被更新的（可以用于子组件可以节约性能）

在react的逻辑里面，只要父组件有更新，则子组件无条件更新
判断组件更新前后的文本，看是否真的需要重新渲染，减少重复渲染
![image-20210513092222261](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210513092222261.png)

**SCU一定要配合setState不可变值，才会有效**

#### componentWiiUpdate()

**这个是上一个生命周期函数返回true之后执行的函数，在组件即将被更新之前被执行**

#### render(）

**渲染过程中执行，可以多次执行**

#### componentDidUpdate（）

**组件更新完成之后被执行**

#### componentWillReceiveProps()

**这个是有props参数的子组件，在父组件render函数被重新执行了，才会执行的生命周期函数，如果是父组件第一次被显示，都是不会被执行的。在 shoudComponentUpdate()之前执行**

- 如果这个组件第一次存在与父组件中，不会被执行
- 如果这个组件已经存在与父组件中,才会被执行







## 用redux监听input改变的流程

![image-20210513110515267](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210513110515267.png)

![image-20210513112959013](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210513112959013.png)



输入框onchange 函数 监听 changeInputValue函数  先在此函数 发送一个action，action必须有type属性，告诉store，我这个组件要干什么。可以带上value值（input中输入改变后的值）**

![image-20210513110119650](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210513110119650.png)

![image-20210513110128618](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210513110128618.png)

![image-20210513110139016](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210513110139016.png)

**发送到store后，store会转发给reducer进行对此action的处理**，需要注意的是reducer是个纯函数，且只能接收state，并不能改变state,下面拿到state并复制一个新的然后改变新的再return

![image-20210513110349495](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210513110349495.png)

![image-20210513110420563](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210513110420563.png)

此时state改变了但是组件还没监听state的改变需要用到store.subscribe方法监听（订阅模式:监听store 只要store发生变化，那么就需要触发setState，更新组件的状态）

![image-20210513110733882](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210513110733882.png)

![image-20210513110746424](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210513110746424.png)





## redux拆分action和统一管理action

为避免拼写错误带来的困扰，提高代码的可维护性，有利于自动化测试

![image-20210513111918164](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210513111918164.png)

![image-20210513111957447](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210513111957447.png)

![image-20210513112022840](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210513112022840.png)

![image-20210513112038239](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210513112038239.png)





## 对于reducer纯函数的理解

> 纯函数是指：给定固定的输入在任何时刻就会有固定的输出，而且不会有任何的副作用

非纯函数是怎么的，如下,如果将inputvalue的值返回是一个不定值（时间）它就是非纯函数了，有ajax请求也不是了，

![image-20210513112642137](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210513112642137.png)

reducer也不能对参数修改，不然就产生了副作用了





## UI组件和逻辑组件

- UI组件负责渲染
- 逻辑组件负责js代码的逻辑





## redux发送异步请求

![image-20210513151402333](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210513151402333.png)

![image-20210513151248072](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210513151248072.png)

![image-20210513151321032](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210513151321032.png)





## redux-thunk中间件发送ajax请求

利于项目的维护和自动化测试

### 安装与配置

```js
import { createStore, applyMiddleware, compose } from 'redux'
import reducer from './reducer'
import thunk from 'redux-thunk'
// import rootReducer from './reducer/index'
const composeEnhancers =
  window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ ?
    window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({}) : compose;

const enhancer = composeEnhancers(
  applyMiddleware(thunk),
  // other store enhancers if any
);
const store = createStore(reducer,
  enhancer

)

export default store
```

**使用redux-thunk中间件之后使得action不仅可以返回一个对,而且还可以是一个函数了**

![image-20210513163855822](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210513163855822.png)

![image-20210513163837480](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210513163837480.png)

![image-20210513163941811](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210513163941811.png)





## 什么是redux的中间件

> 中间件肯定是指的谁和谁的中间

![image-20210513164653483](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210513164653483.png)