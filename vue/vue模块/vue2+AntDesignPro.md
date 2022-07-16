## 对vue理解

[介绍 — Vue.js (vuejs.org)](https://cn.vuejs.org/v2/guide/)

- 1.1Vue是一套构建用户界面的渐进式框架,也可以理解为是一个视图模板引擎,强调的是状态到界面的映射。Vue.js 的目标是通过尽可能简单的 API 实现**响应的数据绑定**和**组合的视图组件**。vue所有的东西都是响应式的

- 1.2对生命周期的理解

  创建对象——监控——初始化事件——编译——销毁

- 1.3vue双向绑定原理（**Object.defineProperty()**）

  vue的双向绑定是由**数据劫持结合发布者－订阅者模式实现的，**那么什么是数据劫持？vue是如何进行数据劫持的？说白了就是通过**Object.defineProperty()**来**劫持对象属性的setter**和**getter操作**



### Ant Design

[Ant Design of Vue - Ant Design Vue (antdv.com)](https://www.antdv.com/docs/vue/introduce-cn/)

-  Ant Design 的 Vue 实现，开发和服务于企业级后台产品。

- 安装

  ```shell
  npm install ant-design-vue --save
  ```

### umi

[快速上手 (umijs.org)](https://v3.umijs.org/zh-CN/docs/getting-started)

- 安装使用

```shell
npm i yarn tyarn -g

tyarn -v

yarn create @umijs/umi-app

yarn

yarn start
```



### AntDesign Pro

[开始使用 - Ant Design Pro](https://pro.ant.design/zh-CN/docs/getting-started)

Ant Design就是一套组件库

- 安装和启动

  ```shell
  npm i @ant-design/pro-cli -g
  pro create myapp
  
  cd myapp && npm install
  
  npm run start
  ```

  

AntDesign Pro是开箱即用的中台前端/设计解决方案

- 其实，Ant Design Pro相对于Ant Design主要特点就在于“开箱即用”，也就是它是建立在Ant Design(UI部分)+umi(数据逻辑部分）等框架之上的一个“高级框架”。不太严谨地说，Ant Deisgn Pro其实是一个网站生成器。使用一条命令就可以生成一个完整可用的后台网站，而我们接下来需要做的就是删掉没用的部分，然后把有些文案标题什么的改一下，然后根据自己的需要去组合自己想要的页面和功能。

- 用Ant Design Pro写代码有点像插积木，大的框架和组件都给你准备好了，你只需要取得自己需要的东西，然后在代码中组合起来即可。

- 不过需要注意的是，Ant Design Pro其实只是一个前端框架，如果需要后端API服务的话，需要自己另外写代码～

![img](https://pic3.zhimg.com/80/v2-3d4d8e36625abaaf7bfe75e6dcfeef6e_720w.jpg)



如上图所示，如果想为自己的网站添加一个登录页面，执行

```shell
npx umi block add UserLogin --path=/user/login
```

命令即可，但需要将--path的参数修改为你自己的路径。这个路径是比较重要的，决定了两个东西：

1. 这个页面在URL中的路径是什么。Ant Design Pro的每个页面都对应了一个独立的URL Path。
2. 这个页面在菜单栏以及面包屑中的位置和层级关系。



