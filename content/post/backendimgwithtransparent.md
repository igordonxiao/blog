+++
title = "给图片加上一个深色透明遮罩效果"
description = ""
categories = [
    "Web"
]
tags = [
    "CSS3",
]
date = "2015-11-25 14:34:49"
+++

很多App的图文列表里会将标题置于图片之上,而图片本身则是被处理加上一层深色半透明背景,当然图片的效果可以完全直接通过PS软件作出来,但对于维护来说增加了设计师的工作量.另外一个选择则是通过`CSS`来实现.

本文实现的图片效果主要使用到了`after`这个伪元素, 在图片之外的`div`容器设置一个遮罩的效果,具体请参考以下.

### 原图效果
![](example.png)      

### 处理之后的效果 
![](hadledexample.png)    


附完整`CSS`代码     
```css

.img-container {
    position: relative;
    width: 291px;
}
.img-container:after{
    content: '';
    display: inline-block;
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    opacity: 0.7;
    background: #191919;
}
.title {
    position: absolute;
    width: 100%;
    top:40%;
    transform: translateY(-50%);
    text-align: center;
    color: #ffffff;
    z-index: 2;
}
.title span {
    border: 1px solid #ffffff;
    padding: 4px;
}
   
```     
   
   
附完整`HTML`代码     
```html  
<div class="img-container">
    <img src="example.png" alt=""/>
    <h2 class="title"><span>夕阳下的风景</span></h2>
</div>
``` 

