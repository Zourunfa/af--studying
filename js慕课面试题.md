## 强制类型转换  隐式类型转换

强制类型转换

- parseInt
- pareFloat
- toString

 隐式类型转换

- if
-  逻辑运算
-  == 
- +



## 闭包

- 函数作为参数被传入，作为返回值被返回
- 自由变量的查找，要在函数定义的地方，而非执行的地方
- 变量会常驻内存，得不到释放，闭包不要乱用



## 函数声明和函数表达式的区别

- 函数声明  function fn(){}
- 函数表达式 const fn = function(){}
- 函数声明会在代码执行前预加载（函数提升），函数表达式声明的时候就相当于一个变量赋值Undefined了





## 如何捕获js中的异常

 ![image-20210315144145050](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210315144145050.png)





## 什么是JSON

- json是一种数据格式，本质上是一段字符串
- json格式和js对象结构一致，对js语言更加友好
- window.JSON是一个全局对象:  JSON.stringgify JSON.parse

## 获取当前页面的url参数

- 传统方式：查找location.search
- 新的api，URLSearchParams ：   const params = new URLsearchParams('username')



## requestAnimationFrame

- 想要动画流畅，更新频率要60帧每秒.即16.67ms更新一次
- setTimeout要手动控制频率，而RAF浏览器会自动控制
- 后台标签或隐藏iframe中,RAF会暂停，而setTImeout依然执行







