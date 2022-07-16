# wechat_project



#### 仿pc端微信页面：

使用vue，用单页面写出一个如下：

![image-20220628232749611](C:\Users\lin\AppData\Roaming\Typora\typora-user-images\image-20220628232749611.png)

#### 页面所需组件：

+ 

+ | 组件名字              | 作用                                           |
  | --------------------- | ---------------------------------------------- |
  | `<wechat-container/>` | 容器背景，内置所有组件                         |
  | `<left-sidebar/>`     | 侧边栏，可以放置功能选项                       |
  | `<search/>`           | 搜索或者添加好友                               |
  | `<friends/>`          | 好友栏                                         |
  | `<fold/>`             | 当置顶好友大于一定数量会出现，可以折叠置顶好友 |
  | `<top-func/>`         | 与当前好友聊天，关闭缩小放大容器               |
  | `<chat-html>`         | 聊天记录背景                                   |
  | `<input/>`            | 用户输入                                       |

+ 放置：

  + `<left-sidebar/>`：
  + `<friends/>`：1.`<search/>`2.`<fold/>`
  + `<chat-html>`：1.`<top-func/>`2.`<input/>`

#### 开始

*shell*

```shell
vue create mywechat

cd mywechat

npm run serve
```



#### 仿手机端wechat：



![image-20220630115941451](C:\Users\lin\AppData\Roaming\Typora\typora-user-images\image-20220630115941451.png)

- 所需组件：
  + topBar
  + searchBar
  + friendsBar
  + boBar