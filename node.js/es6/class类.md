<h1 align="center">class 类</h1>





```javascript
  /* ----------------------------------------------------------------------------------------------------------------*

  --类--

    ----------------------------------------------------------------------------------------------------------------*/
class Father {

  constructor(x, y) {
      this.x = x;
      this.y = y;
    }
    say() {
      console.log("father " + this.x);
    }
    sum() {
      console.log(this.x + this.y);
    }

  }

// constructor接收传进来的参数，返回实例化对象。new即调用constrctor
// extends 可以使用父类的 一些属性和方法：但是this指向不是指向子类，this指向父类。要用super关键字

  class Son extends Father {

    constructor(x,y,z) {

// super 可以调和 公用的方法 的this 指向
     super(x,y); //调用父类的构造函数
   // super构造函数必须放在this之前，否则会报错*
      this.z = z;
   }
   say() {
// console.log('sun ' + this.z); //调用子类的say方法

super.say(); //super.say 输出父类的普通函数

    }

  }

  var son = new Son(1, 2, 3);

  son.say();//father 1

  son.sum(1, 2);//3


  // 注：es6 没有变量提升
```

 



 ```html
 <!DOCTYPE html>
 <html lang="en">
 
 <head>
     <meta charset="UTF-8">
     <meta http-equiv="X-UA-Compatible" content="IE=edge">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Document</title>
 </head>
 
 <body>
     <button id="btn">
         你好
     </button>
     <script>
         class Father {
             constructor(x, y) {
                 this.x = x;
                 this.y = y;
                 this.btn = document.getElementById("btn");
                 // 这里的调用的 say 方法的  this指向的是 btn 即调用的主体
                //用bind（）方法改变 
                 this 
                 this.btn.onclick = this.say.bind(this);
             }
 
             say() {
                 console.log(this.x + this.y);
             }
         }
         var btn = new Father('庞仁海', 'bigbitch')
     </script>
 </body>
 
 </html>
 ```

![image-20220501115520674](D:\users\admin\Desktop\学习笔记\node.js\es6\01.png)
