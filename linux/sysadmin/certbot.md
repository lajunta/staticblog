Certbot setting 
===

### 安装 certbot

```text
sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository universe
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update

sudo apt-get install certbot
```

### 手动安装证书

```text
sudo certbot certonly --manual --preferred-challenges dns
```

### 查看
```text
sudo certbot certificates
```
### 更新

```text
sudo certbot renew
proxychains sudo certbot certonly --manual --preferred-challenges dns -d "*.qq.com,*.qq.com"
```

### 证书位置

```text
  /etc/letsencrypt/live/canbe.top/fullchain.pem
  /etc/letsencrypt/live/canbe.top/privkey.pem
```