---
title: "hugo云服务器建站"
date: 2021-11-30T23:51:43+08:00
lastmod: 2021-11-30
subtitle:    "hugo+nginx"
description: "记录配置nginx过程中遇到的一些问题"
author:      MoyuCoder
image:       "img/theme/go1.png"
tags:        ["Hugo"]
categories:  ["TECH"]
---

# 基本步骤

通过github来传输静态文件

安装nginx

配置nginx配置



# 遇到的问题

1、配置好nginx.conf之后，访问所有文件都是返回403 Forbidden

原因：403 是因为nginx配置内的user是nginx，需要改为root，nginx权限不足

2、nginx 无法导入到首页index.html的原因。

原因：配置好root后，配置index变量时也指定了绝对路径，即root/index.html，当指定了root之后，index就不需要绝对路径了，只要保证root指定的路径下有index.html，然后index指定为index.html即可。