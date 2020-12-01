Frpc and frps config
===

### frpc client config file

```text
[common]
server_addr = your_public_ip
server_port = your_public_port
token = your_secret_token
tls_enable = true
protocol = tcp
pool_count = 5 

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = your_public_ssh_port

[lab]
type = https
local_port = 443
subdomain = lab 

[ku]
type =  https
local_port = 443
custom_domains = your.ku.com

```

## frps server config file

```text
[common]
bind_addr = 0.0.0.0
bind_port = port
vhost_http_port = 80
vhost_https_port = 443

bind_udp_port = 7001
kcp_bind_port = 7000

allow_ports = 7700-8000,8900-9000,9010-9030
token = your_secret_port

max_pool_count = 20
max_ports_per_client = 0
custom_404_page = /var/www/html/404.html
subdomain_host = your_top_domain_host

```