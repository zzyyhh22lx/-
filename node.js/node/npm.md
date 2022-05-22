# npm

**npm简单命令**

- #### npm网站

| npmjs.com

- #### npm命令行工具

npm的第二层含义是一个命令行工具，只要你安装了node就已经安装了npm

npm也有版本概念

可以在命令行输入

```javascript
npm --version
```

升级npm( 自己升级自己 )

```javascript
npm instal --global npm
```

- npm init

+ + npm init -y 可以跳过向导，快速生成

  

+ npm install

​		下载全部



+ npm install 包名

+ + 只下载包名
  + 简写 npm i 包名

  

+ npm install --save 包名

  + 下载并且保存依赖项 （ package.json 文件中的dependencies 选项 ）

  + npm i -S   ( S是大写 )

  

- npm uninstall

- + 只删除 如果有依赖项会保存
  + npm un 包名

  

- npm uninstall -save 包名

+ + 删除的同时把依赖项也删除
  + npm un -S



- npm [] --help

​         查看指定命令的使用帮助（ npm install --help ）  



#### 解决 npm 被墙问题

npm存储包文件的服务器在国外，有时候会被墙，速度慢

![3f41d3e2a733959b3442d747e26b014](D:\users\admin\Desktop\学习笔记\node.js\img\14.png)![7ae8f9b922905bbb4276d028a76bc10](D:\users\admin\Desktop\学习笔记\node.js\img\14.png)