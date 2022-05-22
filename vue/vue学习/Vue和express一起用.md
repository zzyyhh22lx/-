## Vue和express一起用

**自学**

- 创建目录文件：
  + app.js （ 项目入口请求 ）
  + router.js （ 路由分发模块 ）
  + controller （ 处理业务逻辑 没有涉及处理数据的CRUD ）
  + model （ 负责操作数据库，进行数据的CRUD  =>create read update delete ）
  + public （ 公共访问区域 ）
  + views （ 模板引擎渲染 ）
  + src （ 能被webpack打包的目录，里面放置index.js和index.html ）



- npm安装：

- 配置文件：



### 注意：

- 首先：express的render(...,{})使用art-template 模板引擎使用也是用 {{}} 包括起来，跟vue是重合的，浏览器会首先渲染模板引擎 ，而忽略vue打包

  + 可以使用  delimiters: ['$$', '$$'],将vue 的 插入数据格式改为 $$ message $$

  ```javascript
    var app = new Vue({
              delimiters: ['$$', '$$'],
              el: '#components-demo',
              data: {
                  message: 'hello'
              }
          })
  ```

  + 不过如果是在组件中使用 ，Vue自定义指令对 template 标签不生效

  比如：

  ```javascript
        Vue.component('button-counter', {
              data: function() {
                  return {
                      count: 0,
                      mes:'you click'
                  }
              },
              template: '<button @click="count++">You click {{count}}</button>'
          });
  ```

  如果有使用模板引擎的时候，count不能被输出，只能用`v-text="mes+count"`进行输出

  + 