# vuex

[开始 | Vuex (vuejs.org)](https://vuex.vuejs.org/zh/guide/#最简单的-store)

- 在Vue项目的开发过程中必然会使用到vuex，对vue项目公用数据进行管理，从而解决组件之间数据相互通信的问题，如果不使用vuex，那么一些非父子组件之间的[数据通信](https://so.csdn.net/so/search?q=数据通信&spm=1001.2101.3001.7020)将会变得极为繁琐。

- 每一个 Vuex 应用的核心就是 store（仓库）。“store”基本上就是一个容器，它包含着你的应用中大部分的**状态 (state)**。Vuex 和单纯的全局对象有以下两点不同：

1. Vuex 的状态存储是响应式的。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。
2. 你不能直接改变 store 中的状态。改变 store 中的状态的唯一途径就是显式地**提交 (commit) mutation**。这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用。

### 安装

```shell
npm install vuex@next -S
```

### 使用

`.vue`

```vue
<template>
  <div>测试组件</div>
  <hr>
  <!-- 页面中直接使用渲染时与vue2中的使用方法相同 -->
  <div>获取Store中的state、getters: {{$store.getters.formatInfo}}</div>
  <button @click='handleClick'>点击</button>
</template>

<script>
import { useStore } from 'vuex'

export default {
  setup () {
    // this.$store.state.info
    // Vue3中store类似于Vue2中this.$store
    // useStore()方法创建store对象，相当于src/store/index.js中的store实例对象
    const store = useStore()
    console.log('=============' + store.state.info) // hello
    // 修改info的值
    const handleClick = () => {
      // 触发mutations，用于同步修改state的信息
      // store.commit('updateInfo', 'nihao')
      // 触发actions，用于异步修改state的信息
      store.dispatch('updateInfo', 'hi')
    }
    return { handleClick }
  }
}
</script>
```

`@/store/index.js`

```js
import { createStore } from 'vuex'

const store = createStore({
  state: {
    info: 'hello'
  },
  mutations: {
    // 定义mutations，用于修改状态(同步)
    updateInfo (state, payload) {
      state.info = payload
    }
  },
  actions: {
    // 定义actions，用于修改状态(异步)
    // 2秒后更新状态
    updateInfo (context, payload) {
      setTimeout(() => {
        context.commit('updateInfo', payload)
      }, 2000)
    }
  },
  getters: {
    // 定义一个getters
    formatInfo (state) {
      return state.info + ' Tom'
    }
  },
  modules: {
  }
})

// 挂载vue实例
export default store
```

`main.js`

```js
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import store from './store/index'

// 挂载store，变成内置属性
createApp(App).use(router).use(store).mount('#app')

```

- `mutations`：
  + 只能同步
  + 触发：`store.commit('XXX')`

- `actions`：

  + Action 提交的是 mutation，而不是直接变更状态。

  + Action 可以包含任意异步操作。
  + 触发：`store.dispatch('XXX')`























