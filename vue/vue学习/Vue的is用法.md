# Vue的is用法

回顾vue官方文档的过程中发现了is这个特性，虽然以我的写代码风格实在用不上，不过还是记录一下这个知识点

## is的作用

```apache
 <ul>
  <li></li>
  <li></li>
  <li></li>
</ul>
```

总所周知，ul里面嵌套li的写法是html语法的固定写法（还有如table,select等）。

```xml
//code1
 <ul>
  <my-component></my-component>
  <my-component></my-component>
</ul>
```

my-component是我们自己写的组件，但是html在渲染dom的时候，my-component对ul来说并不是有效的dom，甚至会报错。

## is的诞生

正是因为html模板的限制，于是就诞生了is。接下来我们就用is解决上面的问题~

```xml
 <ul>
  <li is='my-component'></li>
</ul>
```

首先你得注册my-component组件，全局或者局部都成。 **<li is='my-component'></li>其实就相当于<my-component></my-component>，语义上是一样一样的，就是解决了html模板的限制。**

### is的用法

#### <component> + is 的骚操作

```xml
<!-- 组件会在 `件名` 改变时改变 -->
<component :is="组件名变量"></component>
```

只要在data里弄个变量，给变量赋值就能动态的切换组件。这个其实在某些场景还是非常好用的安利一下。

### 不受html模板限制的情况

vue官网提醒以下来源使用模板的话，这条限制是不存在的：
字符串 (例如：template: '...')
单文件组件 (.vue)

<script type="text/x-template">