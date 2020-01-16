```
title:       "for range 原理研究"
subtitle:    ""
description: "值传递？？？"
date:        2020-01-16
author:      NoobCoder
image:       ""
tags:        ["工作"]
categories:  ["Tech" ]
```



# golang 之 for range

## 背景

今天在遍历一个切片结构体时，对遍历到的结构体进行更改操作，结束时发现并没有生效，于是对这一现象进行研究。

