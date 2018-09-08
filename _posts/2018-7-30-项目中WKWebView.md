---
layout: post
title: 项目中的WKWebView加载页面跳转失败的解决方案
date: 2018-7-30 17:18:59 +0300
description: 项目中使用了webview加载h5页面，并且需要Webview与JS交互。 # Add post description (optional)
img: wkwebview.jpg  # Add image post (optional)
---

    "Talk is cheap. Show me the code."

                                - Linus Torvalds(Linux 的创始人 Linus Torvalds)

    能说算不上什么，有本事就把你的代码给我看看

##### 使用背景
---
今天在开发中碰到一个场景，自己的WKWebview访问一个外部链接进行三方页面的加载，由于含有http/https两种访问方式，所以首先想到的是
