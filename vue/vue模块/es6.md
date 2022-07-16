# 模块化规范

### CMD，AMD，CommonJS，ES6：

- CMD，AMD，CommonJS是社区开发版本，不是官方规范，存在差异：

  + CMD，AMD语法适用于浏览器端
  + CommonJS语法使用于服务器端
  + ES6是官方推出的版本，实现社区版的大一统。

- CommonJS：

  ```js
  module.exports //导出
  require //引入
  ```

- 、

- ES6

  ```js
  //每一个js文件都是独立的模块
  export //导出
  import //引入
  ```



### ES6使用：

- 初始化

```shell
npm init -y
```

- 在`package.json`的根节点中添加一个选项

  ```json
  {
      "type": "module"
  }
  ```

  + 可以使用es6模块，默认是commonjs模块

- export default { } 导出 只允许使用一次

- 可以和按需配合使用

  ```js
  //01.js
  let a = 0;
  
  export default {
      a
  }
  
  //使用按需导出
  export let s1 = 'aaa'
  export function s2(){}
  ```

- import XX from XX 导入

  ```js
  import a from './01.js'
  
  //按需导入
  //as 重命名  ： s1 as str
  import a, { s1 as str , s2 } from './01.js'
  console.log(str) //aaa
  
  console.log(a)//{a: 0}
  ```

- 直接导入（ 没有向外共享 ）

  ```js
  //01.js
  console.log(1)
  
  //另外一个模块中 02.js
  import './01.js'
  
  //node 02.js  直接输出1
  ```



#### promise

+ fs使用

  ```js
  var fs = require('fs');
  
  function pReadFile(filePath) {
      return new Promise(function(resolve, reject) {
          fs.readFile(filePath, 'utf8', function(err, data) {
              if (err) {
                  reject(err);
              } else {
                  resolve(data);
              }
          })
      })
  }
  
  
  pReadFile('./data.txt')
      .then(function(data) {
          console.log(data);
          return pReadFile('./data2.txt');
      })
      .then(function(data) {
          console.log(data);
      })
      .catch(function(err) {
          console.log(err);
      })
      .catch(function(err) {
          console.log(err);
      })
      .finally(function() {
          console.log('done');
      })
  
  ```

  

+ then-fs使用：返回一个promise对象

+ 

  ```shell
  ##shell
  npm i then-fs
  ```

  

  ```js
  //then-fs 是 一个包，跟fs区别是返回一个promise对象
  import thenFs from 'then-fs'
  
  thenFs.readFile('./01.txt','utf8').then(r1 => {
      console.log(r1)
      return thenFs.readFile('./01.txt','utf8')
  }, err1 =>{
      console.log(err1.message)
  }).then(r2 => {
      console.log(r2)
  })
  ```

#### async/await

+ aysnc 使得函数值返回一个promise对象

+ await 把返回值为promise对象变成一个值

+ **await必须有aysnc修饰**

+ await代码前是同步执行，后面都是异步执行

  ```js
  async function getAllFile(){
      console.log('a')
      const r1 = await thenFs.readFile('./01.txt','utf8')
      console.log(r1)
      console.log('b')
  }
  
  getAllFile();
  console.log('c')
  
  //输出：(01.txt文件的内容是 txt )
  // a c txt b 
  ```

  

#### EventLoop

防止耗时任务导致程序假死：

- 同步任务：在主线程排队执行的任务（ 形成执行栈 ）
- 异步任务：不进入主线程，而进入任务队列

```html
机制：
同步任务由js主线程次序执行，异步任务委托给宿主环境执行，已完成的异步回调函数会放在任务队列中等待执行；
主线程的执行栈执行完，会读取任务队列的回调函数，次序执行；
主线程不断重复这几步
```

- 
  + 任务队列：
    + 宏任务：
      + 异步ajax，setTimeout，setInterval，文件操作，其他
    + 微任务
      + promise，process.nextTick，其他
    
    事件循环：先看到一个宏任务，再看看有没有微任务，如果有，执行所有微任务，如果没有，执行下一个宏任务
    
    **在一个宏任务中，先执行所有微任务，再执行下一个宏任务**

```js
new Promise(function (resolve, reject) {
  console.log(1);
  
  setTimeout(function () {
    console.log(2)
  },0)
    
  resolve(true);
})
	.then(function () {
  console.log(3);
});

console.log(4);
 // 1432
    
```

```js
      setTimeout(() => {
        console.log("4");
        setTimeout(() => {
          console.log("8");
        }, 0);
        new Promise((r) => {
          console.log("5");//构造函数是同步的
          r();
        }).then(() => {
          console.log("7");//then()是异步的，这里已经入队
        });
        console.log("6");
      }, 0);

      new Promise((r) => {
        console.log("1");//构造函数是同步的
        r();
      }).then(() => {
        console.log("3");//then()是异步的，这里已经入队
      });
      console.log("2");

// 1 2 3 4 5 6 7 

```

