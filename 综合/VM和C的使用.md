# VM和C的使用



VUE是基于MVVM的设计模式开发的，今天说一下MVC和MVVM的区别。

MVC：

m:model数据模型层   v:view视图层  c:controller控制器   

原理：c层需要控制model层的数据在view层进行显示

 

MVC两种方式，图片说明：

 



 

代码实例：

我们做一个很简单的DIV显示隐藏的效果，点击toggle可以切换下面div显示隐藏



html:

<div id="box">
        <button class="btn">toggle</button>
        <button class="btn2">big</button>
        <div class="box">

        </div>
    </div>
JS:

下面是我们不用设计模式写的JS，这种写法不利于维护，纯粹的面向过程去写代码：

        let btn = document.getElementsByClassName("btn")[0];
        let boxDom = document.getElementsByClassName("box")[0];
        let flag = true;
        btn.onclick = function(){
            if(flag){
                boxDom.style.display = "none";
                flag = false;
            }else{
                boxDom.style.display = "block";
                flag = true;
            }
        }
MVC的写法：

         //view
        let boxDom = document.getElementsByClassName("box")[0];
        //model
        let model = {
            isShow:true,
            isBig:false
        }
     
        //controller 业务逻辑
        function Controller(){
            this.init();//初始化
        }
        Controller.prototype = {
            constructor:Controller,
            init:function(){
                this.addEvent()
            },
            addEvent:function(){
                let btn = document.getElementsByClassName("btn")[0];
                let btn2 = document.getElementsByClassName("btn2")[0];
                let that = this;
                btn.onclick = function(){
                    model.isShow = !model.isShow;
                    //更改视图了
                    that.render();
                }
                btn2.onclick = function(){
                    model.isBig = true;
                    //更改视图了
                    that.render();
                }
            },
            render:function(){//数据驱动视图的更改
                boxDom.style.display = model.isShow?"block":"none";
                boxDom.style.width = model.isBig?"300px":"100px";
            }
        }
     
        new Controller();//初始化一下
虽然MVC代码比较长，不过以后用起来很方便，只要是相同的效果拿过来用就行

下面说一下MVVM

MVVM：

m:model数据模型层   v:view视图层  vm:ViewModel
vue中采用的是mvvm模式，这是从mvc衍生过来的
MVVM让视图与viewmodel直接的关系特别的紧密，就是为了解决mvc反馈不及时的问题  

图片说明一下：



说到MVVM就要说一下双向绑定和数据劫持的原理，

1、双向绑定的原理是什么？
（当视图改变的时候更新模型层，当模型层改变的时候更新视图层）
vue中采用了数据劫持&订阅发布模式：
vue在创建vm的时候，会将数据配置在实例当中，然后会使用Object.defineProperty对这些数据进行处理，为这些数据添加getter与setter方法。当获取数据的时候，会触发对应的getter方法，当设置数据的时候，会触发对应的setter方法，从而进一步触发vm上的watcher方法，然后数据了，vm进一步去更新视图。

2、 数据劫持：

vue.js 则是采用数据劫持结合发布者-订阅者模式，通过Object.defineProperty()来劫持各个属性的setter，getter。在数据变动时发布消息给订阅者，触发响应的监听回调。

Object.defineProperty代码实例：

//Object.defineProperty  因为使用了ES5的很多特性 
        let _data = {}
        let middle = 111;
        Object.defineProperty(_data,"msg",{
            get(){
                return middle;
            },
            set(val){
               middle = val;
            }
        });

        console.log(_data.msg);//获取数据的时候，会调用对应对象属性的getter方法
        _data.msg = 222;//设置数据的时候，会调用对应对象属性的setter方法
        console.log(_data.msg);
总结：

 mvvm与mvc最大的区别：
MVVM实现了view与model的自动同步，也就是model属性改变的时候， 我们不需要再自己手动操作dom元素去改变view的显示，而是改变属性后该属性对应的view层会自动改变。