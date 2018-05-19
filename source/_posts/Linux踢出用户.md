---
title: Linux踢出用户
date: 2018-05-19 22:56:57
tags:
---

# Linux踢出用户
## 查看当前登录用户
```
$ whatis w  
w                    (1)  - Show who is logged on and what they are doing  
$ w  
09:49:30 up 1 day, 17:19,  4 users,  load average: 0.00, 0.00, 0.00  
USER     TTY      FROM              LOGIN@   IDLE   JCPU   PCPU WHAT  
root     tty3     -                09:25   23:25   0.10s  0.08s -bash  
root     pts/0    192.168.105.188  09:32    9:38   0.02s  0.02s -bash  
root     pts/1    192.168.105.188  09:36    9:32   0.03s  0.02s -bash  
wilsh    pts/2    192.168.105.188  09:41    0.00s  0.00s  0.00s w  
```

## pkill 踢出用户
```
$ whatis pkill  
pkill [pgrep]        (1)  - look up or signal processes based on name and other attributes  
```

```
pkill -kill -t pts/2
```
```
pkill -9 -u wilsh
```

## fuser 踢出用户
```
$ whatis fuser        
fuser (1)            - identify processes using files or sockets
```

```
fuser -k /dev/pts/2
```
