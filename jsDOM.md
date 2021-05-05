**DOM: document Object Model**

js通常通过DOM来对html文档进行操作

- 文档：就是整个html文档
- 对象：对象表示将网页的每一个部分转换为一个对象
- 模型： 使用模型来表示对象之间的关系



**什么是节点**

节点（node）是构成我们网页的基础部分，网页中的每一个部分都可以称之为节点，分为下面四类

1. 文档节点  整个html文档
2. 元素节点  html元素中的标签
3. 属性节点  元素的属性
4. 文本节点  html中的文本内容
5. 注释节点





## DOM获取元素API

```js
window.idxxx或者直接输入id
document.getElementById('id')
document.getElementsByTagName('div')[0]
document.getElementsByClassName('red')[0]
document.querySelector("#id")
document.querySelectorAll("#id")[0]
//工作中常用document.querySelector("#id")，document.querySelectorAll("#id")[0]
document.querySelectorAll("#id")[0]
```

### 获取特定元素

```
document.documentElement // 获取大写的标签名HTML
document.head //获得head
document.body //获得body
window   //获得window window不是标签
document.all // 获得所有元素,document.all第六合falsy值，直接用可以
```

![image-20210313085904354](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210313085904354.png)

```javascript
//创建一个标签节点
let div1 = document.createElement('div')
document.createElement('style')
document.createElement('scripet')
document.createElement('li')
//创建一个文本节点
text1 = document.createTextNode('你好')
//标签里面插入文本
div1.appendChild(text1)
div1.innerText = '你好' 
div1.textContent = '你好'
//但是不能用div1.appendChild('你好')
```

### 添加节点

**appendChild的细节点**

```
//JS创建的div标签在页面是看不出来的，必须插入body标签才可以显现
document.body.appendChild(div1)//已在页面中的元素使用(.appendChild(div))
//apendChild需要注意的是多个位置添加同一个元素只会生效最后一个，前面的会被移除
//一个元素不能出现多个标签里  除非拷贝一份再次添加
let div2 = div1.cloneNode(true)//深拷贝
document.body.appendChild(div1)
document.head.appendChild(div2)
```

### 删除节点

彻底删除需要在remove后赋值为空

```
div1.parentNode.removeChild(div1)
//新方法
div1.remove()//兼容性较差
document.body.appendChild(div1)//重新添加回去
//彻底删除需要在remove后赋值为空
div1.remove()
div1 = null
```

### 改变节点

```
div1.id = 'newId'
div1.className = 'newClass' 
div1.className += '   newClass' 
//
div1.classList
div1.classList.add('green')
div1.style = 'color:blue' //不推荐，因为会覆盖之前的样式
div1.style.color = 'blue'
div1.style.backgroundColor = 'black'
div2.getAttribute('data-x')
div2.datase.x = 'frank'
//href注意要点
test.href//读取相对路径时浏览器会自动加上域名前缀
test.getAtrtribute('href')//正常操作
test.onclick = function(x){
    console.log(this)
    console.log(x)
}//test.onclick.call(test,event)
//改HTML内容
test.innerHTML = `<strong>hi</strong>`//修改HTML
//改标签内容
div.inerHTML = ''
div.appendChild(div2)
//改父级元素
newParent.appendChild(div)
```



### 查找元素

查找父级元素

```
node.parentNode 或者node.parentElement 
```

查父级元素的父级元素 (以此类推向上查找)

```
node.parentNode.parentNode //
node.childNodes或者node.children  //查子节点，需要注意的是，childNodes.length显示的长度包括每个节点前的换行，推荐使用后者,这两个方法会实时更新节点的length
let c = document.querySelector('li')
console.log(c.length)
test.querySelector('li').remove()
console.log(c.length)//querySelector不会更新节点的length
```

查兄弟姐妹,同级

```
node.parentNode.childNodes //需要排除自己
node.parentNode.childNodes //需要排除自己还有文本节点
let siblings = []
let c = div2.parentElement.children //获取所有同级元素
for(let i = 0;i<c.length;i++){
    if(c[i] !== div2){//排除自己
        siblings.push(c[i])
    }
}

```

查同级节点中第一个节点

```
node.firstChild
```

查看同级节点中最后一个节点

```
node.lastChild
```

查同级节点中上一个节点

```
node.nextSibling
```

查同级节点中下一个节点

```
node.nextSibling
```

### 递归查看一个div里面的所有元素

```
travel = (node,fn) =>{
    fn(node)
    if(!node.Children){
        for(let i=0;i<node.children.length;i++){
            travel(node.children[i],fn)
        }
    }
}
travel(div1,(node)=>{
    console.log(node)
})

```

## DOM操作是跨线程的

**浏览器分为渲染引擎和JS引擎**

![image-20210313091746272](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210313091746272.png)

![image-20210313091756474](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210313091756474.png)

![image-20210313091811794](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210313091811794.png)

### 属性同步

对div的标准属性的修改，会被浏览器同步到页面中
比如id、className、title等（data-*属性）

非标准属性
对非标准属性的修改,则只会停留在JS线程中，不会同步到页面中 代码示意

```
div.x = 'frank'
```

### Property vs Attribute

#### property属性

JS线程中div1的所有属性，叫做div1的property

#### Attribute也是属性

渲染引擎中div1对应标签的属性，叫做attribute

### 区别

大部分时候，同名的property和attribute值相等，但如果不是标准属性，那么它们两个只会在一开始时相等