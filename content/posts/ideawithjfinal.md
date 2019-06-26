---
title: "用Jetty在IDEA中开发基于Jfinal的项目"
tags: [
    "Java",
]
date: "2015-12-31"
---

> 背景:之前用Tomcat在IDEA中成功搭建了项目环境, 配合Jrebel神器可以实现大部分代码的热加载, 但添加一个新的路由后却无法被热加载.

今天终于成功使用Jetty在IDEA中实现热加载, 其实Web项目结构都是一致的, 只是使用不同的Web Container时配置不太一样而已，现记录要点如下。


1. 按照普通IDEA Web项目搭建好基于Jfinal的新项目;
2. 为项目创建好Artifacts, 注意观察输出路径, 如我的是`/Users/gordon/Documents/javaproject/jfinaltest/out/artifacts/jfinaltest_war_exploded`, 同时勾选中下面的`Make on build`
3. 根据Jfianl手册配置Jetty启动应用, 新建一个Application
```shell
Main Class: com.jfinal.core.JFinal
Working directory: 选择当前项目所在位置
Use classpath of module: 选择当前模块所在位置
```   

4. 最重要的一步
Program arguments: 
```shell
out/artifacts/jfinaltest_war_exploded 9000 / 5
```
这是Jfinal启动Jetty时用到的四个参数,以空格分隔. 第一个参数就是第2步中的输出路径中的内容, 如果你更改了输出路径则这里也要相应改变。

搞定以上四步, 就可以点击绿色的播放按钮开始运行了, 一旦我们修改了代码想要热加载一定要选编译一下工程(就是绿色播放按钮左边那个按钮),也可以使用快捷键。

这样新加了路由也能被加载了.
