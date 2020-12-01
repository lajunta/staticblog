## 路由设计

`net/http`包提供了相对完整的http协议的功能,但是对于复杂的路由请求和解析，功能较弱，我们可以用第三方的`gorilla/mux`来完成这个事情。

### 安装 `gorilla/mux`包

```go
go get -u github.com/gorilla/mux
```

### 创建新路由

首先，先创建一个新的请求路由,它将做为你的应用的主路由并传给你的服务器，它接受所有的网页连接。

```go
router := mux.NewRouter()
```

### 注册一个请求处理函数

你可以像以前的处理函数一样使用它

```go
router.HandleFunc(path, handler)
```

### URL参数

`gorilla/mux`最强大的地方就是url的解析功能,它可以从url中提取相关的参数。

```text
/books/go-programming/page/10
```
上面的url有二个动态参数，`go-programming` 和 `10`

为了对以上的动态参数进行匹配，在设计路由时，可以这样做:

```go
router.HandleFunc("/books/{title}/page/{page}",handler)
```

在handler 中可以众花括号中取得相应的值,`mux.Vars(r)`这个函数把请求做为参数：

```go
func(w http.ResponseWriter, r *http.Request){
    vars := mux.Vars(r)
    title := vars["title"] //get title
    page := vars["page"]  //get page
}
```

表单数据依旧用 `r.FormValue("token")`来获取

### 设置 HTTP Server 路由器

把 nil 更换为 router

```go
http.ListenAndServe(":80",router)
```


### 代码

```go
package main

import (
    "fmt"
    "net/http"

    "github.com/gorilla/mux"
)

func main() {
    r := mux.NewRouter()

    r.HandleFunc("/books/{title}/page/{page}", func(w http.ResponseWriter, r *http.Request) {
        vars := mux.Vars(r)
        title := vars["title"]
        page := vars["page"]

        fmt.Fprintf(w, "You've requested the book: %s on page %s\n", title, page)
    })

    http.ListenAndServe(":80", r)
}
```

### `gorilla/mux` 的一些特色

#### 限定方法

```go
r.HandleFunc("/books/{title}", CreateBook).Methods("POST")
r.HandleFunc("/books/{title}", ShowBook).Methods("GET")
r.HandleFunc("/books/{title}", UpdateBook).Methods("PUT")
r.HandleFunc("/books/{title}", DeleteBook).Methods("DELETE")
```

#### 限定主机名和子路径

```go
r.HandleFunc("/book/{title}",BookHandler).Host("www.example.com")
```

#### 子路由器

```go
bookrouter := r.PathPrefix("/books").Subrouter()
bookrouter.HandleFunc("/",AllBooks)
bookrouter.HandleFunc("/{title}",GetBook)
```