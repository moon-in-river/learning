# 常用命令

- 根据 port 查看 pid

```bash
netstat -ano | findstr :4001
# - a：显示所有连接和监听端口
# - n：以数字形式显示地址和端口号（不做 DNS 反解析）
# - o：显示每个连接关联的进程 ID（PID）
```

- 根据 pid 查看进程名

```bash
# windows
tasklist | findstr  pid
```
