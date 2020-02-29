# Linux下部署VUE

```
1、找一个地方新建一个文件夹，例如 叫 server-web
2、初始化环境,进入目录 /server-web,打开命令窗口，输入以下命令
	npm init -y
	npm i express -S   // 安装 express
	npm i compression -S  // 安装 compression  
	npm i pm2 -g   // 安装 pm2 管理项目
3、在server-web 目录下，新建一个启动文件，例如叫 app.js
	在文件里面，输入以下代码
{
    const express = require('express')
    const app = express()

    const compress = require('compression')
    app.use(compress())

	// 这里的 dist 就是vue项目打包后的目录
    app.use(express.static('./dist'))   
	
	// 80端口这里是网站的访问端口，到时候可根据自身需求修改 
    app.listen(80, () => {
        console.log("server running success !!")
    })

}
保存

4、上传代码到服务器
首先打包项目，在本地打开项目，在控制台输入
	npm run build
	打包成功之后，会在项目的目录下面生成一个 /dist 的文件夹
	将 dist 打包发到服务器上的  server-web 目录下，解压

5、利用pm2启动项目
	pm2 start app.js --name admin-web
	
	pm2命令说明
	启动项目：pm2 start 脚本 --name 自定义名称/或者ID(这里的ID，项目启动之后，使用pm2 ls 可以查看)
	查看项目：pm2 ls
	重启项目：pm2 restart 自定义名称/或者ID
	删除项目：pm2 delete  自定义名称/或者ID

```

