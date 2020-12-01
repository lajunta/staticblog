hello
=====
- Category: lang/goweb
- Tags: 
- Created: 2020-11-30T15:19:56+08:00

### 介绍

Go 语言已经内置了一个网页服务器.标准库的`net/http`包包含了HTTP协议的所有功能,如HTTP客户端和服务端功能,在本例中你会发现创建一个网页服务端是多么的简单。


### 路由及处理请求的函数

首先创建一个函数来处理从浏览器发来的所有HTTP连接,函数签名:

```go
func(w http.ResponseWriter, r *http.Request)
```

这个函数包含二个参数:

第一个是你要写入接口,你将会把你的输出写到这个接口中

第二个是浏览器发来的请求，你可以检查并处理其中的相关信息

把路由和函数结合起来,形成最终的处理函数:

```go
http.HandleFunc("/", func (w http.ResponseWriter, r *http.Request)){
    fmt.Fprintf(w, "你请求的网址是: %s\n", r.URL.Path)
}
```

### 监听HTTP连接

路由请求处理函数不能接收到任何外部的HTTP连接,需要一个HTTP服务函数来监听并转发相关的请求到处理函数

```go
http.ListenAndServe(":80",nil)
```

### 完整代码

```go
package main

import (
    "fmt"
    "net/http"
)

// indexHandler handle request to url path "/"
func indexHandler(w http.ResponseWriter, r *http.Request){
    fmt.Fprintf(w, "你请求的网址是: %s\n", r.URL.Path)
}

func main() {
    http.HandleFunc("/", indexHandler)
    http.ListenAndServe(":80", nil)
}
```