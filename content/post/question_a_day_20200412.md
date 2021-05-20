---
title:       "每日一问"
subtitle:    ""
description: ""
date:        2020-04-12
author:      我不道啊？
image:       ""
tags:        ["golang"]
categories:  ["Tech" ]
---

# 每日一问

## golang的channel取值问题

## golang的野指针问题

## golang的数据类型以及内存占用问题

golang有其基本类型以及复合类型

基本类型包括：int、float、string

复合类型包括：map、channel、struct、array、slice、interface

那么像其他语言里面中整形会有short，long 区分，其实golang的int也有这些区分，如 int8、int16、int32、int64，还有无符号类型 uint8...等等

那么我们怎么计算每种类型所占用的内存大小呢，像C语言中的sizeof，Golang也提供了这样的方法。

```go
package main

import (
	"fmt"
	"unsafe"
)
func main() {
    short := int16(1)
    int1 := int32(1)
    long := int64(1)
    ll := int(1)
    fmt.Printf("The size of int16 is %d bytes.\n", unsafe.Sizeof(short))
    fmt.Printf("The size of int32 is %d bytes.\n", unsafe.Sizeof(int1))
    fmt.Printf("The size of int64 is %d bytes.\n", unsafe.Sizeof(long))
    fmt.Printf("The size of int int is %d bytes.\n", unsafe.Sizeof(ll))
}
```

会得到

```go
The size of short is 2 bytes.
The size of int is 4 bytes.
The size of long is 8 bytes.
The size of long long is 8 bytes.
```

