<h1 align="center">js模块化开发</h1>

- **模块化开发的好处：防止命名冲突。**

- **一个js文件就是一个模块**
- 导入的文件(优先加载级：如果名字一样，不加文件后缀)   .js 	 .json	  .node  -->(二进制对象文件)

### 1：引入js文件

在 01.js 文件输入:

```javascript
function sum(a,b){
    return parseInt(a,10)+parseInt(b,10);
}
//导出没有掉用的模块化成员的函数
exports.sum=sum;

//exports 是一个对象 可以为这个对象添加成员实现导出

//想在js文件的var a = require('./01.js');的a直接是exports.sum的sum，而不是 a.sum

//不可以直接exports = sum;    输出来直接是空对象
//可以module.exports = sum ;  module.exports只会访问最后一个，会覆盖
//可以module.exports = { sum : '',obj:{} };这样就可以访问对象的属性 
```

在 02.js 文件输入：

```javascript
//require导入js文件
var module = require('./01.js');
var ret = module.sum(17, 2);
console.log(ret);//19
```



### 2：引入json文件

03.json文件输入：

```json
{
    "username": "张三",
    "age": "18"
}
```

03.js文件输入：

```javascript
var module = require('./03.json');
console.log(module.username);//张三
```



#### Node的模块系统

- 第三方模块：art-template
- 自己写的
- EcmaScript语言
- 核心模块 fs http os(操作系统信息) path url

#### 什么是模块化

- 文件作用域

- 通信规则
  + 加载require
  + 导出



#### 注：

为什么在第一个文件夹要用 exports导出函数？

而第二个不用exports 导出json文件？

**因为函数function没有调用，就只有函数提升{}，没有其他东西。**

**那如果函数被调用呢？也没有用，函数等对象属性都存在堆里面，没有exports导出，另一个文件夹是无法使用该对象的。**

```javascript
//exports 是一个对象 可以为这个对象添加成员实现导出

//想在js文件的var a = require('./01.js');的a直接是exports.sum的sum，而不是 a.sum

//不可以直接exports = sum;    输出来直接是空对象
//可以module.exports = sum ;  module.exports只会访问最后一个，会覆盖
//可以module.exports = { sum : '',obj:{} };这样就可以访问对象的属性 
```



- 在Node中，每一个模块内部都有一个自己的module对象，在module对象中，有一个成员exports也是一个对象

- 谁来require我，就得到module.exports

- 默认在代码最后有一句：return module.exports



我们发现，每次导出成员接口对象都通过module.exports=XXX

所以，Node为了简化操作，专门提供了一个exports对象代替module.exports

` var exports = module.exports`;俩者一致，等价的



![710cae883cdb9726272a397276227f9](D:\users\admin\Desktop\学习笔记\node.js\img\08.png)





![fc34aea7fd560d6c38a09910aac4c5e](D:\users\admin\Desktop\学习笔记\node.js\img\09.png)

