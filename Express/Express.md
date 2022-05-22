# Express

原生的http在某1些方面表现不足以应对我们的开发需求，所以使用框架来加快我们的开发效率，框架为了提高效率，让代码更加统一

二、express流程

\1. 导入相关模块

\2. 执行过 var app = express() 后

使用app.set 设置express内部的一些参数（options）

使用app.use 来注册函数，可以简单的认为是向那个tasks的数组进行push操作

\3. 通过http.createServer 用app来处理请求

#### 安装

`npm i --yes`	==> package.json

`npm i --S express ` ==>module_....

**基本语法**

```javascript
// 0 安装
// 1 引包
var express = require('express');

// 2 创建服务器应用程序
//      相当于原来的http.createServer
var app = express();


//静态资源服务
// ======================================================>
// 公开指定目录  当以/public/开头时，去/public/的目录查找相应信息
// 当没有第一个参数，不要加/public/ ，直接请求就行，默认是请求是'/'
// =======================================================>
app.use('/public/', express.static('./public/'));


//========================================================>
// 基本路由
//========================================================>
// 配置使用模板引擎、
// 第一个参数表示，当渲染以 .art 结尾的文件时，使用art-template模板引擎
// express-art-template专门用来express中吧art-template整合到express中 ==>( 依赖关系 )
app.engine('art', require('express-art-template'));
//art可以换成html



// 当服务器收到get的/请求，执行回掉函数=====>可以链式调用，返回app
app.get('/', function(req, res) {
    res.send('hello express');
    //可以用res.end();//原来的方法可以用：尽量不要用（麻烦，要设置请求头的charset）
})

// 请求头的字符编码已经封装好了
app.get('/about', function(req, res) {
    res.send('hhhhhhs你好');
})

//收到post请求
app.post('/',function(req,res){
    
    // =================================================================>
    // Express为response 响应对象提供一个方法：render
    // render 方法默认不可以使用，配置了模板引擎就可以使用
    // res.rend('html模板名字',{模板数据});
    // 第一个参数不能写路径，默认在项目中的 views 目录中查找模板文件
    // 也就是说express有个约定，默认吧视图文件放在 views 目录中 
    // 后缀名是.art   而不是 .html ===> 跟前面配置使用模板引擎有关 用的是 art后缀
    res.render('index.art', {});
})



// 相当于server.listen
app.listen(3000, function() {
    console.log('app is listening on port 3000');
})
```



如果url请求其他的文件，就会输出`Cannot GET /XXX`

![image-20220502135318002](D:\users\admin\Desktop\学习笔记\Express\img\01.png)

===>**基本路由**

路由器：连接多个用户（上网）

- 交换机和主端口
- 分发的多个端口最后都连接到主端口中

**路由其实就是一张表**

这个表有具体的映射关系

请求谁就有函数一一对应

```javascript
// 当服务器收到get的/请求，执行回掉函数
app.get('/', function(req, res) {
    res.send('hello express');
    //可以用res.end();//原来的方法可以用：尽量不要用（麻烦，要设置请求头的charset）
})

// 请求头的字符编码已经封装好了
app.get('/about', function(req, res) {
    res.send('hhhhhhs你好');
})

//收到post请求
app.post('/',function(req,res){
    res.send('post');
})

```









===>**开启静态资源访问**

`app.use('/public/', express.static('./public/'));`



，没有第一个参数，默认是 / ，url不需要加public就可以访问，否则报错

`app.use(express.static('./public/'));`

![image-20220502153958212](D:\users\admin\Desktop\学习笔记\Express\img\05.png)





## 修改代码自动重启

可以使用第三方命令行工具，`nodemon`来帮我们解决贫富修改代码重启服务器问题。

`nodemon`是一个基于Node.js开发的命令行工具，需要独立安装

`npm i --global nodemon`

安装完毕以后，使用

```shell
node app.js

# 使用nodemon  可以直接自动重启服务器
nodemon app.js

```

**监视文件变化重启服务器**

每次在文件中用Ctrl+s，服务器就会自动重新启动



![image-20220502152208650](D:\users\admin\Desktop\学习笔记\Express\img\04.png)





### 在Express安装art-template

安装

```shell
npm install --s art-template
npm install --s express-art-template
npm install --s express-art-template
```

配置：

```javascript
// 配置使用模板引擎、
// 第一个参数表示，当渲染以 .art 结尾的文件时，使用art-template模板引擎
// express-art-template专门用来express中吧art-template整合到express中 ==>( 依赖关系 )
app.engine('art', require('express-art-template'));
//这里可以改为 html  模板引擎识别后缀名为XXX的
```

使用：

**在函数内部使用 `app.get('/',function(res,req){})`**

```javascript

    // =================================================================>
    // Express为response 响应对象提供一个方法：render
    // render 方法默认不可以使用，配置了模板引擎就可以使用
    // res.rend('html模板名字',{模板数据});
    // 第一个参数不能写路径，默认在项目中的 views 目录中查找模板文件
    // 也就是说express有个约定，默认吧视图文件放在 views 目录中 
    // 后缀名是.art   而不是 .html ===> 跟前面的模板引擎的配置有关
    res.render('index.art', {});
```

**如果需要修改视图文件的目录，可以使用**

`app.set('views','目录路径');`

### express的重定向

`res.statusCode=302;res.setHeader('Loaction','\');`

===>改为

俩个参数：一个是状态码，一个是路径，[自动识别]

`res.redirect('/');`

`res.redirect(301, '/to')`

使用 Express 重定向[`res.redirect()` 方法](https://link.juejin.cn/?target=http%3A%2F%2Fexpressjs.com%2Fen%2F4x%2Fapi.html%23res.redirect)允许您通过发送状态为 [302 的 HTTP 响应](https://link.juejin.cn/?target=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FHTTP_302)，将用户重定向到不同的 URL 。HTTP 客户端（浏览器、[Axios](https://link.juejin.cn/?target=https%3A%2F%2Faxios-http.com%2Fdocs%2Fintro) 等）随后将“遵循”重定向并向新 URL 发送 HTTP 请求，如下所示。





#### post请求

```javascript
app.post('/post',(req,res)=>{})
//可以利用不同的请求方法让同一请求路径使用多次
app.get('/post',(req,res)=>{
    console.log(res.query)//直接获得url？后面的封装后的对象
})
```

因为express没有内置表单的post请求体的API，需要使用一个第三方包,`body-parser`



安装：

```shell
npm install --save body-parse
```



配置：

```javascript
var express = require('express')
var bodyParser = require('body-parser')

var app = express()


//配置：
//加入这里配置，req请求对象就会多出一个属性：body
// parse application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: false }))

// parse application/json
app.use(bodyParser.json())

app.use(function (req, res) {
  res.setHeader('Content-Type', 'text/plain')
  res.write('you posted:\n')
    //可以通过req.body获得post请求内容
  res.end(JSON.stringify(req.body, null, 2))
})
```



### session

```shell
npm install express-session
```

### mySQL

```shell
npm install mysql
```

