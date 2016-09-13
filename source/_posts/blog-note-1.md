---
title: Blog Notes -使用 Hexo & Github 建立个人博客站
tags: Blog
---

一直打算搞一个自己的技术博客站，比起用什么简书啊，CSDN的第三方平台，直接高冷的丢出去一个自制的博客站，简直就是装比于无形，想想就带感好吧。***新任务获得：部署个人网站***
	

<!--more-->

嗯常规套路先看炼成书, `通用合成公式 :－ 公网域名 ＋ 服务器 ＋ 网站代码 ＝ 个人网站`。呃，公网域名么 [狗爹](https://www.godaddy.com/) 上或许能找到便宜的。服务器，呃，[AWS](https://aws.amazon.com)好像有点贵，[aliyun](告www.aliyun.com)凑合吧。网站代码，呃，不就是 `H5+CSS+JS/AJAX` 么，小意思。。。。。。个屁。哥是写Swift的，自己去搞这些web相关的，要搞死哥啊。再翻翻炼成书: `黑暗合成公式 :- hexo（网站代码） + github（公共域名 & 服务器） = 个人网站` 

![](https://cl.ly/022C2w20262o/commic_wow.jpg)

就你了，任务更新：*** 使用 [Hexo](https://hexo.io/docs/) 和 [Github](https://github.com) 制成个人博客。 ***

## 准备工作

### Step.1 开发环境

 1. **Node.js** 	[安装指南](https://nodejs.org/en/download/package-manager/#osx)
 2. **Git** 		[安装指南](https://git-scm.com/book/zh/v1/起步-安装-Git)
 3. 运行如下命令不报错即配置成功。
 	```
	$ npm -v
	```
	```
	$ git --version
	```


### Step.2 GitHub

 1. 新建一个代码仓，命名为`yourname.github.io`
 2. 开启 **gh-pages** 功能
 	* 开启 Reposity **Setting** 页面如下
 	* 点击 **Automatic page generator**
 	
 ![](https://cl.ly/240P2i1D0b3j/hexo_1.png)
 
 3. 能够正常访问网址: `yourname.github.io` ，即配置成功。***这个地址将成为你的博客网址（可以修改）***
 ![](https://cl.ly/2L1R2X0e2j0U/comic_brilliant.jpg)
 
 
### Step.3 Hexo

 1. 新建一个工作目录，打开命令行并切换到新建的工作目录途径
 2. 安装 **Hexo**
 	```
	$ npm install -g hexo-cli
	```
 ---------------------------------------
## 建站
 
### Step.1 新建一个网站
1. 在命令行里执行
	```
	$ hexo init <folder>
	$ cd <folder>
	$ npm install	
	```
### Step.2 本地测试
1. 在命令行里执行
	```
	$ hexo g
	$ hexo s
	```
如果能看到提示：``INFO  Hexo is running at http://0.0.0.0:4000/. Press Ctrl+C to stop.``你就可以去浏览器里打开``http://localhost:4000/``，欣赏你的个人博客了

![](https://cl.ly/1o0m2K121V18/hexo_2.png)

### Step.3 Github部署

1. 需要为自己配置身份信息，打开命令行，执行
```
git config --global user.name "yourname"
git config --global user.email "youremail"
```

2. 去工程目录里找到 `_config.yml` 文件，修改下列属性
```
deploy:
  type: git
  repo: git@github.com:yourname/yourname.github.io.git
  branch: master
```

3. 在命令行里执行
```
$ hexo clean
$ hexo d
```

如果能看到提示 ``INFO  Deploy done: git`` 你就可以去浏览器里打开 ``yourname.github.io``，继续欣赏你的个人博客了。

![](https://cl.ly/441e3k3O1r2G/commic_yeah.jpg)


## 可能遇到的问题

### Q1: 命令行总是指令错误
1. 首先你的确保命令行的路径是你的工作目录（工程文件夹）的途径，在命令行里输入下列命令确认
```
$ pwd
```
2. 其次确保 `node.js` & `git` & `hexo` 确实安装成功了，详见上述准备环节

### Q2: Github部署时，总是提示 “Permission Denied”
这个是因为的Github的SSH连接授权有问题，需要确认本地机器上的ssh公钥与Github上的私钥是匹配的。如果实在无法确认的话，就直接去换套新的吧（作者就折腾了半天），[正面上我](https://help.github.com/articles/generating-an-ssh-key/) 。



## 进阶

有了基本的架子之后，我们想要定制一些专属功能

### Advance.1 修改公网网址
这个其实是属于最基本的网站信息修改，此类修改只要找到工程目录里的 `_config.yml` 文件，然后修改对应的键值就可以了，具体的键名解析如下，[正面上我](https://hexo.io/zh-cn/docs/configuration.html)

### Advance.2 发布博客
1. 在命令行里执行
```
$ hexo new [layout] <title>
```
2. 直接在工程目录的 `/source/_posts/` 下，新建 newblog.md 文件
3. 编辑内容
4. 在命令行里执行
```
$ hexo clean
$ hexo d
```

### Advance.3 更换主题
1. 在 [正面上我](https://github.com/hexojs/hexo/wiki/Themes) 寻找喜欢的主题
2. 下载下来，保存到工程目录里的 `themes` 文件夹下
3. 在 `_config.yml` 文件里修改
```
 Extensions
## Plugins: http://hexo.io/plugins/
## Themes: http://hexo.io/themes/
theme: landscape //themes文件夹中对应文件夹的名称
```

## 待续
**Hexo** 总体上来说还算是个挺不错的框架的，能玩的东西很多，插件，主题等等等等，想要学习更多的可以去 [Hexo官网](https://hexo.io/zh-cn/)看看。

完结，撒花，鼓掌～～～