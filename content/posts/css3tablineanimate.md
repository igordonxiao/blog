---
title: "CSS3实现Tab切换带下划线动画的效果"
tags: [
    "CSS3",
]
date: "2015-11-21"
---

今天带来一个Tab切换带下划线动画的效果,如下图:    
![](css3tablineanimation.gif)

该动画还有一个超过100%宽度再反弹回100%的效果。

这个动画主要是使用到了`CSS3`的`@keyframes`来实现, 其中`animation: tab-line-active-animation .2s`的`.2s`是设置动画的执行时间，具体设置请参考代码。

附完整的`CSS3`代码  
```css
.tab-container {
    width: 200px;
}
.tab-container .tab {
    margin: 0;
    padding: 0;
    list-style-type: none;
}

.tab-container .tab .tab-item {
    width: 50%;
    float: left;
    cursor: pointer;
    text-align: center;
    display: inline-block;
    height: 40px;
    line-height: 40px;
    color: #9d9d9d;
}

.tab-container .tab .tab-item-line {
    width: 100%;
    height: 2px;
}

@keyframes tab-line-active-animation {
    0%   {width: 0;}
    10%  {width: 10%;}
    25%  {width: 25%;}
    35%  {width: 35%;}
    45%  {width: 50%;}
    60%  {width: 50%;}
    75%  {width: 115%;}
    85%  {width: 108%;}
    100% {width: 100%;}
}

.tab-container .tab .tab-line-active {
    animation: tab-line-active-animation .2s;
    background: #ffbd1f;
}
```
     
附完整的`HTML`代码
```html
<div class="tab-container">
    <ul class="tab">
        <li class="tab-item">
            <span class="tab-item-title">好莱坞</span>
            <div class="tab-item-line"></div>
        </li>
        <li class="tab-item">
            <span class="tab-item-title">美式喜剧</span>
            <div class="tab-item-line"></div>
        </li>
    </ul>
</div>
```
           
     
附完整的`JS`代码(依赖于jQuery)
```javascript
$(function() {
        var $tabItemLine = '.tab-item-line';
        var $clsTabLineActive = 'tab-line-active';
        $('.tab-item', '.tab').on('click', function() {
            $(this).find($tabItemLine).addClass($clsTabLineActive);
            $(this).siblings().find($tabItemLine).removeClass($clsTabLineActive);
        });
});
```
