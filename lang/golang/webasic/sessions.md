Sessions 
====

使用 gorilla/sessions 来处理 sessions 数据

```go
go get github.com/gorilla/sessions
```

Cookies 是每次用户访问服务器时会附带的一小段数据,用来维护连接的状态

### 定义sessions

```go
var (
    // key must be 16, 24 or 32 bytes long (AES-128, AES-192 or AES-256)
    key = []byte("super-secret-key")
    cookieName := "your app cookie name"
    store = sessions.NewCookieStore(key)
)
```

### 获得 sessions value

```go
    session, _ := store.Get(r,cookieName)
    // Check if user is authenticated
    if auth, ok := session.Values["authenticated"].(bool); !ok || !auth {
        http.Error(w, "Forbidden", http.StatusForbidden)
        return
    }
```

### 设置 sessions value

```go
    session, _ := store.Get(r, cookieName)

    // Set user as authenticated
    session.Values["authenticated"] = true
    session.Save(r, w)
```

### curl test

```bash
$ curl -s http://localhost:8080/secret
Forbidden

$ curl -s -I http://localhost:8080/login
Set-Cookie: cookie-name=MTQ4NzE5Mz...

$ curl -s --cookie "cookie-name=MTQ4NzE5Mz..." http://localhost:8080/secret
The cake is a lie!
```
