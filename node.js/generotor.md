```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>

    <br />Generator最大的特点就是交出函数的执行权(即暂停执行)
    <br /> 一、function关键字与函数名之间有一个星号；

    <br /> 它不同于普通函数，是可以暂停执行的，
    <br /> 所以函数名之间要加星号，以示区别
    <br /> 二、Generator函数体内部使用yield语句，可以定义不同的内部状态；

    <br /> 状态其实就是数据
    <br /> (内部的状态，就是函数内部的值，它在不同的时候是不一样的)
    <br /> 本质上，整个Generator函数就是一个封装的异步任务，

    <br /> 或者说是异步任务的容器。
    <br /> yield命令是异步不同阶段的分界线，

    <br /> yield可以当成return不过有本质区别
    <br />



    <br>Generator函数 就是一个异步编程 ：需要用.next()方法启动。可以分阶段运行
    </br>每次调用一个函数执行一次yied ：返回一个对象(value 和done 俩个参数) 第一个表示yield或者return里面的值，第二个表示函数是否执行完</h1>
    <br> yield可以用来加强控制，懒汉式加载 调用函数指针和调用生成器是两码事
    <script>
        function a() {
            console.log('普通函数');
        }

        // 定义Generator函数
        function* aa() {
            yield 'a';
            yield 'b';
            yield 'c';
            return 'd end...'; //很显然，return以后的yield全部都将作废，等于不存在
            yield 'f';

        }
        console.log(aa()) //aa {<suspended>}      ( 是一个指向Generator函数的指针对象 )          需要一个启动的方法
            // 让它动起来 需要一个 .next()方法
            // 表示当前阶段的信息，value属性
            //   done属性：-true，表示函数已经执行完了，-false：表示函数没有执行完

        var _aafn = aa();
        // _aafn是一个迭代器的引用

        // 每次调用一个迭代器的next()方法，就把函数内部的指针指向下一个yield

        // 第一个yield语句
        console.log(_aafn.next()); //{value: 'a', done: false}
        // 第二个yield语句
        console.log(_aafn.next()); //{value: 'b', done: false}
        // 第三个yield语句
        console.log(_aafn.next()); //{value: 'c', done: false}
        // 第四个是return  
        console.log(_aafn.next()); //{value: 'd end...', done: true}    函数已经执行完了




        function* gen(x) {
                var y = yield x + 2;
                return y;
            }
            // 上面代码就是一个 Generator 函数。它不同于普通函数，是可以暂停执行的，所以函数名之前要加星号，以示区别。
            // 整个 Generator 函数就是一个封装的异步任务，或者说是异步任务的容器。异步操作需要暂停的地方，都用 yield 语句注明。

        var g = gen(1);
        g.next() // { value: 3, done: false }
        g.next() // { value: undefined, done: true }

        // 上面代码中，调用 Generator 函数，会返回一个内部指针（即遍历器g 。这是 Generator 函数不同于普通函数的另一个地方，即执行它不会返回结果，返回的是指针对象。调用指针 g 的 next 方法，会移动内部指针（即执行异步任务的第一段），指向第一个遇到的 yield 语句，上例是执行到 x + 2 为止。

        console.log('============================================================================================================');
    </script>
    <br>// 上面代码就是一个 Generator 函数。它不同于普通函数，是可以暂停执行的，所以函数名之前要加星号，以示区别。
    <br>// 整个 Generator 函数就是一个封装的异步任务，或者说是异步任务的容器。异步操作需要暂停的地方，都用 yield 语句注明。
    <br>上面代码中，调用 Generator 函数，会返回一个内部指针（即遍历器g 。
    <br> 这是 Generator 函数不同于普通函数的另一个地方，即执行它不会返回结果，返回的是指针对象。
    <br> 调用指针 g 的 next 方法，会移动内部指针（即执行异步任务的第一段），指向第一个遇到的 yield 语句，上例是执行到 x + 2 为止。


    <br> 1.yield并不能直接生产值，而是产生一个等待输出的函数
    <br> 2.除IE外，其他所有浏览器均可兼容（包括win10 的Edge）
    <br> 3.某个函数包含了yield，意味着这个函数已经是一个Generator
    <br> 4.如果yield在其他表达式中，需要用()单独括起来
    <br> 5.yield表达式本身没有返回值，或者说总是返回undefined(由next返回)
    <br> 6.next()可无限调用，但既定循环完成之后总是返回undeinded



    <script>
        console.log('============================================================================================================');

        function* myYield(list) {
            for (let i = 0; i < list.length; i++) {
                yield list[i]
            }
        }
        const numList = myYield([1, 4, 8])
        console.log(numList.next()) //{done: false,value: 1}
        console.log(numList.next()) //{done: false,value: 4}
        console.log(numList.next()) //{done: false,value: 8}
        console.log(numList.next()) //{done: true,value: undefined}



        function* func() {
            var n = 1;
            yield ++n;
            yield ++n;
            yield ++n;
        }

        var _func = func(); // _func 相当于是 一个迭代器 (作用域是单独的)
        console.log(_func.next()); //{value: 2, done: false}
        console.log(_func.next()); //{value: 3, done: false}
        console.log(_func.next()); //{value: 4, done: false}
        console.log(_func.next()); //{value: undefined, done: true}


        // var _func_two = func.next();//报错  只有迭代器有next方法
        var _func_two = func(); //俩个迭代器之间是互相独立的


        console.log('============================================================================================================');
    </script>


    注：函数yield外部的变量或者输出在后面用迭代器.next()调用，第一次调用会开辟空间，将这些数据放在内存中，在迭代器没有结束前都不会释放空间；
    <br />所以在yield外的console只会输出一次

    <script>
        console.log('============================================================================================================');

        // generator迭代器.next()方法可以传递参数；参数可以覆盖前面yield返回的值

        // 不过给第一个 next启动器传递值没有意义：因为next刚启动时候前面没有yield返回的值可以覆盖，给第一个next传递参数没有
        function* fn() {
            var _n = 12;
            console.log('我只调用了一次哦');

            // 第一个yield语句
            var _v = yield _n + 12;


            console.log('========>' + _v); //在第二个next调用后执行
            // 第二个yield语句

            yield _v;
            var _d = _n + _n;

            // 
            yield ++_d;

        }

        var a = fn();
        console.log(a.next()); //{value: 24, done: false} 

        console.log(a.next()); //  ========>undefined  {value: undefined, done: false} 
        // 为什么是undefined？因为进行第一个yield语句的时候，这个var进行的赋值运算没有执行(没有在yield语句内);并且后面的_v的值都是undefined

        console.log(a.next()); //{value: 25, done: false}

        console.log('============================================================================================================');


        var b = fn();
        console.log(b.next()); //  {value: 24, done: false} 

        console.log(a.next());
        // {value: undefined, done: true}
        // 这里先输出前面  迭代器a的yield调用是true；

        console.log(b.next('abc')); // ========>abc  {value: 'abc', done: false}

        // 再输出========>abc 
        // 为什么是abc；因为传入的参数，其实是把上一个yield语句的返回值给覆盖了；
        // 上一个yield语句的值是 _n+12；传入了'abc' 那么上一个yield语句就变成了了 yield='abc'   然后又赋值给了_v
        // 不会修改前面定义的变量


        console.log(b.next()); // {value: 25, done: false}
    </script>

    for of 迭代器

    <script>
        function* ba() {
            yield 'a';
            yield 'b';
            yield 'c';
        }
        var aba = ba();
        for (var i of aba) {
            console.log(i); //a b c
        }
    </script>


</body>

</html>
```

