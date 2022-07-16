# Vue-cli

- 基于webpack的vue打包工具
- 全局安装

*shell*

```shell
#全局安装
npm i -g @vue/cli

#使用
vue create 项目的名称
```

![image-20220627144130082](C:\Users\lin\AppData\Roaming\Typora\typora-user-images\image-20220627144130082.png)

- 按空格选星号（要下载的插件）

![image-20220627144400681](C:\Users\lin\AppData\Roaming\Typora\typora-user-images\image-20220627144400681.png)

- 把babel，和Eslint配置在独立的文件，package.json放版本信息就好

- 会设置仓库，依赖的包

- ## 运行：

*shell*

```shell
npm run serve
```

![image-20220627145003762](C:\Users\lin\AppData\Roaming\Typora\typora-user-images\image-20220627145003762.png)





# Vue-cli使用

*shell*

```shell
vue create 项目名称
cd 项目名称
npm run serve
```

- vue项目中src目录的组成

```html
assets文件夹：存放项目中用到的静态资源文件，如：css样式表，图片资源

components文件夹：程序员封装的，可复用的组件，都放在components目录下

main.js是项目的入口文件，整个项目的运行，先执行main.js

app.vue是项目的根组件，页面的结构
```

