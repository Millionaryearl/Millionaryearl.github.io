---
title: Build Your Own Blog  - 个性化设置(一)
date: 2016-09-11 14:29:52
tags: Hexo
categories: "Blog"
---

有了基础的博客框架之后，我们就要去做一些个性化的设置了。毕竟同行千千万，内容取胜不太现实，所以咱就剑走偏锋把自己的博客给搞漂亮一些得了。
***新任务获得：美化个人网站-界面*** 

<!--more-->



## 更换主题
### 替换
1. 在 [正面上我][1] 寻找喜欢的主题，这里我们使用 [NEXT][2] 主题（活跃度最好，API也比较全，推荐一哈）
2. 下载下来，保存到`主工程目录下 \themes `文件夹
3. 在 `主工程目录下的_config.yml` 文件里修改 `themes` 键值

 ```
 theme: next //themes文件夹中对应文件夹的名称
 ```

### 选定Scheme
Scheme 是 NexT 提供的一种特性，简单点说呢就是这个主题可以通过改变 `Scheme` 的值来变成三种不同的布局：

 * Muse - 默认 Scheme，这是 NexT 最初的版本，黑白主调，大量留白
 * Mist - Muse 的紧凑版本，整洁有序的单栏外观
 * Pisces - 双栏 Scheme，小家碧玉似的清新

在 **主题配置文件**`(/themes/next/_config.yml)`里修改 `scheme` 键值。

    #scheme: Muse
    #scheme: Mist
    scheme: Pisces
   
### 站点语言
通过修改 **站点配置文件**`(工程主目录下的_config.yml)`，将`language` 设置成所需的语言编码

    language: zh-Hans
    
可选的语言编码如下表:

| Language | Code |
| -------- | ---- |
| English | en |
| 简体中文 | zh-Hans |
| Français | fr-FR |
| Português | pt |
| 繁體中文 | zh-hk 或者 zh-tw |
| Русский язык | ru |
| Deutsch | de |
| 日本語 | ja |
| Indonesian | id |

### 菜单栏

1. 设置菜单项目. 找到**主题配置文件**`(/themes/next/_config.yml)`里`menu`字段，按照如下格式加入菜单项及其文件路径. 
```
    menu_option : folder_directory
    (e.g.)
    categories : /categories
```
2. 设置菜单项目. 注意大部分菜单途径需要用户自己生成，在命令行工程主路径下，  
``` 
 	$ hexo new page "menu_option"
 	(e.g.)
 	$ hexo new page "categories"
```

3. 设置菜单项目. 然后编辑下 `/source/menu_option／index.md`， 大概弄成这样就成了,

	```
    ---
    title: menu_option
    date: 自动生成的
    type: "menu_option"
    comments: false (如果你加了评论的话)
    ---
    e.g.
    ---
    title: categories
    date: 2016-09-18 16:12:40
    type: "categories"
    comments: false
    ---
	```
4. 设置菜单项目名称. 找到**主题对应语言文件**`(/themes/next/languages/your_language_name.yml)`里`menu`字段，按照如下格式加入菜单项名称,

 ```
    menu_option : menu_name
    (e.g.)
    categories: 分类
	
```

5. 设置菜单项目图标. 找到**主题配置文件**`(/themes/next/_config.yml)`里`menu_icons`字段，按照如下格式加入菜单项图标名称。这里的图标名称都是由 [Font Awesome][3] 提供的,
 	
 	```
    menu_option : menu_icon_name
    (e.g.)
    categories: th
 	```

### 设置头像

1. 找到**主题配置文件**`(/themes/next/_config.yml)`里`avatar`字段, 设置图片地址
```
    avatar: /images/avatar.png
```

2. 把你的头像文件命名为 `avatar.png` 然后丢到 `(/themes/next/source/images)`文件夹里


### 颜色字体

这个其实算是最简单的差异化修改了，只要找到`/themes/next/source/css/variables/base.styl`文件里，把对应的颜色和字体改成自己想要的值就可以了，例如作者就修改了

     $black-light  = #336699
     $black-deep   = #660066

## 尾记

至此我们的博客站在界面布局方面就算是大功告成了，其实本期我们的主要工作就是把**主题配置文件**`(/themes/next/_config.yml)`和 **站点配置文件**`(工程主目录下的_config.yml)`的一些属性给配置起来，其他的很多配置工作都是通过修改其中对应的键值实现的，具体的键名解析请 [正面上我](https://hexo.io/zh-cn/docs/configuration.html)。 

欣赏一下，感悟一下，陶醉一下，然后分享给你的小伙伴们吧，一大波崇拜的目光即将到来，嘿嘿嘿。

![][4]

[1]:https://github.com/hexojs/hexo/wiki/Themes
[2]:http://theme-next.iissnan.com
[3]:http://fontawesome.io
[4]:https://cl.ly/0k390e3v3z1v/comic_beautiful.jpg