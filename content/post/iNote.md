+++
title = "iNote单页博客编译安装方式"
description = ""
categories = [
    "Web"
]
tags = [
    "Golang",
]
date = "2015-08-17 15:42:13"
+++


# iNote是一款开源，免费，简洁的单页博客
> iNote是在[beego(golang语言)](http://beego.me/ "beego(golang语言)")&[bootstrap](http://getbootstrap.com/ "bootstrap")等开源项目基础之上开发的

## 功能简介
- 前后端完全分离
- 响应式布局
- 内嵌[markdown](https://pandao.github.io/editor.md/ "markdown")编辑器
- URL支持hash(#)+id 文章导航
- 支持更换首页banner大图背景
- 文章功能
- 文章内容照片墙预览
- 文章标签功能
- 文章留言功能
- 留言回复功能
- Web后台管理


## Linux环境编译安装（OSX,Win环境类似）
step 1. 安装Go    
   参考[install golang](http://golang.org/doc/install#tarball "install golang")

step 2. 安装mysql    
   参考[install mysql](http://dev.mysql.com/doc/refman/5.6/en/installing.html "install mysql")

step 3. 安装beego, bee工具(可选), mysql驱动     

```shell
go get github.com/astaxie/beego
go get github.com/beego/bee
go get github.com/go-sql-driver/mysql
```
step 4. 安装iNote    

```shell
go get github.com/igordonxiao/inote
```

step 5. 新建数据库inote并导入初始化脚本
        `($GOPATH/src/github.com/igordonxiao/inote/dbinit/inote.sql)`    
        
step 6. 按照实际情况修改iNote配置文件中的程序运行模式、监听端口及数据库参数
    
```
        ###################### 程序基本配置 ############################
        
        # 程序运行实例名称
        appname = inote
        
        # 程序运行模式  dev:开发模式  prod:产品模式
        runmode = dev
        
        # 程序运行监听端口
        httpport = 8080
        
        # MYSQL地址
        dbhost = localhost
        
        # MYSQL端口
        dbport = 3306
        
        # MYSQL用户名
        dbuser = root
        
        # MYSQL密码
        dbpassword = root
        
        # MYSQL数据库名称
        dbname = inote
```

step 7. 编译iNote    

```shell
cd $GOPATH/src/github.com/igordonxiao/inote
go build
```

step 8. 运行iNote(nohup模式)
```shell
nohup ./inote &
```

step 9. 访问iNote     
首页：`localhost:8080`    
后台登录：`localhost:8080/login`    
默认密码：`admin`


![Image of iNote](https://raw.githubusercontent.com/igordonxiao/inote/master/screenshot/21A9C0EB-30AB-4512-96C3-4FCC754F9E80.png)


