---
title: "使用Hugo + Github Page构建自己的个人博客"
date: 2019-08-23T00:50:49+08:00
subtitle:    ""
description: ""
date:        2018-06-04
author:      "NoobCoder"
image:       ""
tags:        ["hugo", "github page"]
categories:  ["Tech" ]
---

## 使用Hugo + Github Page构建自己的个人博客

##前言

个人博客折腾历程

1. 开始自学编程，意识到要用博客记录一些自己的学习笔记、遇到的问题、以及解决办法等等，最开始选择在CSDN上，一开始也还不懂用markdown，学git操作的时候还通过pull request给廖雪峰的git测试库传过doc格式的git学习总结。。。后被拒绝。。。
2. 后来学python web编程，学习用django自己建站，第一个站就是个人博客，照着教程也算是写出来了，前后端都自己实现，也没用博客模板，写的稀烂，后来买的云服务器过期了，就不再写了
3. 工作后没多少时间再去实现博客的整个前后端了，于是把目光瞄准了github page，虽然一开始以为是静态的不能交互，后来搞着搞着发现有gitalk等插件，可以通过issue等来进行评论交互，于是通通都搞上。就有了现在的博客。
4. 现在就专注记录学习了。

##开始

1. 安装hugo

   macos: brew install hugo

   windows: 源码安装后记得将可执行文件加如到环境变量中

2. 使用hugo创建网站

   hugo new site blog

   完成后会生成文件目录格式为

   > ├── archetypes # 内容类型，在创建新内容时自动生成内容的配置
   > ├── content # 网站文章内容，Markdown 文件
   > ├── data
   > ├── layouts # 网站模版，选择主题后会将主题中的 `layouts` 文件夹中的内容复制到此文件夹中
   > ├── static # 包含 CSS、JavaScript、Fonts、media 等，决定网站的外观。选择主题后会将主题中的 `static` 文件夹中的内容复制到此文件夹中
   > ├── themes # 存放主题文件,可以放多主题，最后在config.toml里进行配置
   > └── config.toml # 网站的配置文件

3. 生成文章

   hugo new post/first-post.md

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

   以上信息为自动生成，可以自己添加参数

   ```go
   title: "使用Hugo + Github Page构建自己的个人博客"
   date: 2019-08-23T00:50:49+08:00
   subtitle:    ""
   description: ""
   date:        2019-09-03T20:56:47+08:00
   author:      "NoobCoder"
   image:       ""
   tags:        ["hugo", "github page"]
   categories:  ["Tech" ]
   ```

4. 加载主题

   `git init `

   `git submodule 主题git themes/主题名`

   由于主题也是git库，所以需要添加为子模块的形式

5. 修改配置文件 `config.toml`

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

6. 本地构建

   hugo server

   即可在http:127.0.0.1:1313/ 查看到构建后的页面了

7. 构建页面

   hugo 

   即可将网页生成到默认的public子目录里。当然你可以指定一个别的文件夹

   ```shell
   echo 'publishDir = "docs"' >> config.toml
   ```

   即以后生成的网页静态文件都到docs子目录下了。

8. 发布到github.io

   将生成在docs下的网页文件推送到 `[your_username].github.io`库下，稍等一会儿你就可以看到你生成的网页了。



问题：

以上方法是得写好文章，然后将构建好的静态文件更新到github.io库下，即每次都得构建一次，是不是可以有一种，只用将文章写好后，推送到github.io上即可自动化构建然后推送呢？持续交付？

已完成。



优秀的Hugo建站博文

[静态网站构建手册-使用Hugo构建个人博客](https://jimmysong.io/hugo-handbook/)

[Hugo 从入门到会用](https://blog.olowolo.com/post/hugo-quick-start/)

[Hugo 使用问题集](https://www.qd1024.com/post/hugo/)

[从Hexo迁移到Hugo-送漂亮的Hugo Theme主题](https://www.flysnow.org/2018/07/29/from-hexo-to-hugo.html)