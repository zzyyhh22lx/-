# linux操作

> ls 	( 列出目录 )
>
> -a 参数 （可以看出隐藏文件）
>
> -l 参数 （包括属性，没有隐藏文件）

可以同时使用 ls -a 





> cd

../ 上一级路径      /下一级路径

cd /home   到下一级路径home文件夹

cd .. 返回上一级目录

cd ~ 回到当前用户目录 root





> pwd 显示当前所在目录





> mkdir  ( 创建目录 )

mkdir lhy      

**注意：想要递归创建目录：**

- mkdir lhy/xzy/love     ----->错误
- mkdir -p lhy/xzy/love --------->成功

必须加个 -p



> rmdir  ( 删除目录 )

无法删除不为空的目录，如果有目录，需要先删除一些文件，用递归-p 删除其中的目录，才可以







> cp  资源  目录名        拷贝资源到 指定目录