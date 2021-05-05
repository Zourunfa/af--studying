## Vue3.0新特性

1. **性能**
   - 双向响应原理由`Object.defineProperty`改为基于ES6的`Proxy`，使其颗粒度更大，速度更快，且消除了之前存在的警告；
   - 重写了 Vdom ，突破了 Vdom 的性能瓶颈
   - 进行了模板编译的优化
   - 进行了更加高效的组件初始化

**2. Tree-Shaking 的支持**
支持了 tree-shaking （剪枝）：像修剪树叶一样把不需要的东西给修剪掉，使 Vue3 的体积更小。

需要的模块才会打入到包里，优化后的 Vue3.0 的打包体积只有原来的一半（13kb）。哪怕把所有的功能都引入进来也只有23kb，依然比 Vue2.x 更小。像 keep-alive 、 transition 甚至 v-for 等功能都可以按需引入。


**3.Composition API**
composition-api 是一个 Vue3 中新增的功能，它的灵感来自于 React Hooks ，是比 mixin 更强大的存在。

composition-api 可以提高代码逻辑的可复用性，从而实现与模板的无关性；同时使代码的可压缩性更强。另外，把 Reactivity 模块独立开来，意味着 Vue3.0 的响应式模块可以与其他框架相组合。


**4.Fragments**
不再限制 template 只有一个根节点。
render函数也可以返回数组了，有点像 React.Fragments

**Better TypeScript Support**
更好的类型推导，使得 Vue3 把 TypeScript 支持得非常好

**Custom Renderer API**
实现用DOM的方式进行 WebGL 编程

### Setup函数

`setup()`函数是Vue3.0中，专门为组件提供的新属性。它为基于`Composition API`的新特性提供了统一的入口。

> 在Vue3中，定义`methods`、`watch`、`computed`、`data`数据都放在了`setup()`函数中

#### 1. 执行时机

`setup()`函数会在`created()`生命周期之前执行。

![image-20210505111430457](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210505111430457.png)

#### 2. 接收props数据

`props`是`setup()`函数的一个形参，组件接收的`props`数据可以在`setup()`函数内访问到。

#### 3. context上下文对象

`context` 是 `setup()` 的第二个形参，它是一个上下文对象，可以通过 `context` 来访问Vue的实例 `this` 。

## Composition-Api

一组低侵入式的、函数式的 API，使得我们能够更灵活地「组合」组件的逻辑。

### 1. reactive()

`reactive()` 函数接收一个普通的对象，返回出一个响应式对象。

**在Vue2.x的版本中，我们只需要在 `data()` 中定义一个数据就能将它变为响应式数据，在 Vue3.0 中，需要用 `reactive` 函数或者 `ref` 来创建响应式数据。**

```js
// 在组件库中引入 reactive
import { reactive } from '@vue/    composition-api'

setup() {
    // 创建响应式对象
    const state = reactive({
        count:0
    });

    // 将响应式对象return出去，暴露给模板使用
    return state;
}

```

`

### 2. ref()

ref()` 函数可以根据给定的**值**来创建一个响应式的数据对象，返回值是一个**对象**，且只包含一个 `.value` 属性。

```js
// 引入 ref
import { ref } from '@vue/composition-api'

setup() {
    // 创建响应式对象
    const count = ref(0);

    return {
        count
    }
}

```

```js
<p>当前的count的值为：{{count}}</p>

<button @click="count++">点击增加count</button>

```

ref 的注意事项

在 setup() 函数内，由 ref() 创建的响应式数据返回的是对象，所以需要用 .value 来访问；

而在 setup() 函数外部则不需要 .value ，直接访问即可。

可以在 reactive 对象中访问 ref() 函数创建的响应式数据。

新的 ref() 会覆盖旧的 ref() 。


### 3. computed()

`computed()` 用来创建计算属性，返回值是一个 `ref()` 实例。按照惯例，使用前需要先引入。

- computed创建只读计算属性

  给 `computed()` 传入一个函数，可以得到一个只读的计算属性：

```js
const count = ref(1)

// 创建一个计算属性，使其值比 count 大 1
const bigCount = computed(() => count.value + 1)

console.log(bigCount.value) // 输出 2
bigCount.value++ // error 不可写
```

- computed创建可读可写计算属性

```js
const count = ref(1)

// 创建一个 computed 计算属性，传入一个对象
const bigCount = computed({
    // 取值函数
    get: () => (count.value + 1),
    // 赋值函数
    set: val => {
      count.value = val - 1
    }
})

// 给计算属性赋值的操作，会触发 set 函数
bigCount.value = 9
// 触发 set 函数后，count 的值会被更新
console.log(count.value) // 8

```

### 4. readonly()

传入一个响应式对象、普通对象或 ref ，返回一个**只读**的对象代理。这个代理是深层次的，对象内部的数据也是只读的

```js
const state = reactive({ count: 0 })

const copy = readonly(state)

watchEffect(() => {
  // 依赖追踪
  console.log(copy.count)
})

// state 上的修改会触发 copy 上的侦听
state.count++

// 这里只读属性不能被修改
copy.count++ // warning!

```

### 5. watchEffect()

`watchEffect()` 会立即执行传入的函数，并**响应式侦听其依赖**，并在其依赖变更时重新运行该函数。

```js
const count = ref(0)

// 初次直接执行，打印出 0
watchEffect(() => console.log(count.value))

setTimeout(() => {
  // 被侦听的数据发生变化，触发函数打印出 1
  count.value++
}, 1000)

```

停止侦听

`watchEffect()` 使用时返回一个函数，当执行这个返回的函数时，就停止侦听。

```js
const stop = watchEffect(() => {
  /* ... */
})

// 停止侦听
stop()

```

6. ### watch()

  composition-api 中的 watch 和 Vue2.x 中是一样的，watch 需要侦听数据，并执行它的侦听回调。默认情况下初次渲染不执行。

watch 与 watchEffect 的不同

watch 初次渲染不执行
watch 侦听的更具体
watch 可以访问侦听数据变化前后的值
watch 侦听单个数据源

侦听的数据可以是个 reactive 创建出的响应式数据（拥有返回值的 getter 函数），也可以是个 ref


```js
// 侦听一个 getter
const state = reactive({ count: 0 })
watch(
  () => state.count,
  (count, prevCount) => {
    /* ... */
  }
)

// 直接侦听一个 ref
const count = ref(0)
watch(count, (count, prevCount) => {
  /* ... */
})

```

`watch` 侦听多个数据源

在侦听多个数据源时，把参数以**数组**的形式给 `watch`

```js
watch([ref1, ref2], ([newRef1, newRef2],   [prevRef1, prevRef2]) => {
  /* ... */
})

```

## Composition-Api 依赖工具

下面介绍一下组合式API的依赖方法集：

### 1. isRef()

`isRef()` 顾名思义，是用来判断某个值是否为 `ref()` 创建出来的响应式的值。

当你需要展开某个可能为 `ref()` 创建的响应式的值的时候，会用到它：

```js
import { isRef } from '@vue/composition-api'

const unwrapper = isRef(foo) ? foo.value : foo

```

### 2. toRefs()

`toRefs()` 可以将 `reactive()` 创建出来的响应式对象转换成**内容为 ref 响应式的值的普通对象**

> 在搞清楚 toRefs() 的用法之前，我们需要先了解一下用 reactive() 和 ref() 创建出来的响应式对象的区别：
>
> 用 reactive() 创建的响应式对象，整个对象是响应式的，而对象里的每一项都是普通的值。当你把它用展开运算符展开后，整个对象的普通值都不是响应式的；
> 而用 ref() 创建的响应式的值，本身就是响应式的，并不依赖于其他对象。
>

所以当你需要展开 `reactive()` 创建的响应式对象，又不想让他们失去响应式特点的时候，就需要用 `toRefs()` 将它进行转换：

```js
import { toRefs } from '@vue/composition-api'

setup() {
    // 定义响应式数据对象
    const state = reactive({
        count: 0
    })

    // 定义简单的函数，使count每次+1
    const add = () => {
        state.count++
    }

    // 将setup函数的内容return出去，供外界使用
    return {
        // 将 state 展开导出，同时将其属性都转化为 ref 形式的响应式数据
        ...toRefs(state),
        add
    }
}

```

