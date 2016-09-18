---
title: Swift Notes - 阿里云推送SDK
tags: Swift
---

新项目里的消息推送功能，公司技术部开会讨论后决定让 极光，百度，aws都歇菜，取而代之的是尝试 使用阿里云推送SDK。所以今天就简单记录下调试过程。新任务获得：使用阿里云推送SDK实现消息推送功能～

<!--more-->

嗯常规套路先看官方文档 [正面上我](https://help.aliyun.com/document_detail/30072.html?spm=5176.doc30071.6.156.YoX0P8) 。 嗯，很详细，很耐斯，很桥豆麻袋？库文件是OC写的？？？

![](https://cl.ly/hRAS/ExcuseMe.jpeg)

坑爹啊，哥的项目都是swift写的啊。呃。。。。。。。。。好吧，也许加一个桥接就可以了。任务变更：桥接阿里云推送SDK（OC版）到Swift工程里，而后实现消息推送功能～

![](https://cl.ly/hREs/challenge-accepted-meme.jpg)



## Ready to work
所谓工欲善其事，必先利其器。而且消息推送功能本身就是配置打过逻辑代码的功能，所以我们先要把准备工作做好。

---------------------------------------

### Step.1 - 准备certificate文件

由于消息推送功能的实现，涉及到Apple的官方资源（感兴趣的同志们可以自行去谷歌APNS），所以需要准备特别的证书文件：***development certificate x1***， ***distribution certificate x1***。然后这两个文件具体怎么获得呢，请 [正面上我](https://help.aliyun.com/document_detail/30071.html?spm=5176.doc30072.6.155.ItR8Ib) 。


### Step.2 - 获得AppKey, AppSecret 

这个appkey 和appsecret 是做啥的呢，嗯简单说就是这两个字串是阿里云用来标记你的app的，万一推错了就不好嘛，稍后我们会用点。那么问题又来了这两个字串那里搞呢？

支线任务获得：寻找NPC -公司的推送服务后台开发人员，交付 ***development certificate x1***， ***distribution certificate x1***后，获得 ***AppKey x1***, ***AppSecret x1***.

### Step.3 - 配置App

配置App这个就简单了，打开你的项目代码。先把开发团队调到你们的公司

![](https://cl.ly/hQyC/aliyun_1.jpeg)

然后打开 Post Notifications 功能

![](https://cl.ly/hRCF/aliyun_2.png)
### Step.4 - 引入阿里云SDK

下载好压缩包打开，获得四个库文件，然后全部拖到你的项目工程里去。再然后么把 build settings 里的 Enable Bitcode 给关了。最后再转到 Build Phases 里面的 Link Binary With libraries， 加入四个依赖的系统库: libz.tbd，libresolv.tbd，CoreTelephony.framework，SystemConfiguration.framework 。

---------------------------------------

好啦，到此基本上所有的准备工作都做好了。接下来我们就可以开始写代码了。P.S. ~~其实上面的都可以在阿里云文档里看到，哥只是拿来凑字的~~～～

## Core Work

### Step.1 - 桥接
先建立一个 Header File，命名为 YourProjectName-Bridging-Header.h 。 

![](https://cl.ly/hRGV/aliyun_3.jpeg)

然后呢去到 Build Setting里，找到 Objective-C Bridging Header 填入刚才的文件名

![](https://cl.ly/hRAR/aliyun_4.jpeg)

最后在刚才的桥接文件里，引入阿里云推送的库` #import <CloudPushSDK/CloudPushSDK.h>`, 这样就可以用SDK里的方法了。

### Step.2 - 配置推送
先在APP启动时调用配置方法。

![](https://cl.ly/hRN5/aliyun_5.png)            
```

配置推送注册方法

![](https://cl.ly/hR4P/aliyun_6.png)

推送处理方法

![](https://cl.ly/hR86/aliyun_7.png)

最后运行程序，看到如下结果就可以去交任务啦。

![](https://cl.ly/hQrt/aliyun_8.png)

完结，鼓掌，撒花～～

![](https://cl.ly/hQsX/PrettyGood.png)
