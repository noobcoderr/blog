---
title:       "An Example Post"
subtitle:    ""
description: ""
date:        2018-06-04
author:      "noobcoderr"
image:       ""
tags:        ["tag1", "tag2"]
categories:  ["Tech" ]
---



## 这是travis-ci测试文章2

遇到的几个坑

1、 global:

​			Github Pages

​				GH_REF: github.com/noobcoderr/noobcoderr.github.io.git

​		GH_REF 后的链接加了个https，导致ci时没有access

2、  on:
    branch: blog # 博客源码的分支

​	branch后填博客源码所在库，别填master之类的。



疑问：

1、既然是要自动化部署，那么再本地写文章时要 hugo new post/wenzhang.md 这样嘛？好像是。