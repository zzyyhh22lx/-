<h1>Vue</h1>

- vue使用

```js
var vm = new Vue({
    el:'#app'
})

//=

var vm2 = new Vue({}).$mount('#app')
```

- .vue文件

```vue
<template>
    <div class="testbox">
        <p>这是我自定义的组件=={{username}}</p>
        <button @click="changeName">修改用户名</button>
    </div>
</template>

<script>
// 默认导出，固定写法
export default{
    // data 数据源，必须用函数
    data: function(){
        return {
            username:'lhy'
        }
    },
    method:{
        changeName(){
            // this表示实例
            this.username = 'xzy'
        }
    }
}
</script>

<style>
p{
    font-size:20px;
    color:#ccc
}
button {
    padding:20px;
}
</style>
```



- 使用props 父向子组件传值时加：和不加：的区别：
  + **只有传递`字符串常量`时，不采用v-bind形式，其余情况均采用v-bind形式传递。**
  + 传递变量，number，boolean，object都需要在前加：

- ### 导出组件

```vue
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  //name代表导出的组件的标签名字
    
  props: {
    msg: String
  }
}
</script>

<style scoped lang="less">
h3 {
  margin: 40px 0 0;
}
</style>
```

- 在其他vue组件就可以使用其导出的组件
- `<HelloWorld></HelloWorld>`

`import HelloWorld from '@/components/HelloWorld.vue'`

```vue
<template>
  <div class="home">
    <HelloWorld msg="这里是父组件向子组件传递的字符串常数"/>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'HomeView',
  components: {
    HelloWorld
  }
}
</script>
```

- ### 导出组件对象

*App.vue：*

```vue
<template>
  <div class="app">
    <nav>
      <router-link to="/">Home</router-link>
    </nav>
    <router-view/>
  </div>
</template>

<style lang="less">
.app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
```

*main.js*

```js
import Vue from 'vue'
import App from './App.vue'
import router from './router'

Vue.config.productionTip = false

new Vue({
  router,
  render: h => h(App)//直接渲染出来
}).$mount('#app')
```

### ref引用dom

```vue
<template>
	<h1 ref="myh1" @click="show">
       ref引用DOM元素 
    </h1>
</template>
<script>
export default {
    methods:{
        show(){
            this.$ref.myh1.style.color="red"
        }
    }
}
</script>
```

##### 注意：如果用`v-if`变化来更新元素，不能直接用`this.$ref`来操作dom，不然会得到`undefined`，因为页面渲染需要时间，用`this.$nextTick(cb)`，cb是回掉函数，可以在页面渲染完后再调用

```vue
<template>
	blur失去焦点事件
	<input ref="iptRef" v-if="inputVisible" @blur="showButton" />
    <button v-else @click="showInput">展示输入框</button>
</template>
<script>
export default {
    data(){
      return {
          inputVisible: false
      }  
    },
    methods:{
        showInput(){
            this.inputVisible = true
            this.$nextTick(() => {
                this.$refs.iptRef.focus()
            })
        },
        showButton(){
            this.inputVisible = false
        }
    }
}
</script>
```

##### 为什么不能用updated生命周期函数：因为数据改变就会调用，此时如果`v-if`为false则会报错





### Vue插槽`<slot>`

```vue
<template>
<slot name="default" msg="helloVue" :user="a"></slot>
:的区别是需要不需要用到data的值
</template>
```

- 引用

```vue
<template>

<template #default="obj">
<h1>
    作用域插槽，绑定信息--{{ obj.msg }}
    ==>' helloVue ' 取决于插槽绑定的数据 obj是一个新参，一般写为scope
    name默认是default，可以改为其他，#是v-slot缩写
    
    也可以用结构赋值，#default={msg,user}
    {{msg}}==' helloVue '
    </h1>
</template>

</template>
```

- 注意：

v-slot只能被template包裹起来，也就是放在`<template>`之中，不过当导入其他组件，比如`<right>`组件，也可以**`<right #defalut>`**，因为组件本身就是template包裹起来的

### 全局声明指令

- 在main.js中定义

```js
import Vue from 'vue'
import VueResource from 'vue-resource'
Vue.use(VueResource)

Vue.config.productionTip = false

//全局定义自定义指令
Vue.directive('focus', {
            inserted(el) {
                el.focus();
            }
 })

new Vue({
    // router,
    render: h => h(App)
}).$mount('#app');

```

### Eslint

- 规范代码风格
  + 不能空 >1行
  + 文件末尾要有一行留空
  + 字符串用单引号
  + 对象，属性，值有空格分隔
  + 对象属性末尾不允许多余逗号或者分号
  + 注释用一致的间距
  + 一致的缩进
  + import放在srcipt标签顶部



### 命名规则：

- 组件中class类名为组件名字加 - 



### 挂载axios

- 在main.js中

```js
import axios from 'axios'

Vue.prototype.axios = axios
```

- 在实例对象中

```vue
export default {
	methods: {
		const {data: res} = this.axios.get('XXX')
}
}
```

- 缺点：

不利于api接口复用



### SPA:单应用页面( 路由 )

- hash模式和history模式区别
  + 形式上：hash模式url里面永远带着#号，开发当中默认使用这个模式。如果用户考虑url的规范那么就需要使用history模式，因为history模式没有#号，是个正常的url，适合推广宣传；
  + 功能上：比如我们在开发app的时候有分享页面，那么这个分享出去的页面就是用vue或是react做的，咱们把这个页面分享到第三方的app里，有的app里面url是不允许带有#号的，所以要将#号去除那么就要使用history模式，但是使用history模式还有一个问题就是，在访问二级页面的时候，做刷新操作，会出现404错误，那么就需要和后端人配合，让他配置一下apache或是nginx的url重定向，重定向到你的首页路由上就ok了

- 实现路由

```html
<router-link to="/acount">acount</router-link>
<!--相当于-->
<a href="#/acount">acount</a>

<!--占位符：-->
<router-view></router-view>

<!--另一个vue页面的子路由-->
<router-link to="/acount/register">注册</router-link>
<router-link to="/acount/login">注册</router-link>
```

```js
export default {
	name:'acount',
	data(){},
	created(){
	//只要组件一创建，就默认调用onhashchange事件
    //这里不用function，因为会出现this指向的问题
	window.onhashchange = () => {
			console.log('监听到hash地址改变',location.hash)
			switch(location.hash){
				case '#/login':
				xxxxxxxxxxx
			}
		}
	}
}
```

```js
const routes = [{
                path: '/',
                redirect: '/acount'
            },{
                path: '/acount',
                component: acount,
           //嵌套路由：
                children: [{
                    //一般不加斜线
                    path: "login",
                    component: login
                }, {
                    path: "register",
                    component: register
                }]
            }, ]

const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes,
  linkActiveClass: 'router-link-active' //配置类名，所指路由会有该类名
})
```



组件一创建，就默认调用onhashchange事件，location.hash获得路由变化的值。

**注意：不用function，因为会出现this指向的问题，用箭头函数**

- 动态路由匹配

- **把hash地址中可变的部分定义为参数项 ==>  `/:id`**

```js
path: '/account/1',
```

拿到id的值

```js
//方式一：
created() {
                console.log(this.$route);
    //params属性获得参数项 { id: 1 }，this可加可不加
                this.meg = this.$route.params.id;//1
            }

//方式二：方便记住
export default {
    //通过自己设置
    props:['mid']
}

//index.js
const routes = [{
                path: '/',
                redirect: '/acount'
            },{
                //mid 是自己props设置的
                path: '/acount/:mid',
                component: acount,
                //props:true 用props传参
             	props:true
            }, ]
```



- **query：`?id=10`**

拿到 ?id 后面的值

```html
 <router-link to="/login?id=55&name=lhy">登录</router-link>
```

```js
created: function() {
                console.log(this.$route.query);//{id: 55,name: lhy}
    //query，路径参数
                this.msg = this.$route.query.id;
            }
```

- **获取path路径**

![image-20220704164717836](C:\Users\lin\AppData\Roaming\Typora\typora-user-images\image-20220704164717836.png)

```js
const vm = new Vue({
            el: '#app',
            router: routerObj, //将路由规则对象注册到vm实例上，监听url地址展示

            // 监听非dom元素的改变
            watch: {
                '$route.path': function(newValue, oldValue) {
                    console.log(newValue + '------' + oldValue);
                }
            }
        })
```

- + 要完整路径`/path/login?id=10` 用  `fullPath`
  + 要路径`path/login` 用 `path`

### 声明式导航和编程式导航

声明式导航：用`<a>`标签和vue的`<route-link>`实现跳转

编程式导航：API操作：`location.href` 或者vue 的  :  

`this.router.push('hash地址')，this.router.replace('hash地址')，this.router.go(数值)`



- 编程式导航：`this.router.push()` **注意：是router**

增加click事件：

```js
methods: {
    gotoLK() {//通过编程式导航API，
        //push跳转指定页面，并且增加一条历史记录
        this.$router.push('/movie/1')
        
        //replace 跳转指定hash 地址，并且替换当前历史记录
        
        //go(-1) 后退一个历史记录，前进后退：简写
        //this.$router.back()后退一层
        //this.$router.forword()前进一层
    }
}
```

- 命名视图

```vue
<router-view class="view left-sidebar" name="LeftSidebar"></router-view>
<router-view class="view main-content"></router-view>
<router-view class="view right-sidebar" name="RightSidebar"></router-view>
```

一个视图使用一个组件渲染，因此对于同个路由，多个视图就需要多个组件。确保正确使用 `components` 配置 (带上 **s**)：

```js
const router = createRouter({
  history: createWebHashHistory(),
  routes: [
    {
      path: '/',
      components: {
        default: Home,
        // LeftSidebar: LeftSidebar 的缩写
        LeftSidebar,
        // 它们与 `<router-view>` 上的 `name` 属性匹配
        RightSidebar,
      },
    },
  ],
})
```



### 导航守卫

控制访问权限

js

```js
const router = new VueRouter({})

//调用路由实例对象的 beforeEach 方法，即可声明“全局前置守卫”
//每次发生路由导航切换时，会触发 fn 这个回掉函数
router.beforeEach((to,from,next)=>{
    //to 是将要访问的路由的信息对象
    //from 是将要离开的路由的信息对象
    //next 是一个函数，调用next函数 表示放行，允许这次路由导航
    
    if(to.path === '/main') {
      const token = localStorage.getItem('token')
      if(token){
          next()
      } else {
        next('/login')  
      } else {
          next() //访问的不是后台主页，直接放行
      }
})
```

- 路由放行：当用户没有访问权限，访问路径，强制跳转其他页面：next( '/login' )

- 无法跳转，保留在当前页面：next( false )
- localStorage：创建一个本地存储

#### localStorage使用

```js
localStorage.setItem("lastname", "Smith");
    // 检索
console.log(localStorage.getItem("lastname"))
	// Smith
	//清空
localStorage.removeItem('lastname')
```



### vant组件库

- 不用自己封装组件，直接用

- 下载：

```shell
#vue2
npm i vant@latest-v2 -S

#vue3
npm i vant -S
```

[Vant - 轻量、可靠的移动端 Vue 组件库 (gitee.io)](https://vant-contrib.gitee.io/vant/v1/#/zh-CN/actionsheet)

- 全部导入

```js
import Vue from 'vue';
import Vant from 'vant';
import 'vant/lib/index.css';

Vue.use(Vant);
```

### Vant 组件库的 rem 布局适配

> 如果需要使用 `rem` 单位进行适配，推荐使用以下两个工具：
>
> - [postcss-pxtorem](https://github.com/cuth/postcss-pxtorem) 是一款 PostCSS 插件，用于将 px 单位转化为 rem 单位
> - [lib-flexible](https://github.com/amfe/lib-flexible) 用于动态设置 rem 基准值
>
> 详情请参考 Vant 的官方文档：https://vant-contrib.gitee.io/vant/#/zh-CN/advanced-usage#rem-bu-ju-gua-pei

#### 配置postcss-pxtorem

1. 运行如下的命令：

   ```bash
   npm install postcss-pxtorem@5.1.1 -D
   ```

2. 在 vue 项目根目录下，创建 postcss 的配置文件 `postcss.config.js`，并初始化如下的配置：

   ```js
   module.exports = {
     plugins: {
       'postcss-pxtorem': {
         rootValue: 37.5, // 根节点的 font-size 值
         propList: ['*'] // 要处理的属性列表，* 代表所有属性
       }
     }
   }
   ```

3. 关于 px -> rem 的换算：

   ```text
   iphone6
   
   375px = 10rem
   37.5px = 1rem
   1px = 1/37.5rem
   12px = 12/37.5rem = 0.32rem
   ```

####  配置 amfe-flexible

1. 运行如下的命令：

   ```bash
   npm i amfe-flexible@2.2.1 -S
   ```

2. 在 main.js 入口文件中导入 `amfe-flexible`：

   ```js
   import 'amfe-flexible'
   ```

#### 可以定制主题

- 修改某个组件的样式，并永久保存

#### vant可以实现上拉刷新，下拉刷新：

- **展示组件list**
- [Vant 2 - 轻量、可靠的移动端组件库 (gitee.io)](https://vant-contrib.gitee.io/vant/v2/#/zh-CN/theme)



### 项目结构

- src目录建一个utils目录，用于存放一些工具：

  + axios封装请求根路径

    ```js
    //request.js
    import axios from 'axios'
    
    const request = axios.create({
        //指定请求根路径
        baseURL: "https://neteasecloudmusicapi.vercel.app"
    })
    
    export default request
    
    //使用直接import
    //request.get('/xxx')
    ```

  

- src目录建一个api目录，API接口封装到这个模块中，可以复用：

  + 写api函数( 使用重复的可以直接调用 )

    ```js
    //向外导出
    //initAPI.js
    export const getInitAPI = function(){
        xxx
    }
    
    //使用：
    //import { getInitAPI } from '@/api/initAPI.js'
    //getInitAPI()
    ```

