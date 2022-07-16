<h1 align="center"> Vue基本指令</h1>

​	

## Vue只能操作一个元素，总是操作最后一个

**vue**

```javascript
var app = new Vue({
    el:'#app',
    data:{
    	message:'hello Vue'
	},
    methods: {
              //方法
         reverseData: function() {
              // split('') 把字符串变为数组
              this.message = this.message.split('').reverse().join('');
            }
    }
})
```

- `el`绑定的元素
- `data`数据

一个 Vue 应用会将其挂载到一个 DOM 元素上 (对于这个例子是 `#app`) 然后对其进行完全控制。那个 HTML 是我们的入口，但其余都会发生在新创建的 Vue 实例内部。



**html的元素中：**

- `v-bind`：

  v-bind 主要用于属性绑定，比方你的class属性，style属性，value属性，href属性等等，只要是属性，就可以用v-bind指令进行绑定

- `v-if`：事件判断

```html
<div id="app-3">
        <p v-if="seen">可以见</p>
        <p v-if="nothing">不可以见</p>
    </div>
    <script>
        var app3 = new Vue({
            el: '#app-3',
            data: {
                // 默认是false
                seen: true, //可见   
                nothing: false, //不可以见并且不占空间
            }
        })
   </script>
```



- `v-for`：事件循环

```html
 <div id="app-4">
        <ol>
            <li v-for="book in books">{{book.name}}</li>
        </ol>
 </div>
    <script>
        var app4 = new Vue({
            el: '#app-4',
            // 可以在控制台输出app4.books.push({ name: '新项目' })来增加项目名称
            data: {
                books: [{
                    name: '西游记'
                }, {
                    name: '红楼梦'
                }, {
                    name: '水浒传'
                }
            }
        })
        app4.books.push({
            name: '三国演义'
        });
    </script>
```



- `v-on`：绑定事件 

​		+ v-on:click="函数名字"

- `v-model`：双向绑定

```html
<p>{{ message }}</p>
<input v-model="message">
<!-- input的内容是和p中的内容保持一致的 -->
```



**注册组件**

```html
  <!-- 注册组件 -->
    <div id="app-7">
        <ol>
            <todo-item v-for="item in groceryList" v-bind:todo="item" v-bind:key="item.id"></todo-item>
        </ol>
    </div>
    <script>
        Vue.component('todo-item', {
            props: ['todo'],
            template: '<li>{{ todo.text }}</li>'
        })

        var app7 = new Vue({
            el: '#app-7',
            data: {
                groceryList: [{
                    id: 0,
                    text: '蔬菜'
                }, {
                    id: 1,
                    text: '奶酪'
                }, {
                    id: 2,
                    text: '随便其它什么人吃的东西'
                }]
            }
        })
    </script>
```



- v-bind：讲解：

- ```html
  <!-- 绑定一个属性 -->
  <img v-bind:src="imageSrc">
  
  <!-- 缩写 -->
  <img :src="imageSrc">
  
  <!-- 内联字符串拼接 -->
  <img :src="'/path/to/images/' + fileName">
  
  <!-- class 绑定 -->
  <div :class="{ red: isRed }"></div>
  <div :class="[classA, classB]"></div>
  <div :class="[classA, { classB: isB, classC: isC }]">
  
  <!-- style 绑定 -->
  <div :style="{ fontSize: size + 'px' }"></div>
  <div :style="[styleObjectA, styleObjectB]"></div>
  
  <!-- 绑定一个有属性的对象 -->
  <div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>
  
  <!-- 通过 prop 修饰符绑定 DOM 属性 -->
  <div v-bind:text-content.prop="text"></div>
  
  <!-- prop 绑定。“prop”必须在 my-component 中声明。-->
  <my-component :prop="someThing"></my-component>
  
  <!-- 通过 $props 将父组件的 props 一起传给子组件 -->
  <child-component v-bind="$props"></child-component>
  ```

  