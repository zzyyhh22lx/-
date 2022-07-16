# Vue

- ### vue的函数名加括号和不加括号的区别

- **在事件绑定中：**

  + ##### 加与不加括号的区别在于事件对象参数 event 的处理。

    ##### 不加括号时，函数第一个参数为 event，加了括号后，需要手动传入 $event 才能获得事件对象

  + ```html
    <div id="app">
        <span @click="getEvent"></span>
        <span @click="getE(1,$event)"></span>
    </div>
    
    <script>
    var app = new Vue({
        el:'#app',
        data:{},
        methods:{
            getEvent(e){
    //不传参的时候,e默认是鼠标事件
                console.log(e.target);
            },
            getE(data,e){
                 console.log(e.target);
            }
        }
    })
    </script>
    ```

- ##### 不在事件绑定中：

  + ```html
    <div id="app">
        <span>{{ time() }}</span>
    </div>
    ```

  + 不用v-on绑定，()就是用来直接调用函数

  + 也可以用计算属性，computed，这样就可以不用加括号