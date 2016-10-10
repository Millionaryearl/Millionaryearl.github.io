---
title: Crawl Web Content - 环境搭配与基础爬虫
date: 2016-09-29 11:39:40
tags: Python
categories: "Scrapy"
---

写这篇博客呢，主要是为了响应之前打算自己搞个小说阅览App的想法。作为整个项目的一部分，我们需要自己利用爬虫工具去爬取网上的小说内容。
***新任务获得：制作爬虫并爬取网络内容***

<!--more-->

打开炼成书，查询 网络爬虫 项目：
`scrapy／ pyspider/ beautifulSoup = 爬虫`
这三个都是比较成熟的python爬虫框架。[scrapy][1] 是其中最出名的，[pyspider][2] 是个国人大神写的，[beautifulSoup][3] 老实说但就爬虫功能并不完整，主要是还能干点别的。至于你要是问作者为啥推荐的都是python的，php不能做爬虫么？那肯定是可以的啊，只不过作者太菜了，还无力使用世界上最伟大的语言好吧。
![][4]

***任务更新：使用Scrapy爬取网络内容***

## 配置 Scrapy
### 准备工作
首先我们肯定是把`Python`给安装一下的么，不会的伙计们请[正面上我][5],在命令行里输入`python`，能看到如下就可以了

    Python 2.7.12 (default, Jun 29 2016, 14:05:02) 
    [GCC 4.2.1 Compatible Apple LLVM 7.3.0 (clang-703.0.31)] on darwin
    Type "help", "copyright", "credits" or "license" for more information.
    >>> 
    
### 安装
由于`Scrapy`需要 C 语言编译器及其开发前缀。 在 OSX 里这些都是由`Apple Xcode development tool`提供的， 在命令行里输入：

    $ xcode-select --install
    
然后就是使用`pip`指令，安装`Scrapy`了，在命令行里输入：

    $ pip install Scrapy

再输入`Scrapy`，能看到如下结果就可以了

    Scrapy 1.1.3 - no active project
    Usage:
    	scrapy <command> [options] [args]
    ...
至此么，前期准备工作就都弄好了. 这里我们设定将要爬取的网络内容为这个[小说的章节目录信息][6]P.S.其他系统的安装教程请，[正面上我][7]
***任务更新：使用Scrapy爬取小说的章节目录信息***
## 爬虫制作
### 创建项目
伟大的奥斯忒懦夫司机·作者曾经说过
    
>在使用爬虫之前，你必须先拥有一个爬虫。而在拥有爬虫之前，你需要先有个放置爬虫的地方。

所以我们需要先新建一个Scrapy项目。打开命令行，输入命令：

    $ scrapy startproject fiction
输入命令`cd yourProjName`进入工程目录后，你就能看到自动创建的主体文件

+ `scrapy.cfg`： 项目的配置文件
+ `fiction/`：项目的python模块。（我们代码工作主要在这里面）
+ `fiction/items.py`： 定义抓取的模型
+ `fiction/pipelines.py`： 模型管道文件.
+ `fiction/settings.py`： 爬虫配置文件.
+ `fiction/spiders/`： 放置爬虫代码的目录.

### 定义模型
*Item*是用于承载爬取到的数据的最小容器。和常规的ORM一样，你需要先创建一个`scrapy.Item`的类，然后定义你需要的类属性。比如对于小说的章节目录，我们需要知道每章的名字与链接地址，所以设定如下：

    from scrapy.item import Item, Field
    class FictionItem(Item):
    	title = Field()
    	url = Field()

### 编写爬虫
一个项目里可以拥有多个爬虫，输入命令`$ scrapy genspider -l` 我们就能看到`Scrapy` 提供给我们的四种基本的模版：
    
    Available templates:
    	basic
    	crawl
    	csvfeed
    	xmlfeed
今次我们就直接使用`genspider`命令创建最基础的`basic`模版了，输入命令：
    
    scrapy genspider biquge www.biquge.tw
    //formule
    scrapy genspider -t crwal exmaple example.com

这时候去到`/spiders`文件夹下，就可以看到这个名为`biquge`的爬虫文件了

    # -*- coding: utf-8 -*-
    import scrapy
    class biqugeSpider(scrapy.Spider):
    	name = "biquge"
    	allowed_domains = ["http://www.biquge.tw/"]
    	start_urls = (
        	'http://www.biquge.tw/0_972/',
    	)

    	def parse(self, response):
        	pass
 
打开爬虫文件，这里我们可以看到三个主要属性和一个主要方法：
 
 + name : 爬虫的名字与唯一标识，不可以和其他爬虫重复，
 + allowed_domains : 允许爬取的域名
 + start_urls : 启动爬取的url列表
 + parse(): 解析抓到网页

粘贴如下代码到`fiction/spiders/biquge.py`文件中：
 
 ```
 # -*- coding: utf-8 -*-  

from scrapy.spiders import Spider
from scrapy.selector import Selector
from fiction.items import FictionItem

class BiqugeSpider(Spider):
    name = "biquge"
    allowed_domains = ["http://www.biquge.tw/"]
    start_urls = (
        'http://www.biquge.tw/0_972/',
    )

    def parse(self, response):
        chapters = response.xpath('//dd')
        items = []

        for chapter in chapters:
            item = FictionItem()
            item['url'] = chapter.xpath(
            	'a/@href').extract()
            item['title'] = chapter.xpath(
            	'a/text()').extract()
            items.append(item)

            print item['url'], item['title']

        return items
 ```
 
### 开始爬取
终于可以开始爬数据了，在命令行里输入命令：

    $ scrapy crawl biquge -o result.json
    
![][8]

命令行里你应该能看到如下的结果：

    2016-09-29 16:56:09 [scrapy] INFO: Scrapy 1.1.3 started (bot: fiction)
    2016-09-29 16:56:09 [scrapy] INFO: Optional features available: ...
    2016-09-29 16:56:09 [scrapy] INFO: Overridden settings: {}
    2016-09-29 16:56:09 [scrapy] INFO: Enabled extensions: ...
    2016-09-29 16:56:09 [scrapy] INFO: Enabled downloader middlewares: ...
    2016-09-29 16:56:09 [scrapy] INFO: Enabled spider middlewares: ...
    2016-09-29 16:56:09 [scrapy] INFO: Enabled item pipelines: ...
    2016-09-29 16:56:09 [scrapy] INFO: Spider opened
    ...
    2016-09-29 16:56:09 [scrapy] INFO: Closing spider (finished)
    2016-09-29 16:56:16 [scrapy] INFO: Stored json feed (1622 items) in: result.json

然后打开你的工作目录，你会发现多了一个`result.json`文件。打开该文件你就可以看到本次爬取的结果了－小说的章节目录：

    [
    {"url": ["/0_972/603364.html"], "title": ["\u7b2c\u4e00\u7ae0 \u592a\u9633\u6d88\u5931"]},
    {"url": ["/0_972/603365.html"], "title": ["\u7b2c\u4e8c\u7ae0 \u5168\u7403\u6050\u614c"]},
    {"url": ["/0_972/603366.html"], "title": ["\u7b2c\u4e09\u7ae0 \u9ed1\u6697\u65f6\u4ee3"]},
    {"url": ["/0_972/603367.html"], "title": ["\u7b2c\u56db\u7ae0 \u602a\u7269\u964d\u4e34"]},
    {"url": ["/0_972/603368.html"], "title": ["\u7b2c\u4e94\u7ae0 \u602a\u7269\u9000\u907f"]},
    ...
    
## 尾声
呃，总体上来说这个基础爬虫就算是完成了，虽然说有一大堆问题，什么章节名乱码啊，爬取数据的解析看不懂啊，pipeline, setting文件怎么用没说啊，但至少咱这个爬虫跑起来了不是，而且也抓到了一堆玩意儿是吧。所以今次就这么多，至于那些坑么，作者会在续集里填上的。最后惯例，诸君昌隆～
![][9]

[1]:https://scrapy.org/
[2]:https://www.crummy.com/software/BeautifulSoup/
[3]:https://github.com/binux/pyspider
[4]:https://cl.ly/0Q2e06242H0G/comic_hah.jpg
[5]:http://www.runoob.com/python/python-install.html
[6]:http://www.biquge.tw/0_972/
[7]:http://scrapy.readthedocs.io/en/latest/intro/install.html
[8]:https://cl.ly/0G3e1B16362x/comic_puke.gif
[9]:https://cl.ly/3E1B0x3d1Q3L/comic_disaster_girl.jpg