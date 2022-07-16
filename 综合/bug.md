# Bug处理



### 1、

- ```shell
  Error: error:0308010C:digital envelope routines::unsupported
  ```

- 原因：node版本不兼容

- 解决：

  ```shell
  set NODE_OPTIONS=--openssl-legacy-provider
  ```

  

### 2

- 修改前端页面，刷新后并没有出现改变
- 解决：清除浏览器缓存