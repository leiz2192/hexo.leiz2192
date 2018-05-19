---
title: VS2017编译putty
date: 2018-05-13 22:00:29
tags:
---

# VS2017编译putty
## 下载putty

从[putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)中选择"Unix source archive"下载

## 加载工程 

源码下载后, 解压, 双击"windows\MSVC\putty\putty.dsp", VS2017加载工程

## 修改工程属性

按如下设置VS2017工程属性.
此设置是为了解决问题: 错误 D8016 “/ZI”和“/Gy-”命令行选项不兼容

```
“项目”—>“属性”—>“C/C++”—>“常规”—>“调试信息格式”—>选择“程序数据库(/Zi)”或“无”
“项目”—>“属性”—>“C/C++”—>“代码生成”—>“启用函数集链接”—>选择“是 (/Gy)”
```

## 修改代码

1) 修改windows\version.rc2
修改前后分别如下, 解决version.rc2 cannot open include file 'version.h' 的问题

```
#include "version.h"
#include "licence.h"
```
```
#include "../../../version.h"
#include "../../../licence.h"
```

2) 修改noshare.c
修改前后分别如下, 即注释platform_ssh_share和platform_ssh_share_cleanup的实现.
解决问题:
winshare.obj : error LNK2005: _platform_ssh_share 已经在 noshare.obj 中定义
winshare.obj : error LNK2005: _platform_ssh_share_cleanup 已经在 noshare.obj 中定义

```
int platform_ssh_share(const char *name, Conf *conf,
                       Plug downplug, Plug upplug, Socket *sock,
                       char **logtext, char **ds_err, char **us_err,
                       int can_upstream, int can_downstream)
{
    return SHARE_NONE;
}

void platform_ssh_share_cleanup(const char *name)
{
}
```
```
/*
int platform_ssh_share(const char *name, Conf *conf,
                       Plug downplug, Plug upplug, Socket *sock,
                       char **logtext, char **ds_err, char **us_err,
                       int can_upstream, int can_downstream)
{
    return SHARE_NONE;
}

void platform_ssh_share_cleanup(const char *name)
{
}
*/
```

## 编译工程 

右键工程生成解决方案.