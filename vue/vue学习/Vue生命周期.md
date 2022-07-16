# Vue生命周期





```html
<div id='app'>
        <h3 id="h3">{{mes}}</h3>
    </div>

    <script>
        var app = new Vue({
            el: '#app',
            data: {
                mes: 'hello world'
            },
            methods: {},
            beforeCreate: function() { //这是遇到的第一个生命周期函数，在实例对象前就被创建出来,数据没有被初始化
                console.log(this.mes); //undefined
            },
            created: function() { //这是遇到的第二个生命周期函数，数据初始化完成后执行
                console.log(this.mes); //hello world
            },
            beforeMount() { //这是遇到的第三个生命周期函数，表示模板已经在内存编译完成了，但是没有吧模板渲染到页面中
                console.log(document.getElementById('h3').innerText); //{{mes}}
            },
            mounted() { //这是遇到的第四个生命周期函数，内存中的模板已经挂载到页面中，渲染完全，
                // 实例创建期间最后一个周期函数
                console.log(document.getElementById('h3').innerText); //hello world
            }
        })
    </script>



<div id='app'></div>

<script>
var app = new Vue({
el: '#app',
data: {},
methods: {},
beforeCreate: function() {},
created: function() {},
 beforeMount() {},
mounted() {},
beforeUpdate() {},
updated(){},
 beforeDestroy() {},
Destroy() {}
})
</script>
```

#### 在`beforeCreate`和`created`执行完后，vue进行模板编译：

![d52eb1b2327b856de09b2d55840fc91](C:\Users\lin\AppData\Local\Temp\WeChat Files\d52eb1b2327b856de09b2d55840fc91.png)



#### mounted阶段

![image-20220604192727254](C:\Users\lin\AppData\Roaming\Typora\typora-user-images\image-20220604192727254.png)





![Vue 实例生命周期](https://vuejs.bootcss.com/images/lifecycle.png)