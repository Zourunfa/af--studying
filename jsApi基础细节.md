''，undefined, null, 0， NaN。除了这5个假值以外

其他所有值转布尔类型都是true。一切对象都是真值，包括new Boolean(false



## for in 和 for of 的区别

无论是for...in还是for...of语句都是迭代一些东西。它们之间的主要区别在于它们的迭代方式。





**for...in 语句以原始插入顺序迭代对象的可枚举属性。**:

用于遍历对象的所有属性，`for/in`主要用于遍历对象，不建议用来遍历数组。**会找到对象的原型链上去**

**for...of 语句遍历可迭代对象定义要迭代的数据。**

用来遍历 Arrays（数组）, Strings（字符串）**, Maps（映射）, Sets（集合）**等可迭代的数据结构。





以下示例显示了与Array一起使用时，for...of循环和for...in循环之间的区别。


```js
Object.prototype.objCustom = function() {}; 
Array.prototype.arrCustom = function() {};
 
let iterable = [3, 5, 7];
iterable.foo = 'hello';
 
for (let i in iterable) {
  console.log(i); // logs 0, 1, 2, "foo", "arrCustom", "objCustom"
}
 
for (let i in iterable) {
  if (iterable.hasOwnProperty(i)) {
    console.log(i); // logs 0, 1, 2, "foo"
  }
}
 
for (let i of iterable) {
  console.log(i); // logs 3, 5, 7
}
```





## 可枚举属性 

- enumerable为true的属性即为可枚举属性
- 基本包装类型的原型属性都是不可枚举的,如Object,Array,Number
- Object,defineproperty()定义的属性默认不可枚举
- **Object.keys()只能获取自身的可枚举属性，无法获得原型链上面的可枚举属性**
- **for...in 遍历对象自身的和原型链上的可枚举属性**





## 可迭代对象

- Set
- Map
- String
- Array
- Arguments
- Nodelist

### 判断是否有可迭代的能力

当一个数据具备Symbol.iterator属性的时候，才可以用for...of进行迭代

```js
console.log(Array.prototype.hasOwnProperty(Symbol.iterator));
console.log(Set.prototype.hasOwnProperty(Symbol.iterator));
```





## console对象



## TDZ

TDZ又称暂时性死区，指变量在作用域内已经存在，但必须在let const 声明后才可以使用



```
console.log(str); //报错

let str = 'dahsahfaoe'

// 暂时性死区要求你必须先声明再使用


let a = 1

function fn() {
  console.log(a); //报错
  let a = 2
}



function run(a = b, b = 3) {//报错

}
run() // 
```



## 严格模式

**设置严格模式的目的**

　　- 消除Javascript语法的一些不合理、不严谨之处，减少一些怪异行为;

　　- 消除代码运行的一些不安全之处，保证代码运行的安全；

　　- 提高编译器效率，增加运行速度；

　　- 为未来新版本的Javascript做好铺垫。

**调用方式**

- **针对整个脚本文件**

```
<script>
　　　　"use strict";
　　　　console.log("这是严格模式。");
　　</script>

　　<script>
　　　　console.log("这是正常模式。");kly, it's almost 2 years ago now. I can admit it now - I run it on my school's network that has about 50 computers.
　　</script>
```

- **针对单个函数**

```
　function strict(){
　　　　"use strict";
　　　　return "这是严格模式。";
　　}

　　function notStrict() {
　　　　return "这是正常模式。";
　　}
```

**3.3 脚本文件的变通写法**

因为第一种调用方法不利于文件合并，所以更好的做法是，借用第二种方法，将整个脚本文件放在一个立即执行的匿名函数之中。

```
　(function (){

　　　　"use strict";

　　　　// some code here

　　 })();
```



**特点**

1. 禁止全局变量显示声明，也就是禁止变量没声明就默认它是全局变量这种行为
2. 静态绑定：Javascript语言的一个特点，就是允许"动态绑定"，即某些属性和方法到底属于哪一个对象，不是在编译时确定的，而是在运行时（runtime）确定的。严格模式对动态绑定做了一些限制。某些情况下，只允许静态绑定。也就是说，属性和方法到底归属哪个对象，在编译阶段就确定。这样做有利于编译效率的提高，也使得代码更容易阅读，更少出现意外
3. 禁止使用with语句
4. 创设eval作用域：正常模式下，Javascript语言有两种变量作用域（scope）：全局作用域和函数作用域。严格模式创设了第三种作用域：eval作用域，正常模式下，eval语句的作用域，取决于它处于全局作用域，还是处于函数作用域。严格模式下，eval语句本身就是一个作用域，不再能够生成全局变量了，它所生成的变量只能用于eval内部。
5. 全局环境下的this指向undefined
6. **禁止在函数内部遍历调用栈**
7. **禁止删除变量**
8. 严格模式下，对禁止扩展的对象添加新属性，会报错。
9. **对象不能有重名的属性** ,**函数不能有重名的参数**
10. **arguments对象的限制**：**不允许对arguments赋值**，**arguments不再追踪参数的变化** **禁止使用arguments.callee**







## **字符串方法快速记忆**

|    原型链上的方法    |                             作用                             |
| :------------------: | :----------------------------------------------------------: |
|    toUpperCase()     |                        转化为大写字母                        |
|    toLowerCase()     |                        转化为小写字母                        |
|        trim()        |                         移除左右空白                         |
| trimLeft(),trimRight |            单独移除左边的空白，单独移除右边的空白            |
|       charAt()       |                传入一个index，获取此索引的值                 |
| slice(),substring()  | 第一个参数是开始位置，第二个参数是结束位置，截取此范围的字符串 |
|        substr        |       和上面唯独不同的是第二个参数是代表截取字符的数量       |
|       indexOf        |                 传入参数为字符，返回其索引值                 |
|       search()       | 方法用于检索字符串中指定的子字符串，**也可以使用正则表达式搜索** |
|       includes       |      字符串中是否包含指定的值，第二个参数指查找开始位置      |
|      startsWith      |       是否是指定位置开始，第二个参数为查找的开始位置。       |
|       endsWith       |       是否是指定位置结束，第二个参数为查找的结束位置。       |
|       replace        |     方法用于字符串的替换操作，可以使用正则表达式完成替换     |
|        repeat        |     参数传数字，调用它的字符串会被重复生成 此数字的次数      |

###  类型转换

分隔字母

```text
let name = "hdcms";
console.log(name.split(""));
```

将字符串转换为数组

[
隐式转换](https://houdunren.gitee.io/note/js/3 基本类型.html#隐式转换)[显式转换](https://houdunren.gitee.io/note/js/3 基本类型.html#显式转换)[实例操作](https://houdunren.gitee.io/note/js/3 基本类型.html#实例操作)[Number](https://houdunren.gitee.io/note/js/3 基本类型.html#number)[声明定义](https://houdunren.gitee.io/note/js/3 基本类型.html#声明定义-3)[基本函数](https://houdunren.gitee.io/note/js/3 基本类型.html#基本函数)[NaN](https://houdunren.gitee.io/note/js/3 基本类型.html#nan)[类型转换](https://houdunren.gitee.io/note/js/3 基本类型.html#类型转换-2)[舍入操作](https://houdunren.gitee.io/note/js/3 基本类型.html#舍入操作)[浮点精度](https://houdunren.gitee.io/note/js/3 基本类型.html#浮点精度)[Math](https://houdunren.gitee.io/note/js/3 基本类型.html#math)[取极限值](https://houdunren.gitee.io/note/js/3 基本类型.html#取极限值)[舍入处理](https://houdunren.gitee.io/note/js/3 基本类型.html#舍入处理)[random](https://houdunren.gitee.io/note/js/3 基本类型.html#random)[Date](https://houdunren.gitee.io/note/js/3 基本类型.html#date)[声明日期](https://houdunren.gitee.io/note/js/3 基本类型.html#声明日期)[类型转换](https://houdunren.gitee.io/note/js/3 基本类型.html#类型转换-3)[对象方法](https://houdunren.gitee.io/note/js/3 基本类型.html#对象方法)[moment.js](https://houdunren.gitee.io/note/js/3 基本类型.html#moment-js)[数组类型](https://houdunren.gitee.io/note/js/4 数组类型.html)[Symbol](https://houdunren.gitee.io/note/js/5 Symbol.html)[Set](https://houdunren.gitee.io/note/js/6 Set.html)[Map](https://houdunren.gitee.io/note/js/7 Map.html)[函数进阶](https://houdunren.gitee.io/note/js/8 函数进阶.html)[作用域与闭包](https://houdunren.gitee.io/note/js/9 作用域与闭包.html)[对象](https://houdunren.gitee.io/note/js/10 对象.html)[原型与继承](https://houdunren.gitee.io/note/js/11 原型与继承.html)[类](https://houdunren.gitee.io/note/js/12 类.html)[模块设计](https://houdunren.gitee.io/note/js/13 模块设计.html)[正则表达式](https://houdunren.gitee.io/note/js/14 正则表达式.html)[Promise](https://houdunren.gitee.io/note/js/15 Promise.html)[任务管理](https://houdunren.gitee.io/note/js/16 任务管理.html)[Promise核心](https://houdunren.gitee.io/note/js/17 Promise核心.html)[DOM](https://houdunren.gitee.io/note/js/18 DOM.html)[空间坐标](https://houdunren.gitee.io/note/js/19 空间坐标.html)[事件](https://houdunren.gitee.io/note/js/20 事件.html)[网络请求](https://houdunren.gitee.io/note/js/21 网络请求.html)

## [#](https://houdunren.gitee.io/note/js/3 基本类型.html#类型检测)类型检测

> [houdunren.com (opens new window)](https://www.houdunren.com/)@ 向军大叔

JS提供了非常丰富的数据类型，开发者要学会使用最适合的数据类型处理业务 。

### [#](https://houdunren.gitee.io/note/js/3 基本类型.html#typeof)typeof

`typeof` 用于返回以下原始类型

- 基本类型：number/string/boolean
- function
- object
- undefined

可以使用typeof用于判断数据的类型

```text
let a = 1;
console.log(typeof a); //number

let b = "1";
console.log(typeof b); //string

//未赋值或不存在的变量返回undefined
var hd;
console.log(typeof hd);

function run() {}
console.log(typeof run); //function

let c = [1, 2, 3];
console.log(typeof c); //object

let d = { name: "houdunren.com" };
console.log(typeof d); //object
```

### [#](https://houdunren.gitee.io/note/js/3 基本类型.html#instanceof)instanceof

**`instanceof`** 运算符用于检测构造函数的 `prototype` 属性是否出现在某个实例对象的原型链上。

也可以理解为是否为某个对象的实例，`typeof`不能区分数组，但`instanceof`则可以。

> 后面章节会详细介绍原型链

```text
let hd = [];
let houdunren = {};
console.log(hd instanceof Array); //true
console.log(houdunren instanceof Array); //false

let c = [1, 2, 3];
console.log(c instanceof Array); //true

let d = { name: "houdunren.com" };
console.log(d instanceof Object); //true

function User() {}
let hd = new User();
console.log(hd instanceof User); //true
```

### [#](https://houdunren.gitee.io/note/js/3 基本类型.html#值类型与对象)值类型与对象

下面是使用字面量与对象方法创建字符串，返回的是不同类型。

```text
let hd = "houdunren";
let cms = new String("hdcms"); 
console.log(typeof hd, typeof cms); //string object
```

只有对象才有方法使用，但在`JS`中也可以使用值类型调用方法，因为它会在执行时将值类型转为对象。

```text
let hd = "houdunren";
let cms = new String("hdcms");
console.log(hd.length); //9
console.log(cms.length); //5
```

## [#](https://houdunren.gitee.io/note/js/3 基本类型.html#string)String

字符串类型是使用非常多的数据类型，也是相对简单的数据类型。

### [#](https://houdunren.gitee.io/note/js/3 基本类型.html#声明定义)声明定义

使用对象形式创建字符串

```text
let hd = new String('houdunren');
// 获取字符串长度
console.log(hd.length);
// 获取字符串
console.log(hd.toString());
```

字符串使用单、双引号包裹，单、双引号使用结果没有区别。

```text
let content = '后盾人';
console.log(content);
```

### [#](https://houdunren.gitee.io/note/js/3 基本类型.html#转义符号)转义符号

有些字符有双层含义，需要使用 `\` 转义符号进行含义转换。下例中引号为字符串边界符，如果输出引号时需要使用转义符号。

```text
let content = '后盾人 \'houduren.com\'';
console.log(content);
```

常用转义符号列表如下

| 符号 | 说明     |
| ---- | -------- |
| \t   | 制表符   |
| \n   | 换行     |
| \\   | 斜杠符号 |
| \'   | 单引号   |
| \"   | 双引号R  |

### [#](https://houdunren.gitee.io/note/js/3 基本类型.html#连接运算符)连接运算符

使用 `+` 可以连接多个内容组合成字符串，经常用于组合输出内容使用。

```text
let year = 2010,
name = '后盾人';
console.log(name + '成立于' + year + '年');
```

使用 `+=` 在字符串上追回字符内容

```text
let web = '后盾人';
web += '网址：houdunren.com';
console.log(web); //后盾人网址：houdunren.com
```

### [#](https://houdunren.gitee.io/note/js/3 基本类型.html#模板字面量)模板字面量

使用 ` ...` 符号包裹的字符串中可以写入引入变量与表达式

```text
let url = 'houdunren.com';
console.log(`后盾人网址是${url}`); //后盾人网址是houdunren.com
```

支持换行操作不会产生错误

```text
let url = 'houdunren.com';
document.write(`后盾人网址是${url}
大家可以在网站上学习到很多技术知识`);
```

使用表达式

```text
function show(title) {
	return `后盾人`;
}
console.log(`${show()}`)
```

模板字面量支持嵌套使用

![image-20191011025107379](https://houdunren.gitee.io/note/assets/img/image-20191011025107379.418bb650.png)

```text
let lessons = [
	{title: '媒体查询响应式布局'},{title: 'FLEX 弹性盒模型'},{title: 'GRID 栅格系统'}
];

function template() {
  return `<ul>
      ${lessons.map((item)=>`
          <li>${item.title}</li>
      `).join('')}
  </ul>`;
}
document.body.innerHTML = template();
```

### [#](https://houdunren.gitee.io/note/js/3 基本类型.html#标签模板)标签模板

标签模板是提取出普通字符串与变量，交由标签函数处理

```text
let lesson = 'css';
let web = '后盾人';
tag `访问${web}学习${lesson}前端知识`;

function tag(strings, ...values) {
    console.log(strings); //["访问", "学习", "前端知识"]
    console.log(values); // ["后盾人", "css"]
}
```

下面例子将标题中有后盾人的使用标签模板加上链接

![image-20191011032629455](https://houdunren.gitee.io/note/assets/img/image-20191011032629455.d2c72150.png)

```text
let lessons = [
  { title: "后盾人媒体查询响应式布局", author: "后盾人向军" },
  { title: "FLEX 弹性盒模型", author: "后盾人" },
  { title: "GRID 栅格系统后盾人教程", author: "古老师" }
];

function links(strings, ...vars) {
  return strings
    .map((str, key) => {
      return (
        str +
        (vars[key]
          ? vars[key].replace(
              "后盾人",
              `<a href="https://www.houdunren.com">后盾人</a>`
            )
          : "")
      ); 
    })
    .join("");
}

function template() {
  return `<ul>
    ${lessons
      .map(item => links`<li>${item.author}:${item.title}</li>`)
      .join("")}
</ul>`;
}
document.body.innerHTML += template();
```

### [#](https://houdunren.gitee.io/note/js/3 基本类型.html#获取长度)获取长度

使用`length`属性可以获取字符串长度

```text
console.log("houdunren.com".length)
```

### [#](https://houdunren.gitee.io/note/js/3 基本类型.html#大小写转换)大小写转换

将字符转换成大写格式

```text
console.log('houdunren.com'.toUpperCase()); //HOUDUNREN.COM
```

转字符为小写格式

```text
console.log('houdunren.com'.toLowerCase()); //houdunren.com
```

### [#](https://houdunren.gitee.io/note/js/3 基本类型.html#移除空白)移除空白

使用`trim`删除字符串左右的空白字符

```text
let str = '   houdunren.com  ';
console.log(str.length);
console.log(str.trim().length);
```

使用`trimLeft`删除左边空白，使用`trimRight`删除右边空白

```text
let name = " houdunren ";
console.log(name);
console.log(name.trimLeft());
console.log(name.trimRight()); 
```

### [#](https://houdunren.gitee.io/note/js/3 基本类型.html#获取单字符)获取单字符

根据从0开始的位置获取字符

```text
console.log('houdunren'.charAt(3))
```

使用数字索引获取字符串

```text
console.log('houdunren'[3])
```

### [#](https://houdunren.gitee.io/note/js/3 基本类型.html#截取字符串)截取字符串

使用 `slice、substr、substring` 函数都可以截取字符串。

- slice、substring 第二个参数为截取的结束位置
- substr 第二个参数指定获取字符数量

```text
let hd = 'houdunren.com';
console.log(hd.slice(3)); //dunren.com
console.log(hd.substr(3)); //dunren.com
console.log(hd.substring(3)); //dunren.com

console.log(hd.slice(3, 6)); //dun
console.log(hd.substring(3, 6)); //dun
console.log(hd.substring(3, 0)); //hou 较小的做为起始位置
console.log(hd.substr(3, 6)); //dunren

console.log(hd.slice(3, -1)); //dunren.co 第二个为负数表示从后面算的字符
console.log(hd.slice(-2));//om 从末尾取
console.log(hd.substring(3, -9)); //hou 负数转为0
console.log(hd.substr(-3, 2)); //co 从后面第三个开始取两个
```

### [#](https://houdunren.gitee.io/note/js/3 基本类型.html#查找字符串)查找字符串

从开始获取字符串位置，检测不到时返回 `-1`

```text
console.log('houdunren.com'.indexOf('o')); //1
console.log('houdunren.com'.indexOf('o', 3)); //11 从第3个字符向后搜索
```

从结尾来搜索字符串位置

```text
console.log('houdunren.com'.lastIndexOf('o')); //11
console.log('houdunren.com'.lastIndexOf('o', 7)); //1 从第7个字符向前搜索
```

search() 方法用于检索字符串中指定的子字符串，也可以使用正则表达式搜索

```text
let str = "houdunren.com";
console.log(str.search("com"));
console.log(str.search(/\.com/i));
```

`includes` 字符串中是否包含指定的值，第二个参数指查找开始位置

```text
console.log('houdunren.com'.includes('o')); //true
console.log('houdunren.com'.includes('h', 11)); //true
```

`startsWith` 是否是指定位置开始，第二个参数为查找的开始位置。

```text
console.log('houdunren.com'.startsWith('h')); //true
console.log('houdunren.com'.startsWith('o', 1)); //true
```

`endsWith` 是否是指定位置结束，第二个参数为查找的结束位置。

```text
console.log('houdunren.com'.endsWith('com')); //true
console.log('houdunren.com'.endsWith('o', 2)); //true
```

下面是查找关键词的示例

```text
const words = ["php", "css"];
const title = "我爱在后盾人学习php与css知识";
const status = words.some(word => {
  return title.includes(word);
});
console.log(status);
```

### [#](https://houdunren.gitee.io/note/js/3 基本类型.html#替换字符串)替换字符串

`replace` 方法用于字符串的替换操作

```text
let name = "houdunren.com";
web = name.replace("houdunren", "hdcms");
console.log(web);
```

默认只替换一次，如果全局替换需要使用正则（更强大的使用会在正则表达式章节介绍）

```text
let str = "2023/02/12";
console.log(str.replace(/\//g, "-"));
```

使用字符串替换来生成关键词链接

```text
const word = ["php", "css"];
const string = "我喜欢在后盾人学习php与css知识";
const title = word.reduce((pre, word) => {
  return pre.replace(word, `<a href="?w=${word}">${word}</a>`);
}, string);
document.body.innerHTML += title;
```

使用正则表达式完成替换

```text
let res = "houdunren.com".replace(/u/g, str => {
  return "@";
});
console.log(res);
```

### [#](https://houdunren.gitee.io/note/js/3 基本类型.html#重复生成)重复生成

下例是根据参数重复生成星号

```text
function star(num = 3) {
	return '*'.repeat(num);
}
console.log(star());
```

下面是模糊后三位电话号码

```text
let phone = "98765432101";
console.log(phone.slice(0, -3) + "*".repeat(3));
```

### 类型转换

分隔字母

```text
let name = "hdcms";
console.log(name.split(""));
```

**将字符串转换为数组**

```text
console.log("1,2,3".split(",")); //[1,2,3]
```

**别忘了join()将数组转化为字符串**

```
let arr = ['hdcms', '后盾人'];
console.log(arr.join('')); //hdcms后盾人
```







## Math对象



### [#](https://houdunren.gitee.io/note/js/3 基本类型.html#取极限值)取极限值

使用 `min` 与 `max` 可以取得最小与最大值。

```text
console.log(Math.min(1, 2, 3));

console.log(Math.max(1, 2, 3));
```

使用`apply` 来从数组中取值

```text
console.log(Math.max.apply(Math, [1, 2, 3]));
```

### 舍入处理

取最接近的向上整数

```text
console.log(Math.ceil(1.111)); //2
```

得到最接近的向下整数

```text
console.log(Math.floor(1.555)); //1
```

四舍五入处理

```text
console.log(Math.round(1.5)); //2
```

### random

`random` 方法用于返回 **>=0 且 <1** 的随机数（包括0但不包括1）。

返回0~5的随机数，不包括5

```text
const number = Math.floor(Math.random() * 5);
console.log(number);
```

返回0~5的随机数，包括5

```text
const number = Math.floor(Math.random() * (5+1));
console.log(number);
```



## nodeType（节点类型)

<ul id="container">
  <li>列表</li>
  <li>列表</li>
</ul>
<script> 
window.onload = function(){ 
  let obox = document.getElementById("container");
  let lis = obox.getElementsByTagName("li");
  console.log(lis[1].nodeType) // li为元素节点，对应nodeType为1
}
</script>

|       节点名称       | nodeType |
| :------------------: | -------- |
|       元素节点       | 1        |
|       属性节点       | 2        |
|       文本节点       | 3        |
|       注释节点       | 8        |
|   document文本节点   | 9        |
|   documentType节点   | 10       |
| documentFragment节点 | 11       |

1. div p span 等这些html标签所生成的为元素节点；
2. a标签中的href属性为一个属性节点；
3. 一个标签中的内容，例:回车 换行符 文本 称之为文本节点；
4. 注释节点；
5. document文本节点；
6. DocumentType节点；
7. documentFragment节点。





**如果将script标签放到要获取的dom节点前面，除了用defer属性延迟解析，还可以用异步方法解析成功 比如setTimeOut**





## Symbol

### 创建方式两种

注意第一种方式创建的Symbol，永远不相等

```js
let sy = Symbol()
console.log(typeof sy); //symbol

// 两个Symbol()对象永远不相等
let sy1 = Symbol()
console.log(sy === sy1); //false
```

**Symbol对象是不能够压入属性的**

```js
// Symbol对象是不能够压入属性的
sy.name = 'mySy'
console.log(sy.name); //undefined

console.log(sy.toString()); //Symbol()
// console.log(sy.description);
```

**使用Symbol.for定义系统会帮你记录，可以传入每个symbol的descripton**

```js
// 使用Symbol.for定义系统会帮你记录
let a = Symbol.for('hdcms')
let b = Symbol.for('hdcms')
console.log(a); //Symbol(hdcms)
console.log(a.description); //hdcms
console.log(a === b);
//true 事实证明用Symbol.for定义的相同描述的两个对象是相等的
// 不会重复创建
```

**使用Symbol.keyFor()传入symbol对象可以获取这个symbol对象的description**

```js
let c = Symbol('qwe')
console.log(Symbol.keyFor(a)); //hdcms
console.log(Symbol.keyFor(c)); //undefined
```

### Symbol的应用

#### 使用Symbol解决字符串耦合问题

> 当一个对象之中出现了两个相同的属性时，后面的会覆盖前面的，这个时候可以用symbol,具体用法就是，先给这两个属性分别变成对象，对象里面加上key属性，key属性对应一个Symbol



```js
// 使用Symbol解决字符串耦合问题
let grade = {
  '张明': { js: 100, css: 89 },
  '张明': { js: 59, css: 23 }
}
console.log(grade); //{张明: {…}} 只会有一个，前面的会被覆盖

// 用symbol

let user1 = {
  name: '张明',
  key: Symbol()
}
let user2 = {
  name: '张明',
  key: Symbol()
}

let newGrade = {
  [user1.key]: { js: 100, css: 89 },
  [user2.key]: { js: 59, css: 23 }
}
console.log(newGrade[user2.key]);
```

#### **扩展类型和对象类型保护**

> 对象中的symbol属性是不能用for in和for of+ Object.keys()进行遍历的
>
> 所以这点在一定基础上可以使一些属性没那么容易访问到

**注意symbol类型是不能用for in 和for of进行遍历的**

****

```js
// 注意symbol类型是不能用for in 和for of进行遍历的
for (let key in af) {
  console.log(key); //name
}
for (let key of Object.keys(af)) {
  console.log(key);//name
}

```

**想要遍历symbol属性必须换一个方法**

```js
// 下面的只能遍历symbol属性
for (const key of Object.getOwnPropertySymbols(af)) {
  console.log(key); //Symbol(这是一个sy)
}

// 想要遍历全部属性必须使用静态方法 Reflect.ownKeys
for (let key of Reflect.ownKeys(af)) {
  console.log(key);
}
```

#### 使用Symbol可以创建私有变量

> 注意：这里用到了闭包

```js
let createPrivate = (
  function () {
    const _private = Symbol('sy')
    class createPv {
      constructor(value) {
        this[_private] = value
      }
      getValue() {
        return this[_private]
      }
      setValue(value) {
        return this[_private] = value
      }
    }
    return createPv
  }
)();

let p1 = new createPrivate('1')

console.log(p1._private);
console.log(p1.getValue());
console.log(p1.setValue('蝴蝶效应'));
console.log(p1.getValue());

```

