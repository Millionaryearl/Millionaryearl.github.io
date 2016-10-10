---
title: Build Your Own Blog  - 个性化设置(三)
date: 2016-9-25 11:10:40
tags: Hexo
categories: "Blog"
---

最近有小伙伴反映，啊那个兄台啊，你这个博客还算阔以，就是访问起来也忒麻烦了。百度和狗哥上找不捉，这个网址这么难记，小伙伴们不能日日瞻仰，很是为难呀。
***新任务获得：配置SEO***

<!--more-->

## 搜索引擎优化
### 修改公网网址
这个其实还是属于前篇里说的基础键值配置，找到主工程目录里的 `_config.yml` 文件，然后修改

    url: https://millionaryearl.github.io（改成你的域名）
    
至于域名购买么也简单得很，[狗爹][1] 或者 [阿里云][2] 上买一个好了么

## 提交狗哥
### 确认blog是否被收录。 
打开[狗哥搜索引擎][3]，输入 `site:yourwebaddress`. 要有结果么就继续，没有么就要去查查你的网站是否有部署问题了

### 验证网站
通过验证网站，可以证明你是该域名的拥有者，可以做为站长管理自己的网站－添加子节点啊，查流量啊，访问量啊什么的。
前往 [狗哥站长][4] 登陆后开始验证。
![][5]

### 选择验证方式
验证方式有四种－ ***HTML标记***，***域名提供商***， ***Google Analytics(分析)*** 和 ***Google 跟踪代码管理器***。 这里我们使用第一种
![][6]

按照狗哥的要求，下载验证文件后，传到github里，再去浏览器里打开一下，那个验证按钮应该就亮了。都弄好之后，你就能看到自己的网站了
![][7]

### 上传sitemap
简单点说，sitemap文件就是你的站点地图，做为引索可以很方便的把你的网站内容的组织架构告知狗哥和其他搜索引擎。至于这个sitemap从哪里来么，我们需要

1. 安装插件
	```
    npm install hexo-generator-sitemap --save
   ```
2. 在 **主题配置文件**`(/themes/next/_config.yml)`里添加
   ```
   sitemap:
	path: sitemap.xml
   ```
3. 然后`hexo g`一下，你就能在工程主目录下看到 `sitemap.xml`文件了
4. 最后回到狗哥站长的界面里，添加`sitemap.xml`的文件途径
![][8]

### 完成
至此我们的工作就算完成了，等待差不多一天之后，你就可以在狗哥搜索你的博客名了，基本上第一个就是你的博客站啦
![][9]

## 提交百度
这个因为github把百度爬虫给墙了，所以上面的路径对于百度走不通(会卡在网站验证那一步)。解决起来有点绕，核心思想是，通过cdn或者镜像托管，让百度爬虫可以抓取到我们的博客站。具体的实践攻略博主最近有点忙就先鸽了

## 尾记

至此，我们的博客站主体配置工作就算告一段落了，剩下的活儿就写写博客了。所以hexo篇，至此暂时完结，感觉兴趣的伙计可以给我留言。诸君，武运昌隆！
![][10]

[1]:https://www.godaddy.com/
[2]:https://www.aliyun.com
[3]:https://www.google.com
[4]:https://www.google.com/webmasters/tools/home?hl=zh-CN
[5]:https://cl.ly/0o2L3j1u2O2E/hexo_4_verifyWebHost.png
[6]:https://cl.ly/0n3I3h1F2a0k/hexo_4_2.png
[7]:https://cl.ly/0p0R2q3p3X3F/hexo_4_3.png
[8]:https://cl.ly/2y3G2X1h1N1B/hexo_4_4.png
[9]:https://cl.ly/0q2S3d2a1P1K/hexo_4_5.png
[10]:https://cl.ly/03081Q2b3D1L/comic_lol.gif