## Apache 使用SSL证书

## 启用ssl

```bash
a2enmod ssl
a2ensite default-ssl
```

## 配置 default-ssl.conf

```bash
<VirtualHost example.com:443>
  ServerName example.com
  ServerAlias www.example.com
  ServerAdmin your@email.com
  
  SSLCertificateFile /path/to/your/server/certificates.crt
  SSLCertificateFile /path/to/your/server/private.key
  SSLCertificateChainFile /certificate/of/your/ca-chain-file.crt
  
</VirtualHost>

## Restart apache2 server

```bash
service restart apache2
```

## SSL 证书更新

service file

`/etc/systemd/system/ssl-update.service`

```bash
[Unit]
Description=Certbot Renewal

[Service]
ExecStart=/usr/bin/certbot renew --post-hook "systemctl restart httpd"
```

timer file

`/etc/systemd/system/ssl-update.timer`

```bash
[Unit]
Description=Timer for Certbot Renewal

[Timer]
OnBootSec=300
OnUnitActiveSec=1w

[Install]
WantedBy=multi-user.target
```

