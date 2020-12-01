Systemctl Service and Timer
===

### 服务样本

```text
[Unit]
Description=Your App Service
After=network.target

[Service]
Type=simple
User=one
WorkingDirectory=/path/to/your/apps
Environment=PORT=9009
Environment=USER=someuser
Environment=PASSWORD=somepassword
Restart=on-failure
RestartSec=5s
ExecStart=/path/to/your/command
[Install]
WantedBy=multi-user.target
```

### 定时器样本

__必须和服务同一名称__


```text
[Unit]
Description=Run certbot twice daily

[Timer]
OnCalendar=*-*-* 00,12:00:00
RandomizedDelaySec=43200
Persistent=true

[Install]
WantedBy=timers.target
```

