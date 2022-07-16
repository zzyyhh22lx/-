```javascript
  //promise构造函数是同步执行的，then方法是异步执行的
        new Promise((reslve, reject) => console.log('run'));
        console.log('i run first');


        new Promise(function(resolve, reject) {
            var a = 5;
            var b = 1;
            if (b == 0) reject("Divide zero");
            //如果b==0，则执行catch的回调函数，这里不执行

            else resolve(a / b);
            //执行then的回掉函数，then的参数value为 5  <---a/b

        }).then(function(value) {
            console.log("a / b = " + value); //输出

        }).catch(function(err) {
            console.log(err);

            //resolve或reject执行完最后执行finally
        }).then(function() {
            console.log('throw erro');
            throw "An erro";
        }).catch(function(err) {
            console.log(err);

        }).finally(function() {
            console.log("End");
        });
```

then-fs使用

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

