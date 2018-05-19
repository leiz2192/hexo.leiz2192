---
title: Linux 查看进程和端口
date: 2018-04-14 19:21:13
tags:
---

# Linux 查看进程和端口

## 查看进程占用的端口号

```
# netstat -atunp | more
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      1363/cupsd      
tcp        0      0 192.168.189.128:58887   91.189.88.45:80         ESTABLISHED 2121/http       
tcp6       0      0 ::1:631                 :::*                    LISTEN      1363/cupsd      
udp        0      0 0.0.0.0:68              0.0.0.0:*                           803/dhclient 
```

## 根据端口号查进程

```
# lsof -i:631
COMMAND  PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
cupsd   1363 root    5u  IPv6   6338      0t0  TCP localhost:ipp (LISTEN)
cupsd   1363 root    6u  IPv4   6339      0t0  TCP localhost:ipp (LISTEN)
```

