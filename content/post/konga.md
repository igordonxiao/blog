---
title: "Konga API配置"
date: "2018-01-17"
categories: [微服务]
tags: [
    "Konga"
]
---

> 在现今微服务的大潮下，如何对API进行统一管理势在必行。[Konga](https://github.com/pantsel/konga)是一款针对[API配置管理工具Kong](https://github.com/Kong/kong)的GUI工具。

![](konga_api.png)

##### Name
API名称
##### Hosts
主机名称，选填
##### Uris
URI，即后端微服务的API地址
##### Methods
允许的HTTP Methods, 一般可选的有`GET,POST,DELETE,PATCH,PUT,OPTIONS`等
##### Upstream URL
代理的后端微服务地址，如果是用了`Rancher`这类的微服务管理工具，可填写Rancher内网的IP，可以减少API的路由层数
##### Retries
重连次数，一般使用默认的`5`次就好

如果API接口是用于上传文件可将`Upstream send timeout`，`Upstream read timeout`，`Upstream send timeout`三个参数的值设置大一些，以避免上传文件未处理完毕，`Kong`就返回给前端了。
