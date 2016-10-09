---
title: Crawl Web Content - 粗解爬虫解析流程及结果输出
date: 2016-10-08 16:29:15
tags: Python
categories: "Scrapy"
---

通过上一篇博客的工作，我们拥有了一个简单爬虫。但确实有点简陋的过分了，要啥啥没有，所以今天主要就是简单讲解一下爬虫的解析流程与爬取结果的输出形式。
***新任务获得：粗解爬虫解析流程并设置爬取结果的输出***

<!--more-->

## 抓取流程

![][1]
实际上爬虫的工作流程很是直白的。首先找到包含目标信息的网站的网址，写入爬虫文件的`start_urls`参数，做为爬取的起始点。同时输入适当的网址到`allowed_domains`参数，来约束爬取的范围。

第二步是爬取到网站内容，这个内容基本上会以`HTML&CSS`格式呈现，这一步`scrapy`会帮我们做的，通常我们只需要运行爬虫就可以了，应对一个复杂要求(例如：应对网站反爬取设置)可以在`/settings.py`文件里配置爬虫参数。感兴趣的可以[正面上我][3]

第三步是过滤抓取内容，以获得我们需要的信息。这个要求我们在运行爬虫之前制定好过滤规则，然后在运行爬虫时`scrapy`会直接使用

最后一步就是输出了，无论是直接以log形式在命令行里输出，还是保存到文件里，亦或是直接输入数据库都可以.

至此我们今天的任务就明确了，主要是讲解下在第三步和第四步要怎么做。
***任务更新：合理制定爬虫过滤规则与爬虫输出设置***

## 制定爬虫过滤规则

因为每个抓取站点的h5文件结构都不一样，所以爬虫的可重用性比较低。对于不同的抓取目标，我们需要制定不同的抓取语句。一般的制定流程是先查阅目标站的H5结构，进而定制对应的抓取结构，最后编写抓取语句。

### 查阅目标站点H5结构
1. 在命令行里查看
	使用`Scrapy`的`fetch`函数就可以直接现实H5文件结构
	```
	scrapy fetch http://www.biquge.tw/0_972/
    ```
2. 在浏览器里查看
这个么更直接，在浏览器里打开目标地址，然后进入开发者模式，显示网页源文件就可以看到了

### 定制抓取机制
这个么需要一定的H5和CSS基本知识，一点都不懂的可以去[W3CSchool][2]上了解一下。例如我的目标信息是：小说章节的名称与链接地址。在刚才得到的H5结构文件里，查找对应的数据单元：

    <dd> <a style="" href="/0_972/4743476.html">第一千五百九十五章 黑暗现象</a></dd>

可以看到我们需要的信息是存在于`<dd></dd>`标签内的一个`<a>`标签里的。所以我们需要的抓取机制应该是：

* **先筛选出所有的`<dd></dd>`标签**
* **然后抓取之中的`<a>`标签下的`title`和`href`两个属性下的数据**

那有人可能就会问了，明明我们需要的目标信息的最小标签单元是`<a>`标签,为什么要多此一举的先去筛选出上一级的`<dd></dd>`标签？那是因为网页里有很多其他的`<a>`标签，但它们的信息并非小说的章节信息，为了过滤掉这些合规却又不是有效信息的`<a>`标签内容，我们需要加入额外的筛选条件－上一级的`<dd></dd>`标签。
    
    <li><a href="/nweph.html">排行榜单</a></li>
    <li><a href="/quanben/">完本小说</a></li>
    <li><a rel="nofollow" href="/jilu.php">阅读记录</a></li>

### 编写抓取语句

这个抓取语句主要是写在爬虫文件中的"parse"函数里。这里我们打开`fiction/spiders/biquge.py`文件并编辑如下

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

上面的代码很好理解，前两行是引入相关的`scrapy`基础类，第三行是引入我们自定义的数据结构（具体信息可以去上篇博客里查看－定义模型）. 再下来么就是我们爬虫类`biqugeSpider`，头三行的爬虫属性设置也在上篇说过了，直接看到重点`parse`函数。

可以看到这个函数接收到了两个外部参数, `self` 和 `response`。`self`应该就是指爬虫自己，具体有啥用处，作者只能表示今天天气不错，啊哈哈哈。而`response`参数就是上述`抓取流程－第二步`抓取到全部网站内容，它应该是和上述`定制爬虫过滤规则－查阅目标站点H5结构`里看到的内容一致。

接下来就如上述`定制爬虫过滤规则－定制抓取机制`计划的一样，先过滤出所有的`<dd></dd>`标签内容，并建立一个空数组用以将来存储标签对象`Fiction`。再接下来建立一个`for`循环，从每段`<dd></dd>`标签内容里，进一步过滤出`url`和`title`字段信息，并存入新建的`Fiction`对象，加入到`items`结果数组里，同时在命令行里输出`url`和`title`结果。 最后在`for`循环结束后，返回`items`结果数组(这个暂时没有，稍后我们配置结果输出到json文件时会用到)

### 数据提取机制
虽然说知道了这段代码是怎么工作的，但具体怎么写不知道啊，到底怎么样才能从HTML源码中提取数据呢？其实有些库是可以做到的：

* [BeautifulSoup][4] 是在程序员间非常流行的网页分析库，它基于HTML代码的结构来构造一个Python对象， 对不良标记的处理也非常合理，但它有一个缺点：慢。
比如这些
* [lxml][5] 是一个基于 [ElementTree][6] (不是Python标准库的一部分)的python化的XML解析库(也可以解析HTML)。

但伟大的奥斯忒懦夫司机·作者曾经又说过
>作为一个菜鸡程序员，要有菜鸡程序员的尊严，坚决抵制听啊没听过的东西！

所以有啥简单点的东西呢，找找看官网还真有－`Seletors`就可以完美替代，它是`Scrapy`自己提供的数据提取机制，可以通过特定的`XPath`和`CSS`表达式来选择 HTML文件中的某个部分。本篇作者就是用的这种提取机制，例如：

    response.xpath('//dd') //提取网页内容里的所有<dd></dd>标签内容
    ...
    chapter.xpath('a/@href').extract() //提取chapther中<a>标签的@href属性
    chapter.xpath('a/text()').extract() //提取chapther中<a>标签的text值
更多的`Seletor`提取讲解，可以[正面上我][7]

## 设置爬虫输出设置

至此我们的爬虫可以说已经是大功告成，但数据抓是抓到了，怎么输出来用呢。大概是有三个方向：

### 直接以log形式在命令行里输出
这个最简单了，只要在爬虫文件里的`parse`函数里，使用`print`命令把相应的信息给打出来就可以了

    print item['url'], item['title']
### 保存到文件里
半自动的呢，可以在启动爬虫时，加入输出参数 `-o outputFileName`。需要注意的是`Scrapy`默认支持四种格式:`JSON`, `JSON lines`, `CSV`, `XML`

    scrapy crawl fiction -o result.json 
    // scrapy crawl spiderName -0 outputFileName
    
正规一点的呢，需要修改`/pipelines.py`文件如下：
    
    import json  
    import codecs  
    import re

	class FictionPipeline(object):

    def __init__(self):  
        self.file = codecs.open('cn_zhan.json', 'wb', encoding='utf-8') 

    def process_item(self, item, spider):
        if item['title']:
            found = re.match('\S* \S*', str(item['title']))
            if found:
                print '---------', item['title']
                line = json.dumps(dict(item), ensure_ascii=False) + "\n" 
                self.file.write(line)   
                return item
            else:
                print "+++++++++ invalid chapter found ++++++++++++++++"

    def spider_closed(self, spider):
    	self.file.close()

首先是在初始化方法里新建一个叫做`cn_zhan.json`的文件用以收纳抓取的信息，并约定它是`utf-8`格式（用来显示中文，同时打开它准备写入。第二步在`process_item`里我们根据正则表达式过滤掉不合规的`item`，并把合规的`item`（章节信息）转化成`json`语句存入之间申明的`cn_zhan.json`文件里。最后关闭这个文件。

定义好`/pipelines.py`文件后，我们还需要在`/settings.py`文件里启用刚定义的输送规则：

    ITEM_PIPELINES = {
    	#'fiction.pipelines.DuplicatesPipeline': 100,
    	'fiction.pipelines.FictionPipeline': 300,
	}
注意在`/pipelines.py`文件里我们可以申明多个输送规则，例如这里作者还申明了一个去重原则。同时在`/settings.py`文件里启用时，`Scrapy`会依照数字从低到高的顺序，通过pipeline，通常将这些数字定义在0-1000范围内

### 录入到数据库
这个因为使用的数据库种类不同需要不同的配置，所以就先不讲解了，感兴趣的可以自己去狗哥一下

## 尾声
折腾了这么久，激动人心的时刻终于到来了，打开命令行开始运行爬虫，完成之后你就可以在你的文件夹里发现抓取的结果文件了:`cn_zhan.json`

    {"url": ["/0_972/603364.html"], "title": ["第一章 太阳消失"]}
    {"url": ["/0_972/603365.html"], "title": ["第二章 全球恐慌"]}
    {"url": ["/0_972/603366.html"], "title": ["第三章 黑暗时代"]}
    {"url": ["/0_972/603367.html"], "title": ["第四章 怪物降临"]}
    ...
完美，破费，至此我们的爬虫应该可以说是初步成型了！诸君昌隆！
![][8]


[1]:https://cl.ly/3h3F0j1d1P0s/python_2_1.png
[2]:http://www.w3school.com.cn
[3]:https://doc.scrapy.org/en/latest/topics/settings.html
[4]:http://www.crummy.com/software/BeautifulSoup/
[5]:http://lxml.de/
[6]:http://docs.python.org/library/xml.etree.elementtree.html
[7]:http://scrapy-chs.readthedocs.io/zh_CN/latest/topics/selectors.html#topics-selectors-ref
[8]:https://cl.ly/3S1k1Z3k3q1i/comic_success_kid.jpg
