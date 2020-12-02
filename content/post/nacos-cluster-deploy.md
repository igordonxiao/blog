---
title: "Nacos集群部署"
date: 2020-12-02
categories: ["微服务"]
tags: [
    "Spring"
]
---


## 什么是Nacos

Nacos 致力于帮助您发现、配置和管理微服务。Nacos 提供了一组简单易用的特性集，帮助您快速实现动态服务发现、服务配置、服务元数据及流量管理。

Nacos 帮助您更敏捷和容易地构建、交付和管理微服务平台。 Nacos 是构建以“服务”为中心的现代应用架构 (例如微服务范式、云原生范式) 的服务基础设施。

## 集群部署架构

> 官方推荐的架构图如下

![官方部署架构](nacos-cluster.jpeg)    

官方推荐`域名` + `VIP模式`部署，可读性好，而且换ip方便。一般我们使用`Nginx`替代`VIP`来负载多台Nacos服务即可。


> 注意: 3个或3个以上Nacos节点才能构成集群

## 部署实践

### 环境准备

| 服务    | IP            |
| ------- | ------------- |
| Nacos Server 1 | 192.168.35.32 |
| Nacos Server 2 | 192.168.35.34 |
| Nacos Server 3 | 192.168.35.42 |
| Nginx   | 192.168.35.31 |

> 说明: 这几台服务均是CentOS 7操作系统

### 安装Nacos Server

> 在三台机器上分别安装Nacos Server


+ 下载`Nacos Server`官方安装包    

下载地址: [https://github.com/alibaba/nacos/tags](https://github.com/alibaba/nacos/tags)

> 注意：本文档写作时Nacos Server的版本为`1.4.0`

+ 解压
```bash
unzip nacos-server-1.4.0.zip
```

+ 修改配置
```bash
cd nacos/conf
# 复制一份cluster.conf文件出来
cp cluster.conf.example cluster.conf
vi cluster.conf
```
将三台Nacos Server的机器IP和端口号写入`cluster.conf`文件

```
192.168.35.32:8848
192.168.35.34:8848
192.168.35.42:8848
```
> 三台Nacos Server的`cluster.conf`文件都这样配置

+ 启动Nacos Server    

执行命令：
```bash
cd nacos/bin
sh startup.sh
```

终端输出如下日志：

```bash
......
nacos is starting with cluster
nacos is starting，you can check the /root/app/nacos/logs/start.out
```

查看日志中提到的`start.out`文件：

```bash
'   : |     ;  :   .'   \   :    : `----'  '--'.     /
;   |.'     |  ,     .-./\   \  /            `--'---'
'---'        `--`---'     `----'

2020-12-02 16:10:50,015 INFO The server IP list of Nacos is [192.168.35.32:8848, 192.168.35.34:8848, 192.168.35.42:8848]

2020-12-02 16:10:51,016 INFO Nacos is starting...

2020-12-02 16:10:52,016 INFO Nacos is starting...

2020-12-02 16:10:57,799 INFO Nacos Log files: /root/app/nacos/logs

2020-12-02 16:10:57,799 INFO Nacos Log files: /root/app/nacos/conf

2020-12-02 16:10:57,799 INFO Nacos Log files: /root/app/nacos/data

2020-12-02 16:10:57,800 INFO Nacos started successfully in cluster mode. use external storage

```

可以看到，日志中打印了配置的`[192.168.35.32:8848, 192.168.35.34:8848, 192.168.35.42:8848]`三台机器的IP和端口信息，并且服务以集群模式启动成功，同理，启动另外两台机器上的Nacos Server。

+ 验证

分别使用三台Nacos Server的控制台(`http://ip:port/nacos`)进行登录访问，点击左侧菜单的`集群管理 -> 节点管理`, 可看到三个服务实例都为`UP`状态就说明整个集群模式部署成功了。

![Nacos Nodes](nacos-nodes.png) 

+ Nginx配置

编辑`192.168.35.31`的Nginx配置文件

在`http`节点内添加如下配置：   

```bash
# nacos server 负载
upstream nacos {
    # 配置三台Nacos Server的IP和端口
	server	192.168.35.32:8848; 
	server	192.168.35.34:8848;
	server	192.168.35.42:8848;
}

server {
	listen 8848; # 可以换成其他端口
	server_name 192.168.35.31; # 如果有域名可换成域名
	
	location / {
	  proxy_pass http://nacos;
	}
}
```

重启Nginx: 
```bash
./nginx -s reload
```

+ 通过Nginx访问Nacos Server控制台

访问: [http://192.168.35.31:8848/nacos](http://192.168.35.31:8848/nacos)    


能正常访问，说明Nginx的配置正常。

+ 微服务连接Nacos Server

配置文件中，直接配置Nginx负载的地址即可。

```yaml
spring:
  cloud:
    nacos:
      discovery:
        server-addr: 192.168.35.31:8848 # 配置Nginx代理的地址, 不要配置单个节点的地址
```

## 附：

更多资料请参考Nacos官方文档：[https://nacos.io/zh-cn/docs/cluster-mode-quick-start.html](https://nacos.io/zh-cn/docs/cluster-mode-quick-start.html)


