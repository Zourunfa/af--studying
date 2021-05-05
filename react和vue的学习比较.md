[TOC]



# react和vue的学习比较

1. vue3和react更加趋于一致
2. react比vue的学习成本高



## JSX语法

1. 大括号里面都可以放js的变量和表达式（动态的），vue里面用mustache语法或者动态绑定
2. class要写成className  
3. style={{}} 两层大括号，里面的括号其实表示是一个js对象，外面的大括号表示包含js代码
4. 三元运算符 {this.state.theme === 'black' ? blackBtn:whiteBen}
5. {this.state.theme === 'black' && blackBtn}
6. map数组的函数,key和vue的key一模一样（必填不能是index和random）





## react的事件和为啥要bind(this)



> 比较一下：事件写法和vue区别  onClick   VS   @click

1. 如果不bind绑定this，那么this在函数事件中会是undefined
2. 可以用静态方法定义，永远都会指向当前的实例（箭头函数）



### 事件中的event对象

1. 函数中可以传入event对象  event.preventDefault阻止默认行为  event.stopPropagation 阻止冒泡
2. react中的event不是原生的event是react封装过的event, 叫做SyntheticEvent
3. vue里面的event事件是原生的，事件会被挂载到当前元素
4. react中有一个属性可以获取原生的event,那就是  event.nativeEvent
5. event.nativeEvent.target 是触发事件的元素 event.nativeEvent.currentTarget
6. 所有的事件，被挂载到document上







## react中的受控组件，相当于vue中的v-model

![image-20210308084423258](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308084423258.png)

![image-20210308084430777](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308084430777.png)

![image-20210308084441583](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308084441583.png)



1. 动态绑定value的值，并且用onChange事件监听e.target.value
2. 同时在p标签输出this.state.name的值
3. 受控组件 不是双向绑定  如果  搞个点击事件改变this.state.name    所有地方都会受this.state.name的控制





## 父子组件的传值

### vue父传子

vue的父传子使用props（props还具有数据验证机制）

```vue
<div id="app">
  <!--<cpn v-bind:cmovies="movies"></cpn>-->
  <!--<cpn cmovies="movies" cmessage="message"></cpn>-->

  <cpn :cmessage="message" :cmovies="movies"></cpn>
</div>



<template id="cpn">
  <div>
    <ul>
      <li v-for="item in cmovies">{{item}}</li>
    </ul>
    <h2>{{cmessage}}</h2>
  </div>
</template>

<script src="../js/vue.js"></script>
<script>
  // 父传子: props
  const cpn = {
    template: '#cpn',
    // props: ['cmovies', 'cmessage'],
    props: {
      // 1.类型限制
      // cmovies: Array,
      // cmessage: String,

      // 2.提供一些默认值, 以及必传值
      cmessage: {
        type: String,
        default: 'aaaaaaaa',
        required: true
      },
      // 类型是对象或者数组时, 默认值必须是一个函数
      cmovies: {
        type: Array,
        default() {
          return []
        }
      }
    },
    data() {
      return {}
    },
    methods: {

    }
  }

  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊',
      movies: ['海王', '海贼王', '海尔兄弟']
    },
    components: {
      cpn
    }
  })
</script>


```



### vue子传父

1. 在子组件中，通过$emit()来触发事件
2. 在父组件中，通过v-on来监听子组件

```vue

<!--父组件模板-->
<div id="app">
  <cpn @item-click="cpnClick"></cpn>
</div>

<!--子组件模板-->
<template id="cpn">
  <div>
    <button v-for="item in categories"
            @click="btnClick(item)">
      {{item.name}}
    </button>
  </div>
</template>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
<script>

  // 1.子组件
  const cpn = {
    template: '#cpn',
    data() {
      return {
        categories: [
          {id: 'aaa', name: '热门推荐'},
          {id: 'bbb', name: '手机数码'},
          {id: 'ccc', name: '家用家电'},
          {id: 'ddd', name: '电脑办公'},
        ]
      }
    },
    methods: {
      btnClick(item) {
        // 发射事件: 自定义事件
        this.$emit('item-click', item)
      }
    }
  }

  // 2.父组件
  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊'
    },
    components: {
      cpn
    },
    methods: {
      cpnClick(item) {
        console.log('cpnClick', item);
        console.log(item.name);
        item.name = item.name+'successClick'
      }
    }
  })
</script>

```



### React父传子

父组件像子组件传递函数和属性

![image-20210308085943681](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308085943681.png)

子组件通过this.props接收

![image-20210308091026070](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308091026070.png)

![image-20210308090221314](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308090221314.png)

![image-20210308090947598](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308090947598.png)

这里有个名词叫做状态提升：将list属性 下发到两个组件



### React子传父

**将子元素传递给父元素,实际就是调用父元素传递进来的元素的回调方法,但是参数是子组件的，这个参数就是子组件想传的值，**完成子元素给父元素传值 **原理有点像JSONP的跨域请求原理**

**JSONP需要服务器端的支持，在服务器对传入的函数callback传进参数值**



```react
class ParentCom extends React.Component{
   constructor(props){
     super(props)
     this.state = {
       ChildDate:null
     }
 }
   render(){
     return(
       <div>
           <h1>子元素传递给父元素的数据:{this.state.ChildDate}</h1>
           <ChildCom setChildDate={this.setChildDate}/>
       </div>
     )
   }
//修改数据
   setChildDate=(data)=>{
     this.setState({
       ChildDate:data
     })
   }
}

class ChildCom extends React.Component{
 constructor(props){
   super(props)
   this.state = {
     msg:"helloworld,i am lallaalallalalalalal"
   }
}
 render(){
   return(
     <div>
         <button onClick ={this.senddate}>传递hello给父元素</button>
     </div>
   )
 }
 senddate=()=>{
   console.log(this.state.msg);
   //将子元素传递给父元素,实际就是子组件调用父主键传递进来的回调方法,但是参数是组件的，这个参数就是子组件想传的值，完成子元素给父元素传值
   console.log(this.props.setChildDate);
   this.props.setChildDate(this.state.msg);
 }
}

ReactDOM.render(<ParentCom />,document.querySelector('#root'));

```



## setState

1. setState在vue里面对应的就是data

![image-20210307112124584](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210307112124584.png)

2.state指存在于class组件之中，函数组件默认没有state,并且state要在构造函数中定义

![image-20210308091637743](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308091637743.png)



3.不要直接修改state,使用不可变数据（函数式编程），一定要使用setState函数

![image-20210308092000020](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308092000020.png)

4.setsState操作数组和对象 

**数组千万不要用push,pop方法**



![image-20210308093314309](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308093314309.png)

![image-20210308093757082](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308093757082.png)

![image-20210309151951313](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309151951313.png)



5.setState直接使用异步更新，第二个参数是在setState执行完页面渲染了之后在执行  

![image-20210308094303397](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308094303397.png)



setState还有一种传参方式

**传入回调函数，并在回调函数里面返回新的 `state` 对象，**这个回调函数有两个参数，

  **一个是接收前一个状态值作为第一个参数，并将更新后的值作为第二个参数**

6.实例中的里面的setTimeOut又是同步的（batchUpdate）

```js
        setTimeout(() => {
            this.setState({
                count: this.state.count + 1
            })
            console.log('count in setTimeout', this.state.count)
        }, 0)

```

7.自己定义的DOM事件,setState是同步的

![image-20210308100355687](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308100355687.png)

8.，传入对象 会被合并 类似于 Object.assign

只有传入函数是，才不会被合并，执行结果是3

![image-20210308100946663](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308100946663.png)

![image-20210308101216707](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308101216707.png)

## React生命周期

[react生命周期]: https://blog.csdn.net/weixin_43342105/article/details/107317422?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522161517053116780266277466%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&amp;request_id=161517053116780266277466&amp;biz_id=0&amp;utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v1~rank_blog_v1-1-107317422.pc_v1_rank_blog_v1&amp;utm_term=react%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F

![image-20210308102207399](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308102207399.png)



## Vue（自定义事件通讯）

 $event.emit   绑定自定义事件（尽量绑定函数名字，方便event.$off在beforeUpdate时解绑）

 $event.on   调用自定义事件

**可以通过实例一个vue实例Bus作为媒介，要相互通信的兄弟组件之中，都引入Bus，之后通过分别调用Bus事件触发和监听来实现组件之间的通信和参数传递。**

## Vue中的ref

![image-20210309143028796](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309143028796.png)





![image-20210309143042204](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309143042204.png)



## React中的Ref

在组件的class函数中通过React.createRef()方法创建ref

![image-20210309143201422](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309143201422.png)

![image-20210309143334156](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309143334156.png)

![image-20210309143348127](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309143348127.png)

可用于非受控组件（只能拿到input中的值，并不能控制值的变化）

特点：

input中的值不受state控制；

只是拿state赋了个初始默认值

显示不会和输入同步显示   但之后可以拿出来

![image-20210309144132683](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309144132683.png)



## Vue中的watch



在vue中，使用watch来响应数据的变化。watch的用法大致有三种。下面代码是watch的一种简单的用法：

```
<input type="text" v-model="cityName"/>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
new Vue({
  el: '#root',
  data: {
    cityName: 'shanghai'
  },
  watch: {
    cityName(newName, oldName) {
      // ...
    }
  } 
})
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

直接写一个监听处理函数，当每次监听到 cityName 值发生改变时，执行函数。也可以在所监听的数据后面直接加字符串形式的方法名：

```
watch: {
    cityName: 'nameChange'
    }
 } 
```

### immediate和handler

**这样使用watch时有一个特点，就是当值第一次绑定的时候，不会执行监听函数，只有值发生改变才会执行。如果我们需要在最初绑定值的时候也执行函数，则就需要用到immediate属性。**

比如当父组件向子组件动态传值时，子组件props首次获取到父组件传来的默认值时，也需要执行函数，此时就需要将immediate设为true。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
new Vue({
  el: '#root',
  data: {
    cityName: ''
  },
  watch: {
    cityName: {
    　　handler(newName, oldName) {
      　　// ...
    　　},
    　　immediate: true
    }
  } 
})
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

监听的数据后面写成对象形式，包含handler方法和immediate，之前我们写的函数其实就是在写这个handler方法；

immediate表示在watch中首次绑定的时候，是否执行handler，值为true则表示在watch中声明的时候，就立即执行handler方法，值为false，则和一般使用watch一样，在数据发生变化的时候才执行handler。

### deep

当需要监听一个对象的改变时，普通的watch方法无法监听到对象内部属性的改变，只有data中的数据才能够监听到变化，此时就需要deep属性对对象进行深度监听。

```
<input type="text" v-model="cityName.name"/>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
new Vue({
  el: '#root',
  data: {
    cityName: {id: 1, name: 'shanghai'}
  },
  watch: {
    cityName: {
      handler(newName, oldName) {
      // ...
    },
    deep: true,
    immediate: true
    }
  } 
})
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

设置deep: true 则可以监听到cityName.name的变化，此时会给cityName的所有属性都加上这个监听器，当对象属性较多时，每个属性值的变化都会执行handler。如果只需要监听对象中的一个属性值，则可以做以下优化：使用字符串的形式监听对象属性：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
watch: {
    'cityName.name': {
      handler(newName, oldName) {
      // ...
      },
      deep: true,
      immediate: true
    }
  }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

这样只会给对象的某个特定的属性加监听器。

数组（一维、多维）的变化不需要通过深度监听，对象数组中对象的属性变化则需要deep深度监听。

## vue父子组件的生命周期

react的父子组件生命周期和vue一样

### 加载渲染过程

父组件走到beforeMount之后就会等待子组件走完mounted之后再挂载上去

父beforeCreate->父created->父beforeMount->子beforeCreate->子created->子beforeMount->子mounted->父mounted

### 子组件更新过程



父beforeUpdate->子beforeUpdate->子updated->父updated

### 父组件更新过程

父beforeUpdate->父updated

### 销毁过程

父beforeDestroy->子beforeDestroy->子destroyed->父destroyed

## vuex的基本使用

![image-20210308165134471](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308165134471.png)

- actions才可以进行异步操作

- 全局单例模式

  ### state（响应式的）单一状态树

  > state中保存的变量所有的组件都能够共享

  ```json
  // 保存状态
    state: {
      counter:1000,
      students:[
        {id:110,name:'lizhi',age:18},
        {id:111,name:'theshy',age:22},
        {id:112,name:'zouxinxin',age:38},
      ],
      info:{
        name:"caixukun",
        hobbis:['rap','basketball'],
        age:22
      }
    },
  
  ```

  

  ### Mutations使用方法

  `mutations`方法都有默认的形参：

  (**[state]** **[,payload]**)

  - `state`是当前`VueX`对象中的`state`
  - `payload`是该方法在被调用时传递参数使用的
  
  例如，我们编写一个方法，当被执行时，能把下例中的name值修改为`"jack"`,我们只需要这样做
  
  ```
  index.js
  ```
  
  
  
  ```js
  import Vue from 'vue'
  import Vuex from 'vuex'
  
  Vue.use(Vuex)
  
  const store = new Vuex.store({
      state:{
          name:'helloVueX'
      },
      mutations:{
          //es6语法，等同edit:funcion(){...}
          edit(state){
              state.name = 'jack'
          }
      }
  })
  
  export default store
  ```
  
  ### mutation传递参数
  
  - mutation里面携带的额外的参数被称为载荷payload可以是一个对象
  
    ![image-20210308172814673](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308172814673.png)

![image-20210308172827788](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308172827788.png)

### Mutation的提交风格

![image-20210308173049267](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308173049267.png)

### Mutation中的方法必须是同步方法

![image-20210308173139313](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308173139313.png)



### Getter基本使用

> 有时候我们需要从store中获取一些state变异后的状态



![image-20210308173259763](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308173259763.png)



### action

![image-20210308173422475](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308173422475.png)

接受异步组件

### module

![image-20210308173552588](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308173552588.png)



![image-20210308173601657](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308173601657.png)



## Vue-router 

原理：h5的 hash  和 history

**使用步骤**

1. 导入路由对象，并且调用Vue.use(vueRouter)
2. 创建路由实例，并且传入路由映射配置
3. 在Vue实例中挂载穿件的路由实例



![image-20210309102022120](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309102022120.png)

挂载



![image-20210309102042306](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309102042306.png)



**具体使用router创建实例的过程**

1,创建路由组件

![image-20210309102727791](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309102727791.png)

2，在router/index.js文件中配置组件和路径的映射关系

![image-20210309102815778](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309102815778.png)

3,使用路由

![image-20210309102925980](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309102925980.png)







## React的函数组件

![image-20210308104402297](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308104402297.png)

**函数组件的特点**

1. 纯函数，输入props,输出JSX
2. 没有实例，没有生命周期，没有state
3. 不能扩展其他方法



## React中的Portals

![image-20210309144652873](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309144652873.png)

其中的this.props.children （等于子组件的全部内容）

这里的createPortal方法能定义组件渲染的位置，



![image-20210309144942422](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309144942422.png)





## React中的context

公共信息（语言，和主题）传给每个组件，

用props肯定太繁琐了（嵌套层数多 )

用redux又有点小题大做

所以提供了context



![image-20210309145700208](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309145700208.png)





## Vue ,React中异步组件

当组件比较大，路由需要懒加载时  使用异步组件（性能优化）

vue中用：

```
Vue.component('async-example', function (resolve, reject) {
setTimeout(function () {   
// 向 `resolve` 回调传递组件定义    
resolve({      template: '<div>I am async!</div>'    }) 
}, 1000) })
```



react中：

React.lazy

React.Suspense

![image-20210309150359860](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309150359860.png)

![image-20210309150418258](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309150418258.png)





## React中的性能优化

性能优化对于React更加重要

### shouldComponentUpdate(简称SCU)

判断组件更新前后的文本，看是否真的需要重新渲染，减少重复渲染

![image-20210309152138799](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309152138799.png)

**<u>在react的逻辑里面，只要父组件有更新，则子组件无条件更新</u>** 



但是scu只有需要的时候才用

**SCU一定要配合不可变值**





## PureComponent和memo

在SCU中很容易会比较多层次的数组和对象，实现深度比较

会很耗费性能，于是出现了pureComponent

- PureComponent,SCU中实现了浅比较
- memo就是函数组件中的PureComponent



![image-20210309155228051](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309155228051.png)

![image-20210309155234145](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309155234145.png)

## immutable.js

- 彻底使用immutable.js  可以彻底拥抱不可变值
- 基于共享数据（不是深拷贝,类似于深拷贝）,速度好
- 有一定的学习和迁移成本，按需使用

![image-20210309155515287](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309155515287.png)





## React高阶组件

关于组件公共逻辑的抽离

- mixin（vue也有）早已被react废气
- HOC  high order components
- renderProps

> HOC本质上是一个函数，利用封装的思想，就是将多个组件的公共逻辑抽离。能够将以参数形式传入的组件  塞入这些公共逻辑 后在返回

![image-20210309155829416](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309155829416.png)

网上常见的一种需求就是：很多组件需要监听 鼠标的位置值（x,y）值

![image-20210309181607962](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309181607962.png)



![image-20210309182031426](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309182031426.png)



redux 的connect就是一个高阶组件



**高阶组件的作用：**

**属性代理**: 相当于在我们在调用封装组件之前, 先加一层，处理我们需要做的事情，比如我们需要将添加和减少传入下一个组件的props等

**反向继承**: 相当于该组件继承我们传入的组件，然后通过super来调用父类的render方法

**组合渲染**: 通过高阶组件拼接多个组件 ✔

**条件渲染**: 根据传入条件按条件渲染组件 ✔

**操作props**: 增加或者减少传入组件的props ✔

获取refs: 通过refs获得组件对象 ×

状态管理: 通过HOC来绑定一些状态属性，双向绑定 ✔

操作state: 利用反向代理来操作原组件的state ✔

## Render Props

- 先是一个组件处理好内部数据后， 使用renturn this.props.render()
- 在要使用的函数中包裹这个组件



高阶组件是组件包裹函数，render Props确是函数包裹组件，通过一个函数将一个class组件的state传递给纯函数组件

```react
import React from 'react'

/**
 * 1、先创建组件，在组件内部声明 复用的 state 以及操作 state 的方法
 * 2、将要复用的数据作为 props.render(state) 方法的参数，暴露到组件外部
 */

 // 不能使用函数组件，因为函数组件没有自己的私有数据
 //  1、先创建组件，在组件内部声明 复用的 state 以及操作 state 的方法
 class Mouse extends React.Component {
   state = {
     x: 0,
     y: 0
   }

   handleMouseMove = (e) => {
     this.setState({
       x: e.clientX,
       y: e.clientY
     })
   }

   componentDidMount() {
     window.addEventListener('mousemove',this.handleMouseMove)
   }

   render() {
     // 2、要复用的数据作为 props.render(state) 方法的参数，暴露到组件外部
    //  return this.props.render(this.state)
    return this.props.render()
   }
 }

 class App extends React.Component {
   render() {
     return (
       <div>
         {/* 在使用组件时，添加一个值为函数的prop，通过函数传参  */}
         <Mouse render={(Mouse) => (
           // 如何渲染任意的 UI 
           // 使用该函数的返回值作为要渲染的 UI 内容
          <div>
            <p>x：{Mouse.x}</p>
            <p>y：{Mouse.y}</p>
          </div>
         )}/>
       </div>
     )
   }
 }

 export default App
```

![image-20210309183840732](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309183840732.png)




## Redux使用

> 基本概念



![image-20210309185356424](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309185356424.png)





## react-rooter

**路由的基本使用 **

- 明确好界面中的导航区，展示区

- 导航区的a标签改为Link标签  <Link to="/xxx">Demo</Link>

- 展示区写Route标签进行路径的匹配<Route path="/xxxx" component ={Demo}/>

- <App>的最外侧包裹了一个<BrowserRouter>或<HashRouter>

  

**路由组件与一般组件**

1. 写法不同   

   ​	一般组件：<Demo/>

   ​	路由组件：<Route path="/demo" component={demo}/>

2. 存放位置不同

   ​	一般组件：components目录下

   ​	路由组件：pages下

3. 接收到的props不同 

   ​	一般组件：写组件标签时传递了什么就接收到什么

   ​	路由组件：接收到三个固定的属性(history  location  match)







# React原理

## 函数式编程

- 一种编程范式，概念比较多
- 纯函数
- 不可变值



## 回顾vdom和diff

- h函数() 传入一些属性和子节点，返回一个vnode数据结构
- patch函数 



## JSX的本质

在babel中文网中可以解析jsx代码成js代码



React.createElement函数  相当于 h函数

- 第一个参数传标签名 
- 第二个参数 传入标签的属性
- 第三个参数传入子组件

**`createElement`函数返回的对象是`ReactEelement`对象。**



```
ReactDOM.render`函数把`ReactElement`渲染成真是的`DOM元素
```



## React合成事件的机制

react事件的特点：

- 所有的事件挂载到document上

- event不是原生的，是SyntheticEvent
- 和vue事件不同，和DOM事件也不相同





![image-20210309193548014](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309193548014.png)



现象：

1. event是SyntheticEvent，模拟出来DOM事件所有能力
2. event.nativeEvent是原生事件对象
3. 所有的事件，都被挂载到document上
4. 和DOM事件不一样，和vue事件也不一样





## 为什么要用合成事件机制

- 更好的兼容性和跨平台
- 挂载到到document，减少内存消耗，避免频繁解绑
- 方便事件的统一管理(事物机制)





## setState 和 batchUpdate

setState()的特点：

- 有时是异步（普通使用），有时是同步的(setTimeout,DOM事件)
- 有时合并(对象形式 object.assign)，有时不合并(函数形式)



setState的主流程：

![image-20210309194533899](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309194533899.png)



**在执行setState的新数据会被放置在一个pending队列里面，先要判断其是否处于batch update的机制中，如果是就走左边的异步线路更新，如果不是就走入右边的线路，通过同步的方式更新**

![image-20210309195018565](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309195018565.png)





- 所以setState的本质上无所谓异步还是同步
- 主要是看是否命中batchUpdate机制
- 主要是判断isBatchingUpdates这个布尔值 遇到setTimeout好像还有DOM事件都会变成false 反而变成同步的



![image-20210309195315783](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309195315783.png)





## 组件更新和渲染的过程



# Vue原理

## 如何理解mvvm(model view viewModel)

1. 说到MVVM模式要从它诞生的目的说起，(其实组件化的概念早就出现了)传统组件化，只是静态渲染，跟新需要操作DOM
2. react 和vue都是数据驱动视图
3. vue MVVM           React setState

![image-20210308111130911](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308111130911.png)

## 监听data的核心api

1. 组件data的数据一旦变化，立刻触发视图的跟新
2. vue2.0实现响应式的核心api Object.defineProperty()    (vue3.0使用proxy)
3. proxy的兼容性不好

> 比较了一下object.defineProperty第二个传入的参数是属性值，是要写死的，但是proxy拦截的是任意的key，而且proxy里面不只可以拦截get和setter属性



object.defineProperty的监听模拟代码

```js
// 触发更新视图
function updateView() {
    console.log('视图更新')
}

// 重新定义数组原型
const oldArrayProperty = Array.prototype
// 创建新对象，原型指向 oldArrayProperty ，再扩展新的方法不会影响原型
const arrProto = Object.create(oldArrayProperty);
['push', 'pop', 'shift', 'unshift', 'splice'].forEach(methodName => {
    arrProto[methodName] = function () {
        updateView() // 触发视图更新
        oldArrayProperty[methodName].call(this, ...arguments)
        // Array.prototype.push.call(this, ...arguments)
    }
})

// 重新定义属性，监听起来
function defineReactive(target, key, value) {
    // 深度监听 (需要深度监听)
    observer(value)

    // 核心 API
    Object.defineProperty(target, key, {
        get() {
            return value
        },
        set(newValue) {
            if (newValue !== value) {
                // 深度监听
                observer(newValue)

                // 设置新值
                // 注意，value 一直在闭包中，此处设置完之后，再 get 时也是会获取最新的值
                value = newValue

                // 触发更新视图
                updateView()
            }
        }
    })
}

// 监听对象属性 这个函数只是一个入口函数
function observer(target) {
    if (typeof target !== 'object' || target === null) {
        // 不是对象或数组
        return target
    }

    // 污染全局的 Array 原型
    // Array.prototype.push = function () {
    //     updateView()
    //     ...
    // }

    if (Array.isArray(target)) {
        target.__proto__ = arrProto
    }

    // 重新定义各个属性（for in 也可以遍历数组）
    for (let key in target) {
        defineReactive(target, key, target[key])
    }
}

// 准备数据
const data = {
    name: 'zhangsan',
    age: 20,
    info: {
        address: '北京' // 需要深度监听
    },
    nums: [10, 20, 30]
}

// 监听数据
observer(data)

// 测试
// data.name = 'lisi'
// data.age = 21
// // console.log('age', data.age)
// data.x = '100' // 新增属性，监听不到 —— 所以有 Vue.set
// delete data.name // 删除属性，监听不到 —— 所有已 Vue.delete
// data.info.address = '上海' // 深度监听
data.nums.push(4) // 监听数组

```

Object.defineproperty的缺点

1. 需要深度监听对象的话，需要一次性递归到底，耗费性能大
2. 无法新增删除属性
3. 对数组监听需要重新定义数组的原型





## 响应式系统的构建大致过程

1. 数据初始化- initState   initState的过程中会对props,methods,data,computed和watch做数据的初始化并将他们转化为响应式的对象

2. 父组件props优先模板编译成一个render函数，遇到子组件属性就会进入创建子组件的状态

3. 子组件处理props的过程是发生在组件的update阶段，这个阶段是`Vnode`生成真实节点的过程，期间会遇到子`Vnode`,这时会调用`createComponent`去实例化子组件。而实例化子组件的过程又回到了`_init`初始化

4. 接着就会调用initProps,initProps里面有一个重要的api,object.defineProperty,可以代理对象的getter，setter属性

5. initMethods 保证`methods`方法定义必须是函数，且命名不能和`props`重复，最终会将定义的方法都挂载到根实例上。

6. 所以`initData`方法会先执行拿到组件的`data`数据，并且会对对象每个属性的命名进行校验，保证不能和`props，methods`重复。最后的核心方法是`observe`,`observe`方法是将**数据对象标记为响应式对象**，并对对象的每个属性进行响应式处理

7. `omputed`可以是对象，也可以是函数，但是对象必须有`getter`方法,因此如果`computed`中的属性值是对象时需要进行验证。

   针对`computed`的每个属性，要创建一个监听的依赖，也就是实例化一个`watcher`,`watcher`的定义，可以暂时理解为数据使用的依赖本身，一个`watcher`实例代表多了一个需要被监听的数据依赖。

   
   

## 虚拟DOM

1. DOM的操作非常耗费性能
2. 虚拟dom就是用js模拟dom结构

## diff算法

1. 通过diff算法找出两颗用js模拟的dom树的差别
2. 判断最小的更新和渲染的范围
3. git diff可以判断两个文件的差异



#### diff算法的细节（为什么要使用key）

- 只比较同一层级，不跨级比较
- tag不相同，则直接删掉重建，不再深度比较
- tag和key，两者都相同，则认为是相同节点，不再深度比较
- **深度优先遍历**，同一层级比较**，两个节点之间比较会根据他们石是否拥有子节点或者文本节点做不同操作；比较两组子节点是算法的重点，首先是头尾节点做4次对比尝试，如果没有找到相同节点才会按照通用的方式查找**



### snabbdom源碼解读（解读不清）

1. h.ts文件返回一个vnode函数
2. vnode.ts返回一个对象
3. init方法返回一个patch方法
4. patch(Vnode,Element)  sameNode方法





## vue中的key

- key的值可以是number，也可以是string。
- key主要作用于Vue的virtual DOM算法，在diff new nodes list和old nodes list时，作为识别VNode的一个线索。
- 如果不用key，Vue会用一种算法：最小化element的移动，并且会尝试尽最大程度在同适当的地方对相同类型的element，做patch或者reuse。
- 如果使用了key，Vue会根据keys的顺序记录element，**曾经拥有了key的element如果不再出现的话，会被直接remove或者destoryed。**
- 拥有同一个parent的children必须有unique keys。**重复的key的导致render error。**

![image-20210309085850510](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210309085850510.png)

没有key插入一个元素 会将后面的元素依次往前面排，从最后插入

有key了之后 key和节点一一对应 直接将元素插入中间

**绑定key时的注意点：**

1. 绑定key时 key要和渲染的数组元素 一一对应 

2. 千万不要用index,和random

   

## 模板编译

1. 模板不是html,有指令，插值，JS表达式，能实现判断，循环，一定会转化为js代码

2. 模板转化为js代码为模板编译

3. 模板编译为render函数，执行render函数返回vnode

4. 用webpack环境打包时会做好

5. 可以用render代替template

6. with语法，将{}中的自由变量当做obj属性查找，如果找不到会报错

   

![image-20210308144503044](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308144503044.png)





## Vue组件渲染更新的过程

#### 初次渲染的过程

1. 解析模板为render函数（或在开发环境已完成,vue-loader)
2. 触发响应式，监听data属性的getter setter方法 
3. 执行render函数，生成vnode,patch(elem,vnode)

#### 更新过程

1. 修改data,触发setter(此前在getter中已被监听)
2. 重新执行render函数，生成newVnode
3. 执行patch(vode,newVnode)





#### 流程图

vue2.0最重要的API就是Object.defineProperty ,可以监听对象的getter和setter属性，主要使用发布订阅的模式，也就是观察者模式，其实前端很多地方都用到了这种模式，我感觉一个事件监听就是这种，还有promise 对象.then()方法监听resolved状态，.catch()监听rejected状态

​	

- 模板编译成render函数后，执行render函数会生成vdom树，在这个执行的过程中会 Touch getter属性
- 在模板中触发了那个变量的getter，就会把那个变量观察起来，这个过程称之为收集依赖，
- 依赖修改data的时候，通过setter观察修改的那个data是不是先前被观察起来的，如果是就会re-render。**但是一个数据改变后不会立即刷新，会有一个队列，只要观察到数据的变化，队列就会缓冲同一事件循环中所有发生改变的数据**

![image-20210308145230537](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308145230537.png)

![image-20210308145802336](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308145802336.png)

![image-20210311083045357](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210311083045357.png)

##### 什么时候触发了依赖的收集

- 准备：**（1）**在created和beforeMounted之间生成了AST和render可执行函数，用到了compile的createCompiler生成对象结构的AST语法树，**（2）**再通过**generator**调用了之前初始化的方法处理各种vue内置的方法&指令，如v-for v-if等，通过createElement、createTextVnode等生成虚拟DOM树，**作为updateComponent函数返回**
- 渲染：（3）在beforeMounted和mounted之间通过将执行这个函数，这个函数会将虚拟DOM渲染成真正的DOM，内含有patch的布丁算法，在执行过程中**会触发渲染函数，这时会触发数据属性的get函数，而这个过程的观察者就是渲染函数，在渲染过程中添加数据的订阅者**，watcher订阅者是Observer和Compiler之间的桥梁，将在自身实例化的时候将自己添加到dep当中，watcher自身有一个depend和update方法，待属性变动dep.notice通知调用自身的update方法触发Compile中绑定的回调

**自己组织语言**



> 这个收集依赖的过程可以详细讲一下，对应于vue生命周期的created和beforeMounted阶段，用到编译函数会生成对象结构的抽象语法书和render函数，再通过createElement,createTextVnode等生成虚拟DOM数，将这些做为一个函数被返回
>
> 对应与vue生命周期的beforeMounted和mounted阶段将执行这个函数，会将虚拟DOM渲染成真正的DOM，在执行过程中会触发渲染函数，这是就会触发数据的get属性，而这个过程中观察者就是渲染函数，在渲染过程中添加数据的订阅者，



> 可以看出在react中也是和Vue中的一样，state的值在修改了之后并不会立即被修改，而是也有一个类似的队列，setState通过一个队列机制实现state的更新。当执行setState时，会把需要更新的state合并后放入状态队列，而不会立刻更新this.state，利用这个队列机制可以高效的批量的更新state。
>



### 异步渲染

- 减少DOM的操作次数
- nextTick( () =>)



## 前端路由的原理

### SPA（单页面应用）的理解

> **单页面应用的优势：**
>
> - **无刷新体验**：没有了令人诟病的页面频繁刷新，同时节约浏览器资源，路由响应比较及时，提升了用户的体验。
> - 共享组件：**前端组件化**是将独立完整的功能模块封装到一个组件中，代码结构更加规范，便于代码维护，同时模块化后的组件可以在不同的场景中进行复用，极大地加快了迭代开发的速度。这也是为什么主流的前端框架都提倡组件化编程的原因。
> - 共享API：给后端减负，前端加码的好处就是，前端能有一点口粮吃了（开玩笑，前端那么牛怎么能没饭吃呢？），前端担起家务的事，后端就可以安心地处理业务逻辑了，于是才能写出高质量并可共享的API，供自己或者其他的合作伙伴使用。一个优秀的产品背后，一定有一群出色的前端（小生脸皮太厚）。
>
> **单页面应用的劣势：**
>
> - 抬高了前端门槛：SPA模式的流行，引领了前端技术的飞速发展，与此同时对前端人员在学习和使用上的能力就有了更高的要求，同时工作量也增加了，前端想活的更好就要付出的更多，所以不要再以为前端就是切切图，画画页面这么简单。too young, too naive。
> - **首次加载大量资源**：既然只有一个页面显示，那许多功能元件（组件）所依赖的静态资源就需要在初次时进行加载，加载时间相对比较长。
> - **不利于SEO**：单页面应用，数据都是在前端进行渲染的，所以就影响了SEO。



### vue-router的路由模式

#### hash模式

![image-20210308152012864](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308152012864.png)

- hash变化会触发页面跳转，即浏览器的前进后退
- hash变化不会刷新页面，SPA页面必须的特点
- hash永远不会被提交到server端（前端自生自灭）
- onhashchange可以监听hash值的改变
- location.hash可以获取到页面的hash值

```javascript
 // hash 变化，包括：
        // a. JS 修改 url
        // b. 手动修改 url 的 hash
        // c. 浏览器前进、后退
        window.onhashchange = (event) => {
            console.log('old url', event.oldURL)
            console.log('new url', event.newURL)

            console.log('hash:', location.hash)
        }

        // 页面初次加载，获取 hash
        document.addEventListener('DOMContentLoaded', () => {
            console.log('hash:', location.hash)
        })

        // JS 修改 url
        document.getElementById('btn1').addEventListener('click', () => {
            location.href = '#/user'
        })
```

#### H5 history模式

- 用url规范的路由，但跳转时不刷新页面

- history.pushState （浏览器不会刷新页面）

- window.onpopstate

- **replaceState(),**直接改变了url

  这种方式没有保存历史记录，页面不可返回。

  **go()与forward()**

  ![image-20210316113621479](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210316113621479.png)

跳转也不刷新页面

```js
  // 页面初次加载，获取 path
        document.addEventListener('DOMContentLoaded', () => {
            console.log('load', location.pathname)
        })

        // 打开一个新的路由
        // 【注意】用 pushState 方式，浏览器不会刷新页面
        document.getElementById('btn1').addEventListener('click', () => {
            const state = { name: 'page1' }
            console.log('切换路由到', 'page1')
            history.pushState(state, '', 'page1') // 重要！！
        })

        // 监听浏览器前进、后退
        window.onpopstate = (event) => { // 重要！！
            console.log('onpopstate', event.state, location.pathname)
        }
```

# Vue面试真题

##  v-show和v-if的区别

- 手段：v-if是动态的向DOM树内添加或者删除DOM元素；v-show是通过设置DOM元素的display样式属性控制显隐
- 编译过程：v-if切换有一个局部编译/卸载的过程，切换过程中合适地销毁和重建内部的事件监听和子组件；v-show只是简单的基于css切换
- 编译条件：v-if是惰性的，如果初始条件为假，则什么也不做；只有在条件第一次变为真时才开始局部编译（编译被缓存？编译被缓存后，然后再切换的时候进行局部卸载); v-show是在任何条件下（首次条件是否为真）都被编译，然后被缓存，而且DOM元素保留
- 性能消耗：v-if有更高的切换消耗；v-show有更高的初始渲染消耗
- 使用场景：v-if适合运营条件不大可能改变；v-show适合频繁切换
- 相同点 v-show 都可以动态控制着dom元素的显示隐藏
- 不同点：v-if 的显示隐藏是将DOM元素整个添加或删除，v-show 的显示隐藏是为DOM元素添加css的样式display，设置none或者是block，DOM元素是还存在的
- 在渲染多个元素的时候，可以把一个 元素作为包装元素，并使用v-if 进行条件判断，最终的渲染不会包含这个元素，v-show是不支持 语法

## 为何v-for和v-if不能够连用

> 因为v-for的优先级比v-if的优先级要高，再做循环在看v-if的真假，这样十分消耗性能。但是可以嵌套使用，v-if可以写在v-for的外面或里面。



![image-20210416124241473](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210416124241473.png)

## 为何在v-for中使用key

- 只能用key，而且不能是index和random
- diff算法通过tag和key来判断，石佛那个是sameNode
- 减少渲染次数，提升渲染的性能



## Vue组件的通讯

- 父子组件props和this.$emit
- 自定义事件event.$on event.$off event.$emit
- vuex



## 双向数据绑定v-model实现的原理

- input 元素 动态绑定 :value = this.name
- 绑定 input事件this.name = $event.target.value
- data更新触发re-render

## computed和Methods的区别

- computed是基于响应式数据进行缓存的，只有在响应式数据发生改变时，他们才会重新求值。也就是说计算属性是有缓存的，能够提升性能
- computed中的成员还可以定义成get/set变成可读写属性,method方法没有



## 为什么data必须是一个函数

![image-20210308160234619](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308160234619.png)

- 这其实根本上是个js基础知识的问题
- 因为export default中导出的对象，经过编译后实际上是一个class
- 所以使用这个组件的时候实际上是对这个组件的实例化
- 所以在生成多个组件之后为了是各自的data值不相互影响，data必须是引用类型



## ajax请求应该放在哪个生命周期

1. 应该放在mounted生命周期，因为mounted阶段js代码渲染刚好完成

2. js是单线程的，ajax异步获取数据

   

## 如何将所有的props传递给子组件

- 使用$props![image-20210308160955738](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308160955738.png)

## 如何自定义v-model

![image-20210308161101340](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308161101340.png)

## 何时使用异步组件

- 加载大组件的时候（编辑器或图表）
- 路由异步加载的时候



## 何时使用keep-alive

- 缓存组件，不需要重复渲染
- 多个静态tab页面的切换（动态组件）
- 优化性能

## 何时需要使用beforeDestory

- 解绑自定义事件event.$off
- 清楚定时器
- 解绑自定义的DOM事件，window scroll



## 什么是作用域插槽

> 一句话概括就是父组件替换插槽的标签，但是内容用子组件提供
>
> 本来父组件只能访问到自己父级的作用域的，但是作用域插槽就是使得父组件能够访问到子组件的数据



下面看一个实例

![image-20210308163027621](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308163027621.png)

```javascript

<body>

    <div id="app">
        <cpn></cpn>
        <cpn>
            <br>
            <br>
           <!-- 目的是获取子组件的pLanguage -->
          <template slot-scope="slot">
              <span v-for="item in slot.data">{{item}} - </span>
              <br>
            <span>{{slot.data.join(' - ')}}</span>
          </template>
        </cpn>
    </div>
    <template id="cpn">
        <div>
           <slot :data="pLanguages"><li v-for="item in pLanguages">{{item}}</li></slot>
        </div>
    </template>
    <script>
/**
父组件替换插槽的标签，但是内容是由子组件来提供
*/

        const cpn = {
            template:'#cpn',
            data(){
                return{
                   pLanguages: ['javascript','c++','go','java']
                }
            }
        }
        var vm = new Vue({
            el: '#app',
            data: {
                isShow:false
            },
            methods: {},
            components:{
                cpn
            }
        });
    </script>
</body>

```

## Vuex中的action和mutation有何区别

- action中处理一步，mutation不可以
- mutation一般做原子操作，一下只做一个操作
- action可以整合多个mutation



## 请用vnode描述一个DOM结构

![image-20210308164623605](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308164623605.png)





# React面试真题

## React中的key的作用

> 说道react中的key，就不得不提到diff算法，diff算法在组件更新的时候会比较新旧两颗DOM数的节点，求得最小的更新渲染范围，其中key就是判断节点渲染更新的一个重要标志，元素节点的key要和元素节点又一一对应的关系，否则渲染的时候会有大部分的元素移动。因为key要和元素一一对应 所以key是不能用Index和random随机函数的



## 传入setState的第二个参数的作用是什么

其实setState有两种写法：

1. 第一种是对象写法：第一个参数以对象的形式修改state里面的值，第二个参数可以传入一个回调函数，里面可以进行一些异步操作，和vue的nextTick很像。
2. 第二种是函数写法：**一个是接收前一个状态值作为第一个参数，并将更新后的值作为第二个参数**





## react中的refs的作用是什么

ref的作用一句话概括就是为了获取DOM元素对象，可以通过,React.ref()和useRef() 创建一个属性之后，再到元素节点ref属性引入，这样就可以获取这个元素节点对象





## 在生命周期中哪一步发起ajax请求



React是在componentDidMounted阶段发送ajax请求，componentDidMounted一般只会在组件挂载之后被执行，如果放在componentWillMountd，会触发多次影响性能，放在其他函数中，不能保证请求仅在组件挂载完毕后才会响应







## shouldComponentUpdate作用

因为react有一个重要的细节，那就是父组件的更新一定会触发子组件的更新，所以这点有时候会造成很多不必要的渲染，shouldComponentUpdate这个周期函数就是为了阻止不必要的子组件渲染的，可以根据组件前后的状态数据值，判断组件是否要更新





## componentWillReceiveProp和getDerivedStateFromProps

componentWillReceiveProps是React生命周期函数之一，在初始props不会被调用，它会在组件接受到新的props时调用。一般用于父组件更新状态时子组件的重新渲染

**如果子组件需要的数据是直接由父组件通过props传递过来就不需要在componentwillreceiveprops中setState，但是如果子组件有自己的状态，同时这个状态依赖于父组件的数据，那么就需要子组件单独进行setState操作，否则无法完成子组件的更新。**

**同时有一点，在componentwillreceiveprops中写setState不会再次触发子组件的render。解释一下：父组件setState时会触发子组件的render，此时如果在子组件的componentwillreceiveprops里setState，给人的感觉是会再一次触发子组件的render（加上前面的一次就是两次），实际情况是不会的**



<u>尽量不要在componentWillReceiveProp中使用setState，因为如果没有结束条件会触发无限重渲染</u>





**替换方案：getDerivedStateFromProps**

```react
static getDerivedStateFromProps(nextProps,prevState){
 //该方法内禁止访问this
 if(nextProps.email !== prevState.email){
 //通过对比nextProps和prevState，返回一个用于更新状态的对象
 return {
            value:nextProps.email
 }
 }
 //不需要更新状态，返回null
 return null
}
```



```react
componentDidUpdate(prevProps, prevState, snapshot){
 if(this.props.email){
 // 做一些需要this.props的事
 }
}
```



通过以上使用方法，React相当于把componentWillReceiveProps拆分成getDerivedStateFromProps和componentDidUpdate。拆分后，使得派生状态更加容易预测。







## React绑定事件的时候为什么不能够在onClick使用匿名函数

> 如果onClick使用的是匿名函数，所有的每次重新渲染的时候，会把该onClick 当做一个新的prop来处理，会将内部缓存的onClick事件进行重新赋值，相对于直接使用函数，会降低性能



## 虚拟DOM

虚拟DOM的原因就是操作真实DOM的代价太高了，虚拟dom用js模拟DOM相当于在js和真实DOM之间加了一个缓存，实现虚拟DOM的手段就是把每一个节点抽象成对象，里面它的属性又 标签名  属性 孩子节点，比较虚拟DOM更新的算法是diff算法，

diff算法：

- 只会比较虚拟DOM树的统一层级
- 会比较tag和key，相同就不在深度比较，如果tag不相同 会直接删掉重建
- **调用component的setState方法的时候，React将其标记为-dirty，每一个时间循环结束，React检查所有标记的dirty的component重新绘制**
- 选择性子树渲染，开发人员重写SCU提高diff的性能







## react和vue使用的重要思想

1. MVVM-双向数据绑定，修改数据的时候还会同步DOM操作
2. 虚拟DOM-
3. diff算法





# vue3基础




## Ref

> ref数据响应式监听。ref 函数传入一个值作为参数，一般传入基本数据类型，返回一个基于该值的响应式Ref对象，该对象中的值一旦被改变和访问，都会被跟踪到，就像我们改写后的示例代码一样，通过修改 count.value 的值，可以触发模板的重新渲染，显示最新的值

```javascript
<template>
  
  <h1>{{name}}</h1>
  <h1>{{age}}</h1>
  <button @click="sayName">按钮</button>
</template>

<script lang="ts">
import {ref,computed} from 'vue' 

export default {
  name: 'App',
  setup(){
    const name = ref('zhangsan')
    const birthYear = ref(2000)
    const now = ref(2020)
    const age = computed(()=>{
      return now.value - birthYear.value
    })
    const sayName = () =>{
      name.value = 'I am ' + name.value
    }
    return {
      name,
      sayName,
      age
    }
  }
}
</script>


```


## reactive 

> reactive是用来定义更加复杂的数据类型，但是定义后里面的变量取出来就不在是响应式Ref对象数据了


`所以需要用toRefs函数转化为响应式数据对象`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127154139758.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzM0MjEwNQ==,size_16,color_FFFFFF,t_70#pic_center)
将上面用ref写的代码转化成reactive型的代码
```javascript
<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
  <div>
    <h1>{{ name }}</h1>
    <h1>{{ age }}</h1>
    <button @click="sayName">按钮</button>
  </div>
</template>

<script lang="ts">
import { computed, reactive,toRefs } from "vue";

interface DataProps {
  name: string;
  now: number;
  birthYear: number;
  age: number;
  sayName: () => void;
}

export default {
  name: "App",
  setup() {
   

    const data: DataProps = reactive({
      name: "zhangsan",
      birthYear: 2000,
      now: 2020,
      sayName: () => {
        console.log(1111);
        console.log(data.name);
        
        data.name = "I am " + data.name;
        console.log(data.name);
      },
      age: computed(() => {
        return data.now - data.birthYear;
      }),
    });

    const refData = toRefs(data)
    refData.age
    return {
      ...refData,
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```





### vue3自定义hook



