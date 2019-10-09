---
title:       "工作中遇到的并发"
subtitle:    ""
description: ""
date:        2019-09-18
lastmod:     2019-09-18
author:      NoobCoder
image:       ""
tags:        ["并发", "锁"]
categories:  ["Tech" ]
---

## 场景描述

业务中需要获取token，获取token是个公共服务，获取token流程为

首先去redis内查是否存在且没过期，有则直接返回，

如果没有或者过期了(redis里key过期了应该就删除了吧，就相当于没有了)，再去调用QQ token服务获取新的token

为了防止情况：当上一个token过期的瞬间，有大量请求进入，发现redis里没有key，那都去调QQ token服务，这样会导致生成过多的token，所以给请求QQtoken服务的操作加了锁，只有获得了锁的请求才可以去刷新token。

按道理说，加了锁，那么就应该解决了问题。但是在进行压测(500和2000并发)的时候，当上一个token过期的一瞬间，出现了一个token，存在了没多久后，又出现了一个token，理论上新旧token交替过程中只存在2个token，新的和旧的，但是这里出现了3个token。那一个token存在的时间很短但是也被返回了很多次说明这个是已经存储到redis内了，但是很快的失效(几毫秒的时间)，然后消失掉，导致后续请求又重新刷新。

在思考问题出现的可能：

1、token过期，大量的请求的逻辑变为去拿锁刷token，发现锁也是不可用状态于是sleep后重新询问，但是已经有一个请求获得了锁，并已经处于调QQ token接口获取token然后存到redis的流程里了

回去再去看看休眠重查流程，感觉应该是这里出问题了, 总算理解了斗鱼说的缓冲被击穿的严重性，即当缓存过期的瞬间，有大量请求会去刷新 Q token，而斗鱼的并发量是很高的，上万了估计，那么如果缓存失效，这上万的请求去访问Mysql数据库，感觉应该顶不住那么大压力。

后续是：

如果请求获不到锁，就会休眠1秒，再重新走查redis取锁刷新token流程，循环3次，3次后再没有结果就返回超时。

如果获取到了锁，则会先删除key再去刷Q token，所以上述问题的原因可能是某条请求查询redis失败(实际数据是存在的),然后就进入了获取锁重新刷新token的流程(这个过程中会先删除key值)，于是新的第一个token存在没多久，就又被刷新了。

那么问题的关键点就在key存在的情况下获取失败，如何把这次失败的查询揪出来呢？

在新旧token刷新的过程中，应该只被允许获取1次锁。但是出现了新的问题，在一次测试过程中，获取了4次锁。

这个问题不是每一次测试都会存在，是偶然性的。

发现确实存在key存在，但是查询失败进而去获取锁重新刷新的情况。





## PS

成天想着处理高并发，现在这个问题涉及到了高并发，redis，锁，够你整的了。

这个问题让我对并发的理解更深了。就是并发其实不单单是指在某一刻又大量请求同时涌入，而应该更具体地说是这些请求都被接收了，而没有被拒绝，都在被处理的过程中。虽然CPU同一刻只处理一个请求，但是其实CPU是这些请求之间不断切换造成同一刻处理大量请求的感觉。

还学到了锁的用途，其实之前也学过锁的知识，也通过教程学过并发，但是这次是我认识到并将锁和并发联系起来，用锁去解决并发产生的问题。也算是个收获吧
