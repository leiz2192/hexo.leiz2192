---
title: Linux 32 or 64
date: 2018-05-13 22:03:57
tags:
---

查看linux是32位还是64位方法

```
>  ~ getconf LONG_BIT                
64
```

```
$ file $(where file)
/usr/bin/file: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 2.6.32, BuildID[sha1]=a25ff8d819e9d276b6c282d51a572ad74078da3d, stripped
```

```
$ uname -a
Linux ubuntu-xenial 4.4.0-112-generic #135-Ubuntu SMP Fri Jan 19 11:48:14 UTC 2018 i686 i686 i686 GNU/Linux
```
- ixxx的是32位的
- x86_64的是64位