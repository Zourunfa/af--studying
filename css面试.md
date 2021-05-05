## web程序对象作用域

常用的有三个：

请求作用域，会话作用域，应用上下文。

-  请求作用域req范围最小，需要的资源最少，作用当前请求
-  session会话作用于本次对话，每个对话都有JSessionID，
-  ServletContext作用域范围大：web应用中所有都能够访问，生命周期和web容器一样长，维护所需资源多。 在满足需求内耗费的资源越小越好





## border-top-width

> border-top-width  设置一个元素的顶部边框的宽度，默认值为medium(thin---细，medium---中等，thick---粗，也可自定义为npx)

![image-20210308204310743](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308204310743.png)



## outline-width 

> 指定轮廓的宽度，位于边框边缘外围。默认值为medium(thin---细，medium---中等，thick---粗，也可自定义为npx)

![image-20210308204349129](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210308204349129.png)



## CSS的Sprite的作用和原理



> css sprites：精灵图（雪碧图）：把一堆小图片整合在一张大图上，通过背景图片相关设置（背景图片background-image、背景图是否重复background- repeat、背景图定位background-position），显示图片，减轻服务器对图片的请求数量

2.优点

（1）CSS Sprites能很好地减少网页的http请求，从而大大的提高页面的性能，这是CSS Sprites最大的优点，也是其被广泛传播和应用的主要原因；

（2）CSS Sprites能减少图片的字节；

（3）CSS Sprites解决了网页设计师在图片命名上的困扰，只需对一张集合的图片命名，不需要对每一个小图片进行命名，从而提高了网页制作效率。

（4）CSS Sprites只需要修改一张或少张图片的颜色或样式来改变整个网页的风格。

3.缺点

（1）图片合并麻烦：图片合并时，需要把多张图片有序的合理的合并成一张图片，并留好足够的空间防止版块出现不必要的背景。

（2）图片适应性差：在高分辨的屏幕下自适应页面，若图片不够宽会出现背景断裂。

（3）图片定位繁琐：开发时需要通过工具测量计算每个背景单元的精确位置。

（4）可维护性差：页面背景需要少许改动，可能要修改部分或整张已合并的图片，进而要改动css。在避免改动图片的前提下，又只能（最好）往下追加图片，但这样增加了图片字节。

使用步骤

1、制作一张具有多状态的拼合图片，需要按照一定规律处理
2、给要显示背景的盒（一个固定尺寸，宽width、高height），以背景的方式加载，让其局部显示
3、通过背景图定位（background-position）控制不同的显示状态

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>雪碧图实现原理</title>
</head>
<style>
    ul,h3{
        margin: 0;
        padding: 0;
    }
    h3{
        display: block;
        margin: 0;
        padding: 0;
    }
    .cat{
        position: relative;
        width: 150px;
        background-color: #f8f8f8;
        border: 1px solid #bbb;
    }
    li{
        z-index: 2;
        position: relative;
        display: block;
        height: 31px;
        line-height: 31px;
        overflow: hidden;
        margin: 1px 10px 0;
        vertical-align: bottom;
        border-bottom: 1px solid #dedede
    }
    li:hover{
        background-color:#666666;
    }
    li h3{
        font-size: 13px;
        font-weight: 400;
    }
    li i{
        display: inline;
        float:left;
        margin: 3px 10px 0 0;
        height: 24px;
        width: 30px
    }
    /* 在这里补充雪碧图的样式 */
    li>i{
        background:url('http://img.mukewang.com/539a950e00015ba500710200.jpg');
    }
    .cat-2>i{
        background-position:0 -24px;
    }
    .cat-3>i{
        background-position:0 -48px;
    }
    .cat-4>i{
        background-position:0 -72px;
    }
    .cat-5>i{
        background-position:0 -96px;
    }
    .cat-6>i{
        background-position:0 -120px;
    }
    .cat-7>i{
        background-position:0 -144px;
    }
    .cat-8>i{
        background-position:0 -168px;
    }
</style>
<body>
<div class="cat">
    <ul>
        <li class="cat-1"><i></i><h3>服装内衣</h3></li>
        <li class="cat-2"><i></i><h3>鞋包配饰</h3></li>
        <li class="cat-3"><i></i><h3>运动户外</h3></li>
        <li class="cat-4"><i></i><h3>珠宝手表</h3></li>
        <li class="cat-5"><i></i><h3>手机数码</h3></li>
        <li class="cat-6"><i></i><h3>家电办公</h3></li>
        <li class="cat-7"><i></i><h3>护肤彩妆</h3></li>
        <li class="cat-8"><i></i><h3>母婴用品</h3></li>
    </ul>
</div>
</body>
</html>
```



## 定位

- fixed元素，可定位于相对于浏览器窗口的指定坐标，它始终是以 窗口 为依
- 据relative元素以它原来的位置为基准偏移，在其移动后，原来的位置仍然占据空间
- absolute绝对定位，相对于最近已定位的祖先元素，如果没有已定位的祖先元素，则位置相对于最初的包含块。与文档流无关，因此不占据空间
- static没有定位，元素出现在正常流中。

## transition-timing-function

transition-timing-function: linear|ease|ease-in|ease-out|ease-in-out|cubic-bezier(*n*,*n*,*n*,*n*);

| 值                            |                             描述                             |
| :---------------------------- | :----------------------------------------------------------: |
| linear                        | 规定以相同速度开始至结束的过渡效果（等于 cubic-bezier(0,0,1,1)）。 |
| ease                          | 规定慢速开始，然后变快，然后慢速结束的过渡效果（cubic-bezier(0.25,0.1,0.25,1)）。 |
| ease-in                       | 规定以慢速开始的过渡效果（等于 cubic-bezier(0.42,0,1,1)）。  |
| ease-out                      | 规定以慢速结束的过渡效果（等于 cubic-bezier(0,0,0.58,1)）。  |
| ease-in-out                   | 规定以慢速开始和结束的过渡效果（等于 cubic-bezier(0.42,0,0.58,1)）。 |
| cubic-bezier(*n*,*n*,*n*,*n*) | 在 cubic-bezier 函数中定义自己的值。可能的值是 0 至 1 之间的数值。 |



## css3新特性

最长用的肯定是:

### boder-radius 圆角

```css
border-radius: 10px;
```

### text-shadow 阴影

```css
text-shadow: 5px 2px 6px rgba(64, 64, 64, 0.5);

/* 水平阴影的位置， 垂直阴影的位置， 模糊的距离， 阴影的颜色*/
```





### -webkit-gradient  渐变

```
background-image: -webkit-gradient(
  linear, 
  0% 0%,
  100% 0%,
  from(#2a8bbe),
  to(#fe280e)
);
/* */
```

### display: -webkit-box  水平垂直居中

```
.box {
  display: -webkit-box;
  -webkit-box-orient: horizontal; /* 父容器里子容器的排列方式，是水平还是垂直 */
  -wekit-box-pack: center; /* 父容器里子容器的水平对齐方式 */
  -webkit-box-align: center; /* 父容器里子容器的垂直对齐方式 */
}
```





### transition 过渡

是下面四个属性加起来了简写

- transition-property 对象参与过渡的属性（对应下面的color）
- transition-duration 过渡的持续时间 (5s)
- transition-timing-function 过渡的类型  规定过渡效果的速度曲线。 (ease -in)
- transition-delay 延迟过渡的时间 (1s)

```css
transition: color 5s ease-in 1s;
```







## 另外几种特殊的居中方式

js实现居中    

![image-20210311151255489](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210311151255489.png)



dispaly:table-cell

本身是控制文本的居中，但是可以将文本变成盒子属性，设置子元素为inline-block

**要求父级必须要有固定宽高**



![image-20210311151603297](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210311151603297.png)

## getBoundingClientRect使用指南

```javascript
Element.getBoundingClientRect()
```

方法返回元素的大小及其相对于视口的位置。

返回值是一个 DOMRect 对象，这个对象是由该元素的 getClientRects() 方法返回的一组矩形的集合, 即：是与该元素相关的CSS 边框集合。

属性值：

- top: 元素上边距离页面上边的距离
- left: 元素右边距离页面左边的距离
- right: 元素右边距离页面左边的距离
- bottom: 元素下边距离页面上边的距离
- width: 元素宽度
- height: 元素高度



#### 判断元素是否在可视区域内

```javascript
function isElView(el) {

    var top = el.getBoundingClientRect().top; // 元素顶端到可见区域顶端的距离

    var bottom = el.getBoundingClientRect().bottom; // 元素底部端到可见区域顶端的距离

    var se = document.documentElement.clientHeight; // 浏览器可见区域高度。

    if (top < se && bottom > 0) {

        return true;

    }

    else if (top >= se || bottom <= 0) {

        // 不可见

    }

    return false;

}
```

## 盒模型其实有四种

1. 标准盒模型 ：  设置的 height和width一般都代指content box-sizing:content-box （默认） 如果我要设置宽高固定50*50的盒子，结果加一个border宽和高就变了，这时候就可以使用 IE的怪异盒模型  
2. IE怪异和模型  content 包含padding和border   boder-sizing:border-box, 好像bootStrap css公共样式好像也是采用的 boder-sizing:border-box
3. flex弹性盒模型，移动端用的多
4. 多列布局



## 移动端响应式布局的三大方案

- media
- rem
- flex
- vh/vm  (百分比布局)





rem 是什么呢？
rem（font size of the root element），意思即根据根元素的 font-size 来设置字体的大小。跟 px 一样，它是 CSS 中的一个样式单位，会根据根元素的 font-size 值来转换成 px 单位，公式为：
px = rem * html(font-size)

举例：

```css
html{
  font-size:10px;
}
div{
  width:2rem;  // 2*10=20px
```

**2.方法一：@media + rem**

　　　　***\*@media\**
　　　　　　媒体查询， 可以针对不同的屏幕尺寸设置不同的样式，特别是如果你需要设置设计响应式的页面，@media 是非常有用的。当你重置浏览器大小的过程中，页面也会根据浏览器的宽度和高度重新渲染页面。**

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```css
@media screen and (min-width:350px){
    html{font-size:342%;}
}
@media screen and (min-width:360px){
    html{font-size:351.56%;}
}
@media screen and (min-width:375px){
    html{font-size:366.2%;}
}
@media screen and (min-width:384px){
    html{font-size:375%;}
}
@media screen and (min-width:390px){
    html{font-size:380.85%;}
}
@media screen and (min-width:393px){    /* 小米NOTE */
    html{font-size:383.79%;}
}
@media screen and (min-width:410px){
    html{font-size:400%;}
}
@media screen and (min-width:432px){ /* 魅族3 */
    html{font-size:421.875%;}
}
@media screen and (min-width:480px){
    html{font-size:469%;}
}
@media screen and (min-width:540px){
    html{font-size:527.34%;}
}
@media screen and (min-width:640px){
    html{font-size: 625%;}
}
@media screen and (width:720px){
    html{font-size: 703.125%;}
}
```

　**A:html元素的font-size值又是怎么确定的呢？（例如375px的屏幕宽度）**

　　　　  屏幕宽度大于375px的会按照宽度375px来适配。设计平时给我们的设计稿一般是640px宽度或者750px宽度的，上面的都是假定设计稿是640px宽来计算的，750px也是同理计算。

　　　　**1. 屏幕宽度是375，设计稿宽度是640，ratio = 375/640=0.5859375;**
　　　　**2. 我们要将设计稿上元素用css单位rem写下来，那么该如何转换，1rem应该等于稿子上多少px？**
　　　　  设定1rem = 100px ;可以设定其它值吗，可以，设置为100是方便我们在写css的时候好计算，小数点直接左移两位就可以了。比如，设计稿上一个宽46px按钮，这样转换成rem直接就是0.46rem。
　　　　**3. 1rem代表设计稿上100px,又该是等于设备上最后真实的多少像素呢？**

　　　　　 要用到前面的屏幕宽度和设计稿的宽度比 ratio,设计稿上100px代表了真实的设备100*ratio = 58.59375px。
　　　　　 换句话说 css中写的1rem等于设备58.59375px。又因为1rem等于1倍的html元素的font-size,所以这里的html元素的font-size最终应该设置成58.59375px。

　　　　 　**为什么上述代码中用的是百分比呢？**因为一般浏览器中html元素的默认字号都是16px，但是当用户放大或者缩小浏览器字号设置时，就不是16px了，所以将html的font-size设置成百分比更好，即 58.59375/16= 366.2109375%,也就是上面例子中的366.2%了。　

　　**B:@media + rem适配移动端还有一个不可少的条件**

　　　　要在head标签中写入一个meta标签

　　　　<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1, minimum-scale=1">

　　　　作用是让layout viewport = visual viewport,用户也不可缩放页面





## 使用css,让一个div消失在视野中

#### visibility和display

visibility:hidden
display:none

Visibility和Display属性虽然都可以达到隐藏页面元素的目的，但它们的区别在于如何回应正常文档流。

如果你想隐藏某元素，但在页面上保留该元素的空间的话，你应该使用visibility：hidden。如果你想在隐藏某元素的同时让其它内容填充空白的话应该使用display：none。

#### position:relative 设置left或者right、top、bottom到一个很大的值



#### opacity:0

opacity 对低版本IE不兼容可以用 filter
filter:alpha(50); /*IE6-IE7，数值是0-100的任意整数，0表示透明*/ filter:”progid:DXImageTransform.Microsoft.Alpha(opacity=50)”; /*IE8，数值是0-100的任意整数，0表示透明*/
对于filter 可以解决那些问题，可是自己 查文章



#### background-color:transparent



#### z-index

　z-index属性指定了元素及其子元素的【z顺序】，而【z顺序】可以决定当元素发生覆盖的时候，哪个元素在上面。通常一个较大的z-index值的元素会覆盖较低的那一个

z-index:auto; 默认值

　z-index:<integer>;整数值

  z-index:inherit; 继承



层叠上下文和层叠水平

层叠上下文是HTML元素中的一个三维概念，表示元素在z轴上有了“可以高人一等”

页面根元素天生具有层叠上下文，称之为“根层叠上下文”

z-index值为数值的定位元素也具有层叠上下文



![image-20210311191354433](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210311191354433.png)

**定位元素默认z-index:auto可以看成是z-index:0**（层级理解上）

　定位元素会覆盖普通元素。因为：z-index只对定位元素起作用，z-index:auto的层叠顺序大于图片这个内联元素。

**z-index不为auto的定位元素会创建层叠上下文**；

![image-20210311191614653](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210311191614653.png)

## css选择器的查询是从右向左查询

所以下面的代码的性能反而比上面的代码的性能还要高一些

<u>这点和webpack加载loader的顺序有点相像，也是从右向左</u>

![image-20210311192002308](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210311192002308.png)





## 如何使一个div里面的文字垂直居中，且该文字的大小根据屏幕大小自适应？

### 一、行高（line-height）法

如果要垂直居中的只有一行或几个文字，那它的制作最为简单，只要让文字的行高和容器的高度相同即可，比如：

```css
p { height:30px; line-height:30px; width:100px; overflow:hidden; }
```

### 二、内边距（padding）法

另一种方法和行高法很相似，它同样适合一行或几行文字垂直居中，原理就是利用padding将内容垂直居中，比如：

```
p { padding:20px 0; }
```

### 三、模拟表格法

将容器设置为display:table，然后将子元素也就是要垂直居中显示的元素设置为display:table-cell，然后加上vertical-align:middle来实现。

html结构如下：

```
<div id="wrapper">
    <div id="cell">
        <p>测试垂直居中效果测试垂直居中效果</p>
        <p>测试垂直居中效果测试垂直居中效果</p>
    </div>
</div>
```

 

css代码：

```css
#wrapper {display:table;width:300px;height:300px;background:#000;margin:0 auto;color:red;}
#cell{display:table-cell; vertical-align:middle;}
```

### 四：flex布局

html代码：

```
<div class="flex">
    <div>
       <p>我是多行文字我是多行文字我是多行文字我是多行文字</p>
      <p>我是多行文字我是多行文字我是多行文字我是多行文字</p>
    </div>
</div>
```

CSS代码：

```
.flex{
    /*flex 布局*/
    display: flex;
    /*实现垂直居中*/
    align-items: center;
    /*实现水平居中*/
    justify-content: center;
    
    text-align: justify;
    width:200px;
    height:200px;
    background: #000;
    margin:0 auto;
    color:#fff;
}
```



## css细节之为什么overflow能够清楚浮动

**触发了BFC**

BFC的规则（只列出需要的）：

1. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。
2. 计算BFC的高度时，浮动元素也参与计算。

> 当你为一个元素添加overflow属性为hidden时，这个元素就会产生自己的BFC。
>
> 根据规则2，**在计算BFC的高度时，浮动元素的高度也要参与计算**，即父元素不会忽略自己里面的浮动元素的高度，如果你的父元素的高度设置的auto的话，那么它的高度就会等于浮动元素的高度（假设这个BFC里面只有一个浮动元素）。
>
> 规则2也是为了保证规则1的所述，即BFC就是页面上的一个隔离的独立容器，容器里面的子元素不应该影响到外面的元素，如果父元素不计算浮动元素的高度而是坍塌的话，那么浮动元素就会影响到其后面的元素，所以它需要规则3来保证这么一个行为。
>
> 当然，**你可以说我就选择坍塌，然后把多余的浮动元素裁剪掉**，这样也不影响后面的元素，难道不行吗？**当然可以，所以才有了规则3明确表示我们采取的措施就是计算浮动元素的高度来不影响后面的元素**。
>
> 最后，这并没有违背overflow: hidden;的功能，**它的裁剪的功能依旧存在，只是说现在它的高度被自动设置成了能够包含浮动元素的高度罢了**（如果height: auto; ）。所以如果你显示的声明父元素的高度，那么浮动元素超出的部分也是会被裁剪掉的，**这跟违背hidden的本意没有任何关联**。

# 关于overflow:hidden的作用（溢出隐藏、清除浮动、解决外边距塌陷等等）

https://blog.csdn.net/qq_41638795/article/details/83304388