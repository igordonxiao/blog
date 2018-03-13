+++
title = "解决gjson无法解析数组字符串的问题"
description = "解决gjson无法解析数组字符串的问题"
categories = [
    "Golang"
]
tags = [
    "Golang",
]
date = "2017-10-24 10:36:02"
+++
  

> [gjson](https://github.com/tidwall/gjson)是一款Go语言的`JSON`解析库。

`gjson`解析`JSON`确实非常方便，当parse一个`JSON`对象后返回的是一个`Result`对象，基于这个`Result`对象还可以继续解析，
直到调用`String()`或`Value()`方法时才真正把`JSON`解析出来，这种设计理念和`Java`中的`Stream`很类似。
     
`gjson`除了能够解析整个`JSON`对象外，在解析时还可以使用`.`语法解析出内嵌的对象，这就超级方便了。    
     
但是`gjson`没有API能直解解析`JSON`数组字符串，为解决这个问题需要在数组外面再包裹一层对象，例如：    

  ```go
  package main
  
  import (
    "github.com/tidwall/gjson"
    "github.com/gin-gonic/gin/json"
  )
  
  func main() {
    jsonStr := `[{"name": "阿花", "age": 23}, {"name": "队长", "age": 22}]`
    jsonStr = `{"res": ` + jsonStr + "}"
    arr := gjson.Parse(jsonStr).Value().(map[string]interface{})["res"]
    bytes, _ := json.Marshal(arr)
    println(string(bytes))
  }
  ```
  
输出：
```json
[{"age":23,"name":"阿花"},{"age":22,"name":"队长"}]

```
