## HTTP Server


### 介绍
---

一个基本的服务器包括以下几项工作:

- 处理动态请求
- 提供静态文件服务
- 接受连接

### 处理动态请求
---

`net/http`包包括了所有的请求和处理功能，我们能用`http.HandleFunc`注册一个新的路由。它的第一个参数是一个路径，第二个参数是一个处理函数。

`http.Request`包括所有请求和参数信息，你可能使用`r.URL.Query().Get("token")` 处理`GET`参数，使用`r.FormValue("email")`处理 POST 参数。

### 静态资源服务
---

我们使用内置的`http.FileServer`来处理静态文件如js,css和图片

```go
fs := http.FileServer(http.Dir("static/"))
```

我们的文件服务设置好后，我们需要指定一个路径给它。需要注意的是：我们需要把URL的一部分给去掉，这通常是静态文件的目录名:

```go
http.Handle("/static/",http.StripPrefix("/static/",fs))
```

### 接收请求

```go
http.ListenAndServe(":80",nil)
```

### 代码

```go
package main

import(
    "fmt"
    "net/http"
)

func main(){
    http.HandleFunc("/",func(w http.ResponseWriter,r *http.Request){
        fmt.Fprint(w,"welcome friend")
    })

    fs := http.FileServer(http.Dir("static/"))
    http.Handle("/static/",http.StripPrefix("/static/",fs))

    http.ListenAndServe(":80",nil)
}
```