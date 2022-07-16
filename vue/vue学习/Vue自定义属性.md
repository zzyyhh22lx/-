# Vue自定义属性



- **vue所有的指令都以 v-  开头**

```html
<input type="text" class="form-control" v-model="sear" @input="search" v-focus>
```

```javascript
//第一个参数是指令的命令，参数2是一个对象，有钩子函数，可以在特定时间执行相应操作
Vue.directive('focus', {
            // 当绑定元素插入到 DOM 中。el: 指令所绑定的元素，可以用来直接操作 DOM 。
            inserted: function(el) {
                // 聚焦元素
                el.focus()
            }
        })
```

可以实现默认获得输入框的焦点

**定义局部：**

```javascript
 new Vue({ directives: {
                // 局部注册，优先调用
            directives: {
                // 注册一个局部的自定义指令 v-focus
                focus: {
                    // 指令的定义// 钩子函数// 
                    bind(el) { //在绑定后就执行，只执行一次
                        // el聚焦元素,表示绑定了指令的元素，是原生的js对象

                        // 在元素 刚绑定了指令的时候，还没有插入Dom中去，这时候，调用focus方法没有作用
                        // 因为，一个元素只有插入dom之后，才能获取焦点
                        el.focus(); //无效
                    },
                    inserted: function(el) { //inserted:被绑定元素插入父节点时调用一次（父节点存在即可调用，不必存在于 document 中）。

                        el.focus()
                    },

                    update: function() {
                        //当Vnode更新后会触发，可能多次
                    }
                }
            }
})
```



### 钩子函数

指令定义函数提供了几个钩子函数（可选）：

- `bind`: 只调用一次，指令第一次绑定到元素时调用，用这个钩子函数可以定义一个在绑定时执行一次的初始化动作。
- `inserted`: 被绑定元素插入父节点时调用（父节点存在即可调用，不必存在于 document 中）。
- `update`: 被绑定元素所在的模板更新时调用，而不论绑定值是否变化。通过比较更新前后的绑定值，可以忽略不必要的模板更新（详细的钩子函数参数见下）。
- `componentUpdated`: 被绑定元素所在模板完成一次更新周期时调用。
- `unbind`: 只调用一次， 指令与元素解绑时调用。





### 自定义私有属性

