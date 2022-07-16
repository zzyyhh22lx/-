# Vue3

### **ref**

把数据变为响应式数据，ref把它们变成了对象，如果我们直接去操作代码是修改不了的，你会发现当前name和age还是通过get和set修改页面，这时你需要使用.value，并且ref还是Refimpl

```vue
<template>
	vue3中会检测到你的ref是对象，自动会给你加上.value
  <div class="home">
    <h1>姓名：{{name}}</h1>
    <h1>年龄：{{age}}</h1>
    <button @click="say">修改</button>
  </div>
</template>

<script>
import { ref } from 'vue'
export default {
  name: 'HelloWorld',
  setup () {
    const name = ref('中介')
    const age = ref(18)
    //RefImpl {__v_isShallow: false, dep: Set(1), __v_isRef: true, _rawValue: '波妞', _value: '波妞'}
    
    // 方法
    function say () {
      name.value = '波妞'
      age.value = 18
    }
    return {
      name,
      age,
      say
    }
  }
}
</script>

```

