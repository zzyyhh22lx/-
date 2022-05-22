<h1 align="center">Vue计算属性</h1>

```html
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="lib/vue.js"></script>
    <title>Document</title>
</head>

<body>
    <div id="example">
        <p>Original message: "{{ message }}"</p>
        <p>Computed reversed message: "{{ reversedMessage }}"</p>
        <p>{{ reserved() }}</p>
    </div>
    <script>
        var vm = new Vue({
            el: '#example',
            data: {
                message: 'Hello'
            },
            computed: { //只能computed，计算属性 方法直接调用
                // 计算属性的 getter
                reversedMessage: function() {
                    // `this` 指向 vm 实例
                    return this.message.split('').reverse().join('')
                }
            },
            methods: {
                // 有括号就到methods中来找
                reserved: function() {
                    return this.message.split('').reverse().join('');
                }
            }
        })
    </script>
</body>

</html>
```

- 在{{}}里面
- 有括号，判断是方法，到Vue实例的methods属性找
- 数据到data找
- 计算属性到computed找（ 直接调用的函数 ）

```html
你可以像绑定普通 property 一样在模板中绑定计算属性。Vue 知道 vm.reversedMessage 依赖于 vm.message，因此当 vm.message 发生改变时，所有依赖 vm.reversedMessage 的绑定也会更新。而且最妙的是我们已经以声明的方式创建了这种依赖关系：计算属性的 getter 函数是没有副作用 (side effect) 的，这使它更易于测试和理解。
```



我们可以将同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。然而，不同的是**计算属性是基于它们的响应式依赖进行缓存的**。只在相关响应式依赖发生改变时它们才会重新求值。这就意味着只要 `message` 还没有发生改变，多次访问 `reversedMessage` 计算属性会立即返回之前的计算结果，而不必再次执行函数。

这也同样意味着下面的计算属性将不再更新，因为 `Date.now()` 不是响应式依赖：

```javascript
computed: {
  now: function () {
    return Date.now()
  }
}
```

相比之下，每当触发重新渲染时，调用方法将**总会**再次执行函数。

我们为什么需要缓存？假设我们有一个性能开销比较大的计算属性 **A**，它需要遍历一个巨大的数组并做大量的计算。然后我们可能有其他的计算属性依赖于 **A**。如果没有缓存，我们将不可避免的多次执行 **A** 的 getter！如果你不希望有缓存，请用方法来替代。



### [计算属性 vs 侦听属性](https://vuejs.bootcss.com/guide/computed.html#计算属性-vs-侦听属性)

Vue 提供了一种更通用的方式来观察和响应 Vue 实例上的数据变动：**侦听属性**。当你有一些数据需要随着其它数据变动而变动时，你很容易滥用 `watch`——特别是如果你之前使用过 AngularJS。然而，通常更好的做法是使用计算属性而不是命令式的 `watch` 回调。细想一下这个例子：

```html
<div id="demo">{{ fullName }}</div>
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
```

- watch方法：监听data中改变的值

- ```javascript
          var vm2 = new Vue({
                  el: '#demo',
                  data: {
                      firstName: 'Foo',
                      lastName: 'Bar',
                      fullName: 'Foo Bar'
                  },
                  watch: {
                      // watch属性 监听改变的值（data）
                      //如果firstname或者lastname改变进行操作
                      firstName: function(val) {
                          this.fullName = val + ' ' + this.lastName
                      },
                      lastName: function(val) {
                          this.fullName = this.firstName + ' ' + val
                      }
                  }
              })
  ```

- 也可以用 `vm2.$watch('firstName'),(newvalue,oldvalue)=>{}` 



上面代码是命令式且重复的。将它与计算属性的版本进行比较：

```javascript
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```





#### 侦听器

```html
    <!-- 侦听器 -->

    <div id="watch-example">
        <p>
            Ask a yes/no question:
            <input v-model="question">
        </p>
        <p>{{ answer }}</p>
    </div>
    <!-- < !--因为 AJAX 库和通用工具的生态已经相当丰富， Vue 核心代码没有重复-- >
    <
    !--提供这些功能以保持精简。 这也可以让你自由选择自己更熟悉的工具。-- > -->
    <script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js">
    </script>
    <script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>

    <script>
        var watchExampleVM = new Vue({
            el: '#watch-example',
            data: {
                question: '',
                answer: '可以再你问完问题后给你回答yes or no!( 英文输入 )'
            },
            watch: {
                // 如果 `question` 发生改变，这个函数就会运行
                question: function(newQuestion, oldQuestion) {
                    this.answer = 'Waiting for you to stop typing...'
                    this.debouncedGetAnswer()
                }
            },
            created: function() {
                // `_.debounce` 是一个通过 Lodash 限制操作频率的函数。
                // 在这个例子中，我们希望限制访问 yesno.wtf/api 的频率
                // AJAX 请求直到用户输入完毕才会发出。想要了解更多关于
                // `_.debounce` 函数 (及其近亲 `_.throttle`) 的知识，
                // 请参考：https://lodash.com/docs#debounce
                this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
            },
            methods: {
                getAnswer: function() {
                    if (this.question.indexOf('?') === -1) {
                        this.answer = '问题一般包含问号? ;-)'
                        return
                    }
                    this.answer = 'Thinking...'
                    var vm = this
                    axios.get('https://yesno.wtf/api')
                        .then(function(response) {
                            vm.answer = _.capitalize(response.data.answer)
                        })
                        .catch(function(error) {
                            vm.answer = 'Error! Could not reach the API. ' + error
                        })
                }
            }
        })
    </script>
```

