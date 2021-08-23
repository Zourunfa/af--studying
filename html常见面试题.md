## [HTML5新增的标签及使用](https://www.cnblogs.com/nuanai/p/8856814.html)

一、定义文档类型

在文件的开头总是会有一个标签

```
`DOCTYPE` `html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
```

　这是显示的html的文档类型，上面那个是1.0的，下面这个就是HTML5的类型标签，很是简短

```
`DOCTYPE` `html>
```

二、新的布局方式

如下图所显示的布局结果

正常的HTML布局就是通过div+css进行的，要是布局出这个页面正常的就是用div进行布局，然后给div进行起名字，然后进行样式的修改；

在HTML5中有个块状元素（有意义的div），也就是说不用div标签，而是用名字式的元素，例如：布局出第一个蓝色块是使用div，起名字，然后进行改变css；HTML是使用的header标签进行修改样式，header就是有意义的div

使用HTML5的新的结构标签（header section aside footer），做出上面的布局，代码如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<style>
  header{
    height:150px;
    background-color:#abcdef;
}
div{
    margin-top:10px;
    height:300px;
}
section{
    height:300px;
    background-color:#abcdef;
    width:70%;
    float:left;
}
article{
    background-color:#F33;
    width:500px;
    text-align:center;
    margin:0px auto;
}
aside{
    height:300px;
    background-color:#abcdef;
    width:28%;
    float:right;
}
/*页面底部*/
footer{
    height:100px;
    background-color:#abcdef;
    clear:both;
    margin-top:10px;
}
</style>
<body>
  <header>定义一个页面或是区域的头部</header>
<div>
        <section>定义一个区域</section>
        <aside>定义页面内容的侧边框部分</aside>
</div>
<footer>定义一个页面或是区域的底部</footer>
</body>
</html>
```

![image-20210314093100367](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210314093100367.png)





下面内容转载自 ：https://www.cnblogs.com/progor/p/8945071.html

### 结构化标签

#### header

- 功能：header标签定义页面的页眉信息。【主要用于定义结构，一般来说也可以使用其他方式来创建头部，但使用header就标注出了这个结构是头部，比其他多出了语意】
- 例子：比如一些网上商城的顶部logo信息

```html
<body>
    <header id="header" class="" style="background-color: orange;">
        <div style="float:left">
            Logo
        </div>
        <div style="float:right">
            <span>登录</span>
            <span>登录</span>
        </div>
        <div style="clear:both"></div>
    </header><!-- /header -->
</body>
```

![image-20210314093259599](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210314093259599.png)

#### nav

- 定义导航链接。【主要用于定义结构，一般来说也可以使用其他方式来创建导航，但使用nav就标注出了这个结构是导航，比其他多出了语意】

```html
<nav class="">
        <ul>
            <li><a href="#">食品</a></li><!--
         --><li><a href="#">电器</a></li><!--
         --><li><a href="#">电子数码</a></li><!--
         --><li><a href="#">书籍</a></li>
        </ul>
    </nav>
```



#### article

- article比p，比div多了语意，虽然理论上都可以使用div来实现，但多出了语意。【比如一本书可以一章可以有多个段落，但可以有几章，而article的语意就像章节，它的内容是独立的，而不是仅仅的“段落”】

```
<article>
        <header id="header" class="">
            头部：菜鸡互啄区
        </header><!-- /header -->
        <h2>是道德的沦丧，还是。。。</h2>
        贪玩蓝月，你没玩过的全新版本
        <footer>
            底部：欢迎评论
        </footer>
    </article>
```



#### footer

- 功能：用来定义页尾。【主要用于定义结构，一般来说也可以使用其他方式来创建页脚，但使用footer就标注出了这个结构是页脚，比其他多出了语意】

```
<footer>
        <div style="float:left;margin-right: 10px;">
            <div style="font-weight: bold;">合作伙伴</div>
            <div>支付宝</div>
            <div>baidu</div>
        </div>
        <div style="float:left">
            <div style="font-weight: bold;">帮助信息</div>
            <div>企业信息</div>
            <div>联系方式</div>
        </div>
    </footer>
```



#### section

- 功能：定义网页的中的“区块”【比p多出语意“区块”，而又比div少出样式之意，div更多的用来定义样式，而session可以用来做结构划分】【如果article是一篇文章的话，那么session可以某块围绕一个论点的文章内容】

```
<section>
        <h3>小标题：如何学习马克思</h3>
        <p>...巴拉巴拉</p>
    </section>
```





#### aside

- 功能：aside标签定义其所处内容之外的内容。【aside有语意“其他的内容”，比如说一篇文章有一些其他的相关内容，比如谈到贝叶斯模型，但本文不是主要谈论贝叶斯模型而仅仅以贝叶斯作为一个小点，而又想给出“其他的内容”来描述贝叶斯模型的时候，就可以使用aside】

```
<section>
         <p>xxx可以用到贝叶斯模型、马尔可夫模型xxx</p>
        <aside>
            <h3>贝叶斯模型</h3>
            <p>贝叶斯模型是xxxx</p>
        </aside>
        <aside>
            <h3>马尔可夫模型</h3>
            <p>马尔可夫模型是xxxx</p>
        </aside>
    </section>
```





### 媒体标签

#### audio

- 功能：可以用来定义音乐。

- 属性：

- - src:定义要播放的音乐的url地址
  - contorls:定义是否使用播放控件（控件有声音大小、停止播放等按钮）[![image](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152743412-1079128532.png)](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152742616-1783559189.png)【这些属性的设置既可以仅仅使用contorls,也可以使用controls="controls"】
  - autoplay:定义音乐自动播放【这些属性的设置既可以仅仅使用autoplay,也可以使用autoplay="autoplay"】
  - loop:定义循环播放【这些属性的设置既可以仅仅使用loop,也可以使用loop="loop"】

```
<audio src="一眼万年.mp3" controls autoplay ></audio>
```

有时候可以使用“兼容模式”，由于有些网页不支持某些格式或无法播放某音乐，所以需要“多个源”



```
<audio loop controls>
        <source src="bg.mp3"> <!-- 当第一个无法播放时，播放第二个源的音乐 -->
        <source src="一眼万年.mp3">
    </audio>
```

#### vedio

- 功能：定义一个视频

- 属性：

- - src:指定视频的url
  - controls:定义是否使用播放控件（控件有声音大小、停止播放等按钮）[![image](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152752310-1827053947.png)](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152745982-1388898582.png)【这些属性的设置既可以仅仅使用contorls,也可以使用controls="controls"】
  - autoplay:定义视频自动播放【这些属性的设置既可以仅仅使用autoplay,也可以使用autoplay="autoplay"】
  - loop:定义视频循环播放【这些属性的设置既可以仅仅使用loop,也可以使用loop="loop"】
  - 其他：可以设置标签的width,heigh

```
<video src="K:\cloudmusic\MV\Taylor Swift - Sparks Fly.mp4"  controls></video>
```

同样的，有时候可以使用“兼容模式”，由于有些网页不支持某些格式或无法播放某视频，所以需要“多个源”



```
<video autobuffer autoloop loop controls>
        <source src="0.mp4">
        <source src="K:\cloudmusic\MV\Taylor Swift - Sparks Fly.mp4">

    </video>
```

### 新增的input属性（常用的）：

 

 

#### 新增的type值：

- - date:

  - - 功能：可以选择日期[![image](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152754329-36050379.png)](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152753435-1612339523.png)
    - 示例：

复制代码

```
<input type="date" />
```

- - time:

  - - 功能：可以选择时间的输入框[![image](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152757315-2023859446.png)](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152755619-595690318.png)
    - 示例：

复制代码

```
<input type="time" name="selecttime" >
```

- month:

- - 功能：可以选择年份月份的输入框[![image](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152759720-202539248.png)](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152758816-1013925604.png)

  - 示例：

    复制代码

    ```
    <input type="month" name="selectmonth" >
    ```

- week:

- - 功能：可以选择XX年XX周的输入框[![image](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152802820-383774708.png)](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152801513-71987391.png)

  - 示例：

    复制代码

    ```
    <input type="week" name="selectweek" >
    ```

- search

- - 功能：比普通的文本框增加了一个可全部删除的按钮[![image](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152806230-316762543.png)](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152805513-2045163865.png)【可以点击右边的x来清除所有内容】

  - 示例：

    复制代码

    ```
    <input type="search" >
    ```

- range

- - 功能：这是一个可拖动的滑动框[![image](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152809112-1832950769.png)](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152807615-1467649847.png)

  - 属性:

  - - min:定义滑动块的最小值
    - max:定义滑动块的最大值
    - value:定义默认值
    - step:定义最小滑动距离

  - 示例：

    复制代码

    ```
    <input type="range" min="0" max="10" step="1">
    ```

- url

- - 功能：这个输入框能校验url的合法性[![image](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152811119-680725545.png)](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152810523-1234755378.png)【当提交的时候才会检测】

  - 示例：

    复制代码

    ```
    <form action="" method="post" >
            <input type="url" >
            <input type="submit" >
        </form>
    ```

- email

- - 功能：这个输入框能够校验email的合法性[![image](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152813086-1916257344.png)](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152812329-1995393262.png)

  - 示例：

    复制代码

    ```
    <form action="" method="post" >
            <input type="email" >
            <input type="submit" >
        </form>
    ```

- number

- - 功能：这个输入框能校验输入的是否全部是数字[![image](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152814614-1071052612.png)](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152813614-271461892.png)

  - 示例：

    复制代码

    ```
    <form action="" method="post" >
            <input type="number" >
            <input type="submit" >
        </form>
    ```

###  新增的属性

- list

- - 功能：与datalist配合使用，用来显示输入框的提示选项（已经在datalist中演示了怎么使用了）

- placeholder

- - 功能：设置输入框的提示[![image](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152816311-1685180230.png)](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152815784-1687395347.png)【图中的用户名会随着输入自动消失，如果没有内容就又会显示出来】

  - 示例：

    复制代码

    ```
    <form action="" method="post">
            <input type="text"  placeholder="用户名">
            <input type="submit" >
        </form>
    ```

- multiple

- - 功能：设置可以选择多个值

  - 示例：

    复制代码

    ```
    <input type="file" multiple="multiple" >
    ```

- required

- - 功能：设置输入框为必填框【当提交的时候，如果没有填上就会显示成红色或者给予提示】[![image](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152817820-1534111897.png)](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152817136-1014207996.png)

  - 示例：

    复制代码

    ```
    <form action="" method="post">
            <input type="text"  placeholder="用户名" required="required">
    
            <input type="submit" >
        </form>
    ```

- autocomplete

- - 功能：设置下次是否自动完成【如果设置了，那么下次输入时会有上次输入的提示】[![image](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152818817-1740051212.png)](https://images2018.cnblogs.com/blog/1053079/201804/1053079-20180425152818365-477047782.png)

  - 【注意：输入框需要name属性才能使得autocomplete生效】

  - 也可以把autocomplete放到form中作为属性，代表整个表单都可以autocomplete，（然后也可以在某个输入框中设置autocomplete="off"来特定的不autocomplete）

  - 示例：

    复制代码

    ```
    <form action="" method="get" autocomplete="on" >
            <input type="text"  placeholder="用户名" required="required" autocomplete="on" name="username">
            <input type="submit" >
        </form>
    ```

 



- autofocus：

- - 功能：设置是否自动获取焦点

  - 示例：

    复制代码

    ```
    <form action="" method="post" >
            <input type="text"  placeholder="用户名" required="required" autofocus="autofocus">
    
            <input type="submit" >
        </form>
    ```

- pattern：

- - 功能：可以用来验证输入框的输入是否合法【考虑到行为样式结构相分离，像这样的表单验证应该由javascript完成，所以这里不介绍】

 

 

## canvas svg webGl



### canvas

<canvas></canvas>

是html5出现的新标签，像所有的dom对象一样它有自己本身的属性、方法和事件。 使用canvas的基本方式是，使用js调用canvas的API绘图。例如，绘制一段贝塞尔曲线，需要用写这样一段javascript来生成：

```html
<html>
 <head>
  <script type="application/javascript">
    function draw() {
      var canvas = document.getElementById("canvas");
      if (canvas.getContext) {
        var ctx = canvas.getContext("2d");

        ctx.fillStyle = "rgb(200,0,0)";
        ctx.fillRect (10, 10, 55, 50);

        ctx.fillStyle = "rgba(0, 0, 200, 0.5)";
        ctx.fillRect (30, 30, 55, 50);
      }
    }
  </script>
 </head>
 <body onload="draw();">
   <canvas id="canvas" width="150" height="150"></canvas>
 </body>
</html>
```

[Echart](http://echarts.baidu.com/index.html), 一个由百度前端发起的canvas国产类库。这个echart其实是在canvas类库zrender的基础上做的主题图库，优点有数据驱动，图例丰富，功能强大，支持数据拖拽重计算，数据区域漫游，全中文文档非常过瘾。跟同样是国产的前端脚手架fis一样（官网http://fis.baidu.com/），都是诚意满满的国产套餐，体现了现今国内不俗的前端开发实力。我试用后感觉非常好，在我参与的一些项目中直接采用。关于我使用经验参见：[百度数据可视化图表套件echart实战](http://www.storagelab.org.cn/zhangdi/2014/04/09/百度数据可视化图表套件echart实战/)

最后再说说canvas这个技术本身的优缺点：

缺点：

- 只能绘制2D图像，暂时不支持3D图像。
- canvas绘制图形出并非可以直接操作的dom对象。如果要对其进行类似dom的操作，例如添加属性等等，比较麻烦（这就是为什么必须使用类库）。

优点：

- 由于canvas绘图不会给每个点生成对象，所以绘制速度快，消耗内存少。（这点主要是相对于SVG，VML技术而言）
- 兼容性较好。除了IE6，其他浏览器都可以支持。（IE7，8需要载入扩展JS，终究还是能用的）

###  SVG

关于SVG技术，在w3c的定义如下：

- SVG 指可伸缩矢量图形 (Scalable Vector Graphics)
- SVG 用来定义用于网络的基于矢量的图形
- SVG 使用 XML 格式定义图形
- SVG 图像在放大或改变尺寸的情况下其图形质量不会有所损失
- SVG 是万维网联盟的标准
- SVG 与诸如 DOM 和 XSL 之类的 W3C 标准是一个整体

```html
<html>
<svg>
	<circle cx="25" cy="25" r="22" fill="blue" stroke="gray" stroke-width="2" />
	<ellipse cx="250" cy="250" rx="100" ry="200" fill="yellow"/>
	<line x1="0" y1="0" x2="500" y2="50" stroke="black"/>
</svg>
</html>
```

跟前文中canvas绘图的方式比一比，就知道SVG是多么容易控制了。

当然，使用SVG时我们通常也是使用类库来提升效率。这里的类库主要有三种：

- highcharts.JS, 在现代浏览器中使用SVG绘图，在IE6，7，8中用VML绘图。包含一堆预定义的图表和样式。唯一的问题是，这货收费。只对非商业用途免费。
- raphael.js，以著名画家拉斐尔之名命名的绘图JS库，跟highcharts类似，也是SVG + VML兼容性方案。 但它是开源的，应用也比较广泛。使用它的时候有必要再下一个gRaphael图表库作为参考。
- D3.js，D3js是应用在web开发上的开源JS组件库，是一个数据可视化工具。D3应用的最为广泛，不过只支持SVG，我会重点讲述。

那么接着说说D3的优缺点（基本上也就是SVG的优缺点）：

优点：

- D3最大的优点在于其资料丰富，案例非常多。这是真的是一个极大的优点。
- SVG矢量图形的特点是无损缩放，这个优势在显示2D图形式会有非常好的效果，并且兼容各种分辨率。
- SVG图形的节点可以像dom元素一样控制，这就让自主创作图形变得更容易。相对于canvas这也是非常大的优势。

缺点；

- SVG是2D矢量图，不能画3D图形。（用2D矢量可以画很多带透视效果的伪3D图，那并不是真正的3D图！）
- d3.不支持IE6，7，8。如果想要IE8使用d3，请用r2d3.js（一个结合了 Raphael.js的扩展库）。Raphael.js是一个跨浏览器的矢量图形库，它实现IE6，7，8兼容的方法是：在IE6，7，8中使用VML，在其他浏览器中使用SVG。另外，如果图形复杂，就不要指望用Raphael.js在IE上能跟D3画出一样酷炫的效果。
- SVG的节点都是对象，非常占用内存。例如论坛里一个朋友使用d3绘制超过12000个节点的图，直接导致每个试图打开它的浏览器都崩溃了。这个时候如果不愿意做简化那么应该试试canvas绘图。

### webGL

前面说的绘图技术，无论canvas还是SVG都不能绘制3D图形。这货叫webGL, 是一项使用JavaScript实现3D绘图的技术，浏览器无需插件支持，Web开发者直接使用js调用相关API就能借助系统显卡（GPU）进行编写代码从而呈现3D场景和对象。WebGL 内容作为 HTML5 中的Canvas标签的特殊上下文实现在浏览器中（这下canvas终于可以画3D图了，虽然用的是不同技术）。

- 说白了就是基于Canvas的3D框架

![image-20210314103832094](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210314103832094.png)

## Canvas和SVG的区别

- `svg`绘制出来的每一个图形的元素都是独立的`DOM`节点，能够方便的绑定事件或用来修改。`canvas`输出的是一整幅画布
- `svg`输出的图形是矢量图形，后期可以修改参数来自由放大缩小，不会失真和锯齿。而`canvas`输出标量画布，就像一张图片一样，放大会失真或者锯齿



## Web Worker

**Web Worker 的作用，就是为 JavaScript 创造多线程环境，允许主线程创建 Worker 线程，将一些任务分配给后者运行。在主线程运行的同时，Worker 线程在后台运行，两者互不干扰。等到 Worker 线程完成计算任务，再把结果返回给主线程。这样的好处是，一些计算密集型或高延迟的任务，被 Worker 线程负担了，主线程（通常负责 UI 交互）就会很流畅，不会被阻塞或拖慢。**

Worker 线程一旦新建成功，就会始终运行，不会被主线程上的活动（比如用户点击按钮、提交表单）打断。这样有利于随时响应主线程的通信。但是，这也造成了 Worker 比较耗费资源，不应该过度使用，而且一旦使用完毕，就应该关闭。

**它允许开发人员编写能够长时间运行而不被用户所中断的后台程序，去执行事务或者逻辑，并同时保证页面对用户的响应。**

简而言之，就是**允许JavaScript创建多个线程，但是子线程完全受主线程控制，且不得操作DOM**。

**从而，**可以用webWorker来处理一些比较耗时的计算。

Web Worker 有以下几个使用注意点。

（1）**同源限制**

分配给 Worker 线程运行的脚本文件，必须与主线程的脚本文件同源。

（2）**DOM 限制**

Worker 线程所在的全局对象，与主线程不一样，无法读取主线程所在网页的 DOM 对象，也无法使用`document`、`window`、`parent`这些对象。但是，Worker 线程可以`navigator`对象和`location`对象。

（3）**通信联系**

Worker 线程和主线程不在同一个上下文环境，它们不能直接通信，必须通过消息完成。

（4）**脚本限制**

Worker 线程不能执行`alert()`方法和`confirm()`方法，但可以使用 XMLHttpRequest 对象发出 AJAX 请求。

（5）**文件限制**

Worker 线程无法读取本地文件，即不能打开本机的文件系统（`file://`），它所加载的脚本，必须来自网络









块级元素： div form table ul li  p h1-h6 br,i,em,strong,label,

行内元素： span a  img select input 

inline—block: img input