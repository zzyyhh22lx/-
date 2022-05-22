# require加载规则



#### 优先从缓存加载

require加载一次后会缓存，不会重复加载

```javascript
//main.js
require('./01.js');
require('./02.js');//这里不执行( 前面执行过，缓存不会再次执行 )

```

```javascript
//01.js
console.log('01');
require('./02.js')

```

```javascript
//02.js
console.log('02');

```

**输出结果：01  /n   02**

而不是 01 /n    02 /n   02

原因：require加载规则，只加载一次，不会重复执行

为了避免重复加载，提高效率



#### 标识符分析

```java
//如果是非路径的模块标识：（ 没有/ ）
//路径形式的模块：
//./   ../    /xxx
//首位的 / 在这里表示的是当前文件模块所属磁盘根路径/几乎不用

//后缀名可以省略  require('./main.js')===>require('./main')   //fs==>fs.js
require('模块标识');



//核心模块
//被编译到了二进制文件中了，只需要按照名字来加载就可以了
//不是路径形式的模块
require('fs')//http//os

    
    
    
//第三方模块  art-template
//都必须通过npm来下载
// require('包名')来加载才可以使用
//无路径形式的模块
//不可能有任意一个第三方包和核心模块名字一样的
var template = require('art-template');

//先找到当前目录中的node_modules目录-->art-template目录
//再去找package.json文件-->再找main属性-记录了->再去找index.js入口模块(exports导出)
//实际上加载的还是文件

```



![image-20220429201749190](D:\users\admin\Desktop\学习笔记\node.js\img\10.png)



也可以自己写一个第三方模块，也是按加载方式进行加载 

```javascript
var a = require('a');
//在a的目录下找
//package.json如果main属性什么都没有或者main的属性js错误，会报错 ===>
//如果这些都没有，就会在index.js下找（ 默认备选项 ）
//都不成立进入上一级目录的node_modules目录查找。。。当前磁盘根目录。。。报错
```



![b7d5bcc3209d82719c97bb1b0221e18](D:\users\admin\Desktop\学习笔记\node.js\img\11.png)



#### 注意，不会出现多个node_modules(有且只有一个，放在项目根目录)

所有的项目都可以加载到第三方包



第三方包制定的一整套规则可以方便不用写路径

比如`require( './node_modules/xxxx' );`





#### npm

- node package manager

#### package.json

- 建议每个项目都有一个package.json 文件 （包括描述文件，就像产品的说明书一样)，给人踏实的感觉

这些文件可以`npm init` 的方式自动初始化出来。

- 对于我们来讲最有用的是dependencies选项，可以帮我们保存第三方包的依赖信息
- 建议执行`npm install包名`都加一个`--save`选项，用来保存依赖项信息

![image-20220429210550656](D:\users\admin\Desktop\学习笔记\node.js\img\12.png)



如果node的modules删除了不用担心， `npm install`可以自动通过依赖项来把丢失的文件安装回来