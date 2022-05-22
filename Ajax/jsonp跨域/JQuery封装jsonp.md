# JQuery封装jsonp

#### JSONP的本质：动态创建script，通过src跨域发送请求。并返回函数调用

#### 注意：

jsonp只支持get。浏览器中显示的post 十分误导我们；
这里可以联系到 jsonp最终是将数据以类似src标签的方式加载，而这种加载方式的确是只有get方式；
jsonp请求，一定不要误以为是post，需要找到参数拼接称get请求即可；
get方式请求后转换为json格式即可正常使用；

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script type="text/javascript" src="./jquery.js"></script>
    <script type="text/javascript">
        $(function(){
            $("#btn").click(function(){
                $.ajax({
                    type:'get',
                    url:'http://tom.com/jsonp.php',
                    dataType:'jsonp',
                    jsonp:'cb',//jsonp属性的作用就是自定义参数名字（callback=abc 这里的名字指的是等号前面的键，后端根据这个键获取方法名，jquery的默认参数名称是callback）
                    jsonpCallback:'abc',//这个属性的作用就是自定义回调函数的名字（callback=abc ，这里的名字指的是等号后面的值）
                    data:{},
                      //后台返回的是一个函数调用，被jquery的success回调函数调用
                    success:function(data){
                        console.log(data.username,data.password);
                    },
                    error:function(data){
                        console.dir(data);
                        console.log('error');
                    }
                });
            });
        });
    </script>
</head>
<body>
    <input type="button" value="点击" id="btn">
</body>
</html>
```





#### 自己实现封装jquery的Ajax请求

```javascript
function ajax(obj){
    // 默认参数
    var defaults = {
        type : 'get',
        data : {},
        url : '#',
        dataType : 'text',
        async : true,
        success : function(data){console.log(data)}
    }
    // 处理形参，传递参数的时候就覆盖默认参数，不传递就使用默认参数
    for(var key in obj){
        defaults[key] = obj[key];
    }
    // 1、创建XMLHttpRequest对象
    var xhr = null;
    if(window.XMLHttpRequest){
        xhr = new XMLHttpRequest();
    }else{
        xhr = new ActiveXObject('Microsoft.XMLHTTP');
    }
    // 把对象形式的参数转化为字符串形式的参数
    /*
    {username:'zhangsan','password':123}
    转换为
    username=zhangsan&password=123
    */
    var param = '';
    for(var attr in defaults.data){
        param += attr + '=' + defaults.data[attr] + '&';
    }
    if(param){
        param = param.substring(0,param.length - 1);
    }
    // 处理get请求参数并且处理中文乱码问题
    if(defaults.type == 'get'){
        defaults.url += '?' + encodeURI(param);
    }
    // 2、准备发送（设置发送的参数）
    xhr.open(defaults.type,defaults.url,defaults.async);
    // 处理post请求参数并且设置请求头信息（必须设置）
    var data = null;
    if(defaults.type == 'post'){
        data = param;
        xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
    }
    // 3、执行发送动作
    xhr.send(data);
    // 处理同步请求，不会调用回调函数
    if(!defaults.async){
        if(defaults.dataType == 'json'){
            return JSON.parse(xhr.responseText);
        }else{
            return xhr.responseText;
        }
    }
    // 4、指定回调函数（处理服务器响应数据）
    xhr.onreadystatechange = function(){
        if(xhr.readyState == 4){
            if(xhr.status == 200){
                var data = xhr.responseText;
                if(defaults.dataType == 'json'){
                    // data = eval("("+ data +")");
                    data = JSON.parse(data);
                }
                defaults.success(data);
            }
        }
    }

}
```



```javascript
$.ajax({

url:"http://manager.jt.com/web/testJSONP",

type:"get",//jsonp只能支持get请求 src只能进行get请求.

dataType:"jsonp", //dataType表示返回值类型 必须标识

//jsonp: "callback", //指定参数名称

jsonpCallback: "hello", //指定回调函数名称

success:function (data){ //data经过jQuery封装返回就是json串

alert(data.itemId);

alert(data.itemDesc);

}

});
```







#### 自己封装jquery的jsonp

```javascript

function ajax(obj){
    // jsonp仅仅支持get请求
    var defaults = {
        url : '#',
        dataType : 'jsonp',
        jsonp : 'callback',
        data : {},
        success:function(data){console.log(data);}
    }

    for(var key in obj){
        defaults[key] = obj[key];
    }
    // 这里是默认的回调函数名称
    // expando: "jQuery" + ( version + Math.random() ).replace( /\D/g, "" ),
    var cbName = 'jQuery' + ('1.11.1' + Math.random()).replace(/\D/g,"") + '_' + (new Date().getTime());
    if(defaults.jsonpCallback){
        cbName = defaults.jsonpCallback;
    }

    // 这里就是回调函数，调用方式：服务器响应内容来调用
    // 向window对象中添加了一个方法，方法名称是变量cbName的值
    window[cbName] = function(data){
        defaults.success(data);//这里success的data是实参
    }

    var param = '';
    for(var attr in defaults.data){
        param += attr + '=' + defaults.data[attr] + '&';
    }
    if(param){
        param = param.substring(0,param.length-1);
        param = '&' + param;
    }
    var script = document.createElement('script');
    script.src = defaults.url + '?' + defaults.jsonp + '=' + cbName + param;
    var head = document.getElementsByTagName('head')[0];
    head.appendChild(script);

    // abc({"username":"zhangsan","password":"123"})
}
```





```javascript

function ajax(obj){
    var defaults = {
        type : 'get',
        async : true,
        url : '#',
        dataType : 'text',
        jsonp : 'callback',
        data : {},
        success:function(data){console.log(data);}
    }

    for(var key in obj){
        defaults[key] = obj[key];
    }

    if(defaults.dataType == 'jsonp'){
        ajax4Jsonp(defaults);
    }else{
        ajax4Json(defaults);
    }
}

function ajax4Json(defaults){
    // 1、创建XMLHttpRequest对象
    var xhr = null;
    if(window.XMLHttpRequest){
        xhr = new XMLHttpRequest();
    }else{
        xhr = new ActiveXObject('Microsoft.XMLHTTP');
    }
    // 把对象形式的参数转化为字符串形式的参数
    /*
    {username:'zhangsan','password':123}
    转换为
    username=zhangsan&password=123
    */
    var param = '';
    for(var attr in defaults.data){
        param += attr + '=' + defaults.data[attr] + '&';
    }
    if(param){
        param = param.substring(0,param.length - 1);
    }
    // 处理get请求参数并且处理中文乱码问题
    if(defaults.type == 'get'){
        defaults.url += '?' + encodeURI(param);
    }
    // 2、准备发送（设置发送的参数）
    xhr.open(defaults.type,defaults.url,defaults.async);
    // 处理post请求参数并且设置请求头信息（必须设置）
    var data = null;
    if(defaults.type == 'post'){
        data = param;
        xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
    }
    // 3、执行发送动作
    xhr.send(data);
    // 处理同步请求，不会调用回调函数
    if(!defaults.async){
        if(defaults.dataType == 'json'){
            return JSON.parse(xhr.responseText);
        }else{
            return xhr.responseText;
        }
    }
    // 4、指定回调函数（处理服务器响应数据）
    xhr.onreadystatechange = function(){
        if(xhr.readyState == 4){
            if(xhr.status == 200){
                var data = xhr.responseText;
                if(defaults.dataType == 'json'){
                    // data = eval("("+ data +")");
                    data = JSON.parse(data);
                }
                defaults.success(data);
            }
        }
    }
}
function ajax4Jsonp(defaults){
    // 这里是默认的回调函数名称
    // expando: "jQuery" + ( version + Math.random() ).replace( /\D/g, "" ),
    var cbName = 'jQuery' + ('1.11.1' + Math.random()).replace(/\D/g,"") + '_' + (new Date().getTime());
    if(defaults.jsonpCallback){
        cbName = defaults.jsonpCallback;
    }

    // 这里就是回调函数，调用方式：服务器响应内容来调用
    // 向window对象中添加了一个方法，方法名称是变量cbName的值
    window[cbName] = function(data){
        defaults.success(data);//这里success的data是实参
    }

    var param = '';
    for(var attr in defaults.data){
        param += attr + '=' + defaults.data[attr] + '&';
    }
    if(param){
        param = param.substring(0,param.length-1);
        param = '&' + param;
    }
    var script = document.createElement('script');
    script.src = defaults.url + '?' + defaults.jsonp + '=' + cbName + param;
    var head = document.getElementsByTagName('head')[0];
    head.appendChild(script);
}
```

