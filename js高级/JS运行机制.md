# JS运行机制

- JavaScript异步加载原理分析
  JavaScript运行机制
  网页的渲染过程
  什么是同步(Synchronous)加载？
  什么是异步(Asynchronous)加载？
  主要有三种方式：
  解决异步加载的带来的影响方法

`==========================================================>`

#### JavaScript运行机制

- 什么的单线程

  没有多个线程可供主程序来调用，简单来说，就是同一时刻只能做一件事情，
  在JavaScript中执行代码就是从上往下一行一行执行代码



- 什么是多线程
  其他面向对象的语言Java、C++等等，都是多线程，即一个进程中可以并发多个线程，而每条线程并执行不同的任务，也就是说同一时刻可以同时进行多个任务



- 为什么是单线程
  JavaScript主要就是来服务前端网页的（网页的交互功能），这就注定了JavaScript是单线程语言

**主要就是为了避免 bom 渲染冲突**



- 我们都知道影响渲染dom结构的有HTML和JavaScript，所有为了避免出现二者对同一dom同时操作而照成渲染冲突

1.  JavaScript代码执行的时候，浏览器的渲染会停止
2. 服务端和客户端的JavaScript不能同时执行（否则有多个源头修改dom，会产生冲突）
3. 种种原因使得JavaScript为单线程，而且必须和浏览器渲染共用一个线程
4. 解决JavaScript单线程方法：异步

 

- 其实JS是单线程异步加载，JS的所谓异步加载不过是把异步的代码放在最后执行，说白了JS的异步不过是单线程延时。



#### 网页的渲染过程

页面的渲染过程：DOMTree + CSSTree = renderTree

1. 构建文档对象模型（DOM）

2. 构建CSS对象模型（CSSOM）
3. 构建渲染树（Render Tree）
4. 布局（Layout）
5. 绘制（Painting）

![网页渲染过程](https://img-blog.csdnimg.cn/20200311225550435.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MDA3NDE5,size_16,color_FFFFFF,t_70)





#### 什么是同步(Synchronous)加载？

我们平时使用的最多的一种方式：<script src="http://zhangsan.com/script.js"></script>

同步模式，又称阻塞模式，会阻止浏览器的后续处理，停止后续的解析，只有当当前加载完成，才能进行下一步操作

优点：安全性高

缺点：会照成网页的堵塞

建议：把<script>标签放在<body>结尾处，这样尽可能减少页面阻塞。





#### 什么是异步(Asynchronous)加载？

异步加载又叫非阻塞加载，浏览器在下载执行js的同时，还会继续进行后续页面的处理

代码执行时间：当整个网页解析完，执行代码（在网页而加载完之前）
主要有三种方式：
Script DOM Element（<async>属性是HTML5中新增的异步支持）浏览器部分不支持

<script async></script>

```javascript
(function(){
    var scriptEle = document.createElement('script');
    scriptEle.type = "text/javascript"
    scriptEle.async = true;
    scriptEle.src = "xxx.js";
var head = document.getElementsByTagName("head")[0];
head.insertBefore(scriptEle, head.firstChild); 
    })();
```





加载完就执行，只能加载外部脚本 W3C的标准
该加载方式执行完之前会阻止onload事件的触发，而现在很多页面的代码都在onload时还执行额外的渲染工作，所以还是会阻塞部分页面的初始化处理
如果有多个声明了async的脚本，其下载和执行也是异步的，不能确保彼此的先后顺序
async会在load事件之前执行，但并不能确保与DOMContentLoaded的执行先后顺序
IE专用（IE9以下） (defer)

<script defer></script>

```javascript
(function(){
    var scriptEle = document.createElement('script');
    scriptEle.type = "text/javascript"
    scriptEle.defer = true;
    scriptEle.src = "xxx.js";
    var head = document.getElementsByTagName("head")[0];
head.insertBefore(scriptEle, head.firstChild); 
    })();
```



defer适用于外联脚本，如果script标签没有指定src属性，只是内联脚本，尽量不要使用defer （虽然IE4-IE7还支持对嵌入脚本的defer属性，但在IE8及之后的版本就只支持外部脚本，对不支持的会直接忽略defer属性）
如果有多个声明了defer的脚本，则会按顺序下载和执行
defer脚本会在DOMContentLoaded和load事件之前执行
使浏览器延迟脚本的执行，直到浏览器解析和渲染完页面
自己写一个把<script>标签插入到网页的最后面





```javascript


// 异步载入并执行脚本
function loadasync(url){
    // 找到head标签
    var head = document.getElementsByTagName('head')[0];
    // 创建一个script元素
    var s = document.createElement('script');
    // 设置引入地址
    s.src = url;
    // 插入到head标签中
    head.appendChild(s);
}
```



注意这个loadasync()函数会动态的载入脚本–脚本载入到文档中，成为正在执行的JavaScript程序的一部分，既不是通过web页面内联包含，也不是来自web页面的静态引用

解决异步加载的带来的影响方法

利用定时器的延时

```javascript
// 异步的过程
var script = document.createElement("script");
script.src = "test.js";
document.head.appendChild(script);

setTimeout("test()", 1000);

弊端就是不知道准确时间
加载事件onload

window.onload =  function(){
    test();
```



低版本的IE可能不支持，并且效率太慢
咱自己写一个



```javascript
// url: 地址
// callback: 回调函数
function loadScript(url, callback){
    var scriptEle = document.createElement('script');
    scriptEle.type = "text/javascript";
if(scriptEle.readyState){
    // 状态码 readyState-->complete loaded 表示ie中script加载完成了
    scriptEle.onreadystatechange = function (){
        if(scriptEle.readyState == 'complete' || scriptEle.readyState == 'loaded'){
          callback();
        }
    }
}else{
    //加载完成的标志
    scriptEle.onload = function(){
        // safari  chrome firefox opren
        callback();
    }
}
//下载了指定地址的js文件
scriptEle.src = url;
//挂到DOM树上，此时才执行了js文件中的代码
document.head.appendChild(scriptEle);
```
