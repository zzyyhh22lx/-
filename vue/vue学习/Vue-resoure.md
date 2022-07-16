# `Vue-resoure`



##### 前端用的vue

```html
 <div id='app'>
        <input type="text" v-model="username">
        <input type="text" v-model="password">
        <input type="button" @click.preventDefault="get" value="get">
        <input type="button" @click.preventDefault="post" value="post">
        <input type="button" @click.preventDefault="jsonp" value="jsonp">
        <template v-if="show">
            <h5> 总共: $$ all $$</h5>
            <p> 输入对象:<b>$$ message $$</b> </p>
        </template>
    </div>

    <script>
        var app = new Vue({
            el: '#app',
            data: {
                username: 'lhy',
                password: '123',
                show: false,
                all: '',
                message: ''
            },
            delimiters: ['$$', '$$'],
            methods: {
                get: function() {
                    //发送get请求
                    //$http.get 第二个参数是请求内容
                    this.$http.get('/user', {
                        params: {
                            username: this.username,
                            password: this.password
                        },
                    }).then(function(res) {
                        this.show = true;
                        this.all = res;
                        this.message = res.bodyText;
                    }, function() {
                        this.show = true;
                        this.message = 'get请求失败啦'
                    });

                },
                post() {
                    this.$http.post('/user', {
                        username: this.username,
                        password: this.password
                    }, {
                        emulateJSON: true
                    }).then(function(res) {
                        this.all = res;
                        this.message = res.bodyText;
                    }, function() {
                        this.show = true;
                        this.message = 'post请求出错啦'
                    });
                },
                jsonp() {
                    this.$http.jsonp('/id', {
                        params: {
                            username: this.username,
                            password: this.password
                        },
                        jsonp: 'lhy'
                    }).then(function(res) {
                        this.show = true;
                        this.all = res;
                        this.message = res.body;
                    }).catch(function(err) {
                        this.show = true;
                        this.message = 'post请求出错啦'
                    })
                }
            }
        })
    </script>

```



##### 后端用的express框架：

```js
router.get('/', function(req, res) {
    res.render('index.html', { title: 'Vue基础学习' });
})
router.get('/user', function(req, res) {
    res.send(req.query);
})
router.post('/user', function(req, res) {
    console.log(req.body);
    res.send(req.body);
})
router.get('/id', function(req, res) {
    res.send(req.query.lhy + "(123)");
})
```



##### jsonp:

jsonp跨域请求：

- 请求不同的端口，不同的域名会出现跨域问题：

- 用jsonp请求，后台会返回一个函数调用

  + 前端设置jsonp的命名  `jsonp:'lhy'` : 则会像后台发送==>
  
  + `http://127.0.0.1:3000/id?username=lhy&password=123&lhy=_jsonpeyi5fwlt1ta `
  
  + `_jsonpeyi5fwlt1ta`默认设置的，也可以用 ` jsonpcallback`设置函数名字
  
    ```js
    router.get('/id', function(req, res) {
        res.send(req.query.lhy + "(123)");//括号里面传参数，也就是传值
    })
    ```
  
  + 前端vue收到后台返回的消息，自动调用该函数
  
  + ```js
     jsonp() {
                        this.$http.jsonp('/id', {
                            params: {
                                username: this.username,
                                password: this.password
                            },
                            jsonp: 'lhy'
                        }).then(function(res) {
                            this.show = true;
                            this.all = res;
                            this.message = res.body;
                        }).catch(function(err) {
                            this.show = true;
                            this.message = 'post请求出错啦'
                        })
                    }
    ```
  



### post请求需要配置：

```html
<script>
        // 配置http全局属性 请求的数据接口通过配置根域名，请求的url需要用相对路径
        Vue.http.options.root = 'http://127.0.0.1:3000/';
        // 全局启用emulateJSON属性，可以不用body的post后面设置
        Vue.http.options.emulateJSON = true;
    
    
    
    
    //也可以：
    this.$http.post('/user', {
                        username: this.username,
                        password: this.password
                    }, {
                        emulateJSON: true
                    })
</script>
```



