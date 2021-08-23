对下面性能优化方面回答问题的总结：

1. js会阻塞页面的渲染  — 用script的defer属性延迟加载，或者用异步加载（script标签的`async="async"`属性，Script DOM Element 动态创建script标签）
2. CSS的加载会阻塞DOM的渲染，css的加载不会阻塞DOM树的解析，但是会阻塞DOM树的渲染
3. CSS的加载会阻塞js的运行吗？所以会出现白屏，为了减少白屏的时间可以

> 1.使用CDN(因为CDN会根据你的网络状况，替你挑选最近的一个具有缓存内容的节点为你提供资源，因此可以减少加载时间)
> 2.对css进行压缩(可以用很多打包工具，比如webpack,gulp等，也可以通过开启gzip压缩)
> 3.合理的使用缓存(设置cache-control,expires,以及E-tag都是不错的，不过要注意一个问题，就是文件更新后，你要避免缓存而带来的影响。其中一个解决防范是在文件名字后面加一个版本号)
> 4.减少http请求数，将多个css文件合并，或者是干脆直接写成内联样式(内联样式的一个缺点就是不能缓存)-css-Sprites(雪碧图)





## js会阻塞页面渲染吗

### html文件的执行过程

一般的，一个包含外部样式表文件和外部脚本文件的HTML载入和渲染的过程如下：

1. 浏览器下载HTML文件并开始解析DOM
2. 遇到样式表文件link标签[link rel=stylesheet]时，将其加入资源文件下载队列，继续解析DOM
3. 遇到脚本文件时，暂停DOM解析并立即下载脚本文件
4. 下载结束后立即执行脚本文件，在脚本中可访问当前script以上的DOM
5. 脚本执行结束后，继续解析DOM
6. 整个DOM解析完成，触发DOMContentLoaded事件



### 什么是阻塞？

> 在页面中我们通常会引用外部文件，而浏览器在解析HTML页面是从上到下依次解析、渲染，如果<head>中引用了一个a.js文件，而这个文件很大或者有问题，需要2秒加载，那么浏览器会停止渲染页面（此时是白屏显示，就是页面啥都没有），2秒后加载完成才会继续渲染，这个就是阻塞。



### 为什么会阻塞？

> 　因为浏览器不知道a.js中执行了哪些脚本，会对页面造成什么影响，所以浏览器会等js文件下载并执行完成后才继续渲染，如果这个时间过长，会白屏。



## js的onload和DOMContentLoaded

DOMContentLoaded事件在页面文档加载解析完毕之后马上执行。而不会等待图片文件和子框架页面的加载



load事件会在页面所有资源被加载进来之后才会触发load事件。load时间在DOMContentLoaded事件触发之后



作者：Small_Song
链接：https://www.jianshu.com/p/c4b9d819e735
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



### 解决方法

- <script src="" defer></script>延迟加载

在文档解析完成开始执行，并且在DOMContentLoaded事件之前执行完成，并且会按照他们在文档出现的顺序去下载解析。效果和把script放在最后</body>之前是一样的

defer最好用在引用外部文件中使用，**用了defer不要使用document.write方法；使用defer时候最好不要请求样式信息，因为样式表可能尚未加载，浏览器会禁止该脚本等待样式表加载完成**



- 异步加载

async异步加载：就是告诉浏览器不必等到加载完外部文件，可以边渲染边下载，什么时候下载完什么时候执行

<script type="text/javascript" src="a.js" async></script>

用了async不要使用document.write()方法;使用async时最好不要请求样式信息，原因和defer一样。

- script dom element法

  ：这个方法是用js动态创建一个script元素添加在document中。

  ```
  <script type="text/javascript">
      (function() {
       var s = document.createElement('script');
       s.type = 'text/javascript';
       s.async = true;                                        //这句可以删除，但是效果不变。
       s.src = 'js/a.js';
       var x = document.getElementsByTagName('script')[0];
       x.parentNode.insertBefore(s, x);
   })();
  </script>
  ```

  注意：这种方法会阻止onload事件

- onload时异步加载

  这个和script dom element法差不多但是他不是同时执行js和html，他是等html的文件，图片之类的、页面所有的资源全部加载完成后再下载执行js，这样的方法可以避免阻塞onload事件的触发。

  ```
   (function() {
        function async_load(){
            var s = document.createElement('script');
            s.type = 'text/javascript';
            s.async = true;
            s.src = 'js/yibujiaz.js';
            var x = document.getElementsByTagName('script')[0];
            x.parentNode.insertBefore(s, x);
        }
        if (window.attachEvent)
            window.attachEvent('onload', async_load);
        else
            window.addEventListener('load', async_load, false);
    })();
  ```





## JavaScript异步加载的几种方式

### script标签的`async="async"`属性

- `async`属性是HTML5新增属性，需要Chrome、FireFox、IE9+浏览器支持
- `async`属性规定一旦脚本可用，则会异步执行
- `async`属性仅适用于外部脚本

<script type="text/javascript" src="xxx.js" async="async"></script>



### Script DOM Element 动态创建script标签

```
(function(){
    var scriptEle = document.createElement("script");
    scriptEle.type = "text/javasctipt";
    scriptEle.async = true;
    scriptEle.src = "http://cdn.bootcss.com/jquery/3.0.0-beta1/jquery.min.js";
    var x = document.getElementsByTagName("head")[0];
    x.insertBefore(scriptEle, x.firstChild);		
 })();

```

### onload时的异步加载

```
(function(){
	if(window.attachEvent){
    	window.attachEvent("load", asyncLoad);
    }else{
    	window.addEventListener("load", asyncLoad);
    }
    var asyncLoad = function(){
    	var ga = document.createElement('script'); 
        ga.type = 'text/javascript'; 
        ga.async = true; 
        ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js'; 
        var s = document.getElementsByTagName('script')[0]; 
        s.parentNode.insertBefore(ga, s);
    }
)();

```



## CSS的加载会阻塞DOM的渲染

css的加载不会阻塞DOM树的解析，但是会阻塞DOM树的渲染

> 其实我觉得，这不会泽爱可能也是浏览器的一种优化机制。因为你加载css的时候，可能会修改下面DOM节点的样式，如果css加载不阻塞DOM树渲染的话，那么当css加载完之后，DOM树可能又得重新重绘或者回流了，这就造成了一些没有必要的损耗。所以我干脆就先把DOM树的结构先解析完，把可以做的工作做完，然后等你css加载完之后，在根据最终的样式来渲染DOM树，这种做法性能方面确实会比较好一点。





## CSS的加载会阻塞js的运行吗？

css加载会阻塞后面js语句的执行

因此，为了避免让用户看到长时间的白屏时间，我们应该尽可能的提高css加载速度，比如可以使用以下几种方法:

1.使用CDN(因为CDN会根据你的网络状况，替你挑选最近的一个具有缓存内容的节点为你提供资源，因此可以减少加载时间)
2.对css进行压缩(可以用很多打包工具，比如webpack,gulp等，也可以通过开启gzip压缩)
3.合理的使用缓存(设置cache-control,expires,以及E-tag都是不错的，不过要注意一个问题，就是文件更新后，你要避免缓存而带来的影响。其中一个解决防范是在文件名字后面加一个版本号)
4.减少http请求数，将多个css文件合并，或者是干脆直接写成内联样式(内联样式的一个缺点就是不能缓存)





## async await是如何做到异步阻塞的？



在vscode中



## 如何实现DNS预解析？DNS预解析在什么时候执行？

DNS预解析是浏览器试图在用户访问链接之前解析域名，这是DNS解析的机制。域名解析后，如果用户确实访问该域名，那么DNS解析的时间不会有延迟

DNS与域名解析
DNS全称为Domain Name System，即域名系统，是域名和IP地址相互映射的一个分布式数据库。

域名解析即通过主机名，最终得到该主机名对应的IP地址的过程。

浏览器对网站第一次的域名DNS解析查找流程依次为：

**浏览器缓存-系统缓存-路由器缓存-ISP DNS缓存-递归搜索**

#### DNS Prefetching（预解析）简介

> DNS请求需要的带宽非常小，但是延迟却有点高，这点在手机网络上特别明显。DNS预解析 能让延迟明显减少一些，例如用户点击链接时。在某些情况下，延迟能减少一秒钟。

X-DNS-Prefetch-Control 头控制着浏览器的DNS预解析功能

X-DNS_prefetch-Control: on|off

on：启用DNS预解析。在浏览器支持DNS预解析的特性时及时不适用该标签浏览器依然会进行预解析。

off：关闭DNS预解析。这个属性在页面上的链接并不是由你控制的或是你根本不想向这些域名引导数据时非常有用。

- 打开和关闭DNS预读取

你可以通过在服务器端发送 X-DNS-Prefetch-Control 报头，或是在文档中使用值为 http-equiv 的标签：

DNS Prefetch 应该尽量的放在网页的前面，推荐放在 <meta charset="UTF-8"> 后面。具体使用方法如下：

<meta http-equiv="x-dns-prefetch-control" content="off">





<meta> 元素可提供有关页面的元信息（meta-information），比如针对搜索引擎和更新频度的描述和关键词。


<meta> 标签位于文档的头部，不包含任何内容。<meta> 标签的属性定义了与文档相关联的名称/值对。

#### 特性

Chrome会记住最近使用的10个domain，并且在开启浏览器时自动解析，因此在打开这些常用页面的时候，并不会有DNS Lookup的延迟情况。





## CDN原理

> 8年前，还没有火车票代售点一说，12306.cn更是无从说起。那时候火车票还只能在火车站的售票大厅购买，而我所住的小县城并不通火车，火车票都要去市里的火车站购买，而从县城到市里，来回就是4个小时车程，简直就是浪费生命。后来就好了，小县城里出现了火车票代售点，可以直接在代售点购买火车，方便了不少，全市人民再也不用在一个点苦逼的排队买票了。
>
> **CDN就可以理解为分布在每个县城的火车票代售点，用户在浏览网站的时候，CDN会选择一个离用户最近的CDN边缘节点来响应用户的请求，这样海南移动用户的请求就不会千里迢迢跑到北京电信机房的服务器（假设源站部署在北京电信机房）上了**

##### CDN的优势

- 1）CDN节点解决了跨运营商和跨地域访问的问题，访问延时大大降低；
- 2）大部分请求在CDN边缘节点完成，CDN起到了分流作用，减轻了源站的负载。

缓存是一个到处都存在的用空间换时间的例子。通过使用多余的空间，我们能够获取更快的速度。

用户在浏览网站的时候，浏览器能够在本地保存网站中的图片或者其他文件的副本，这样用户再次访问该网站的时候，浏览器就不用再下载全部的文件，减少了下载量意味着提高了页面加载的速度。

如果中间加上一层CDN，那么用户浏览器与服务器的交互如下

![image-20210402112906689](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210402112906689.png)



客户端浏览器先检查是否本地缓存过期，如果过期，则向cdn边缘节点发起请求，cdn边缘节点会监测用户请求数据的缓存是否过期，如果没有过期，则直接响应用户的请求，此时完成一个http请求结束，如果数据已经过期，那么cdn还需想源战发请求



## http的组成

> HTTP报文分两类：请求报文和响应报文。不管是请求报文还是响应报文每条HTTP报文是由3部分组成，分别是：**1、起始行 2、首部 3、主体**

**一、起始行**

**请求行和响应行**

**请求行：**请求报文请求服务器对资源进行一些操作。通俗讲就是客户端想服务器发起请求的请求命令。包含了

方法：如：get、post等

请求URL：如：/src/test.pdf

版本号：版本号是指HTTP/1.1或HTTP/2.0等

**响应行：**响应报文承载了状态信息和操作产生的所有结果数据，将其返回给客户端。包含了响应报文使用的HTTP版本，状态码（如：200、404等），以及描述操作状态的文本形式的原因短语

**二、首部**

首部分类：**通用首部**（既可以出现在请求报文中，也可以出现在响应报文中）、**请求首部**、**响应首部**、**实体首部**（描述主题的长度和内容，或者资源本身）、**扩展首部**

在这里简单列一些常见的首部

**通用首部**

cache-control：用于传输对象的缓存信息。

Date: 用来给出报文创建的日期和时间。

**请求首部：**

Accept：客户端用Accept首部来通知服务器可以接受哪些媒体类型。例如：Accept: text/*, image/*、Accept: text/*, image/gif, image/jpeg; q=1等

Accept-Charset:用来通知服务器它可以接受哪些字符集或哪些是优选字符集。例如：Accept-Charset: iso-latin-1

Accept-Encoding:客户端用来告知服务器它可以接受哪些编码方式。

Accept-language：用来告诉服务器可接受或优选接受哪些语言。

**Authorization：客户端用来向服务器回应自己的身体的验证信息。**

From：用来说明请求来自何方。

If-Modified-since：用来发起条件请求。

If-Match

If-None-Match



**响应首部：**

Age：告诉接受端响应已产生了多长时间。

Allow：用于通知客户端可以对特定资源使用哪些HTTP方法。例如：Allow: GET, HEAD

实体首部：

Content-Encoding：用于说明是否对某对象进行过编码。

Content-Language：用来告诉对方，想要理解对象的客户端，应该理解哪种自然语言。

Content-Length：用来说明实体主体部分的长度和尺寸。

Content-Type：用来说明报文中对象的媒体类型。例如：Content-Type: text/html; charset=iso-latin-1

ETag：为报文中包含的实体提供了实体标记。实体标记实际上就是一种标识资源的方式。

Expiress：给出了响应失效的日期和时间。这样，像浏览器这样的客户端就可以缓存一份副本，在这个时间到期之前，不用去询问服务器它是否失效。

Last-Modified

**三、实体**

实体就是HTTP要传输的内容。





## HTTP1.0,HTTP2.0,HTTP3.0



### Http1.0存在的问题

无法复用链接

每次发送请求都需要进行一次TCP链接，而TCP的链接的释放过程又是比较费事的，这种无连接的特性会使得网络的利用率变低



**队头阻塞：由于HTTP1.0规定下一个请求必须在前一个请求响应到达之前才能发送，假设前一个请求响应一直不到达，那么下一个请求就不会发送**





### HTTP1.1

HTTP1.1继承了HTTP1.0的简单，克服了HTTP1.0性能上的问题



### 长链接

HTTP1.1增加Connection字段，通过设置keep-Alive保持HTTP链接的不断卡，避免每次客户与服务器之间请求都要重复建立释放建立TCP链接，提高了网络的利用率

如果客户端想关闭HTTP链接，可以在请求头中携带Connection:false来告知服务器关闭请求



### 管道化-尴尬的假并行传输

HTTP1.1支持请求的管道化，基于HTTP1.1长连接，使得请求管线化成为可能，管线化使得请求能够并行传输

例如：

> 假如响应的主体是一个html页面，页面中包含了很多img，这个时候keep-alive就了很大作用。能够“并行”发送多个请求。(注意，这里的“并行”并不是真正意义上的并行传输)
>
> 需要注意的是：服务器必须按照客户端请求的先后顺序依次回送相应的结果，以保证客户端能够区分出每次请求的响应内容。
>
> 也就是说，HTTP管道化可以让我们把先进先出队列从客户端(请求队列)迁移到服务端(响应队列)

**缓存处理 — 强缓存、协商缓存，启发式缓存(新增)**

此外，HTTP1.1还加入了缓存处理(强缓存和协商缓存)，新的字段如cache-control，支持断点传输，以及增加了Host字段(使得一个服务器能够用来创建多个Web站点)

**HTTP2.0**



**二进制分帧**

**HTTP2.0通过在应用层和传输层之间增加一个二进制分层帧，突破了HTTP1.1的性能限制，改进传输性能。**

**多路复用(链接共享)— 真并行传输**

- 流(stream)：已建立连接上的双向字节流。
- 消息：与逻辑消息对应的完整的一系列数据帧。
- 帧(frame)：HTTP2.0通信的最小单位，每个帧包含头部，至少也会标识出当前所属的流(stream_id)





## ajax原理

用自己的话：ajax全称为Asynchronous JavaScript and XML,用异步的方式ajax出现的目的是为了以前web解决一次很小的http请求就需要重新刷新页面的弊端，它只是在服务器和浏览器之间充当了一个缓冲，让用户误以为服务器没有中断，就是这一小小的改变产生了巨大改变

它使用异步的传输方式，异步传输是面向字符的传输，同步传输是面向比特的传输，，异步传输是将比特分成小组来进行传送

它使用的核心api是XMLHttpRequest对象，**Ajax的原理简单来说通过xmlHttpRequest对象来向服务器发异步请求，简单来说，也就是javascript可以及时向服务器提出请求和处理响应，而不阻塞用户，达到无刷新的效果**





https://www.cnblogs.com/mingmingruyuedlut/archive/2011/10/18/2216553.html

**ajax技术的意义**

 ajax 的全称是Asynchronous JavaScript and XML，其中，Asynchronous 是异步的意思，它有别于传统web开发中采用的同步的方式

只是在服务器和客户端之间充当了一个缓冲器，让用户误以为服务没有中断。精确的说，ajax并不能提高从服务器端下载数据的速度，而只是使这个等待不那么令人沮丧。但是正是这一点就足以产生巨大的影响和震动

**关于同步和异步**

**异步传输是面向字符的传输，它的单位是字符；而同步传输是面向比特的传输，它的单位是桢，它传输的时候要求接受方和发送方的时钟是保持一致的。**

具体来说，异步传输是将比特分成小组来进行传送。一般每个小组是一个8位字符，在每个小组的头部和尾部都有一个开始位和一个停止位，它在传送过程中接收方和发送方的时钟不要求一致，也就是说，发送方可以在任何时刻发送这些小组，而接收方并不知道它什么时候到达。一个最明显的例子就是计算机键盘和主机的通信，按下一个键的同时向主机发送一个8比特位的ASCII代 码，键盘可以在任何时刻发送代码，这取决于用户的输入速度，内部的硬件必须能够在任何时刻接收一个键入的字符。这是一个典型的异步传输过程。异步传输存在 一个潜在的问题，即接收方并不知道数据会在什么时候到达。在它检测到数据并做出响应之前，第一个比特已经过去了。这就像有人出乎意料地从后面走上来跟你说 话，而你没来得及反应过来，漏掉了最前面的几个词。因此，每次异步传输的信息都以一个起始位开头，它通知接收方数据已经到达了，这就给了接收方响应、接收 和缓存数据比特的时间；在传输结束时，一个停止位表示该次传输信息的终止。按照惯例，空闲（没有传送数据）的线路实际携带着一个代表二进制1的信号。步传输的开始位使信号变成0，其他的比特位使信号随传输的数据信息而变化。最后，停止位使信号重新变回1，该信号一直保持到下一个开始位到达。例如在键盘上数字“1”，按照8比特位的扩展ASCII编码，将发送“00110001”，同时需要在8比特位的前面加一个起始位，后面一个停止位。

同步传输的比特分组要大得多。它不是独立地发送每个字符，每个字符都有自己的开始位和停止位，而是把它们组合起来一起发送。我们将这些组合称为数据帧，或简称为帧。

　　数据帧的第一部分包含一组同步字符，它是一个独特的比特组合，类似于前面提到的起始位，用于通知接收方一个帧已经到达，但它同时还能确保接收方的采样速度和比特的到达速度保持一致，使收发双方进入同步。

　　帧的最后一部分是一个帧结束标记。与同步字符一样，它也是一个独特的比特串，类似于前面提到的停止位，用于表示在下一帧开始之前没有别的即将到达的数据了。

　　同步传输通常要比异步传输快速得多。接收方不必对每个字符进行开始和停止的操作。一旦检测到帧同步字符，它就在接下来的数据到达时接收它们。另外，同步传输的开销也比较少。例如，一个典型的帧可能有500字节（即4000比特）的数据，其中可能只包含100比特的开销。这时，增加的比特位使传输的比特总数增加2.5%，这与异步传输中25 %的增值要小得多。随着数据帧中实际数据比特位的增加，开销比特所占的百分比将相应地减少。但是，数据比特位越长，缓存数据所需要的缓冲区也越大，这就限制了一个帧的大小。另外，帧越大，它占据传输媒体的连续时间也越长。在极端的情况下，这将导致其他用户等得太久。

   了解了同步和异步的概念之后，大家应该对ajax为什么可以提升用户体验应该比较清晰了，它是利用异步请求方式的。打个比方，如果现在你家里所在的小区因 某种情况而面临停水，现在有关部门公布了两种方案，一是完全停水8个小时，在这8个小时内完全停水，8个小时后恢复正常。二是不完全停水10 个小时，在这10个小时内水没有完全断，只是流量比原来小了很多，在10个小时后恢复正常流量，那么，如果是你你会选择哪种方式呢？显然是后者。

**ajax所包含的技术**

1. 使用css和XHTML来表示
2. 使用DOM模型来交互和动态显示
3. 使用SMLHttpRequest和服务器进行异步的通信
4. 使用javascript来绑定和调用





**ajax原理和XMLHttpRequest对象**

Ajax的原理简单来说通过xmlHttpRequest对象来向服务器发异步请求，从服务器获取数据，用javascript来操作DOM来更新页面。这其中最关键的一步就是从服务器获得请求数据，要清楚这个过程和原理必须对XMLHttpRequest有所了解

核心对象XMLHTTPRequest，是一种支持异步请求的技术，简单来说，也就是javascript可以及时向服务器提出请求和处理响应，而不阻塞用户，达到无刷新的效果

它的属性有：

- onreadystatechange 每次状态改变所触发事件的事件处理程序

-  responseText  从服务器进程返回的数据的字符串的形式

-  responseXML 从服务器进程放回的DOM兼容的文档数据对象

- status  从服务器返回的数字代码，比如常见的404  和  200

- status Text  伴随状态码的字符串信息

- readyState   对象状态值

  ​	　　　0 (未初始化) 对象已建立，但是尚未初始化（尚未调用open方法）

  　　　　1 (初始化) 对象已建立，尚未调用send方法

  　　　　2 (发送数据) send方法已调用，但是当前的状态及http头未知

  　　　　3 (数据传送中) 已接收部分数据，因为响应及http头不全，这时通过responseBody和responseText获取部分数据会出现错误，

  　　　　4 (完成) 数据接收完毕,此时可以通过通过responseXml和responseText获取完整的回应数据

**ajax的优点**

  1、最大的一点是页面无刷新，在页面内与服务器通信，给用户的体验非常好。

　 2、使用异步方式与服务器通信，不需要打断用户的操作，具有更加迅速的响应能力。

　 3、可以把以前一些服务器负担的工作转嫁到客户端，利用客户端闲置的能力来处理，减轻服务器和带宽的负担，节约空间和宽带租用成本。并且减轻服务器的负担，ajax的原则是“按需取数据”，可以最大程度的减少冗余请求，和响应对服务器造成的负担。

  4、基于标准化的并被广泛支持的技术，不需要下载插件或者小程序。



```js
interface Config{
    url:string
    type:string
    data?:string
    dataType:string
}

function ajax(config:Config){
    const xhr = new XMLHttpRequest()
    xhr.open(config.type,config.url,true)
    xhr.send(config.data)

    xhr.onreadystatechange = function(){

        if(xhr.readyState ==4 && xhr.status === 200){
            console.log('success');

            if(config.dataType === 'json'){
                console.log(JSON.parse(xhr.responseText))
            }else{
                console.log(xhr.responseText);
                
            }
        }

    }

}

ajax({
    type:'get',
    url:'http://a.itying.com/api/productlist',
    dataType:'json'
})


```



## Cookie和Session

###  **什么是Cookie**

　　Cookie意为“甜饼”，是**由W3C组织提出**，最早由Netscape社区发展的一种机制。目前Cookie已经成为标准，所有的主流浏览器如IE、Netscape、Firefox、Opera等都支持Cookie。

　　由于HTTP是一种无状态的协议，服务器单从网络连接上无从知道客户身份。怎么办呢？就**给客户端们颁发一个通行证吧，每人一个，无论谁访问都必须携带自己通行证。这样服务器就能从通行证上确认客户身份了。这就是Cookie的工作原理**。

　　Cookie实际上是一小段的文本信息。客户端请求服务器，如果服务器需要记录该用户状态，就使用response向客户端浏览器颁发一个Cookie。客户端浏览器会把Cookie保存起来。当浏览器再请求该网站时，浏览器把请求的网址连同该Cookie一同提交给服务器。服务器检查该Cookie，以此来辨认用户状态。服务器还可以根据需要修改Cookie的内容。





### Cookie的修改、删除

Cookie并不提供修改、删除操作。如果要修改某个Cookie，只需要新建一个同名的Cookie，添加到response中覆盖原来的Cookie。

　　如果要删除某个Cookie，只需要新建一个同名的Cookie，并将maxAge设置为0，并添加到response中覆盖原来的Cookie。注意是0而不是负数。负数代表其他的意义。读者可以通过上例的程序进行验证，设置不同的属性。

 

　　注意：修改、删除Cookie时，新建的Cookie除value、maxAge之外的所有属性，例如name、path、domain等，都要与原Cookie完全一样。否则，浏览器将视为两个不同的Cookie不予覆盖，导致修改、删除失败。

### **Cookie的域名**

　　Cookie是不可跨域名的。域名www.google.com颁发的Cookie不会被提交到域名www.baidu.com去。这是由Cookie的隐私安全机制决定的。隐私安全机制能够禁止网站非法获取其他网站的Cookie。

　　正常情况下，同一个一级域名下的两个二级域名如www.helloweenvsfei.com和images.helloweenvsfei.com也不能交互使用Cookie，因为二者的域名并不严格相同。如果想所有helloweenvsfei.com名下的二级域名都可以使用该Cookie，需要设置Cookie的domain参数，例如：

Cookie cookie = new Cookie("time","20080808"); // 新建Cookie

cookie.setDomain(".helloweenvsfei.com");      // 设置域名

cookie.setPath("/");                // 设置路径

cookie.setMaxAge(Integer.MAX_VALUE);        // 设置有效期

response.addCookie(cookie);             // 输出到客户端

 

　　读者可以修改本机C:\WINDOWS\system32\drivers\etc下的hosts文件来配置多个临时域名，然后使用setCookie.jsp程序来设置跨域名Cookie验证domain属性。

　　注意：domain参数必须以点(".")开始。另外，name相同但domain不同的两个Cookie是两个不同的Cookie。如果想要两个域名完全不同的网站共有Cookie，可以生成两个Cookie，domain属性分别为两个域名，输出到客户端。





### Session机制

> 除了使用cookie，web应用程序中还经常使用Session来记录客户端的状态，Session是服务器端使用的一种记录客户端状态的机制，使用上比cookie简单一些，也相应的增加了服务器的存储压力



### 什么是Session

> Session是一种记录客户端状态的机制，不同的是Cookie保存在客户端浏览器中，而Session保存在服务器上，客户端浏览器访问服务器的时候，服务器把客户端信息以某种形式记录在服务器上，这就是Session，客户端浏览器再次访问时只需要从该Session中查找该客户端状态就可以了
>
> 如果说cookie机制是通过检查客户身上的通行证来确定客户身份的话，那么Session机制就是通过检查服务器上的客户明细表来确认客户的身份，Session相当于程序在服务器上建立的一份客户档案，客户采访的时候只需要查询客户档案表就可以了



### **实现用户登录**

　Session对应的类为javax.servlet.http.HttpSession类。每个来访者对应一个Session对象，所有该客户的状态信息都保存在这个Session对象里。**Session对象是在客户端第一次请求服务器的时候创建的**。Session也是一种key-value的属性对，通过getAttribute(Stringkey)和setAttribute(String key，Objectvalue)方法读写客户状态信息。Servlet里通过request.getSession()方法获取该客户的Session，

```
HttpSession session = request.getSession();       // 获取Session对象

session.setAttribute("loginTime", new Date());     // 设置Session中的属性

   

out.println("登录时间为：" +(Date)session.getAttribute("loginTime"));      // 获取Session属性
```

### **Session的生命周期**

　　Session保存在服务器端。**为了获得更高的存取速度，服务器一般把Session放在内存里。每个用户都会有一个独立的Session。如果Session内容过于复杂，当大量客户访问服务器时可能会导致内存溢出。因此，Session里的信息应该尽量精简。**

　　**Session在用户第一次访问服务器的时候自动创建**。需要注意只有访问JSP、Servlet等程序时才会创建Session，只访问HTML、IMAGE等静态资源并不会创建Session。如果尚未生成Session，也可以使用request.getSession(true)强制生成Session。

　　**Session生成后，只要用户继续访问，服务器就会更新Session的最后访问时间，并维护该Session**。用户每访问服务器一次，无论是否读写Session，服务器都认为该用户的Session“活跃（active）”了一次。

### **Session的有效期**

　　由于会有越来越多的用户访问服务器，因此Session也会越来越多。**为防止内存溢出，服务器会把长时间内没有活跃的Session从内存删除。这个时间就是Session的超时时间。如果超过了超时时间没访问过服务器，Session就自动失效了。**

　　Session的超时时间为maxInactiveInterval属性，可以通过对应的getMaxInactiveInterval()获取，通过setMaxInactiveInterval(longinterval)修改。

　　Session的超时时间也可以在web.xml中修改。另外，通过调用Session的invalidate()方法可以使Session失效。

### **Session的常用方法**

　　Session中包括各种方法，使用起来要比Cookie方便得多。Session的常用方法如表1.2所示。

表1.2 HttpSession的常用方法

| 方 法 名                                          | 描  述                                                       |
| ------------------------------------------------- | ------------------------------------------------------------ |
| void setAttribute(String attribute, Object value) | 设置Session属性。value参数可以为任何Java Object。通常为Java Bean。value信息不宜过大 |
| String getAttribute(String attribute)             | 返回Session属性                                              |
| Enumeration getAttributeNames()                   | 返回Session中存在的属性名                                    |
| void removeAttribute(String attribute)            | 移除Session属性                                              |
| String getId()                                    | 返回Session的ID。该ID由服务器自动创建，不会重复              |
| long getCreationTime()                            | 返回Session的创建日期。返回类型为long，常被转化为Date类型，例如：Date createTime = new Date(session.get CreationTime()) |
| long getLastAccessedTime()                        | 返回Session的最后活跃时间。返回类型为long                    |
| int getMaxInactiveInterval()                      | 返回Session的超时时间。单位为秒。超过该时间没有访问，服务器认为该Session失效 |
| void setMaxInactiveInterval(int second)           | 设置Session的超时时间。单位为秒                              |
| void putValue(String attribute, Object value)     | 不推荐的方法。已经被setAttribute(String attribute, Object Value)替代 |
| Object getValue(String attribute)                 | 不被推荐的方法。已经被getAttribute(String attr)替代          |
| boolean isNew()                                   | 返回该Session是否是新创建的                                  |
| void invalidate()                                 | 使该Session失效                                              |

## **cookie和session的区别**

- **cookie数据存放在客户的浏览器上，session数据放在服务器上**

当你登录一个网站的时候，如果web服务器端使用的是session，那么所有的数据都保存在服务器上面，客户端每次请求服务器会发送当前会话的session_id,服务器根据当前的session_id判断响应的用户数据标志，以确定用户是否登录，或者具有某种权限

由于数据是存储在服务器 上面，所以你不能伪造，但是如果你能够获取某个登录用户的session_id，用特殊的浏览器伪造该用户的请求也是能够成功的。

- cookie不安全，别人可以分析存放在本地的cookie并进行cookie欺骗，考虑到安全应当使用session

- session会在一定事件内保存在服务器上，当访问增多，会比较占用你的服务器的性能，考虑到减轻服务器性能方面，应当使用cookie
- 单个cookie保存的数据不能超过4k,很多浏览器都限制一个站点最多保存20个cookie,(Session对象没有对存储数据量的限制)

   **session很容易失效,用户体验很差;**

   **虽然cookie不安全,但是可以加密 ;**

   **cookie也分为永久和暂时存在的;**

   **浏览器 有禁止cookie功能 ,但一般用户都不会设置;**

   **一定要设置失效时间,要不然浏览器关闭就消失了;**

 记住密码功能就是使用永久cookie写在客户端电脑，下次登录时，自动将cookie信息附加发送给服务端。

​      application是全局性信息，是所有用户共享的信息，如可以记录有多少用户现在登录过本网站，并把该信息展示个所有用户。

 

 **两者最大的区别在于生存周期，一个是IE启动到IE关闭.(浏览器页面一关 ,session就消失了)，一个是预先设置的生存周期，或永久的保存于本地的文件。(cookie)**





## koa2-洋葱模型

### Koa.js

`Koa.js` 是一个极其精简的 Web 服务框架，主要提供以下功能：

- `HTTP` 服务：主要处理 `request` 和 `response`
- 中间件数据处理机制（洋葱模型）

### 什么是 AOP？

AOP为Aspect Oriented Programming的缩写，中文意思为面向切面编程，它是函数式编程的一种衍生范式



假如我想把一个**苹果**（源数据）处理成**果盘**（最终数据）我该怎么做？

① 苹果（源数据） ---->
② 洗苹果 ---->
③ 切苹果 ---->
④ 放入盘子 ---->
⑤ 果盘（最终数据）

共有 5 个步骤，如果我想升级一下果盘，打算在切苹果之前先**削皮**，放入盘子后**摆成五角星形状**那么我的步骤应该如下：

① 苹果（源数据） ---->
② 洗苹果 ---->
③ 削皮 ---->
④ 切苹果 ---->
⑤ 放入盘子 ---->
⑥ 摆成五角星形状 ---->
⑦ 果盘（最终数据）

上面每个步骤都可以看成相应的方法，步骤 ③ 和 ⑥ 加入与否都不影响我制作出果盘这个结果，可以看出这样是非常灵活的

### 什么是 `Koa.js` 洋葱模型？

洋葱模型其实就是中间件处理的流程，中间件生命周期大致有：

- 前期处理
- 交给并等待其他中间件的处理
- 后期处理

多个中间件处理，形成了所谓的洋葱模型





## 闭包的应用场景

1. setTimeout
2. onclick
3. 创建私有变量



## js利用高阶函数实现函数缓存

> 高阶函数就是那种输入参数里面有一个或者多个函数，输出的也是函数的函数，在这个js里面主要是利用闭包实现的，最简单的就是经常看到的在一个函数内部输出另一个函数

```js
var test = function() {
    return function() {}
}
```



**主要的思想就是利用闭包保持作用域**

```js
var add = function() {
    var num = 0;
    return function(a) {
        return num = num + a;
    }
}
add()(1);      // 1
add()(2);      // 2
```



这里的两个add()(1)和add()(2)不会相互影响，可以理解为每次运行add函数后返回的都是不同的匿名函数，每次add运行后return 的function其实都是不同的，所以运行的结果是不会相互影响的

```js
var add = function() {
    var num = 0;
    return function(a) {
        return num = num + a;
    }
}
var adder = add();
adder(1); // 1
adder(2); // 3
```

这样的话就会在之前运算结果基础上继续运算，意思就是这两个 adder 运行的时候都是调用的同一个 num



## 发布订阅模式

### DOM0和DOM2

- 语法上的区别

```js
box.onclick = function(){}
```

```js
box.addEventListener('click',function(){})
```

- 底层运行机制上的区别

> DOM0就是给元素的某个属性绑定方法（有效绑定的方法只有一个）
>
> DOM2是基于时间池机制完成，没增加一个绑定的方法，都会往事件池之中存放一个...,当事件触发会依次执行事件池中的事情=>发布订阅模式就是模仿事件池机制，（可以给同一个元素的某个事件绑定多个不同的方法

- DOM2中可以给一些特殊的事件类型绑定方法啊，这些事件类型DOM0不支持绑定，例如DOMContentLoaded , transitioned 

![image-20210406163706899](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210406163706899.png)







## 