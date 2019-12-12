# linux centos7构建node服务器+MYSQL+部署VUE

## 0.将window连上linux服务器连上

前提是linux服务器已经启动服务

window安装git 然后ssh 用户名@ip 

eg:ssh dc2-user@116.85.46.202

## 1.安装node

### 1.1在自己的电脑上先下载node.js的安装包

网址：[http://nodejs.cn/download/![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image002.jpg)](http://nodejs.cn/download/)

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image004.jpg)

### 1.2将node安装包传输到linux

第一步下载FileZilla

第二步通过FileZilla将window连接到linux centos服务器

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image006.jpg)

第三步将window的node安装包上传到linux centos上 的用户下面

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image008.jpg)

### 1.3通过本地git登录服务器安装node安装包

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image010.jpg)

切换到存放node安装包的目录下

使用tar -xvf node-v12.13.1-linux-x64.tar.xz（当前包版本） 进行解压

## 2.配置node的环境变量

### 2.1先获取node安装包解压后它的bin路径

/home/dc2-user/node-v12.13.1-linux-x64（你的安装包）/bin

### 2.2创建全局变量文件node.sh

cd /etc/profile.d/

sudo touch node.sh

sudo vim node.sh

先按esc键输入如下

export PATH=$PATH: /home/dc2-user/node-v12.13.1-linux-x64（你的安装包）/bin

完毕后按下:键，并输入wq回车

重启全局变量

source /etc/profile

验证

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image012.jpg)

安装cnpm （很多时候用npm安装其他的插件会失败，用cnpm，当然也有相反情况）

npm install -g cnpm --registry=[https://registry.npm.taobao.org](https://registry.npm.taobao.org/)

## 3.将本地后端的文件通过上传到linux

### 第零步：初始化

cnpm init

### 第一步：创建server文件并在当前文件夹下安装需要用到的框架Express和body-parser,mysql

在终端输入

cnpm install express

cnpm install body-parser

cnpm install mysql(方便JS连接MYSQL数据库)

### 第二步创建相关js文件

### index.js(服务器的主文件)

引入Express框架，bodyParser（解析前端传过来的json数据），各类Api（要执行的SQL）

```
const  getHtml = require('./Api/getHtml');
const  bodyParser = require('body-parser');
const  express =  require('express');
const  app = express();

app.use(bodyParser.json());
app.use(bodyParser.urlencoded({extended:false}));
app.use('/api/getHtml',getHtml);


app.listen(3000);
console.log('服务端口启动 localhost:3000');
```

 

### db.js(验证数据库)

输入数据库的地址，用户名，密码，端口，要链接的数据库

```
module.exports={
  mysql:{
    host: 'localhost',
    user: 'root',
    password: '123456',
    database: 'web',//你的数据库名称
    port: '3306'
  }
}
```

 

### Api文件夹(存放相关api.js操作文件) 

getHtml.js

```
const  mySql_yanzheng = require('../db') ;
const  express        = require('express');
const  router         = express.Router();
const  mysql          = require('mysql');

let connt = mysql.createConnection(mySql_yanzheng.mysql);
connt.connect();//开始连接

router.post('/query',(req,res)=>{
  let parmas = req.body;//获取前端参过来的参数

  let sql = "SELECT * FROM web.html  where  bigTitle = 'HTML 基础教程' and smallTitle = 'HTML 教程' ";

  console.log(parmas);//前端参过来的参数

  connt.query(sql,function (err,result) {
    console.log(sql);//输出进行查询的sql语句
    if(err){
      console.log(err);
    }
    if(result){
      if(typeof result == 'underfind'){
        res.json({code:1,message:'操作失败'});
      }
      else{
        res.json(result);//将数据转换成json形式返回给前端
        console.log(result);
      }
    }
  })
});

router.post('/add',(req,res)=>{

});

router.post('/updata',(req,res)=>{

});

router.post('/delete',(req,res)=>{

});

module.exports=router;
```

 

 

## 4. 安装相关node包

cnpm i --save express

cnpm i --save ejs body-parser

cnpm i (疯狂)

 

## 5.[linux centos7安装mysql](https://www.cnblogs.com/liuxixia/p/10930615.html)

1、下载并安装官方的 yum repository （新建了mysql文件夹）

wget -i -c http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
 

2、安装下载过来的文件（yum repository）

yum -y install mysql57-community-release-el7-10.noarch.rpm
 

3、进入正题：yum安装mysql

yum -y install mysql-community-server

 

4， 启动mysql

service mysqld start

 

5 查看是否启动成功

service mysqld status

出现以下标准则代表启动成功

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image014.jpg)

6 查看初始密码

 grep "password" /var/log/mysqld.log

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image016.jpg)

7 进入数据库

mysql -uroot -p     这里到-p就行了，回车会提示你输入密码的

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image018.jpg)

8设置新密码为root

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image020.jpg)

这里提示我的密码不能满足要求，这里我们可以修改下密码的限制

命令set global validate_password_policy=0;

设置密码不限制字符类型

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image022.jpg)

命令set global validate_password_length=1;

设置密码不限制位数

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image024.jpg)

 

ok,现在就可以重新设置新密码了，我这里设置的是root

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image026.jpg)

设置root账户密码不过期

命令ALTER USER 'root'@'localhost' PASSWORD EXPIRE NEVER;

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image028.jpg)

9刷新权限

命令flush privileges;

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image030.jpg)

设置用户 root 可以在任意 IP 下被访问：

命令grant all privileges on *.* to root@"%" identified by "root";

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image032.jpg)

设置用户 root 可以在本地被访问：

命令*grant all privileges on \*.* to root@"localhost" identified by "new password";*

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image034.jpg)

刷新权限生效

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image036.jpg)

退出

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image038.jpg)

资料来自：

https://www.cnblogs.com/liuxixia/p/10930615.html

https://www.cnblogs.com/hjw-zq/p/9791596.html

 

## 6.本地远程连接滴滴云服务器下的mysql

6.1滴滴云下建立可以访问的进入访问的安全组端口3306

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image040.jpg)

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image042.jpg)

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image044.jpg)

6.2本地电脑MYSQL通过SSH连接服务器上的mysql

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image046.jpg)

## 7.滴滴云也要允许服务器监听端口进来

例如你服务器设置的监听端口为3000

滴滴云下建立可以访问的进入访问的安全组端口3000

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image048.jpg)

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image049.jpg)

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image044.jpg)

 

## 8.导入数据库

举例

导出数据库：在cmd窗口中运行

D：

cd mysql-5.7.17-winx64

D:\mysql-5.7.17-winx64> cd bin

mysqldump -u root -p student > d:\student(16240520).txt

导入：

在MySQL Workbench中如果没有数据库student，则先创建数据库，

再运行student.txt中的sql命令

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image051.jpg)

## 9部署VUE

npm run build

打包成dist后，通过filezilla将它放到你的后端程序里面。

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image052.png)

cnpm path –save –g

编辑 index.js 修改语句如图所示

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image054.jpg)

## 9启动服务器

在你服务器后台的目录下启用

例如:

node index

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image056.jpg)

结果：终于成功了！！！！

![img](file:///C:/Users/吴彦祖/AppData/Local/Temp/msohtmlclip1/01/clip_image058.jpg)

## 时间：

 

2019/11/27