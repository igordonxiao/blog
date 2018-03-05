+++
title = "Docker常用命令"
description = ""
categories = [
    "MicroService"
]
tags = [
    "Docker",
]
date = "2018-01-19 13:23:23"
+++


##### 删除所有`tag`为`none`的镜像
```shell
docker images|grep none|awk '{print $3 }'|xargs docker rmi

docker images -a |grep none|awk '{print $3 }'|xargs docker rmi
```
##### 删除所有镜像
```shell
docker images -a |awk '{print $3 }'|xargs docker rmi
```
##### 删除所有的运行的`Docker`镜像
```shell
docker ps -a | awk '{print $1 }' | xargs docker stop

docker ps -a | awk '{print $1 }' | xargs docker rm 
```
##### 进入交互式终端
```shell
docker exec -it [CONTAINER ID or Name] bash
```
##### 构建镜像
```shell
docker build -t webservice:v3 .
docker build -t webservice:v3 -f Dockerfile.dev .
```
##### 存出镜像
```shell
docker save -o ubuntu_14.04.tar ubuntu:14.04
```
##### 载入镜像
```shell
docker load --input ubuntu_14.04.tar
```