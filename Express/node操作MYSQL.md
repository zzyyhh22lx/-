# node操作MYSQL



**安装**

`npm install --save mysql`



```javascript
var mysql = require('mysql');

// 创建连接
var connection = mysql.createConnection({
    host: '47.112.128.141',
    user: 'lhy',
    password: '123',
    database: 'lhy'
});

// 连接数据库
connection.connect();

// 操作
connection.query('SELECT * FROM booksystem', (err, results, fields) => {
    if (err) { throw err; }
    console.log(results);
})

// 关闭连接
connection.end();
```

