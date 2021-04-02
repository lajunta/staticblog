生成随机数
===

xxd
---

```bash
xxd -g 2 -l 64 -p /dev/random | tr -d "\n"
```

