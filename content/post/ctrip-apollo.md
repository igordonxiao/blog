---
title: "源码安装携程Apollo分布式配置中心"
date: "2018-01-26"
categories: [微服务]
tags: [
    "Apollo",
]
---

> Apollo（阿波罗）是携程框架部门研发的分布式配置中心，能够集中化管理应用不同环境、不同集群的配置，配置修改后能够实时推送到应用端，并且具备规范的权限、流程治理等特性，适用于微服务配置管理场景。 --[https://github.com/ctripcorp/apollo](https://github.com/ctripcorp/apollo)

## 安装
下载`https://github.com/nobodyiam/apollo-build-scripts`到本地
```shell
git cline https://github.com/nobodyiam/apollo-build-scripts
```

创建`ApolloPortalDB`, `ApolloConfigDB`MySQL数据库。
修改编译脚本的数据库连接信息
文件位置: `/apollo/apollo-master/scripts/build.sh`

```shell
# apollo config db info
apollo_config_db_url=jdbc:mysql://localhost:3306/ApolloConfigDB?characterEncoding=utf8
apollo_config_db_username=root
apollo_config_db_password=root

# apollo portal db info
apollo_portal_db_url=jdbc:mysql://localhost:3306/ApolloPortalDB?characterEncoding=utf8
apollo_portal_db_username=root
apollo_portal_db_password=root
```
执行编译:
```shell
sh /apollo/apollo-master/scripts/build.sh
```

编译成功就会出现`apollo-configservice`，`apollo-adminservice`，`apollo-portal`三个文件夹。

### `apollo-configservice`
核心配置服务
### `apollo-adminservice`
后户管理服务
### `apollo-portal`
Web界面服务

分别将这个三个文件夹下`/target/apollo-[服务名]-0.9.0-SNAPSHOT-github.zip`拷贝到新的文件夹并解压。分别执行其目录下的`scripts/startup.sh`启动服务。
访问`apollo-portal`所在`IP`及`端口`访问即可。

笔主在使用时，Apollo尚有一些功能在开发中，相信日后会更完善的。
