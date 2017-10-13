+++
title = "阿里云ubuntu 16.04.2 LTS安装Rancher稳定版本步骤"
description = ""
categories = [
    "MicroService"
]
tags = [
    "Docker",
]
date = "2017-06-20 17:36:02"
+++

## step 1
ssh到Ubuntu服务器安装Docker 17.03.1
参考文章[http://www.cnrancher.com/install-docker/](http://www.cnrancher.com/install-docker/)    
执行以下命令:    
```shell
curl -sSL https://github.com/gitlawr/install-docker/blob/1.0/17.03.1.sh?raw=true | sh
```

## step 2
为Docker添加加速器
你可以去[DaoCloud](http://www.daocloud.io/) 申请
执行以下命令:
```shell
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s <你申请的地址>
```
重启docker:    
```
sudo systemctl restart docker
```

## step 3
安装Mysql
执行以下命令:
```
sudo apt-get install mysql-server
```
设置好root用户密码

## step 4
为Mysql打开远程访问   
执行以下命令:   
```
vim /etc/mysql/mysql.conf.d/mysqld.cnf
```    
注释掉
```
bind-address = 127.0.0.1
```
这一行

保存文件,重启mysql服务, 
```
/etc/init.d/mysql restart
```

## step 5
创建rancher服务的mysql数据库及用户   

进入mysql:
```
mysql -u root -p
```

执行以下命令:   
 ```
CREATE DATABASE IF NOT EXISTS rancherdb COLLATE = 'utf8_general_ci' CHARACTER SET = 'utf8';   
GRANT ALL ON rancherdb.* TO 'root'@'%' IDENTIFIED BY 'rancherpass';
GRANT ALL ON rancherdb.* TO 'root'@'localhost' IDENTIFIED BY 'rancherpass';
```     
退出MySQL客户端

## step 6
获取dokcer0网卡的地址:    
执行以下命令:    
```
ifconfig
```

## step 7
安装rancher稳定版    
执行以下命令:    
```
sudo docker run -d --restart=unless-stopped -p 8080:8080 rancher/server:stable --db-host <docker0的网卡地址> --db-port 3306 --db-user root --db-pass rancherpass --db-name rancherdb
```