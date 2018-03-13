---
layout:     post
title:      chrome调试
subtitle:   使用chrome调试android前端页面
date:       2016-12-12
author:     huangqing
header-img: img/post-bg-chrome.jpg
catalog: true
categories: [browser]
tags:
    - chrome
---

# 使用chrome调试android前端页面

[下载Chrome浏览器](http://www.google.cn/intl/zh-CN/chrome/browser/desktop/index.html)

[官方教程：remote-debugging](https://developer.chrome.com/devtools/docs/remote-debugging)

[开源工具Stetho](http://facebook.github.io/stetho/)

## 功能：

安卓远程调试目前支持所有操作系统（Windows,Mac, Linux, and Chrome OS.）中调试，支持：

+  调试站点的页面

+  调试安卓原生App中的WebView

+   实时将安卓设备的屏幕图像同步显示到开发机器。

+  通过端口转发（port forwarding）与虚拟主机映射（virtual host mapping）实现安卓移动设备与开发服务器进行交互调试。


## 调试要求：

+  开发环境安卓桌面版Chrome32+

+  一条USB数据线，连接电脑与移动设备，安装相应机型的[USB驱动](http://developer.android.com/tools/extras/oem-usb.html)

+  如果是调试网页，移动设备需要安装Chrome for Android ，且安卓系统须为Android 4.0+

+  对于APP WebView的调试，需要系统为Android 4.4+ 并且原生应用内的Webview须进行相应的调试声明配置

## 主要的步骤：

### 1.  电脑上安装Android SDK  

[下载 android sdk](http://www.android-doc.com/sdk/)

### 2.  移动设备为Android 4.2及更高版本

android版本低于4.2的系统不支持chrome浏览器调试功能

### 3.  在pc安装最新版本的chrome

[下载Chrome浏览器](http://www.google.cn/intl/zh-CN/chrome/browser/desktop/index.html)

### 4.  WebView调用代码支持调试

调试WebView需要满足安卓系统版本为Android 4.4+已上。并且需要在你的APP内配置相应的代码：

在WebView类中调用静态方法`setWebContentsDebuggingEnabled`，如下：

~~~javascript
    if (Build.VERSION.SDK_INT >=Build.VERSION_CODES.KITKAT) {  
        WebView.setWebContentsDebuggingEnabled(true);    
    }  
~~~

安卓WebView是否可调试并不取决于应用中manifest的标志变量debuggable，如果你想只在debuggable为true时候允许WebView远程调试，请使用以下代码段：

~~~javascript
if (Build.VERSION.SDK_INT>= Build.VERSION_CODES.KITKAT) {  
    if (0 != (getApplicationInfo().flags &=ApplicationInfo.FLAG_DEBUGGABLE))  {
        WebView.setWebContentsDebuggingEnabled(true);
    }  
} 
~~~

### 5.   打开手机USB调试

手机中打开“设置”->"开发人员选项"->"USB调试"

![20150511160350007.png](/images/chrome/947566-f087c3bdf6585d71.png)

![20150511160359055.png](/images/chrome/947566-a0f77d43428d4af2.png)

### 6.  允许usb调试

用USB数据线连接设备，驱动装好连接成功后，你可能会在设备上看到一个弹框请求允许使用这台计算机通过usb调试。

![20150108192728031.jpg](/images/chrome/947566-d21f0cebec64e4cb.jpg)

### 7.  inspect

打开pc侧chrome, 在地址栏中输入[chrome://inspect/#devices](chrome://inspect/#devices)，或者[about:inspect](about:inspect)。选中`discover usb devices`。可以看到我们的手机设备，如下图所示:

![20150511160443825.png](/images/chrome/947566-9615360d896e4534.png)

### 8.   DevTools

如果USB连接成功，这时候我们可以看到移动设备的型号和设备上运行的页面和允许调试的WebView列表。找到需要调试的目标页面，点击inspect即可打开DevTools 。

![20150511160819170.png](/images/chrome/947566-6462a8743609dd5d.png)

### 9.  翻墙

**点击`inspect`时，如果遇到启动了一个白屏界面，说明要翻墙才能使用。**一般情况下，只在第一次使用`inspect`时需要翻墙，以后会缓存在本地。

+ 无条件翻墙的可以尝试修改本机host的方法来访问Google相关服务，host内容参考[google-hosts](https://github.com/txthinking/google-hosts/blob/master/hosts)。

+ 使用[自由翻墙：ChromeGAE 7.2](http://www.anybfans.com/1378.html)

+ 使用360浏览器：安装谷歌访问助手

## 术语解释：

adb：Android Debug Bridge

## 相关资料：

+  [Chrome调试Android应用(Debug)](http://ask.dcloud.net.cn/article/69)

+  [Safari调试iOS应用](http://ask.dcloud.net.cn/article/143)

+  [在安卓设备上使用Chrome远程调试功能](http://wiki.jikexueyuan.com/project/chrome-devtools/remote-debugging-on-android.html#debugging-tabs)


# 在安卓设备上使用 Chrome 远程调试功能

[Google 中国开发者](https://developers.google.cn/)

[chrome devtools document](https://developer.chrome.com/devtools/index)

[goole developers web tools document](https://developers.google.com/web/tools/setup/)

网页内容在移动设备上的体验可能和电脑上完全不同。Chrome DevTools 提供了远程调试功能，这让你可以在安卓设备上实时调试开发的内容。

![remote-debug-banner](/images/chrome/947566-7da7d04a9847a38a.png)

安卓远程调试支持：

+  在浏览器选项卡中调试网站。

+  在原生安卓应用中调试网页内容。

+  将屏幕从你的安卓设备上投影到你的开发机器上。

+  使用端口转发和虚拟主机映射来让安卓设备访问开发使用的服务器。

要开始远程调试，你需要：

+  安装 Chrome 32 或者之后的版本。

+  连接安卓设备用的 USB 线缆。

+  对于通过浏览器调试：安卓 4.0 以上并且安装了 [Chrome for Android](https://play.google.com/store/apps/details?id=com.android.chrome&hl=en)。

+  对于通过应用调试：安卓 4.4 以上并且应用包括[可用于调试的WenView 组件](https://developer.chrome.com/devtools/docs/remote-debugging#debugging-webviews)。

>提示：远程调试需要你电脑端的 Chrome 版本要高于安卓端的版本。想更好地使用此功能，请使用电脑端的 Chrome Canary（Mac/Windows） 或者 Dev channel发行版（Linux）。