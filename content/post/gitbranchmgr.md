---
title: "Gitlab版本式分支管理规范"
date: 2019-08-18
categories: [项目管理]
tags: [Gitlab]
---


为解决分支管理混乱，提交代码随意等问题，本文介绍了使用`gitlab Release branches flow`结合业界优秀实践制定以下分支管理规范及协作流程。


## Release branches with GitLab flow


![](release_branches.png)



使用master作为起点创建stable分支，并尽可能晚地去创建分支。这样可以最大限度地缩短必须将错误修复应用于多个分支的时间。
在发布分支后，只向该分支添加严重的错误修复，将这些错误修复合并到master中，然后再将它们挑选到发布分支中。先合并到master分支，然后通过“cherry-pick”合并到发布分支中，这种发布策略被称为“上游优先”策略，谷歌和红帽也实行这一策略。

每次在发布分支中包含错误修复时，通过设置新标记来增加补丁版本。



## gitlab角色与研发角色对应关系

| Gitlab角色名 | Gitlab角色名中文释义                                         | 研发角色   |
| ------------ | ------------------------------------------------------------ | ---------- |
| Owner        | 拥有者                                                       | 配置人员   |
| Maintainer   | 拥有建立和管理Git项目、合并分支和代码、给master打tag版本号等权限 | 开发管理员 |
| Developer    | 拥有浏览、push非主分支、提交pull request工单权限             | 研发人员   |
| Reporter     | 只能浏览代码，无push、pull request等权限，可以提交issues     | 测试人员   |
| Guest        | 只能浏览代码，无push、pull request等所有写权限               | 其他人员   |



## 分支命名规范

| 分支名称                         | 分支说明       | 备注                 |
| -------------------------------- | -------------- | -------------------- |
| master                           | 主分支         | 受保护分支，固定分支 |
| {version}-stable                 | 预发布分支     | 固定分支             |
| {version}-feature-{feature-name} | 特性开发分支   | 临时分支             |
| {version}-fix-{bug-id/bug-desc}  | 问题修复分支   | 临时分支             |
| {version}-cust-{name}            | 定制化开发分支 | 固定或临时分支       |



## 分支约定

**临时分支**：在开发完成会被删除

1. 功能分支 `{version}-feature-{feature-name}` ，用于新功能的开发
2. 修复分支`{version}-fix-{bug-id/bug-desc}` ，用于bug的修复

**固定分支**

1. 开发分支 `master` - 用于发布到测试环境，上游分支为 `feature` 和 `fix`，该分支为**受保护分支**
2. 发布分支 `{version}-stable` ，用于发布到预发环境，上游分支为 `master`，该分支要尽可能晚的创建，每次更新此分支都要更新一个小版本号

**注意：**

> 功能分支和修复分支合并进`master`分支，必须通过 Merge Request。



## 协作流程

### 演示用户列表

| 用户名                | 角色       | 备注 |
| --------------------- | ---------- | ---- |
| owner@gitlab.com      | Owner      |      |
| maintainer@gitlab.com | Maintainer |      |
| developer@gitlab.com  | Developer  |      |



### 创建代码仓库

#### 创建仓库

owner@gitlab.com登录gitlab创建代码仓库`example-demo`。

![1565336699801](create-repository.png)

#### 添加项目成员

添加`maintainer@gitlab.com`、`developer@gitlab.com`为项目成员，其角色分别为`Maintainer`、`Developer`。

![1565336183629](add-members.png)



#### 设置保护分支

`master`分支默认已经是保护分支。使用通配符`*-stable`设置预发布分支为保护分支，代码合并、代码推送的权限均设置为`Maintainers`。

![](protect-branchs.png)



### 开发新功能

developer@gitlab.com创建新功能开发分支`1.0.x-feature-login`。

![](create-feature-login.png)

开发完新功能后，提交`merge request`到`master`分支。

![](craete-merge-request.png)

![](submit-merge-request.png)

提交`merge request`时，**指定`assignee`为`maintainer`**，同时可将`Delete source branch when merge request is accepted.`和`Squash commits when merge request is accepted.`两个选项勾中。

### 合并请求

maintainer@gitlab.com登录gitlab查看提交的`merge-request`。点击`merge`按钮即可完成请求合并。

![](accept-mr.png)

在合并之前，开发管理员还应对代码进行检视。



### 发布新版本

对`master`进行开发测试和测试人员测试之后，如具备发版条件就可以发版了。

maintainer@gitlab.com登录gitlab创建预发版分支。

![](release-stable.png)

如有条件，还需要测试人员对预发版分支进行测试。

创建好预发版后，在该分支创建版本`tag`，创建成功后就可以看到发布的版本了。

![](release-1.0.0.png)



同理，发布2.0.0版本。

![](release-2.0.0.png)



### 修复1.0.0版本发现的bug

developer@gitlab.com从`master`分支创建bug修复分支`1.0.x-fix-login`。修复后向`master`分支提交`merge-request`。

maintainer@gitlab.com对`merger-request`进行合并操作。合并后要提交给测试进行测试，测试验证通过后再同步到其他有同样问题的分支。

由于这是在`1.0.0`的版本上的问题，说明`2.0.x`同样有这样的问题，因此，需要通过`Cherry-pick`将这个修复同步合并到`1.0.x-stable`和`2.0.x-stable`分支。

先`Cherry-pick`到`1.0.x-stable`分支。

![](chrry-pick-1.0.x.png)



再`Cherry-pick`到`2.0.x-stable`分支。

![](chrry-pick-2.0.x.png)

**注意：**

> 由于我们之前勾选了`Delete source branch when merge request is accepted`选项，在进行`Cherry-pick`时会自动生成一个类似`cherry-pick-{commit id}`的分支，合并时如果没有选中`Delete source branch`的话可以手动删除该分支。
>
> 

### 发布修复版本

基于`1.0.x-stable`发布`1.0.1-release`，基于`2.0.x-stable`发布`2.0.1-release`。

![](fixed-release.png)



### 定制化分支

定制化功能分支应从某个`stable`分支拉出。

![](cust-branch.png)

**注意：**

> 定制化的分支不能将代码合并到`master`分支和其他的`stable`分支。