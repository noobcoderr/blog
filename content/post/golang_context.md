---
title:       "Golang基础包-Context"
subtitle:    ""
description: ""
date:        2019-10-09
author:      NoobCoder
image:       ""
tags:        ["Golang"]
categories:  ["Tech" ]
---

##  背景

学一门知识总时得带着目的来学，不然漫无目的的学，很容易做无用功。最近在项目中使用Time包内的ratelimit模块实现请求限流时用到了Context，但是我不清楚Context在其中的作用，看了好多博客都是千篇一律在那些ratelimit如何用，并没有分析代码。

所以我带着以下目的来学习

1、ratelimit实现时的Context是起什么作用的？

2、项目中使用ECHO框架，其处理函数的参数为`ctx echo.context`此处的context和golang内置的Context有什么联系与区别？

3、golang里的context是什么？原理、使用？

