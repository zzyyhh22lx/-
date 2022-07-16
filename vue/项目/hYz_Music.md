# hYz_Music

- 下载准备：
- 基于vue_cli([Vue 3] less, babel, router, eslint)
- axios、vant3、vuex、

```shell
##下载axios
npm i axios -S
##下载vant3 ( 组件库 )
npm i vant -S
##下载vuex ( 仓库 )
npm install vuex@next -S
```

### 项目结构：

```powershell
├─public
└─src
    ├─Api(接口)
    │
    ├─assets
    │  ├─icons (图标)
    │  └─images (图片)
    │      ├─Error
    │      └─Found
    │
    ├─components (组件)
    │
    │
    ├─plugins (插件)
    │  ├─utils.js (工具函数)
    │
    ├─pages (页面)
    │
    │
    ├─router (路由)
    │  └─index.js (展示)
    │
    ├─utils(工具)
    │  └─require.js (封装axios)
    │
    ├─store (仓库)
    │  └─index.js (用户状态)
    │
   
```



- 跨域配置，在`vue.config.js`下

  ```js
  module.exports = {
    devServer: {
      proxy: { // 配置多个跨域
        '/api': {
          target: 'https://netease-cloud-music-api-ruddy-six-14.vercel.app',
          ws: true, // websocket支持
          changeOrigin: true,
          pathRewrite: {
            '^/api': '' // 重写请求
          }
        }
      }
    }
  }
  
  ```

  

- src目录建一个utils目录，用于存放一些工具：

  + axios封装请求根路径

    ```js
    import axios from 'axios'
    
    const request = axios.create({
      // 指定请求根路径
      baseURL: '/api',
      timeout: 30000,
      withCredentials: true// `withCredentials` 表示跨域请求时是否需要使用凭证
    })
    
    export default request
    
    ```
  
  
  
- src目录建一个api目录，API接口封装到这个模块中，可以复用：

  + 写api函数( 使用重复的可以直接调用 )

    ```js
    //向外导出
    //initAPI.js
    export const getInitAPI = function(){
        xxx
    }
    
    //使用：
    //import { getInitAPI } from '@/api/initAPI.js'
    //getInitAPI()
    ```



- src目录下建一个store目录，仓库

  ```js
  //index.js
  import { createStore } from 'vuex'
  
  const state = {
    isLogin: false
  }
  
  const store = createStore({
    state,
    mutations: {
      // 设置登录状态
      updataLoginState (state, flag = false) {
        state.isLogin = flag
      }
    }
  })
  
  // 挂载vue实例
  export default store
  
  ```

  





