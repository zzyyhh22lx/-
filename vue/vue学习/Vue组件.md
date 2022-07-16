# Vue组件



**每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。**



**vue组件注册是通过 `Vue.component`进行全局注册**

```javascript
 Vue.component('button-counter', {
     ...//button-counter是组件名字
 })
```

**注意：**

- v-for中的key可以让一些元素进行绑定，比如input:radio和input:text进行绑定，增加其他元素时，不会改变位置
- ![image-20220522143723350](C:\Users\lin\AppData\Roaming\Typora\typora-user-images\image-20220522143723350.png)

+ 对组件的使用，必须要用new Vue({})先进行操作，才能使用组件，否则不显示

- props是自定义属性，相当于attribute，有值进行传递时，就变成组件实例的一个property，property 是元素对象的属性

+ property和attribute的区别：
  + property是自带的属性 
  + attribute可以是自定义属性
  +  input.value和 input.getAttribute('value') 的值在改变input输入时会出现不同

```html
   <input id="input" value="test value">
    <input type="button" value="aaa" id="btn">
    <script>
        let input = document.getElementById('input');
        document.getElementById('btn').onclick = function() {
            console.log(input.getAttribute('value')); // test value
            console.log(input.value);
        }
    </script>
```

- 在组件的全局注册中，有个问题，就是组件的template的内容，比如：

  + ```javascript
    Vue.component('button-counter', {
        data:function(){
            return {
                mes:'为什么组件的data要用函数，组件使用不止一次，可以修改组件的内容而不影响其他组件'
            }
        },
         template:'<li>{{mes}}</li>'
     })
    ```

  + 这里的{{mes}}指的是哪里的mes？**不是`new Vue({})`中data的值，而是component中的data的值**

- vue注册事件的大小写问题：

  + ```javascript
    Vue.component('component-a',{});
    //是全局事件，有时用webpack打包会造成代码无用
    ```

  + 因为componentA的驼峰命名法，html大小写不敏感，所以必须用 - 来限制

  + component-a 识别为ComponentA，大小写`<ComponentA></ComponentA>`



- ##### vue注册事件的全局和局部问题

  + ```javascript
    new Vue({
        el:'',
        components:{
            componentA:'component-a'
        }
    })
    ```

  + 实现局部可以如上

  + 注意局部注册的组件在其子组件中不可以使用（希望 `ComponentA` 在 `ComponentB` 中可用 ）

  + ```js
    const ComponentA = {
      /* ... */
    }
    
    const ComponentB = {
      components: {
        'component-a': ComponentA
      }
      // ...
    }
    ```





- ##### props：父组件传递参数给子组件

```html
  <div id="app-7"><ol>
            <todo-item v-for="item in groceryList" v-bind:todo="item" v-bind:key="item.id"></todo-item></ol></div>
    <script>
        Vue.component('todo-item', {
            // props是自定义属性，相当于attribute，有值进行传递时，就变成组件实例的一个property，property 是元素对象的属性
            // props父组件传递值给子组件的桥梁，否则子组件无法获得父组件的数据
            props: ['todo'],
            template: '<li>{{ todo.text }}</li>'
        })
        var app7 = new Vue({
            el: '#app-7',
            data: {
                groceryList: [{
                    id: 0,text: '蔬菜'}, {id: 1,text: '奶酪'}, {id: 2, text: '随便其它什么人吃的东西'}]}
        })
    </script>
```



- ##### emit：子组件传递参数给父组件

```html
<div id="app"><div id="counter-event-example">
            <p>{{ total }}</p>
            <button-counter v-on:increment="incrementTotal"></button-counter>
            <button-counter v-on:increment="incrementTotal"></button-counter>
           <!-- increment 子组件传递的点击事件 子组件先执行点击事件后冒泡给父组件执行点击事件 -->
        </div> </div>
    <script>
        Vue.component('button-counter', {
            template: '<button v-on:click="incrementHandler">{{ counter }}</button>',
            data: function() {
                return {counter: 0}},
            methods: {
                incrementHandler: function() {
                    this.counter += 1
                    this.$emit('increment');//点击事件传递给父组件，有点像冒泡
                }}, })
        new Vue({
            el: '#counter-event-example',
            data: {total: 0},
            methods: {
                incrementTotal: function() {this.total += 1}}})
    </script>
```



- ##### 组件上的 v-model 默认会利用名为 value 的 prop 和名为 input 的事件。

```html
<input v-model="parentData">
```

等价于：

```html
<input 
    :value="parentData"
    @input="parentData = $event.target.value"
>
```





- #### 使用组件方式2

```html
    <div id="com">
        <com></com>
    </div>

    <template id="tmp">
        <h1>啦啦啦</h1>
    </template>


    <script>
        var com = {
            template: '#tmp',
        }

        // 直接在
        new Vue({
            el: '#com',
            components: {
                com // com : com
            }
        })
    </script>
```

- #### render

```html
<body>
    <div id='app'>
        <p>我没了这里不会显示，被render重新渲染了</p>
    </div>

    <script>
        var login = {
            template: '<div>注册</div>'
        }
        var app = new Vue({
            el: '#app',
            data: {}
            // 用render渲染
            // createElements是一个方法，能够把指定的组件模板，渲染到html页面中
            render: function(createElement) {
                return createElement(login)
                    // 用render渲染的html会把 el 所代表的容器全部清空，只放一组件进去
            }
        })
    </script>
</body>
```











