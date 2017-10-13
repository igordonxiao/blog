+++
title = "使用Spring MVC ResponseEntity进行文件下载时需注意HttpStatus的设置"
description = ""
categories = [
    "Web"
]
tags = [
    "Spring",
]
date = "2017-04-12 18:24:18"
+++

```
return new ResponseEntity<>(FileUtils.readFileToByteArray(new File(attach.getPath())), headers, HttpStatus.CREATED);
```

使用`HttpStatus.CREATED`在Chrome中能正常下载, 但在IE, 搜狗浏览器中却无法下载, 并且也无法使用迅雷来下载.

解决办法就是将`HttpStatus.CREATED`改为`HttpStatus.OK`即可.