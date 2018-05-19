---
title: Hexo搭建个人博客
date: 2018-05-13 21:51:31
tags:
---

# 准备工作
1) 安装Git. 将本地的网页和文章等内容发布或提交到GitHub上需要用到Git.

2) 安装Node.js. Hexo是基于Node.js的高效静态站点生成框架.

3) 安装Hexo. 在Node.js安装完成后, 通过控制台执行以下命令即可.

```
$ npm install hexo-cli -g
```
# 创建本地博客
```
$ hexo init leiz2192.github.io       //leiz2192.github.io是项目名
$ cd leiz2192.github.io
$ hexo new "YourBlogTitle"         //YourBlogTitle是博客名, 会生成YourBlogTitle.md
### 撰写YourBlogTitle.md
$ hexo g   //生成网页
$ hexo s   //s是server的缩写，启动服务, 通过http://localhost:4000/本地访问
```

# 发布博客
```
npm install hexo-deployer-git —save //安装部署插件
```

```
hexo d -g  //生成网页并发布到GitHub
```