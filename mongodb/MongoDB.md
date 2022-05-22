# MongoDB

**默认在c盘 /data/db中存储数据，否则打开不了**：

- 修改：

`mongod --dbpath=数据存储目录路径`

- 打开：(默认连接本机MongoDB数据库)

`mongod`

- 停止

`ctrl+c`

`exit`	退出连接

- 查看数据库列表

`show dbs`

- 查看当前的数据库

`db`

- 切换到指定数据库

`use 数据库名称`

​	（如果没有就会新建一个）

