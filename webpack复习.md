## 前端代码为什么要进行构建和打包

- 打包后的代码体积更小，打包时会自动压缩合并，代码比如将webpack的mode设置development的时候还会自动使用tree-shaking
- 能够编译识别高级语法ES6 (babel-loader + .babelrc文件配置)+Ts等
- 兼容性-postcss-loader 能够提高css代码的兼容性 polyfill能够识别更高级的ES代码
- 语法错误的提示 eslint  [webpack怎么配置eslint](https://segmentfault.com/a/1190000018604688)  eslintrc.js 文件
- 配置自动刷新-devServer默认和热更新-new webpack.HotModuleReplacementPlugin()
- 统一高效的开发环境，统一的构件和产出标准，集成公司构件规范





## webpack的构件流程

1. 初始化参数：解析webpack配置参数，形成配置结果-compiler对象
2. 开始编译，上一步得到的参数初始化compiler对象，注**册所有配置的插件，插件监听webpack构件生命周期的事件节点，做出反应，并执行run方法开始编译**
3. 确定入口，从配置的entry入口，开始解析文件构件的AST语法树，找出依赖，并递归，递归解析Entry依赖的所有Module，每找到一个Module，就会根据Module.rules里配置的Loader规则进行相应的转换处理，对Module进行转换后，再解析出当前Module依赖的Module，这些Module会以Entry为单位进行分组，即为一个Chunk。**因此一个Chunk，就是一个Entry及其所有依赖的Module合并的结果**。最后Webpack会将所有的Chunk转换成文件输出Output。在整个构建流程中，Webpack会在恰当的时机执行Plugin里定义的逻辑，从而完成Plugin插件的优化任务。
4. 编译模块，递归中根据文件的类型和loader的配置，调用所有配置的loader对文件进行转换，再找出该模块的依赖的模块，再递归本步骤知道所有入口依赖的文件都经过本步骤的处理
5. 完成模块的编译并输出，递归完事后，得到每个文件的结果，包含每个模块以及他们之间的依赖关系，根据entry配置生成的代码块chunk
6. 输出完成，输出所有的chunk到文件系统



：在构建生命周期中有一系列插件在做合适的时机做合适事情，比如UglifyPlugin会在loader转换递归完对结果使用UglifyJs压缩覆盖之前的结果。

 



## loader和plugin的区别

- loader模块转换器和编译的东西，如less->css
- plugin扩展插件,HtmlWebpackPlugin



## 对webpack插件的理解和常用的webpack插件

> webpack 插件是一个具有 apply 属性的 JavaScript 对象。apply 属性会被 webpack compiler 调用，并且 compiler 对象可在整个编译生命周期访问

- plugin是用来拓展webpack功能的
- plugin是一个具有 apply 属性的 JavaScript 对象
- apply 属性会被 webpack compiler 调用
- compiler 对象是 webpack 的编译器对象
- 编译器对象会有一系列hooks
- 利用hooks在不同阶段完成对被编译者的处理

![image-20210310163535249](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310163535249.png)

## babel和webpack的区别

- babel-js新语法编译工具，不关心模块化
- webpack-打包构件工具，是多个loader plugin的集合



## 如何产出一个lib

webpack.dll.js  

维护第三方的工具

![image-20210310161454097](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310161454097.png)



## 为何Proxy不能被Ployfill

- 如class可以用function模拟
- 如Promise可以用callback来模拟
- 但是Proxy功能不能用Object.defineProperty



## webpack的基本配置

先记录一下 以前学vue的webpack.config.js知识配置点知识

```js
const path = require('path')
const webpack = require('webpack')

module.exports = {
    entry:{
        app:'./src/main.js',
    },
    output:{
        path:path.resolve(__dirname,'./dist'),
        filename:"js/[name].js",
    },
    module:{
        rules:[
            {
                test:/\.js$/, //用正则匹配文件，用require或者import引入的都会匹配到
                loader:"babel-loader", //加载器名，就是上一步安装的loader
                exclude:/node_modules/ //排除node_modules目录，我们不加载node模块中的js哦~
            },
            {
                test:/\.css$/,
                use:[
                    'style-loader',
                    'css-loader',
                    // 'postcss-loader'
                ]
                //use后面数组的加载顺序是从后往前
                //依次使用以上loader加载css文件，postcss-loader可以暂时不加，后面再深入修改webpack配置的时候再说用处
                //
                //也可以写成这样 loader："style-loader!css-loader!postcss-loader"
            },
            {
                test:/\.(png|jpe?j|gif|svg)(\?.*)?$/,
                loader:'url-loader',
                options:{
                    limit:10000,
                    name:'img/[name].[ext]?[hash]'
                }
                //图片文件大小小于limit的数值，就会被改写成base64直接填入url里面，
                //不然会输出到dist/img目录下，[name]原文件名，[ext]原后缀，[hash]在url上加上一点哈希值避免缓存。
            },
            {
                test:/\.(woff2?|eot|ttf|otf)(\?.*)?$/,
                loader:"url-loader",
                options:{
                    limit:10000,
                    name:'fonts/[name].[ext]?[hash]'
                }
                //和上面一致
            },
            {
                test:/\.vue$/,
                loader:'vue-loader'
                //这一个loader当然是vue项目必须的加载器啦，不加其他规则的话，
                //简单的这样引入就可以了，vue-loader会把vue单文件直接转成js。
            },
        ]
    },
    resolve:{
        //引入路径是不用写对应的后缀名
        extensions: ['.js', '.vue', '.json'],
        //缩写扩展
        alias:{
            //正在使用的是vue的运行时版本，而此版本中的编译器时不可用的，我们需要把它切换成运行时 + 编译的版本
            'vue$':'vue/dist/vue.esm.js',// 'vue/dist/vue.common.js' for webpack 1
            //用@直接指引到src目录下，如：'./src/main'可以写成、'@/main'
            '@': path.resolve(__dirname,'./src'),
        }
    },
}
```





拆分配置和merge    

- webpack.prod.js  生产环境 （线上环境）
- webpack.dev.js  开发环境
- webpack.common.js  公共环境

需要require ( webpack-merge)





环境中的module.export有：

mode；模式

entry： 入口

module:  rules 下面可以引用关键的loader    

plugins:

devServer: webpack dev server 启动本地服务

​       proxy和可以代理  （如果本地的和服务端的路径与不同）







### 处理ES6转ES5   babel-loader

![image-20210310085249571](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310085249571.png)

用babel-loader的时候会用到babel  用到babel会进行如下配置

![image-20210310085310705](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310085310705.png)



### 处理样式

loader的执行顺序是从后往前

![image-20210310085644837](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310085644837.png)



- postcss-loader浏览器兼容css   
- 再css-loader解析css代码
- 最后用style-css插入到html的head中



注意postcss还需要进行如下配置：

![image-20210310085859770](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310085859770.png)



### 处理图片

下面直接上webpack.config（即通过url-loader的limit配置项来完成，单位byte，小于limit的图片资源会进行base64编码转换）：

![image-20210310090201814](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310090201814.png)



- 图片资源进行base64编码转换并不能压缩文件（反而会略有增加）；
- 只有较小的图片资源适合进行base64编码转换，因为进行base64编码转换的图片资源往往是放在css中，过大的图片资源转换后会导致css文件膨胀，进而影响页面加载效率（css会阻塞页面的渲染，而图片则不会），而较小的图片转换后虽然css大小略有增加，但可以减少一个http请求。



### bundle加hash

下面的contentHash 还可以写成 hash ,chunkhash   他们有不同的意思

hash一般是结合CDN缓存来使用，通过webpack构建之后，生成对应文件名自动带上对应的MD5值。如果文件内容发生改变的话，那么对应文件hash值也会改变，对应的HTML引用的URL地址也会改变，触发CDN服务器从原服务器上拉取对应数据，进而更新本地缓存

[webpack4中hash、chunkhash和contenthash三者的区别](https://blog.csdn.net/bubbling_coding/article/details/81561362)

![image-20210310153221505](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310153221505.png)

![ ](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310090548180.png)

<u>所以bundle代码的hash值不变就会命中缓存</u>






**基本配置只能做一些demo，不能做线上项目**

下面的配置面试尤为重要



### 多入口配置

最近在做项目的时候遇到了一个场景：一个项目有多个入口，不同的入口，路由、组件、资源等有重叠部分，也有各自不同的部分。由于不同入口下的路由页面有一些是重复的，因此我考虑使用 Webpack 多入口配置来解决这个需求。

![image-20210419214209322](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210419214209322.png)

下面是配置两个入口的方法

![image-20210310091105767](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310091105767.png)

两个入口就需要建两个entry的对应路径

![image-20210310091156755](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310091156755.png)

output的filename要改成[name].[contentHash：8].js的以变量存在的形式  ，**注意这里的[name] 必须要写成这样才有效果**

**[name]对应的就是index 和 other的名字**

![image-20210310091359107](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310091359107.png)

针对每个入口都要生成HtmlWebpackPlugin,  **chunks会引入对应的js文件**

![image-20210310091713614](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310091713614.png)



### 抽离css文件

> 在没抽离css文件之前，css代码是通过打包的js代码 插入进html文件的，这样的性能消耗比较大

应 用一个插件

![image-20210310094009295](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310094009295.png)

**注意下面的MiniCssExtractPlugin.loader替换了style-loader**

![image-20210310094027610](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310094027610.png)

![image-20210310094101416](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310094101416.png)

到上面这一步就已经完成抽离了，下面的步骤是压缩css，下面压缩的插件也是需要先用

require引入的，只是这里没显示



![image-20210310094131891](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310094131891.png)

### 理解optimazation

**optimization来对打包结果进行优化（生产环境优化）**

[webpack中的optimization属性部分功能归纳](https://blog.csdn.net/qq_43377853/article/details/113250257)



### 抽离公共代码和第三方代码

> 抽离公共代码和第三方代码的目的完全是为了性能优化，比如现在要引入第三方的库loadash，如果直接在index.js文件引入的话，那么每次一个小的修改，就会引起bundle后面的hash值发生变化，及其耗费性能

webpack.prod.js

![image-20210310100127461](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310100127461.png)

![image-20210310100415374](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310100415374.png)

![image-20210310100941534](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310100941534.png)



上面chunks引入





### webpack如何实现异步加载

如果一个js文件比较大，需要懒加载



![image-20210310101428362](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310101428362.png)

回顾vue,React异步组件  也是用这个语法实现的





### 配置中有chunk的地方有哪些

1. entry中产出(定义)chunk
2. 可以在有些插件中使用chunk上 比如HtmlWebpackPlugin
3. 在配置公共代码或者第三方库代码抽离时 optimation中的splitChunks也会定义chunk
4. 大文件需要用到异步加载是也会定义chunk





### 处理JSX

安装和  和在.babelrc 稍微配置一下

![image-20210310102152930](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310102152930.png)



![image-20210310102132470](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310102132470.png)

### 处理vue

直接使用vue-loader

![image-20210310102326857](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310102326857.png)



## 什么是happyPack多进程打包

js是单线程的，如果开启多进程打包，肯定能够提高构建的速度

webpack.prod.js下:

![image-20210310144614136](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310144614136.png)

下面开启了babel打包的多线程：



![image-20210310144731177](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310144731177.png)

在plugins中

![image-20210310144745131](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310144745131.png)



## ParalleUglifyPlugin多进程压缩js代码

webpack内置的Uglify工具压缩js，但是单线程的

![image-20210310145158804](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310145158804.png)



![image-20210310145217609](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310145217609.png)



## 关于开启多进程

- 项目较大，打包较慢，开启多进程能提高速度
- 项目较小，打包很快，开启多进程可能会降低速度

因为进程也是要开启和销毁的，这部分被称之为进程开销



## webpack怎么配置热刷新

**自动刷新（保存代码之后 页面自动刷新一面）和热跟新是不一样的**

下面配置devServer之后就会开启自动刷新



![image-20210310145800417](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310145800417.png)



自动刷新和热更新的比较：

- 自动刷新;整个网页全部刷新，速度较慢
- 自动刷新；整个网页会全部刷新，状态会丢失
- 热更新 ：新代码生效，网页不刷新，状态不会丢失

![image-20210310150120028](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310150120028.png)



![image-20210310150157243](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310150157243.png)



![image-20210310150235580](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310150235580.png)



![image-20210310150247434](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310150247434.png)



## 何时使用DllPlugin动态链接库插件

> 对于知名的前端框架vue,react等他们的体积大，构件慢
>
> 而且比较稳定，不常省级版本，所以一般同一个版本只构件一次即可了

通常来说，我们的代码都可以至少简单区分成**业务代码**和**第三方库**。如果不做处理，每次构建时都需要把所有的代码重新构建一次，耗费大量的时间。然后大部分情况下，很多第三方库的代码并不会发生变更（除非是版本升级），这时就可以用到dll：**把复用性较高的第三方模块打包到动态链接库中，在不升级这些库的情况下，动态库不需要重新打包，每次构建只重新打包业务代码**。

webpack已经内置了DllPlugin支持

- DllPlugin - 打包出dll文件
- DllReferencePlugin - 使用dll文件

![image-20210310151302699](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310151302699.png)

![image-20210310151653594](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310151653594.png)

![image-20210310151741253](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310151741253.png)

## module chunk bundle的区别

- module就是源码文件，webpack中所有文件都可称之为模块
- chunk-是个多个模块的合成，代码的集合， 可以在entry里面定义
- bundle 是最终的输出文件

![image-20210310102850876](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310102850876.png)



## webpack的性能优化

- 优化babel-loader

1. 开启缓存  cacheDirector
2. 排除范围 include 

![image-20210310103512944](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310103512944.png)





- 小图片base64编码

- bundle加上hash

- 懒加载
- 使用production
- scope Hosting



## 开启production环境的一些优点

1. mode:producition （线上环境）会自动压缩代码。mode: development  不会自动压缩  要配置uglify
2. vue React 等会自动删掉调试的代码（在线上环境是）
3. 会自动使用Tree - shaking



## 什么是tree-shaking

> 在线上环境时会自动删除一些没有用到的代码，减少了代码的体积提高了性能
>
> ，在使用webpack的production环境时，会自动启用tree-shaking(但是必须要ES6
>
> 的module才能生效，用commonjs确不行)





## ES6的Module和Commonjs

- ES6的Moudule静态引入（无条件引用，不能够通过代码的判断它是不是引用），编译时引入
- Commonjs是动态引用，执行时引入
- webpack的打包只是一个静态分析，所以只有ES6 Module才能实现Tree-shaking
- **CommonJS模块的加载机制是，输入的是被输出的值的拷贝。也就是说，一旦输出一个值，模块内部的变化就影响不到这个值**。
- **ES6 模块输出的是值的引用**



下面静态和动态引用一目了然



![image-20210310111107578](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310111107578.png)



## 什么是Scope Hosting

可以将多个函数作用域合并成一个函数作用域

- 代码体积更小
- 创建的函数作用域更少
- 代码可读性更好



配置如下：

![image-20210310112824763](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310112824763.png)





## babel的基本概念

**环境搭建**

![image-20210310113530717](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310113530717.png)

![image-20210310113544735](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310113544735.png)

> 其实，babel一开始只是一个空壳，需要在plugins中引入一些插件，来实现语法之间的转换，但是为什么这里的plugins中又没有值了，因为这里的presets属性已经预设了一些plugins的引入，所以@babel/preset-env是一些plugin的集合



**启用babel编译的方法**

![image-20210310113718081](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310113718081.png)



## babel-plyfill

**什么是ployfill**

很早之前前端就有这种ployfill，一般被认为是补丁的意思

**core-js和regenetator**

core-js 是一个标准库，集成了Es6,ES7这些新语法的补丁，core-js并不支持genector语法，所以还需要引用regenerator这个库

**babel-ployfill就是上面两个的集合**



babel只处理语法，不处理模块化

![image-20210310120510328](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310120510328.png)





**babel-ployfill文件比较大，一般只需要一部分功能，所以需要按需引入**

如下

![image-20210310120641212](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310120641212.png)







**babel-ployfoll还是存在一些问题的，比如会污染全局环境**



使用babel-runtime能够解决这个问题，所以开发第三方的库必须使用babel-runtime

![image-20210310143501423](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310143501423.png)

![image-20210310143546252](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310143546252.png)





## webpack优化构建速度

### 可用于生产环境

- 优化babel-loader
- IgnorePlugin (忽视一些模块的引入)
- noParse （webpack noParse作用主要是过滤不需要解析的文件，比如打包的时候依赖了三方库（jquyer、lodash）等，而这些三方库里面没有其他依赖，可以通过配置noParse不去解析文件，提高打包效率）
- happyPack
- ParallelUglifyPlugin
- 使用cdn

IgnorePlugin和noParse的大致配置：

![image-20210310153503404](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310153503404.png)

![image-20210310153436657](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310153436657.png)



**使用cdn**

![image-20210310153738328](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210310153738328.png)

除了上面一步还有一步就是把你打包的这些的文件放到cdn

### 不用与生产环境

- 自动刷新
- 热跟新
- DllPlugin





## 项目的webpack中用到了那些东西

1. 令我印象很深刻的就是在devServer中用到了proxy代理来解决跨域
2. 多入口配置，项目中有多个入口，不同的入口，路由，组件，资源等有重叠的部分，也有各自不同的部分