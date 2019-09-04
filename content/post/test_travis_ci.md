---
title:       "hugo博客使用持续集成工具travis-ci"
subtitle:    ""
description: ""
date:        2019-09-04
author:      "NoobCoder"
image:       ""
tags:        ["travis", "hugo","CI"]
categories:  ["Tech" ]
---



## hugo博客使用持续集成工具travis-ci

遇到的几个坑

1、 global:

​			Github Pages

​				GH_REF: github.com/noobcoderr/noobcoderr.github.io.git

​		GH_REF 后的链接加了个https，导致ci时没有access

2、 travis ci `mkdir': File exists @ dir_s_mkdir - /tmp/d20190901-4695-w65n55/work (Errno::EEXIST)

​	问题所在 on:
​    			branch: blog # 博客源码的分支

​	branch后填博客源码所在库，别填master之类的。

3、CI显示没出错，部署成功，但是github.io库里根本每显示更新了文件，blog库也没更新。

​	暂无解决，应该是之前设置了生成到docs，但是默认的又是生成到public文件夹下。

​	是ci时，没有用户名密码，无法获取github.io库的commit、push权限，所以不成功

疑问：

1、既然是要自动化部署，那么再本地写文章时要 hugo new post/wenzhang.md 这样嘛？好像是。



参考文章：

[使用 Travis CI 自动部署 Hugo 博客](https://mogeko.me/2018/028/)

[第一个坑](https://github.com/chenwangji/blog/issues/1)