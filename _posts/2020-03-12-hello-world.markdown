---
layout:     post
title:      "Prologue"
subtitle:   "Hello World | My Very First Post""
date:       2020-03-12 12:00:00
author:     "AH"
header-img: "img/post-bg-2015.jpg"
tags:
    - Prologue
---


## 前言

开博客的想法由来已久了，起因是每次做大作业的时候，发现自己总在反复调试同一个bug，却不记得上次是怎么解决的。很大原因是因为自己只顾照搬google却不理解其原理。再加上换了一次电脑，感受到把数据搬来搬去实在是太麻烦的事情，不如找个靠谱的地方把吃过的这些亏收集起来——所以本博客就诞生啦。

当然，搭博客的过程也不是那么一帆风顺的。

由于自己太菜又比较懒，自己搭肯定是不现实的，肯定要借助现成的轮子。一开始瞄上了Github自带的模板，发现功能太单一了，遂放弃。后来看到[Huxpro大神](http://huangxuan.me/)的模板，太心动了。尽管如此，借轮子仍然碰到不少坑，在这里记录一下。

---

## 正文

Fork完之后，登录博客，发现CSS都失效了，打开控制台只看到一串红色。第一条是这样的：

> Mixed Content: The page at 'https://abigailhua.github.io/' was loaded over HTTPS, but requested an insecure stylesheet 'http://cdn.staticfile.org/font-awesome/4.2.0/css/font-awesome.min.css'. This request has been blocked; the content must be served over HTTPS.

所以也就是说应该把http换成https，但是找来找去找不到该改哪里。后来通过[issue](https://github.com/Huxpro/huxpro.github.io/blob/master/_includes/head.html)解决了。

同样的问题还出现在post中，需要修改'/\_layouts/post.html'中的'http://cdn.bootcss.com/anchor-js/1.1.1/anchor.min.js'

然而刷新后仍然是老样子，再看报错是找不到'huxblog-boilerplate/css/bootstrap.min.css'，'huxblog-boilerplate/css/hux-blog.min.css'和'huxblog-boilerplate/css/syntax.css'。这个'huxblog-boilerplate'就很可疑，再仔细看之前那个'head.html'，找到了'prepend: site.baseurl'，于是去'\_config.yml'，把'site.baseurl'改成了''，刷新博客，终于能看到美貌的CSS了。

至于'\_config.yml'中其他一些基本信息的配置就不赘述了。

---

待更新：鼠标悬停小标题左侧出现#，理想结果应该是一个🔗符号，不知道怎么改

