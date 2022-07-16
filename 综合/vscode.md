# vscode

- ##### 自定义快捷键：

  + 打开文件==>首选项==>用户片段:

  + ```json
    {
    	"PHP":{
    		"prefix":"php",
    		"body":["<?php \n $0 \n?>",
    			],
    			"description":"php"
    	}
    }
    ```

  + `"prefix"`：代码快捷键，`"body"`：代码，`"description"`：描述



- ##### 解决vscode用alt+b打开是静态资源的问题

  + 静态资源=>无法加载与http网络有关的资源，只能是本地资源，无法使用ajax发送请求

  + ##### 去vscode插件库 下载 `Live Server`，

  + 以后直接在文件 目录下点击鼠标右键的 `open with Live Server` ，vscode可以开个没被占用的端口开启网络服务