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
今天在开发中碰到一个场景，自己的WKWebview访问一个外部链接进行三方页面的加载，由于含有http/https两种访问方式，所以首先想到的是`ATS`。将`AllowsArbitraryLoads`设置为`YES`。神奇的一幕出现了，在iOS10以上的系统，加载页面没有问题，但是切换到iOS9的模拟器上面，发现页面的跳转出现问题（加载一个主链接，内部进行了多次重定向）。

在好奇的驱使下
```
- (void)webView:(WKWebView *)webView didFailNavigation:(WKNavigation *)navigation withError:(NSError *)error{
    NSLog(@"=====%@",[error description]);
}

- (void)webView:(WKWebView *)webView didStartProvisionalNavigation:(WKNavigation *)navigation{
    NSLog(@"====%@",webView.URL.absoluteString);
}

- (void)webView:(WKWebView *)webView didFailProvisionalNavigation:(WKNavigation *)navigation withError:(NSError *)error{
    NSLog(@"%@",[error description]);
}
```
在这三个方法里面进行打印输出，查看具体的失败信息。
```
The certificate for this server is invalid. You might be connecting to a server that is pretending to be “mysite.com” which could put your confidential information at risk.
```
输出含有这样关键日志的错误报告，于是定位到网站证书认证的问题
```
- (void)webView:(WKWebView *)webView didReceiveAuthenticationChallenge:(NSURLAuthenticationChallenge *)challenge completionHandler:(void (^)(NSURLSessionAuthChallengeDisposition disposition, NSURLCredential *credential))completionHandler {
    NSLog(@"Allowing all");
    SecTrustRef serverTrust = challenge.protectionSpace.serverTrust;
    CFDataRef exceptions = SecTrustCopyExceptions (serverTrust);
    SecTrustSetExceptions (serverTrust, exceptions);
    CFRelease (exceptions);
    completionHandler (NSURLSessionAuthChallengeUseCredential, [NSURLCredential credentialForTrust:serverTrust]);
}
```
外加上
```
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

问题完美解决，记录下来，和大家分享
