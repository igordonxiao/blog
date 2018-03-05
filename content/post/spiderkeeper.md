+++
title = "使用SpiderKeeper管理爬虫定时任务"
description = ""
categories = [
    "Python"
]
tags = [
    "scrapy", "scrapyd","SpiderKeeper",
]
date = "2018-01-26 13:23:23"
+++

> 我们经常会使用[Scrapy](https://scrapy.org/)来爬取数据。但爬取行为一般都是使用定时任务来控制。[Scrapyd](https://github.com/scrapy/scrapyd)就是一款非常方便的`[Scrapy](https://scrapy.org/)｀任务调度管理工具。和[Konga](https://github.com/pantsel/konga)与[Kong](https://github.com/Kong/kong)的关系一样，[SpiderKeeper](https://github.com/DormyMo/SpiderKeeper)是一款针对[Scrapyd](https://github.com/scrapy/scrapyd)的GUI管理工具。

我们可以将[Scrapyd](https://github.com/scrapy/scrapyd)打进`Docker`容器运行。

```shell
FROM python:3
ENV PYTHONUNBUFFERED 1
ENV ENV production
RUN mkdir /code/
WORKDIR /code
ADD . /code/
RUN apt-get update
RUN apt-get install build-essential g++ flex bison gperf ruby perl libsqlite3-dev libfontconfig1-dev libicu-dev libfreetype6 libssl-dev libpng-dev libjpeg-dev python libx11-dev libxext-dev -y
RUN tar -xvjf phantomjs-2.1.1-linux-x86_64.tar.bz2
RUN cp phantomjs-2.1.1-linux-x86_64/bin/phantomjs .
RUN cp phantomjs-2.1.1-linux-x86_64/bin/phantomjs ./skullspider/spiders
RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
EXPOSE 6800
CMD bash -c 'scrapyd-deploy --build-egg output.egg'
RUN pip install -i http://mirrors.aliyun.com/pypi/simple --trusted-host mirrors.aliyun.com -r requirements.txt
CMD bash -c 'scrapyd'
```

如果你的项目中也使用到了`phantomjs`，请将`Dockerfile`作适当调整。

### 运行`ScrapyKeeper`服务
```shell
docker run -p 6800:6800 scrapyd
```

### 通过`EGG File`部署
在你的`scrapy`爬虫项目中使用`scrapyd`生成`EGG File`
```shell
scrapyd-deploy --build-egg output.egg
```

打开`SpiderKeeper`Web页面，进入`Deploy`页面
![](spiderkeeper_01.png)


上传刚才的`EGG`文件，点击提交。

### 部署定时任务
![](spiderkeeper_02.png)

##### Spider
选择部署成功的某一个`spider`
##### Priority
优先级
##### Chose Daemon
如果只部署了一个`EGG`文件，选择`auto`即可，否则请选择对应的
##### Choose Month,Choose Day of Week,Choose Day of Month,Choose Hour,Choose Minutes
配置爬虫运行的周期性时间
##### Cron Expressions (m h dom mon dow)
如果你熟悉`Cron`表达式，也可以使用该方式配置
