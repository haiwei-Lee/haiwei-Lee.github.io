---
layout: post
title: 项目中的WKWebView
date: 2018-7-30 17:18:59 +0300
description: 项目中使用了webview加载h5页面，并且需要Webview与JS交互。 # Add post description (optional)
img: wkwebview.jpg  # Add image post (optional)
---

    "Talk is cheap. Show me the code."

                                - Linus Torvalds(Linux 的创始人 Linus Torvalds)

    能说算不上什么，有本事就把你的代码给我看看

##### 使用背景
---
Xcode8发布以后，编译器开始不支持IOS7，所以很多应用在适配IOS10之后都不在适配IOS7了，其中包括了很多大公司，网易新闻，滴滴出行等。因此，我们公司的应用只是支持iOS9（含iOS9）以上。
看了一下项目中使用Webview的地方并不统一，于是就萌生了WKWebView替换原来的UIWebView进行统一
