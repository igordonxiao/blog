---
title: "使用Vagrant建立Django独立开发环境"
date: "2017-07-03"
categories: [后端]
tags: [
    "Django",
]
---

### Vagrant及Ubuntu环境搭建

step 1. 安装`Vagrant 1.9.5`

step 2. 本地创建`Django`项目, 使用`PyCharm 2017.1.4` 创建`CMS_Service`并进入该目录    

```shell
cd CMS_Service
```
step 3. 初始化`Vagrantfile`    

```shell
vagrant init
```

step 4. 安装`Ubuntu 16.04.2 LTS`
```shell
vim Vagrantfile
```

替换第15内容config.vm.box = "base"为
```shell
config.vm.box = "ubuntu/trusty64"
```
打开第34行的注释, 
```shell
config.vm.network "private_network", ip: "192.168.33.10"
```

保存Vagrantfile

step 5. 启动`vagrant`配置的`Ubuntu`系统    
```shell
vagrant up
```

step 6. 进入`vagrant`配置好的```Ubuntu```系统
```shell
vagrant ssh
```

### Python环境搭建

step 1. 备份系统镜像源
```shell
sudo mv  /etc/apt/sources.list  /etc/apt/sources.list.bak
```

step 2. 新建国内快速镜像源
```shell
sudo vim /etc/apt/sources.list
```

粘贴以下内容到文件中并保存

```shell
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ trusty-security main restricted universe multiverse
```

step 3. 更新镜像源并安装必要的系统包

```shell
sudo apt-get update
sudo apt-get install git libpq-dev
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev
```

step 4. 安装`Pyenv`

```shell
curl -L https://raw.githubusercontent.com/pyenv/pyenv-installer/master/bin/pyenv-installer | bash
```

step 5. 配置`Pyenv`环境变量

```shell
sudo vim ~/.bashrc
```

添加以下内容并保存:

```shell
export PATH="/home/vagrant/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
source .bashrc
```  

step 6. 安装`Python 3.4.3`

```shell
wget http://mirrors.sohu.com/python/3.4.3/Python-3.4.3.tar.xz -P ~/.pyenv/cache/;pyenv install 3.4.3 
pyenv local 3.4.3
pyenv global 3.4.3
```


step 7. 进入`/vagrant`, 安装`virtualenv`

```shell
pip install virtualenv
```

step 8. 创建虚拟环境并激活
```shell
pyenv virtualenv pycmsenv
pyenv activate pycmsenv
```

### 安装并创建Django
step 1. 安装`Django`
```shell
pip install Django==1.11.3
```

查看`Django`版本号

```shell
python -m django --version
```

step 2. 修改`settings.py`中的
```shell
ALLOWED_HOSTS = []
```
为
```shell
ALLOWED_HOSTS = ['*']
```

step 3. 启动项目

```shell
./manage.py runserver 0.0.0.0:8080
```

在本机通过`http://192.168.33.10:8080/`访问, 看到`It worked!`则成功了