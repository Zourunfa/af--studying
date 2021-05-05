# Vue-router 钩子函数

#### 路由配置文件中的钩子函数

beforeEnter

**有三个参数：**

- to:路由将要跳转的路径信息，信息是包含在对象里面的
- from:路径跳转前的路径信息，也是一个对象的形式
- next:路由的控制参数，常有next(true)  next(false)



在路由配置文件(/src/router/index.js)中写钩子函数

![image-20210324190813687](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210324190813687.png)



#### 写在模板中的钩子函数

**beforeRouteEnter**

> 在路由进入前的钩子函数，不能，获取组件实例，因为组件实例还没被创建

**beforeRouteUpdate**

> 当前路由改变，该组件被复用时，对于一个带有动态参数的路径/foo/:id,在/foo/1和/foo/2之间的跳转，由于会渲染同样的Foo组件，因此组件会被复用，而这个钩子就会在这个情况下被调用

**beforeRouteLeave**

> 在路由离开前的钩子函数，可以访问组件实例 'this'