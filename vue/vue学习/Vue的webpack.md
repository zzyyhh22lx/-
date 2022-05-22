# Vue的webpack

**作用：工程化**

可以解决浏览器不适配的问题，打包压缩代码



### 准备阶段

有个`src`文件夹 放有`index.html`和`index.js`



### 下载:( 安装包文件 )

```shell
npm init -y
npm install webpack webpack-cli --save-dev
```

`--save-dev`可以写成`-D`



### 配置：

在项目根目录添加一个`webpack.config.js`的webpack配置文件，初始化如何基本配置：

```javascript
const path = require('path');

module.exports = {
    mode: 'development', //mode 用来指定构建模式，可以选值有 development 和 production
    entry: path.join(__dirname, './src/index.js'), //打包文件路径
    output: {
        path: path.join(__dirname, './dist'), //输出文件存放路径
        filename: 'bundle.js' //输出文件名称
    }
}
```

在`package.json` 中  scripts标签加入配置：

```java
 "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack --mode production",
    "dev": "webpack --mode development"
  },

```

"build": "webpack --mode production", // 配置生产环境 追求体积质量
 "dev": "webpack --mode development" // 配置开发环境  追求打包速度



### 创建入口文件

*webpack 默认的入口文件是 src文件夹下的index.js文件*

创建src文件夹和 index.js文件



### 创建命令

```shell
npm run build
npm run dev
```



创建完项目多一个目录`dist`，里面有`main.js`

![image-20220518165357845](C:\Users\lin\AppData\Roaming\Typora\typora-user-images\image-20220518165357845.png)



**导入：**

这里的 `main.js` 是根据 `index.js` 的来构建的

直接使用==>

`<script	src="./dist/main.js"></script>`

就可以



### webpack

- 默认的打包文件入口在- src -> index.js

- 默认的输出文件路径在 dist -> main.js



在main和dev中可以修改

在webpack.config.js中修改默认约定

```json
{
  "name": "change-color",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts":{
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "jquery": "^3.6.0"
  },
  "devDependencies": {
    "webpack": "^5.72.1",
    "webpack-cli": "^4.9.2"
  }
}
```

![c683882b3ef9dfcaf2287eb13d7e7f3](C:\Users\lin\AppData\Local\Temp\WeChat Files\c683882b3ef9dfcaf2287eb13d7e7f3.jpg)





### 插件：`wenpack-dev-server`

##### 可以自动刷新webpack

安装插件：

`wenpack-dev-server`

类似于`nodemon`

```shell
npm install webpack-dev-server@3.11.2 -D
```



然后在package.json 修改

```json
"scripts":{
    "dev":"webpack server"
}
```



##### 并且

修改前面的

//在前面的config.js已经声明了bundle.js 输出

```html
<script src="/bundle.js"></script>
```



运行：`npm run dev`

在http://localhost:8080可以看项目打包情况



![image-20220519165640167](C:\Users\lin\AppData\Roaming\Typora\typora-user-images\image-20220519165640167.png)





## 下载插件 html-webpack-plugin

##### 安装 

用处：首页根目录可以直接是页面而不是目录文件选择



注意：是复制出来的首页，不是项目根目录的首页，也放在了内存中

自动生成引入bundle.js,不需要script标签引入

安装：

```shell
npm install html-webpack-plugin@5.3.2 -D
```



##### 配置：

```javascript
const HtmlPlugin = require('html-webpack-plugin');

const htmlPlugin = new HtmlPlugin({
    template: './src/index.html',//指定源文件存放路径
    filename: './index.html',//生成文件存放路径
})

module.exports = {
  mode: 'development',
  plugins: [htmlPlugin],//通过plungins节点，使htmlPlaugin生效
}

```





### loader处理非.js的模块



因为webpack只能处理以.js结尾的文件，要处理其他文件后缀需要用不同的loader5

在 `http://localhost:8080/` 

想在index.js导入

```javascript
import './css/index.css'
```



下载loader-css

```shell
npm i style-loader@3.0.0 css-loader@5.2.6 -D
```



在config.js的module->rules 数组中，添加loader规则：

```javascript
module:{
    rules:[
        {test:/\.css$/,use:['style-loader','css-loader']}
    ]
}
```





### 配置less

![fe59151833306e796fd121ce378c734](C:\Users\lin\AppData\Local\Temp\WeChat Files\fe59151833306e796fd121ce378c734.jpg)

![32b8e94531356b64eb1050ddc46dc3d](C:\Users\lin\AppData\Local\Temp\WeChat Files\32b8e94531356b64eb1050ddc46dc3d.jpg)



### 演示图片loader加载

![image-20220520145735702](C:\Users\lin\AppData\Roaming\Typora\typora-user-images\image-20220520145735702.png)

- 经过base64 译码过得图片浏览器可以直接输出

- 浏览器读取`<img src="./img/01.jpg"></img>`是先请求地址，把图片的信息转为base64然后再输出的
- 直接用src="译码"速度会更快，不过占用内存可能更多，比如图片大小可能10kb=>15kb



**打包图片：**

下载

```shell
npm i url-loader@4.1.1 file-loader@6.2.0
```

配置

```javascript
module:{
    rules:[
        { test: /\.jpg|png|gif$/,use:"url-loader?imit=22229"}
    ]
}
```





### 高级js语法

装饰器

```javascript
// 装饰器函数
function info(target) {
     target.info = 'Person info'
 }

// 定义一个普通类
// 装饰器，info给下面的类一个属性info
 @info
 class Person {}

 console.log(Person.info) //Person info
```



下载

```shell
npm i babel-loader@8.2.2 @babel/core@7.14.6 @babel/plugin-proposal-decorators@7.14.5 -D
```



可以转化自己的代码变成可以识别的代码



配置：

**创建一个babel.config.js**

```js
module.exports={
     plugins: [
        ['@babel/plugin-proposal-decoators', { legacy: true }]
    ],
}
```





在webpack.config.js

```js
     
{test: /\.js$/, use:'babel-loader',exclude:'/node_module'}
//exclude排除文件，转换第三包包速度太慢， 而且第三方包已经解决兼容性问题
```



### 配置webpack的项目打包

在package.json文件单位scripts节点如下：

```json
"scripts:{
"dev":"webpack serve",
"build":"webpack --mode production"
}
```

首页文件放在物理磁盘上，让后端可以访问到

### 自动删除dist目录下的旧文件

每次会删掉dist里面的文件再生成一个

安装

```shell
npm install --save-dev clean-webpack-plugin
npm i clean-webpack-plugin@3.0.0 -D
```



在webpack.config.js配置

```js
const { CleanWebpackPlugin } = require('clean-webpack-plugin');


.....
plugins:[new CleanWebpackPlugin()]
```



### Source Map

配置文件

在config.js中的modole.exports={}加

```js
 devtool:'eval-source map',
```

解决错误的提示行数问题

可能报错在36行，但是他那里报错34行，是浏览器自生成的

#### 在项目发布 npm run build 关闭source map注释掉

别人吧无法找源代码错误进行操作

# 最终：



**命令：**

```shell
npm init -y
npm install webpack webpack-cli --save-dev
npm install webpack-dev-server@3.11.2 -D
npm install html-webpack-plugin@5.3.2 -D
npm i url-loader@4.1.1 file-loader@6.2.0
```



**运行**

在开发中用dev

在发布用build

```shell
npm run dev
npm run build
```



```html
<script src="/bundle.js"></script>
<!-- 导入内存中可以自动更新 -->
```



**`webpack.config.js`**

```javascript
const path = require('path');
const HtmlPlugin = require('html-webpack-plugin');

const htmlPlugin = new HtmlPlugin({
    template: './src/index.html', //指定源文件存放路径
    filename: './index.html', //生成文件存放路径
})
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = {
    mode: 'development', //mode 用来指定构建模式，可以选值有 development 和 production
    entry: path.join(__dirname, './src/index.js'), //打包文件路径
    output: {
        path: path.join(__dirname, './dist'), //输出文件存放路径
        filename: 'js/bundle.js' //输出文件名称
    },
    devtool:'eval-source map',
    // 插件的数组，将来webpack在运行时，会加载这些插件
    plugins: [htmlPlugin, //通过plungins节点，使htmlPlaugin生效
        new CleanWebpackPlugin() //配置清理
    ],

    devServer: {
        open: true, //打包完成后自动打开浏览器
        port: 8080, //设置端口号
        host: 'localhost', //设置打开ip地址
    },

    module: {
        rules: [ //配置规则
            // 处理css
            { test: /\.css$/, use: ['style-loader', 'css-loader'] },
            // 处理图片   use可以是数组或者字符串，看传递的图片多少个，多选还是单选
            // ？后面的imit是限制，如果图片大小过大就不用 去转为base64，否则会增加负担
            // 在多个参数之间用&进行分割
            { test: /\.jpg|png|gif$/, use: "url-loader?imit=470&outputPath=img" },
            // 配置打包高级js代码
            { test: /\.js$/, use: 'babel-loader', exclude: '/node_module/' }, //exclude 排除项，不给第三方包打包0
        ]
    }
}

// // 装饰器函数
// function info(target) {
//     target.info = 'Person info'
// }

// // 定义一个普通类
// // 装饰器，info给下面的类一个属性info
// @info
// class Person {}

// console.log(Person.info) //Person info
```



**`package.json`**

```json
{
  "name": "change-color",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack server",
    "build":"webpack --mode production"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "jquery": "^3.6.0"
  },
  "devDependencies": {
    "html-webpack-plugin": "^5.3.2",
    "webpack": "^5.72.1",
    "webpack-cli": "^4.9.2",
    "webpack-dev-server": "^3.11.2"
  }
}
```



index.js

```javascript
// es6导入jquery包
import $ from 'jquery'

// 直接加载就行，不用接受 不用使用加载结果
import './css/index.css'

// 导入图片文件
import logo from './img/01.jpg'
$('#logo').attr('src', logo);

$(function() {
    // 实现各行变色
    $('li:odd').css('background', 'red');

    $('li:even').css('background', 'blue')
})
```

