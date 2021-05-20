# API设计风格之RESTful和RPC风格

最近几件事让我对API的设计风格有了更多的认识。

1、开发谷歌支付后端时，需要调用谷歌billing库对谷歌订单进行确认操作，这样用户三天之内就无法退款了，该接口描述是`If successful, this method returns an empty response body.` 一开始我以为是调用成功是返回status200，如果成功则是空body，失败则是包含具体错误信息的json，但是后来发现成功是204和空body，失败是4xx系列错误和包含错误信息的jsonbody，当时还理解不了为什么这么设计。

2、在看一门golang的视频教程的时候，视频一开始就进行API设计，其实讲的就是REST风格的设计。

![image-20200421010908761](../../static/img/api_style_rest.png)

之前的疑惑，顿时有了茅塞顿开的感觉，从最开始的时候不知道什么是RESTful风格，到后懵懂的觉得应该是用各种http方法来执行请求，再到后来的基于资源的请求，让我慢慢的解开了之前的一些疑惑，并且，加深了我对HTTP状态码的理解(记得之前校招面一次看一次，过不久就忘了)，之前都是200，然后成功/错误信息全放里面。

搜了搜REST风格的详情之后，发现还有些其他常见的API风格，如RPC，GraphQL等，而自己目前写的起始就是RPC风格的API，其实哪一种好哪一种不好，目前我也谈不上，暂且记录和总结一下自己看过的文章吧。

## RPC风格

最常见，面向过程

## REST风格

逐渐变的多了起来，面向资源

## GraphQL风格

联想一下ES的查询接口，就是这个风格，查询放body里面

## 总结

其实没必要死磕哪一种风格，哪一种风格最匹配当前的业务场景，就用哪一种。

## 参考文章

- [RESTful API 最佳实践](https://www.ruanyifeng.com/blog/2018/10/restful-api-best-practices.html)

- [API设计中关于RPC和REST 两种风格选择的个人理解](https://bbs.huaweicloud.com/blogs/107004)

- [浅谈三种API设计风格RPC、REST、GraphQL](https://zhuanlan.zhihu.com/p/56955812)

