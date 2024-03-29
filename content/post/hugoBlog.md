---
title: "使用Hugo + Github Page构建自己的个人博客"
description: "Hugo 从入门到拉跨"
date: 2019-08-23T00:50:49+08:00
lastmod: 2019-09-10T20:56:47+08:00
subtitle:    ""  
author:      NoobCoder
image:       ""
tags:        ["hugo", "github page"]
categories:  ["Tech" ]
keywords: [ "Hugo", "keyword" ]
---

# 使用Hugo + Github Page构建自己的个人博客

## 前言

个人博客折腾历程

1. 开始自学编程，意识到要用博客记录一些自己的学习笔记、遇到的问题、以及解决办法等等，最开始选择在CSDN上，一开始也还不懂用markdown，学git操作的时候还通过pull request给廖雪峰的git测试库传过doc格式的git学习总结。。。后被拒绝。。。
2. 后来学python web编程，学习用django自己建站，第一个站就是个人博客，照着教程也算是写出来了，前后端都自己实现，也没用博客模板，写的稀烂，后来买的云服务器过期了，就不再写了
3. 工作后没多少时间再去实现博客的整个前后端了，于是把目光瞄准了github page，虽然一开始以为是静态的不能交互，后来搞着搞着发现有gitalk等插件，可以通过issue等来进行评论交互，于是通通都搞上。就有了现在的博客。
4. 现在就专注记录学习了。

## 开始

### 安装hugo

macos: ` brew install hugo`

windows: 源码安装后记得将可执行文件加如到环境变量中

### 使用hugo创建网站

`hugo new site blog`

完成后会生成文件目录格式为

> ├── archetypes # 内容类型，在创建新内容时自动生成内容的配置
> ├── content # 网站文章内容，Markdown 文件
> ├── data
> ├── layouts # 网站模版，选择主题后会将主题中的 `layouts` 文件夹中的内容复制到此文件夹中
> ├── static # 包含 CSS、JavaScript、Fonts、media 等，决定网站的外观。选择主题后会将主题中的 `static` 文件夹中的内容复制到此文件夹中
> ├── themes # 存放主题文件,可以放多主题，最后在config.toml里进行配置
> └── config.toml # 网站的配置文件



### 生成文章

`hugo new post/first-post.md`

这个操作需要在该博客的根目录blog下操作，完成后会在content目录下生成post/first-post.md文件

生成的md文件会携带一些信息

```go
---
title: "first-post"
date: 2019-09-03T20:56:47+08:00
draft: true
---
```

`draft:true` 代表该篇文章为草稿。

如果要在文章内插入图片，可以

1. 将图片放到CDN，直接放cdn图片地址
2. 就放在static/img下，在config.toml内加入baseurl，引入图片时，以static为根目录，即我的baseurl为`https://noobcoderr.github.io`,我的静态文件存放在`static/img/xx.jpg`,那么我在文中引入图片时的路径为`![](/img/xx.jpg)`，这样做在写md时可能显示不出来图片，但是部署到github pages上时，图片时可以显示的。

以上信息为自动生成，可以自己添加参数

```go
title: "使用Hugo + Github Page构建自己的个人博客"
description: "hugo实践"
date: 2019-08-23T00:50:49+08:00
lastmod: 2019-09-03T20:56:47+08:00

subtitle:    ""  
author:      NoobCoder
image:       ""
tags:        ["hugo", "github page"]
categories:  ["Tech" ]
keywords: [ "Hugo", "keyword" ]

# CJKLanguage: Chinese, Japanese, Korean
isCJKLanguage: true

# 如果draft为true，除非使用 --buildDrafts 参数，否则不会发布文章
draft: false

# 设置文章的过期时间，如果是已过期的文章则不会发布，除非使用 --buildExpired 参数
# expiryDate: 2020-01-01
# 设置文章的发布时间，如果是未来的时间则不会发布，除非使用 --buildFuture 参数
# publishDate: 2020-01-01

# 排序你的文章
weight: 40

# 使用这两个参数将会重置permalink，默认使用文件名
#url: 
#slug: 

# 别名将通过重定向实现
#aliases:
#  - 别名1
#  - /posts/my-original-url/
#  - /2010/01/01/another-url.html

# type 与 layout 参数将会改变 Hugo 寻找该文章模板的顺序，将在下一节细述
#type: review
#layout: reviewarticle

# 三个比较复杂的参数
#taxonomies:
#linkTitle: 
#outputs:
# 实验性的参数
markup: "md"

# 你还可以自定义任何参数，这些参数可以在模板中使用
#include_toc: true
#show_comments: false
```

### 加载主题

`git init `

`git submodule 主题git themes/主题名`

由于主题也是git库，所以需要添加为子模块的形式

### 修改配置文件 `config.toml`

```shell
baseURL = "https://noobcoderr.github.io" # 博客地址
title = "NoobCoder Blog" # 浏览器的标题
languageCode = "zh-cn" # 语言
hasCJKLanguage = true # 开启可以让「字数统计」统计汉字
theme = "" # 主题名 (需要自己下载)
paginate = 10 # 每页的文章数
enableEmoji = true # 支持 Emoji
enableRobotsTXT = true # 支持 robots.txt
googleAnalytics = "" # Google 统计 id
preserveTaxonomyNames = true

[params]
    since = 2017
    author = "NoobCoder"                          # Author's name
    avatar = "/images/me/avatar.png"           # Author's avatar
    subtitle = "Just for Fun"                  # Subtitle
    cdn_url = ""           # Base CDN URL
    home_mode = "" # post or other
    enableGitalk = true # gitalk 评论系统
    google_verification = ""
    description = "" # (Meta) 描述
    keywords = "" # site keywords
    beian = ""
    baiduAnalytics = ""
    license= '本文采用<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/" target="_blank">知识共享署名-非商业性使用 4.0 国际许可协议</a>进行许可'
[params.social]
    GitHub = "xxoo"
    Twitter = "xxoo"
    Email   = "xxoo"
    Instagram = "xxoo"
    Wechat = "/images/me/wechat.png"  # Wechat QRcode image
    Facebook = "xxoo"
    Telegram = "xxoo"
    Dribbble = "xxoo"
    Medium = "xxoo"
[params.gitalk] # Github: https://github.com/gitalk/gitalk
    clientID = "" # Your client ID
    clientSecret = "" # Your client secret
    repo = "" # The repo to store comments
    owner = "" # Your GitHub ID
    admin= "" # Required. Github repository owner and collaborators. (Users who having write access to this repository)
    id= "location.pathname" # The unique id of the page.
    labels= "gitalk" # Github issue labels. If you used to use Gitment, you can change it
    perPage= 15 # Pagination size, with maximum 100.
    pagerDirection= "last" # Comment sorting direction, available values are 'last' and 'first'.
    createIssueManually= false # If it is 'false', it is auto to make a Github issue when the administrators login.
    distractionFreeMode= false # Enable hot key (cmd|ctrl + enter) submit comment.

```

### 本地构建

`hugo server`

即可在http:127.0.0.1:1313/ 查看到构建后的页面了

### 构建页面

`hugo `

即可将网页生成到默认的public子目录里。当然你可以指定一个别的文件夹

```shell
echo 'publishDir = "docs"' >> config.toml
```

即以后生成的网页静态文件都到docs子目录下了。

### 发布到github.io

将生成在docs下的网页文件推送到 `[your_username].github.io`库下，稍等一会儿你就可以看到你生成的网页了。

## 引入持续集成方案 - travisci

github获取的auth_token好像会过期？一个用了几天发现自动部署时push失败，然后重新生成一个token换上去之后又好了。

## 为博客添加站内搜索-algolia

关于站内搜索，参考了很多博客，试了google的站内搜索方案，也试了algolia，最后选择了algolia

其实被这个问题卡了2晚上，明明照着教程走，但是偏偏实现不了，再前端展示时，点击搜索按钮就404。最后参考 [采用ALGOLIA作为HUGO搜索方案 | BLOG折腾小记（6）](https://withdewhua.space/2018/11/23/search-with-algolia-in-hugo/) 解决了问题，原来问题的关键在于第一步，我没有建立 /search 页面：hugo new search/_index.md  ，所以才一直404。所以不论是看的哪篇文章的同志，请务必要先走这一步。

这里也不重复制造废话了，直接参考上面的链接把。

## 如何让别人搜到你的网站内容？

当然时提交自己的博客地址到各大搜索引擎啦

#### Google

[Google Search Console](https://search.google.com/search-console?resource_id=https%3A%2F%2Fnoobcoderr.github.io%2F)

Google [www.google.com/webmasters/…](https://link.juejin.im/?target=https%3A%2F%2Fwww.google.com%2Fwebmasters%2Ftools%2Fsubmit-url%3Fhl%3Dzh-CN)

#### Bing

必应 [www.bing.com/toolbox/sub…](https://link.juejin.im/?target=https%3A%2F%2Fwww.bing.com%2Ftoolbox%2Fsubmit-site-url)

#### Baidu

Baidu [ziyuan.baidu.com/linksubmit/…](https://link.juejin.im/?target=https%3A%2F%2Fziyuan.baidu.com%2Flinksubmit%2Findex)

参考文章 [要让Githubpages被索引到]([https://orianna-zzo.github.io/sci-tech/2018-01/blog%E5%85%BB%E6%88%90%E8%AE%B05-%E8%A6%81%E8%AE%A9github-pages%E8%A2%AB%E7%B4%A2%E5%BC%95%E5%88%B0/](https://orianna-zzo.github.io/sci-tech/2018-01/blog养成记5-要让github-pages被索引到/))



## SEO优化

## 访问量、阅读量统计

访问量可以使用google分析与百度分析站长工具。

## 自定义域名

## 网站运行时间、文章阅读量、网站访问量

[Blog折腾小记](https://withdewhua.space/2018/09/16/Blog_2/)

## 侧边toc，并且随着滑动监听



问题：

以上方法是得写好文章，然后将构建好的静态文件更新到github.io库下，即每次都得构建一次，是不是可以有一种，只用将文章写好后，推送到github.io上即可自动化构建然后推送呢？持续交付？

已完成。



```json
# CJKLanguage: Chinese, Japanese, Korean
isCJKLanguage: true

# 如果draft为true，除非使用 --buildDrafts 参数，否则不会发布文章
draft: false

# 设置文章的过期时间，如果是已过期的文章则不会发布，除非使用 --buildExpired 参数
# expiryDate: 2020-01-01
# 设置文章的发布时间，如果是未来的时间则不会发布，除非使用 --buildFuture 参数
# publishDate: 2020-01-01

# 排序你的文章，好像可以当作置顶功能
# weight: 10

# 使用这两个参数将会重置permalink，默认使用文件名
#url: 
#slug: 

# 别名将通过重定向实现
#aliases:
#  - 别名1
#  - /posts/my-original-url/
#  - /2010/01/01/another-url.html

# type 与 layout 参数将会改变 Hugo 寻找该文章模板的顺序，将在下一节细述
#type: review
#layout: reviewarticle

# 三个比较复杂的参数
#taxonomies:
#linkTitle: 
#outputs:
# 实验性的参数
markup: "md"

# 你还可以自定义任何参数，这些参数可以在模板中使用
#include_toc: true
#show_comments: false
```





优秀的Hugo建站博文

[优秀blgo教程](https://orianna-zzo.github.io/tags/blog/)

[静态网站构建手册-使用Hugo构建个人博客](https://jimmysong.io/hugo-handbook/) `静态博客一整套，从头到尾`

[Hugo 从入门到会用](https://blog.olowolo.com/post/hugo-quick-start/)

[Hugo 使用问题集](https://www.qd1024.com/post/hugo/)

[从Hexo迁移到Hugo-送漂亮的Hugo Theme主题](https://www.flysnow.org/2018/07/29/from-hexo-to-hugo.html)