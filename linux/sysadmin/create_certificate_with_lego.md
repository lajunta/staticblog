# Lego介绍

LEGO 是一个Let's Encrypt加密客户端和ACME库

## 特点

- 注册CA
- 获取证书
- 更新证书
- 撤销证书
- 实现所有ACME验证
    - HTTP
    - DNS
    - TLS
- 附带多个DNS供应商
- 证书绑定

## 软件安装

```bash
go get -u github.com/go-acme/lego/v4/cmd/lego
```
或者
```bash
git clone git@github.com:go-acme/lego.git
make        # tests + doc + build
make build  # only build
```

## 命令行操作
在运行前假设80 443端口没有被占用

### 获取一个证书
```bash
lego --email="foo@bar.com" --domains="example.com" --http run
```
在当前工作目录下的 `.lego` 目录下可以查看获得的证书
```bash
ls -1 ./.lego/certificates
example.com.crt
example.com.issuer.crt
example.com.json
example.com.key
[对其它域名还有其它的文件]
```

#### 文件说明：

- `example.com.crt` 是服务器证书文件(其中包括了CA证书)
- `example.com.key` 是服务器证书的私有密钥
- `example.com.issuer.crt` 是CA证书
- `example.com.json` 包括了一些相关的元信息

对每个域名都会对应这四个文件，对于 通配符域名证书(`*.example.com`), 文件名类似这样： `_.example.com.crt`, `.crt`和`.key`文件是PEM编码X509格式的证书和私有密钥。 如果你要找 `cert.pem` 和 `privkey.pem` ，你可以使用 `example.com.crt` 和  `example.com.key`


#### 获取证书后运行脚本
```bash
lego --email="foo@bar.com" --domains="example.com" --http run --run-hook="./myscript.sh"
```
此时会产生以下四个环境变量

```bash
- LEGO_ACCOUNT_EMAIL:  email帐号
- LEGO_CERT_DOMAIN: 证书对应的域名.
- LEGO_CERT_PATH: 证书对应的路径.
- LEGO_CERT_KEY_PATH: 证书对应的加密密钥.
```

## 更新证书

```bash
lego --email="foo@bar.com" --domains="example.com" --http renew
```

#### 更新45天后过期的证书
```bash
lego --email="foo@bar.com" --domains="example.com" --http renew --days 45
```

## 如果你已经有一个运行的web服务器

此时，需要加入 --http --http.webroot 选项， 命令运行后会写入口令到 `.well-known/acme-challenge` , 此时不会`lego`命令不会启动web服务。

这个目录在服务器上对应路径上是可以公开访问的。否则不能验证通过.

或者能过 `rewrite` 指令把路径指向指定的目录

```bash
.well-known/acme-challenge  - >  其它目录
```

通过以下命令就可以生成通过HTTP-01验证方式把验证口令写入到文件中。
`<webroot dir>/.well-known/acme-challenge/`

```bash
lego --accept-tos -m foo@bar.com --http --http.webroot /path/to/webroot -d example.com run
```

___链接___

https://go-acme.github.io/lego/usage/cli/examples/
