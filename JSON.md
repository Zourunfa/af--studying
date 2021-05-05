## JSON是什么

> JSON,全称javascript Object Notation ，javascript对象标记法，JSON是一种轻量级，基于文本的，可读的格式，虽然全称中有javascript，但这是指其语法规则是参考javascript对象，而不仅仅用于javascript语言



json和xml的比较：

1. json没有结束标签，长度更短，读写更快
2. 能够直接被javascript解析器解析
3. 可以使用数组

JSON:

XML

![image-20210414104516042](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210414104516042.png)





## 字符串转化为对象

javascript不引用第三方库有两种方式

1. eval()
2. JSON.parse

```js
// JSON转化为JS可操作类型
let af = {
  name: 'af',
  age: 18,
  hobbis: {
    sports: ['basketball', 'football'],
    art: ['guitar', 'sing']
  }
}

let json = JSON.stringify(af)
console.log(json);
// 注意JSON.parse是可以穿第二个参数(回调函数) 用来操作转换后的格式
let obj = JSON.parse(json, (key, value) => {
  if (key == 'name') {
    value = '阿锋-' + value
  }
  return value
})
console.log(obj);



// 还有一种解析为对象的方法

let obj2 = eval('(' + json + ')')
console.log(obj);
```





## JSON和XML之间的转换

![image-20210414111319468](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210414111319468.png)