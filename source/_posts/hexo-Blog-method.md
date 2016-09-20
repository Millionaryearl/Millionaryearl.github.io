---
title: Build Your Own Blog  - 个性化设置(二)
tags: Hexo
categories: "Blog"
---

主题相关的界面工作完成之后，这个博客总算是有点样子。但是我看来看去，赶脚似乎大概眉笔好像有那么一点点简陋的哇。所以这一期我们打算看看有哪些附属功能可以加到博客上。
***新任务获得：美化个人网站-附属功能*** 

<!--more-->

## 功能组件
### 评论模块
有的theme可能带有评论，而有的没有。可选的插件有 [***DISQUS***](https://disqus.com) （更简洁偏国外） 和 [***多说***](http://duoshuo.com)（更社交偏国内）, 这里我们选用 **DISQUS**

1. 在**DISQUS**上注册账号
2. 在**DISQUS**上注册一个网站,得到网站的shortname
3. 在工程目录里找到`_config.yml`，加入如下代码

```
# Disqus
disqus_shortname: shortname

```
4. 更多评论样式设置或者相关疑问，[正面上我](http://morris821028.github.io/2014/04/12/web/hexo-comment/)


### 打赏模块
这个虽然说是正常人都不太会用的废物功能，但万一呢，是吧。万一又个慧眼独具，目光深邃的好汉看出来本人骨骼精奇，死乞白赖的非要给我打上呢是吧，所以本着用户至上的原则，咱还是加上这个模块吧

首先去看下`/themes/your_theme/layout/macro`途径下有没有 `reward.swig`文件，没有的话就新建，内容如下

```
{% if theme.alipay or theme.wechatpay %}
  <div style="padding: 10px 0; margin: 20px auto; width: 90%; text-align: center;">
    <div>{{ theme.reward_comment }}</div>
    <button id="rewardButton" disable="enable" onclick="var qr = document.getElementById('QR'); if (qr.style.display === 'none') {qr.style.display='block';} else {qr.style.display='none'}">
      <span>赏</span>
    </button>
    <div id="QR" style="display: none;">
      {% if theme.wechatpay %}
        <div id="wechat" style="display: inline-block">
          <img id="wechat_qr" src="{{ theme.wechatpay }}" alt="{{ theme.author }} WeChat Pay"/>
          <p>微信打赏</p>
        </div>
      {% endif %}
      {% if theme.alipay %}
        <div id="alipay" style="display: inline-block">
          <img id="alipay_qr" src="{{ theme.alipay }}" alt="{{ theme.author }} Alipay"/>
          <p>支付宝打赏</p>
        </div>
      {% endif %}
    </div>
  </div>
{% endif %}
```
然后看下同路径下的`post.swig`文件, 确保在`<footer class="post-footer">`之前有这么一段

```
  	{% if not is_index %}
        {% include 'reward.swig' %}
      {% endif %}
    </div>
```

再后去**站点配置文件**`(工程主目录下的_config.yml)`，设置如下键值对

```
# Donate 文章末尾显示打赏按钮
reward_comment: 我知道是不会有人点的，但万一有人想不开呢？
wechatpay: https://cl.ly/3W3I3O3t1622/wexinpay.JPG
alipay: https://cl.ly/3t1O403j2P1F/alipay.JPG
```

最后记得把那两个二维码的图片地址换成你自己的，不然就算你骨骼精奇，人也是把钱汇给我了。。。当然你要真倔强不换，我也是很欢迎滴。
![](https://cl.ly/0a0n3y3t3136/comic_spiderman.jpg)

### RSS开启

这RSS开启了之后么，就可以方便别人订阅你的博客了，要装上也挺简单。先去命令行里主工程目录下运行

    $ npm install hexo-generator-feed --save
然后去**站点配置文件**`(工程主目录下的_config.yml)`里配置一下. P.S.似乎`Next`主题可以跳过这步，因为在**主题配置文件**`(/themes/next/_config.yml)`里，已经设好了。
```
feed:
    type: atom
    path: atom.xml
    limit: 20
```

### 社交连接

去到**主题配置文件**`(/themes/next/_config.yml)`里，先找到`social`键，按如下格式添加键值对

    social_name: link_address
    e.g.
    GitHub: https://github.com/Millionaryearl
    Weibo: http://weibo.com/2334525960/profile?topnav=1&wvr=6&is_all=1
    Personal: http://dukewei.typify.io
    
然后去给对应的连接加上图标，这个图标和上篇里菜单项目图标一样，也是由 [Font Awesome](http://fontawesome.io) 提供的

    social_name: icon_name
    (e.g.)
    GitHub: github
    Twitter: twitter
    Weibo: weibo
    Personal: home
    
### 结尾样式-版权说明

在主工程目录下，新建一个名为`scripts`的文件夹，在其中，新建一个AddTail.js脚本文件，内容如下

```
// Filename: AddTail.js

// Add a tail to every post from tail.md
// Great for adding copyright info

var fs = require('fs');

hexo.extend.filter.register('before_post_render', function(data){
	if(data.copyright == false) return data;
	
	// Add seperate line
	data.content += '\n___\n';
	
	// Try to read tail.md
	try {
		var file_content = fs.readFileSync('tail.md');
		if(file_content && data.content.length > 50) 
		{
			data.content += file_content;
		}
	} catch (err) {
		if (err.code !== 'ENOENT') throw err;
		
		// No process for ENOENT error
	}

  	// 添加具体文章链接, 不需要去掉即可
	var permalink = '\n本文链接：' + data.permalink;
	data.content += permalink;
  
	return data;
});
```
然后在工程主目录下新建一个`tail.md`文件，其中写上你的博客结尾内容，比如作者就写了下版权的破事儿

### 背景

感觉用 ***NEXT*** 的博主特别多，所以咱要稍微搞些不一样的东西，比如换换背景什么的。
首先去`/themes/next/source/js/src`路径下新建你的样式文件，例如`particle.js`

    !function(){function n(n,e,t){return n.getAttribute(e)||t}function e(n){return document.getElementsByTagName(n)}function t(){var t=e("script"),o=t.length,i=t[o-1];return{l:o,z:n(i,"zIndex",-1),o:n(i,"opacity",.5),c:n(i,"color","0,0,0"),n:n(i,"count",99)}}function o(){c=u.width=window.innerWidth||document.documentElement.clientWidth||document.body.clientWidth,a=u.height=window.innerHeight||document.documentElement.clientHeight||document.body.clientHeight}function i(){l.clearRect(0,0,c,a);var n,e,t,o,u,d,x=[w].concat(y);y.forEach(function(i){for(i.x+=i.xa,i.y+=i.ya,i.xa*=i.x>c||i.x<0?-1:1,i.ya*=i.y>a||i.y<0?-1:1,l.fillRect(i.x-.5,i.y-.5,1,1),e=0;e<x.length;e++)n=x[e],i!==n&&null!==n.x&&null!==n.y&&(o=i.x-n.x,u=i.y-n.y,d=o*o+u*u,d<n.max&&(n===w&&d>=n.max/2&&(i.x-=.03*o,i.y-=.03*u),t=(n.max-d)/n.max,l.beginPath(),l.lineWidth=t/2,l.strokeStyle="rgba("+m.c+","+(t+.2)+")",l.moveTo(i.x,i.y),l.lineTo(n.x,n.y),l.stroke()));x.splice(x.indexOf(i),1)}),r(i)}var c,a,u=document.createElement("canvas"),m=t(),d="c_n"+m.l,l=u.getContext("2d"),r=window.requestAnimationFrame||window.webkitRequestAnimationFrame||window.mozRequestAnimationFrame||window.oRequestAnimationFrame||window.msRequestAnimationFrame||function(n){window.setTimeout(n,1e3/45)},x=Math.random,w={x:null,y:null,max:2e4};u.id=d,u.style.cssText="position:fixed;top:0;left:0;z-index:"+m.z+";opacity:"+m.o,e("body")[0].appendChild(u),o(),window.onresize=o,window.onmousemove=function(n){n=n||window.event,w.x=n.clientX,w.y=n.clientY},window.onmouseout=function(){w.x=null,w.y=null};for(var y=[],s=0;m.n>s;s++){var f=x()*c,h=x()*a,g=2*x()-1,p=2*x()-1;y.push({x:f,y:h,xa:g,ya:p,max:6e3})}setTimeout(function(){i()},100)}();
 

然后在`/themes/next/layout`路径下的`_layout.swig`文件里，最后的`body`标签上，引用我们刚新建的js文件


    <script type="text/javascript" src="/js/src/particle.js"></script>



## 尾记

至此基本上博客的功能就比较全了，其他的功能也还有很多，比如 PV啦，友链啦，搜索啦，笔者晚点会补上的。这期就先这样了，诸君，好运。
![](https://cl.ly/1V0a2f2p0a1y/comic_dance.gif)