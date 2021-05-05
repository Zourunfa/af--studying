## 简单上手 Node.js 程序 



### 解析执行 JavaScript

```
/ test.js
console.log("Hello world!");
```

在 cmd 窗口对应文件夹下执行：

```
node test.js  // 结果：Hello world!
```

### 读写文件

```
readFile('要读取的文件路径',function(error, data){});
```

```
writeFile('要写入的文件路径','要写入的文件内容',function(error){});
```

```
// 加载文件模块
var fs = require("fs");
// writeFile:写入文件
// 第一个参数是要写入的文件路径
// 第二个参数是要写入的文件内容
// 第三个参数是一个回调函数
fs.writeFile('./data/test.txt', '江苏海洋大学', function(error) {
    console.log("写入文件成功！");
})
// readFile：读取文件
// 第一个参数是要读取的文件路径
// 第二个参数是一个回调函数
fs.readFile('./data/test.txt', function(error, data) {
    if (error) {
        console.log("读取文件失败！");
    } else {
        console.log(data.toString());
    }
});
```

### 简单的 HTTP 服务器

```
/// <reference path="./typings/index.d.ts" />
// 1.加载http模块
var http = require('http');
// 2.创建一个Server实例
var server = http.createServer();
// 3.注册请求和响应事件
// request：获取客户端的一些请求信息，如请求路径等
// response：给客户端发送响应消息
server.on('request', function(request, response) {
    // 返回请求路径
    console.log('收到客户端的请求了，请求路径是：' + request.url);
    // 根据不同的请求路径返回不同的响应结果
    // write()方法可以向客户端发送响应消息
    // end()用来结束响应，否则客户端会一直等待
    // 也可以直接使用end()来发送消息，发送后关闭响应
    response.setHeader('Content-Type', 'text/plain;charset=utf-8'); // 设置字符集编码
    if (request.url === '/') {
        response.end('首页');
    } else if (request.url === '/login') {
        response.end('登录页');
    } else if (request.url === '/register') {
        response.end('注册页');
    } else {
        response.end('未找到该页面');
    }
});
// 4.绑定端口号，启动服务器
server.listen(8888, function() {
    console.log('服务器启动成功，可以通过 http://127.0.0.1:8888/ 来访问！');
})
```

## Node.js 中的 JavaScript



**浏览器中的 JS 和 Node.js 中的 JS 区别**



- 浏览器中的 JS：

- - 由 ECMAScript、BOM 和 DOM 组成
  - 没有文件操作能力

- Node.js 中的 JS：

- - 仅由 ECMAScript 组成，没有 BOM 和 DOM
  - 可以操作文件

## 初步实现 Apache 功能

```
// 初步实现Apache功能
// 1. 加载模块
var http = require('http'); // 加载http模块
var fs = require('fs'); // 加载文件操作模块
// 2.创建一个Server实例
var server = http.createServer();
var wwwDir = 'C:/Users/lyy/Desktop/NodeJS/www';
// 3.注册请求和响应事件
server.on('request', function(req, res) {
    var url = req.url; // 获取请求路径
    var filePath = '/index.html'; // 文件路径
    // 如果获取的 url 不是 '/'，则将改 url 赋值给文件路径
    if (url !== '/') {
        filePath = url;
    }
    // 响应客户端请求
    //res.setHeader('Content-Type', 'text/plain;charset=utf-8'); // 设置字符集编码
    fs.readFile(wwwDir + filePath, function(err, data) {
        if (err) {
            return res.end('404 Not Found.');
        }
        return res.end(data);
    })
});
// 4.绑定端口号，启动服务器
server.listen(8888, function() {
    console.log('Server is running, you can visitied by http://127.0.0.1:8888/ ');
})
```

## 服务端渲染与客户端渲染



服务端渲染就是在服务端使用模板引擎，模板引擎最早诞生于服务端，后来才发展到了前端

客户端渲染是异步的，很难被爬虫抓取到，所以不利于 SEO 搜索引擎优化

网站一般采用异步和服务器渲染结合的方式

## Node.js 中的包管理



## NPM



### 什么是 NPM



NPM（node package manager）是 Node.js 的包管理工具



NPM 可以根据依赖关系，把所有依赖的包都下载下来并管理起来



- 检查 npm 版本：`npm -v`
- 安装包：`npm install art-template`  （install 可以简写为 i）
- 安装多个包（中间用空格隔开）：`npm i art-template jquery bootstrap`
- 初始化项目(会自动生成一个package.json 文件)：`nmp init` （`npm init -y` 全部选项默认）

```
{
  "name": "npm-demo",
  "version": "0.0.1",
  "description": "这是一个测试项目",
  "main": "main.js",
  "dependencies": {
    "art-template": "^4.13.2"
  },
  "devDependencies": {},
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "lyy",
  "license": "ISC"
}
```

### 使用淘宝镜像 CNPM 代替 NPM 



由于 NPM 存储包文件的服务器在国外，所以下载包会很慢，这时可以使用[淘宝镜像](http://npm.taobao.org/)



**方法一**



1. 安装淘宝镜像的 `cnpm`



```
npm install --global cnpm
```



- 在任意目录执行都可以
- `--global` 表示安装到全局，而非当前目录



1. 使用 ` cnpm` 安装包



```
# 通过淘宝的服务器来下载 jQuery
cnpm install jquery
```



注意：安装 `cnpm` 之后，`npm` 还是可以继续使用



**方法二**



如果不想安装 `cnpm` ，但是又想使用淘宝镜像，可以通过以下方式：



```
npm install jquery --registry=https://registry.npm.taobao.org
```



但是这样需要每次都手动添加 `--registry=https://registry.npm.taobao.org` 参数，可以将参数写到配置文件中：



```
npm config set registry https://registry.npm.taobao.org
```



查看 npm 配置信息：`npm config list`



注意：写入配置文件后，以后使用 `npm install` 都会通过淘宝镜像来下载

### 加载 `require`

语法：



```
var 自定义变量名称 = require('模块')
```



两个作用：

- 执行被加载模块中的代码
- 得到被加载模块中的`exports`导出接口对象



### 导出 `exports`



- Node 中的模块作用域，默认文件中的所有成员只在当前文件模块有效
- 对于希望可以被其他模块访问的成员，只需要把这些公开的成员都挂载到`exports`接口对象中就可以了



导出多个成员（必须在对象中）：

```
exports.a = 123
exports.b = 'hello'
exports.c = function(){
  console.log('ccc')
}
exports.d = {
  foo:'bar'
}
```

### 加载 `require`

语法：



```
var 自定义变量名称 = require('模块')
```



两个作用：

- 执行被加载模块中的代码
- 得到被加载模块中的`exports`导出接口对象



### 导出 `exports`



- Node 中的模块作用域，默认文件中的所有成员只在当前文件模块有效
- 对于希望可以被其他模块访问的成员，只需要把这些公开的成员都挂载到`exports`接口对象中就可以了



导出多个成员（必须在对象中）：



```
exports.a = 123
exports.b = 'hello'
exports.c = function(){
  console.log('ccc')
}
exports.d = {
  foo:'bar'
}
```



导出单个成员（拿到的就是函数、字符串）：



```
module.exports = 'hello'
```



以下情况会覆盖：



```
module.exports = 'hello'

module.exports = function(x,y){
  return x + y
}
```



也可以这样来导出多个成员：



```
module.exports = {
    add:function(){
     return x + y
  },
  str = 'hello'
}
```

## 模块



### 什么是模块



在 Node 环境中，一个.js文件就称之为一个模块（module），Node 中的模块有3种：

- 核心模块
- 用户自定义模块
- 第三方模块

### 使用模块的好处

- 大大提高了代码的可维护性
- 可以避免函数名和变量名冲突。相同名字的函数和变量完全可以分别存在不同的模块中，因此，我们自己在编写模块时，不必考虑名字会与其他模块冲突

### 核心模块



Node 为 JS 提供了很多服务器级别的 API，这些 API 绝大多数都被包装到一个具名的核心模块中



常用的核心模块有：

- 文件操作：fs
- HTTP服务构建：http
- 路径操作：path
- 操作系统信息：os

要想使用核心模块，必须使用`require`先加载该模块



```
var os = require("os"); // 加载操作系统模块
console.log(os.cpus()); // 获取cpu信息
```

### 用户自定义模块



**什么是用户自定义模块**



由于在 Node 中，使用`node xx.js`执行 JS 文件时，一次只能执行一个 JS 文件，不能像浏览器中的 JS 那样通过`script`标签引入其他 JS 文件来同时执行，但是在 Node 中，可以通过`require('文件路径')`的方式来引入其他的 JS 文件，被引入的 JS 文件就叫做用户自定义模块



```
// test2.js
console.log("这是来自test2.js中的消息");

// test.js
require('./test2.js');  
```

在终端中执行`node .\test.js`的结果为：这是来自test2.js中的消息



注意：

- 相对路径必须加`./`
- 在加载用户自定义模块时，可以省略文件后缀名，`require('./test2');`

**模块作用域**



由于在 Node 中，没有全局作用域，只有模块作用域，所以 JS 文件不能直接访问被引用模块内部的成员

```
// test2.js
var a = 1;

// test.js
require('./test2'); 
console.log(a);
```

但是有时候，加载模块不仅仅是为了执行其中的代码，还想要能够访问到被加载模块内部的成员，这时，可以使用`exports`



`exports`是每个文件模块中都会提供的一个对象，这个对象默认为空，当使用`require`加载模块的时候，其实这个对象也被加载过来了，只需要把需要访问的成员挂载到`exports`这个对象上，就可以访问了