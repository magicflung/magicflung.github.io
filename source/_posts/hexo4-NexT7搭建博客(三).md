---
title: 快速搭建博客：美化主题第一篇
copyright: true
abbrlink: 53889e4c
date: 2019-12-23 22:27:29
tags: hexo
categories: hexo
top: 100
description: 搭建博客系列：美化主题
---

> 注意：有些样式加载不出，有可能是浏览器缓存css的原因，换浏览器或者清缓存。

## 1. 添加博客图标

如：![1](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122623/20191226231720-467532.png)

在主题目录下_config.yml文件查询 favicon，只需要修改前两个：small和medium，图片的像素得为16像素和32像素

```yml
favicon:
  small: /images/favicon-16x16-next.png
  medium: /images/favicon-32x32-next.png
  apple_touch_icon: /images/apple-touch-icon-next.png
  safari_pinned_tab: /images/logo.svg
  #android_manifest: /images/manifest.json
  #ms_browserconfig: /images/browserconfig.xml
```

你可以看到这两个属性的值，/images其实完整的路径为：themes/next/source/images 这条路径来定位的。

也可以定义外部URI。

可以去图标素材：[iconfont](https://www.iconfont.cn/)，[easyicon](https://www.easyicon.net/)。中下载16x16和32x32大小的PNG格式图片，然后放到themes/next/source/images下。

可以得出，要在主题某处添加什么图片都可以放在这个目录下。



## 2. 鼠标点击特效（二选一）

### 2.1 红心特效

刚刚说了主题图片可以放在themes\next\source\images，而themes\next\source有一个js文件，我们在themes\next\source\js\src（如果没有就自己创建） 下新建文件clicklove.js，然后把下面的代码copy到该文件中。

```js
!function(e,t,a){function n(){c(".heart{width: 10px;height: 10px;position: fixed;background: #f00;transform: rotate(45deg);-webkit-transform: rotate(45deg);-moz-transform: rotate(45deg);}.heart:after,.heart:before{content: '';width: inherit;height: inherit;background: inherit;border-radius: 50%;-webkit-border-radius: 50%;-moz-border-radius: 50%;position: fixed;}.heart:after{top: -5px;}.heart:before{left: -5px;}"),o(),r()}function r(){for(var e=0;e<d.length;e++)d[e].alpha<=0?(t.body.removeChild(d[e].el),d.splice(e,1)):(d[e].y--,d[e].scale+=.004,d[e].alpha-=.013,d[e].el.style.cssText="left:"+d[e].x+"px;top:"+d[e].y+"px;opacity:"+d[e].alpha+";transform:scale("+d[e].scale+","+d[e].scale+") rotate(45deg);background:"+d[e].color+";z-index:99999");requestAnimationFrame(r)}function o(){var t="function"==typeof e.onclick&&e.onclick;e.onclick=function(e){t&&t(),i(e)}}function i(e){var a=t.createElement("div");a.className="heart",d.push({el:a,x:e.clientX-5,y:e.clientY-5,scale:1,alpha:1,color:s()}),t.body.appendChild(a)}function c(e){var a=t.createElement("style");a.type="text/css";try{a.appendChild(t.createTextNode(e))}catch(t){a.styleSheet.cssText=e}t.getElementsByTagName("head")[0].appendChild(a)}function s(){return"rgb("+~~(255*Math.random())+","+~~(255*Math.random())+","+~~(255*Math.random())+")"}var d=[];e.requestAnimationFrame=function(){return e.requestAnimationFrame||e.webkitRequestAnimationFrame||e.mozRequestAnimationFrame||e.oRequestAnimationFrame||e.msRequestAnimationFrame||function(e){setTimeout(e,1e3/60)}}(),n()}(window,document);

```

然后在\themes\next\layout_layout.swig文件末尾添加：

```js
<!-- 页面点击小红心 -->
<script type="text/javascript" src="/js/src/clicklove.js"></script>
```



### 2.2 爆炸烟花

在themes\next\source\js\src（如果没有就自己创建） 下新建文件firework.js，然后把下面的代码copy到该文件中。

```js
"use strict";function updateCoords(e){pointerX=(e.clientX||e.touches[0].clientX)-canvasEl.getBoundingClientRect().left,pointerY=e.clientY||e.touches[0].clientY-canvasEl.getBoundingClientRect().top}function setParticuleDirection(e){var t=anime.random(0,360)*Math.PI/180,a=anime.random(50,180),n=[-1,1][anime.random(0,1)]*a;return{x:e.x+n*Math.cos(t),y:e.y+n*Math.sin(t)}}function createParticule(e,t){var a={};return a.x=e,a.y=t,a.color=colors[anime.random(0,colors.length-1)],a.radius=anime.random(16,32),a.endPos=setParticuleDirection(a),a.draw=function(){ctx.beginPath(),ctx.arc(a.x,a.y,a.radius,0,2*Math.PI,!0),ctx.fillStyle=a.color,ctx.fill()},a}function createCircle(e,t){var a={};return a.x=e,a.y=t,a.color="#F00",a.radius=0.1,a.alpha=0.5,a.lineWidth=6,a.draw=function(){ctx.globalAlpha=a.alpha,ctx.beginPath(),ctx.arc(a.x,a.y,a.radius,0,2*Math.PI,!0),ctx.lineWidth=a.lineWidth,ctx.strokeStyle=a.color,ctx.stroke(),ctx.globalAlpha=1},a}function renderParticule(e){for(var t=0;t<e.animatables.length;t++){e.animatables[t].target.draw()}}function animateParticules(e,t){for(var a=createCircle(e,t),n=[],i=0;i<numberOfParticules;i++){n.push(createParticule(e,t))}anime.timeline().add({targets:n,x:function(e){return e.endPos.x},y:function(e){return e.endPos.y},radius:0.1,duration:anime.random(1200,1800),easing:"easeOutExpo",update:renderParticule}).add({targets:a,radius:anime.random(80,160),lineWidth:0,alpha:{value:0,easing:"linear",duration:anime.random(600,800)},duration:anime.random(1200,1800),easing:"easeOutExpo",update:renderParticule,offset:0})}function debounce(e,t){var a;return function(){var n=this,i=arguments;clearTimeout(a),a=setTimeout(function(){e.apply(n,i)},t)}}var canvasEl=document.querySelector(".fireworks");if(canvasEl){var ctx=canvasEl.getContext("2d"),numberOfParticules=30,pointerX=0,pointerY=0,tap="mousedown",colors=["#FF1461","#18FF92","#5A87FF","#FBF38C"],setCanvasSize=debounce(function(){canvasEl.width=2*window.innerWidth,canvasEl.height=2*window.innerHeight,canvasEl.style.width=window.innerWidth+"px",canvasEl.style.height=window.innerHeight+"px",canvasEl.getContext("2d").scale(2,2)},500),render=anime({duration:1/0,update:function(){ctx.clearRect(0,0,canvasEl.width,canvasEl.height)}});document.addEventListener(tap,function(e){"sidebar"!==e.target.id&&"toggle-sidebar"!==e.target.id&&"A"!==e.target.nodeName&&"IMG"!==e.target.nodeName&&(render.play(),updateCoords(e),animateParticules(pointerX,pointerY))},!1),setCanvasSize(),window.addEventListener("resize",setCanvasSize,!1)}"use strict";function updateCoords(e){pointerX=(e.clientX||e.touches[0].clientX)-canvasEl.getBoundingClientRect().left,pointerY=e.clientY||e.touches[0].clientY-canvasEl.getBoundingClientRect().top}function setParticuleDirection(e){var t=anime.random(0,360)*Math.PI/180,a=anime.random(50,180),n=[-1,1][anime.random(0,1)]*a;return{x:e.x+n*Math.cos(t),y:e.y+n*Math.sin(t)}}function createParticule(e,t){var a={};return a.x=e,a.y=t,a.color=colors[anime.random(0,colors.length-1)],a.radius=anime.random(16,32),a.endPos=setParticuleDirection(a),a.draw=function(){ctx.beginPath(),ctx.arc(a.x,a.y,a.radius,0,2*Math.PI,!0),ctx.fillStyle=a.color,ctx.fill()},a}function createCircle(e,t){var a={};return a.x=e,a.y=t,a.color="#F00",a.radius=0.1,a.alpha=0.5,a.lineWidth=6,a.draw=function(){ctx.globalAlpha=a.alpha,ctx.beginPath(),ctx.arc(a.x,a.y,a.radius,0,2*Math.PI,!0),ctx.lineWidth=a.lineWidth,ctx.strokeStyle=a.color,ctx.stroke(),ctx.globalAlpha=1},a}function renderParticule(e){for(var t=0;t<e.animatables.length;t++){e.animatables[t].target.draw()}}function animateParticules(e,t){for(var a=createCircle(e,t),n=[],i=0;i<numberOfParticules;i++){n.push(createParticule(e,t))}anime.timeline().add({targets:n,x:function(e){return e.endPos.x},y:function(e){return e.endPos.y},radius:0.1,duration:anime.random(1200,1800),easing:"easeOutExpo",update:renderParticule}).add({targets:a,radius:anime.random(80,160),lineWidth:0,alpha:{value:0,easing:"linear",duration:anime.random(600,800)},duration:anime.random(1200,1800),easing:"easeOutExpo",update:renderParticule,offset:0})}function debounce(e,t){var a;return function(){var n=this,i=arguments;clearTimeout(a),a=setTimeout(function(){e.apply(n,i)},t)}}var canvasEl=document.querySelector(".fireworks");if(canvasEl){var ctx=canvasEl.getContext("2d"),numberOfParticules=30,pointerX=0,pointerY=0,tap="mousedown",colors=["#FF1461","#18FF92","#5A87FF","#FBF38C"],setCanvasSize=debounce(function(){canvasEl.width=2*window.innerWidth,canvasEl.height=2*window.innerHeight,canvasEl.style.width=window.innerWidth+"px",canvasEl.style.height=window.innerHeight+"px",canvasEl.getContext("2d").scale(2,2)},500),render=anime({duration:1/0,update:function(){ctx.clearRect(0,0,canvasEl.width,canvasEl.height)}});document.addEventListener(tap,function(e){"sidebar"!==e.target.id&&"toggle-sidebar"!==e.target.id&&"A"!==e.target.nodeName&&"IMG"!==e.target.nodeName&&(render.play(),updateCoords(e),animateParticules(pointerX,pointerY))},!1),setCanvasSize(),window.addEventListener("resize",setCanvasSize,!1)};
```

然后在\themes\next\layout_layout.swig文件末尾添加：

```js
<!-- 爆炸烟花 -->
<canvas class="fireworks" style="position: fixed;left: 0;top: 0;z-index: 1; pointer-events: none;" ></canvas> 
<script type="text/javascript" src="//cdn.bootcss.com/animejs/2.2.0/anime.min.js"></script> 
<script type="text/javascript" src="/js/src/firework.js"></script>
```



## 3. 设置头像

在主题目录（themes/next/）中的_config.yml，查找 avatar

```yml
# Sidebar Avatar
avatar:
  # Replace the default image and set the url here.
  # 把你要的头像文件名字改为avatar并放到相应路径下，如果后缀不同，则在这里改后缀，不要改图片后缀
  url: /images/avatar.jpg 
  # If true, the avatar will be dispalyed in circle.
  rounded: true
  # If true, the avatar will be rotated with the cursor.
  rotated: true
```



## 4. 侧边栏社交小图标设置

在主题目录打开_config.yml，查询  social

```yml
social:
  GitHub: https://github.com/magicflung || github
  #E-Mail: mailto:yourname@gmail.com || envelope
  #Weibo: https://weibo.com/yourname || weibo
  #Google: https://plus.google.com/yourname || google
  #Twitter: https://twitter.com/yourname || twitter
  #FB Page: https://www.facebook.com/yourname || facebook
  #StackOverflow: https://stackoverflow.com/yourname || stack-overflow
  #YouTube: https://youtube.com/yourname || youtube
  #Instagram: https://instagram.com/yourname || instagram
  #Skype: skype:yourname?call|chat || skype
  #RSS: /atom.xml || rss
```

然后启动你想要的，并修改成自己的路径。目前只有这些。



## 5. 文章末尾的标签图标修改

默认是 带“#”，可以把它换成图标。还是主题目录打开_config.yml中查询  tag_icon

```yml
# Use icon instead of the symbol # to indicate the tag at the bottom of the post
tag_icon: true
```

效果：

![2](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122623/20191226231727-159960.png)



## 6. 访问量统计

在主题目录打开_config.yml，查询  busuanzi

```yml
# Show Views / Visitors of the website / page with busuanzi.
# Get more information on http://ibruce.info/2015/04/04/busuanzi
busuanzi_count:
  enable: true # 是否开启访问量统计
  total_visitors: false # 本站访客数
  total_visitors_icon: user
  total_views: true # 本站总访问量
  total_views_icon: eye
  post_views: false # 文章访问量，但我不喜欢用这个，因为在我的网站首页不会显示每篇博文的访问量，而是得点进博文，我用别的来统计，待会会说
  post_views_icon: eye

```

但是我当时配置时失效，我百度一下，找到解决方案，原来是不蒜子使用的七牛的域名被强制过期。。

在`\themes\next\layout\_third-party\statistics中的busuanzi-counter.swig`中，看最前面，如图：

![3](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122623/20191226231729-372918.png)



我是改了的，你把src中的改为：

```
https://busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js
```

在主题目录的`\themes\next\layout\_partials\footer.swig`，在最后添加上下面这条，可以自己修改显示的位置。我忘记了是不是本来就有配，并且是否有下面代码的第一条，如果有则在里面添加。没用就直接复制下面的。

```
{%- if theme.footer.powered.enable %}
  <span id="busuanzi_container_site_pv">
    本站总访问量<span class="busuanzi-value" id="busuanzi_value_site_pv"></span>次
  </span>
{%- endif %}
```

注意：本地测试的访问量刚开始会很大不正常，但是部署后是正常的，这点不用理。



## 7. 添加站内搜索

在站内按照搜索，站点目录打开Git Bash，输入：

```
sudo npm install hexo-generator-search --save
```

然后在站点目录的_config.yml文件把下面代码添加进去，不用修改。我测试过可以不配置下面的代码，插件默认只索引文章(post)，要想页面(page)也能被检索，则把path的值改为all。

具体可参考官方：[hexo-generator-search](https://github.com/wzpan/hexo-generator-search)

```yml
# 搜索
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```

最后在主题目录的_config.yml文件，查询 local_search

```yml
# Local Search
# Dependencies: https://github.com/theme-next/hexo-generator-searchdb
local_search:
  enable: true # 开启站内搜索
  # 如果自动，则通过更改输入触发搜索。
  # 如果是手动，则按回车键或搜索按钮触发搜索。
  trigger: auto
  # 显示每篇文章的前n个结果，通过设置-1显示所有结果
  top_n_per_article: 1
  # 将html字符串转换为可读字符串。
  unescape: false
  # 加载页面时预加载搜索数据。
  preload: false
```



## 8.  启动阅读更多按钮

在主题目录的_config.yml中，查询：read_more_btn

```yml
# Read more button
# If true, the read more button will be displayed in excerpt section.
read_more_btn: true # 启动
```

效果：

![4](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122623/20191226231733-348452.png)



这里需要额外说一下，必须在每篇博文的最前面有一堆默认键值对代码那里，添加上一个键: description，

然后冒号： :，然后再空格，添加上自己的小文段。

 ![5](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122623/20191226231737-415857.png)

但是每次都得自己加太麻烦了，所以在站点目录下的scaffolds文件中有个post文件，这个文件就是博文模板，点开就可以添加每篇博文的通用信息。我的如下：

```yml
---
title: {{ title }}
date: {{ date }}
tags:
categories:
copyright: true
top:
description:
---
```



## 9. 文章顶置（二选一，看看哪种简单）



### 9.1 方法一

在站点目录的 node_modules/hexo-generator-index/lib/generator.js 中，把改文件的代码全改成

```js
'use strict';
var pagination = require('hexo-pagination');
module.exports = function(locals){
  var config = this.config;
  var posts = locals.posts;
    posts.data = posts.data.sort(function(a, b) {
        if(a.top && b.top) { // 两篇文章top都有定义
            if(a.top == b.top) return b.date - a.date; // 若top值一样则按照文章日期降序排
            else return b.top - a.top; // 否则按照top值降序排
        }
        else if(a.top && !b.top) { // 以下是只有一篇文章top有定义，那么将有top的排在前面（这里用异或操作居然不行233）
            return -1;
        }
        else if(!a.top && b.top) {
            return 1;
        }
        else return b.date - a.date; // 都没定义按照文章日期降序排
    });
  var paginationDir = config.pagination_dir || 'page';
  return pagination('', posts, {
    perPage: config.index_generator.per_page,
    layout: ['index', 'archive'],
    format: paginationDir + '/%d/',
    data: {
      __index: true
    }
  });
};
```

然后可以在博文模板post中加个top键，值为整数，并且值为大越靠前。



### 9.2 方法二

在站点目录下打开Git Bash，先把原先的顶置插件卸载了，然后再装上hexo-generator-index-pin-top，输入：

```git
npm uninstall hexo-generator-index --save
npm install hexo-generator-index-pin-top --save
```

然后可以在博文模板（站点目录下的scaffolds文件夹）post中加个top键，值为整数，并且值为大越靠前。

```yml
---
title: {{ title }}
date: {{ date }}
tags:
categories:
copyright: true
top:
description:
---
```

然后新建的md文档就会带出top属性，然后才能修改top数值。

最后，两种都可以用的，就是在顶置博文时，没用一个顶置图标感觉有点怪怪的，所以可以加个图标。在主题目录中的themes\next\layout\\_macro的post文件，打开查询下面代码。

```html
<div class="post-meta">
```

然后这个div的下一行添加上：

```swig
{% if post.top %}
	<i class="fa fa-thumb-tack"></i>
	<font color=7D26CD>置顶</font>
	<span class="post-meta-divider">|</span>
{% endif %}
```

效果：

![6](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122623/20191226231802-763884.png)



## 10. 在文章底部增加版权信息

### 10.1 方式一

最简单，在主题目录的_config.yml查询 creative_commons

```yml
# Creative Commons 4.0 International License.
# See: https://creativecommons.org/share-your-work/licensing-types-examples
# Available values of license: by | by-nc | by-nc-nd | by-nc-sa | by-nd | by-sa | zero
# You can set a language value if you prefer a translated version of CC license, e.g. deed.zh
# CC licenses are available in 39 languages, you can find the specific and correct abbreviation you need on https://creativecommons.org
creative_commons:
  license: by-nc-sa
  sidebar: false
  post: true # 开启
  language:
```

但是这种如何改变样式我没研究，我喜欢下面的方式，毕竟还可以自己加上样式、文字等。

### 10.2 方式二（比较麻烦，但是好看）

我用网上的直接复制的代码，但是在这段代码中的sweetalert外部链接不知道咋了，访问超时，导致我的博文访问一篇文章贼慢，我以为是哪里出错。**这也是一个启示：如果博文突然变得慢，打开web工具检查一下那些加载过慢，考虑是否要删去，或者是哪里出错。**如下图：11s的加载。

![7](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122623/20191226231804-399466.png)

所以出错就是在这一句：

```js
<script src="https://unpkg.com/sweetalert/dist/sweetalert.min.js"></script>
```

我下载一个本地的js，然后自己改成本地连接就好了。

[点击下载](https://www.lanzous.com/i88qxni)，然后解压，把sweetalert.min.js文件放到themes\next\source\js中，

在themes\next\layout\\_macro中新建一个 my-copyright.swig文件，并添加：

```js
  {% if page.copyright %}
<div class="my_post_copyright">
  <script src="//cdn.bootcss.com/clipboard.js/1.5.10/clipboard.min.js"></script>

  <script src="https://cdn.bootcss.com/jquery/2.0.0/jquery.min.js"></script>
   <!-- JS库 sweetalert 引用本地路径 -->
  <script src="/js/sweetalert.min.js"></script>
  <p><span>文章作者:</span><a href="/" title="访问 {{ theme.author }} 的个人博客">{{ theme.author }}</a></p>
  <p><span>发布时间:</span>{{ page.date.format("YYYY年MM月DD日 - HH:MM") }}</p>
  <p><span>原始链接:</span><a href="{{ url_for(page.path) }}" title="{{ page.title }}">{{ page.permalink }}</a>
    <span class="copy-path"  title="点击复制文章链接"><i class="fa fa-clipboard" data-clipboard-text="{{ page.permalink }}"  aria-label="复制成功！"></i></span>
  </p>
  <p><span>许可协议:</span> 转载请保留原文链接及作者。</p>  
</div>
<script> 
    var clipboard = new Clipboard('.fa-clipboard');
      $(".fa-clipboard").click(function(){
      clipboard.on('success', function(){
        swal({   
          title: "",   
          text: '复制成功',
          icon: "success", 
          showConfirmButton: true
          });
        });
    });  
</script>
{% endif %}
```

然后在themes\next\source\css\\_common\components\post中新建my-post-copyright.styl 文件，添加上我们的版权样式。

```css
.my_post_copyright {
  width: 85%;
  max-width: 45em;
  margin: 2.8em auto 0;
  padding: 0.5em 1.0em;
  border: 1px solid #d3d3d3;
  font-size: 0.93rem;
  line-height: 1.6em;
  word-break: break-all;
  background: rgba(255,255,255,0.4);
}
.my_post_copyright p{margin:0;}
.my_post_copyright span {
  display: inline-block;
  width: 5.2em;
  color: #b5b5b5;
  font-weight: bold;
}
.my_post_copyright .raw {
  margin-left: 1em;
  width: 5em;
}
.my_post_copyright a {
  color: #808080;
  border-bottom:0;
}
.my_post_copyright a:hover {
  color: #a3d2a3;
  text-decoration: underline;
}
.my_post_copyright:hover .fa-clipboard {
  color: #000;
}
.my_post_copyright .post-url:hover {
  font-weight: normal;
}
.my_post_copyright .copy-path {
  margin-left: 1em;
  width: 1em;
  +mobile(){display:none;}
}
.my_post_copyright .copy-path:hover {
  color: #808080;
  cursor: pointer;
}

```

在themes/next/source/css/_common/components/post/post.styl文件中最后添加：表示加载刚刚的样式。

```
@import "my-post-copyright"
```

然后在themes/next/layout/_macro/post.swig中，搜索 END POST BODY，然后再下一行添加：

```
<div>
	{% if not is_index %}
		{% include 'my-copyright.swig' %}
	{% endif %}
</div>
```

然后回到站点目录下的scaffolds文件中有个post文件，添加copyright: true。也就是我上面那图，这就会在每篇博文下添加版权信息，也方便别人复制你的url。



## 11. 添加打赏

在主题文件的_config.yml，查询：reward

```yml
# Reward (Donate)
# Front-matter variable (unsupport animation).
reward_settings:
  # If true, reward will be displayed in every article by default.
  enable: true # 开启打赏
  animation: true # 开启动画抖动，可以自己试试
  #comment: Donate comment here.

reward:
  wechatpay: /images/wechatpay.jpg # 放微信的收款码
  alipay: /images/alipay.jpg # 放支付宝的收款码
  #bitcoin: /images/bitcoin.png
```



## 12. 文章加密访问

我不推荐用，毕竟只是前端校验而已。虽然一般人破不出。



## 13. 启动代码复制按钮

打开主题目录的_config.yml，查询 copy_button。

```yml
codeblock:
  # Code Highlight theme
  # Available values: normal | night | night eighties | night blue | night bright | solarized | solarized dark | galactic
  # See: https://github.com/chriskempson/tomorrow-theme
  highlight_theme: normal
  # Add copy button on codeblock
  copy_button:
    enable: true # 开启代码复制按钮
    # Show text copy result.
    show_result: true
    # Available values: default | flat | mac
    style: mac # 代码框样式，我喜欢用mac的，上面有三个选择都可以试试
```



## 14. 在Footer添加站点运行时间

在主题目录的/themes/next/layout/_partials/footer.swig中找到下面这句。

```
{%- if theme.footer.theme.enable %}  
```

然后在这句的下面添加：

```js
<div id="days"></div>
	</script>
	<script language="javascript">
	function show_date_time(){
	window.setTimeout("show_date_time()", 1000);
	BirthDay=new Date("11/29/2019 20:00:00");
	today=new Date();
	timeold=(today.getTime()-BirthDay.getTime());
	sectimeold=timeold/1000
	secondsold=Math.floor(sectimeold);
	msPerDay=24*60*60*1000
	e_daysold=timeold/msPerDay
	daysold=Math.floor(e_daysold);
	e_hrsold=(e_daysold-daysold)*24;
	hrsold=setzero(Math.floor(e_hrsold));
	e_minsold=(e_hrsold-hrsold)*60;
	minsold=setzero(Math.floor((e_hrsold-hrsold)*60));
	seconds=setzero(Math.floor((e_minsold-minsold)*60));
	document.getElementById('days').innerHTML="已运行"+daysold+"天"+hrsold+"时"+minsold+"分"+seconds+"秒";
	}
	function setzero(i){
	if (i<10)
	{i="0" + i};
	return i;
	}
	show_date_time();
	</script>
```

在这条语句里面我把原先的代码都注释了，不想用。为了怕说不清，贴图：

![8](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122623/20191226231815-105794.png)



## 15. 实现文章统计功能

安装一个 hexo-symbols-count-time插件，可以统计字数和阅读分钟数，

```
npm install hexo-symbols-count-time --save
```

然后在站点目录的config.yml最后添加：

```yml
symbols_count_time:
  symbols: true                # 文章字数统计
  time: true                   # 文章阅读时长
  total_symbols: true          # 站点总字数统计
  total_time: true             # 站点总阅读时长
  exclude_codeblock: false     # 排除代码字数统计
```

不喜欢的可以使用false让它不启动。并且文章字数统计时，他是连空格，标点符号等都统计了，可能会导致文章的字数会不正确，偏大。

效果：

![9](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122623/20191226231824-661591.png)



## 16. 修改博文目录

在主题目录的_config.yml 查询 toc

```yml
# 侧边栏中的目录
# Front-matter variable (unsupport wrap expand_all).
toc:
  enable: true # 是否启动博文目标
  # 自动将列表号添加到目录。
  number: false
  # 如果为true，则如果标题宽度大于边栏宽度，则所有单词都将放在下一行。width.
  wrap: false
  # 如果为true，则将显示帖子中所有级别的TOC，而不是其中激活的部分。
  expand_all: false
  # 生成的子标题的最大深度。
  max_depth: 6
```



## 17. RSS订阅（非必要）



原因：RSS订阅可以对文章内容进行订阅。如下：就不必每次去收藏夹找该博客网站

![10](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122901/20191229013659-263920.png)

首先在站点目录下打开Git Bash 安装

```
npm install hexo-generator-feed --save
```

然后打开站点目录的_config.yml在后面添加：

```
# RSS
plugins: hexo-generate-feed
```

然后在站点目录的_config.yml 查找 RSS

然后把它的注释去掉，即开启，并且不用去创建该页面，会自动生成

```
RSS: /atom.xml || rss
```

然后部署到网站就可以看到，但是我们点开时是一堆代码，我们需要安装一个RSS阅读器的插件，在浏览器搜索

RSS，谷歌商城一般登不上，登得上一样查RSS就行，那在火狐的附加工具搜，第一个就是下面这个：

![11](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122623/20191226232019-428953.png)



安装这个，使用方法：

![12](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122901/20191229013913-420349.png)



![13](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122901/20191229013847-814814.png)

![14](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122901/20191229014002-593018.png)

RSS的URL就是我们点开后一堆代码的上面的URL，比如我的是：

```
https://flunggg.cn/atom.xml
```





## 18. GitHub Fork Me

这个可以在我们的网站右上角出现一个到GitHub的跳转按钮，其实也跟侧边栏那个社交小图标差不多功能

在主题目录的_config.yml查询 github_banner

```
# `Follow me on GitHub` banner in the top-right corner.
github_banner:
  enable: true # 开启
  permalink: https://github.com/magicflung # 把它改为你的github首页
  title: Follow me on GitHub
```



## 19. bookmark

Bookmark是一个插件，允许用户保存他们的阅读进度。用户只需单击页面左上角的书签图标即可保存滚动位置。当他们下次访问您的博客时，他们可以自动恢复每个页面的最后滚动位置。

在主题目录的_config.yml查询 bookmark

```
bookmark:
  enable: true # 开启
  # Customize the color of the bookmark.
  color: "#836FFF" # 自定义颜色
  # If auto, save the reading progress when closing the page or clicking the bookmark-icon.
  # If manual, only save it by clicking the bookmark-icon.
  save: auto # 会自动保存阅读进度
```

效果：

![15](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122901/20191229013325-336995.png)



## 20. 添加lazyload（跟后面的图片点击全屏看不能共存）

对于图片进行延迟加载，访问到图片位置时才去请求图片资源，这样可以提高博客的访问速度，节省流量。下面一整条都复制。

```
npm install --save lozad
```

然后在你的主题目录的配置文件 `_config.yml` 中，查询 lazyload

```yml
# Vanilla JavaScript plugin for lazyloading images.
# For more information: https://github.com/ApoorvSaxena/lozad.js
lazyload: true # 开启
```



## 21. 显示当前浏览进度（两者可共存）

### 21.1 方式一

在主题目录的_config.yml查询scrollpercent，默认为false，改为true

```
back2top:
  enable: true
  # Back to top in sidebar.
  sidebar: false
  # Scroll percent label in b2t button.
  scrollpercent: true # 开启
```

### 21.2 方式二

在主题目录的_config.yml查询reading_progress

```
# Reading progress bar
reading_progress:
  enable: true # 开启
  # Available values: top | bottom
  position: top # top会出现在页面最顶部，bottom会出现在那里
  color: "#836FFF" # 可修改颜色
  height: 3px
```



## 22. Footer / 页脚设置

在主题目录的_config.yml 查询 footer

```
footer:
  # Specify the date when the site was setup. If not defined, current year will be used.
  #since: 2019

  # Icon between year and copyright info.
  icon:
    # Icon name in Font Awesome. See: https://fontawesome.com/v4.7.0/icons/
    # `heart` is recommended with animation in red (#ff0000).
    name: heart # 可以修改图片，可以去 https://fontawesome.com/v4.7.0/icons/ 找
    # If you want to animate the icon, set it to true.
    animated: true
    # Change the color of icon, using Hex Code.
    color: "#F5F5F5" # 图标颜色

  # If not defined, `author` from Hexo `_config.yml` will be used.
  copyright:
  
  # Powered by Hexo 字样，不喜欢可以设置为 false
  powered:
    # Hexo link (Powered by Hexo).
    enable: true
    # Version info of Hexo after Hexo link (vX.X.X).
    version: true
    
  # 主题字样，不喜欢可以 false
  theme:
    # Theme & scheme info link (Theme - NexT.scheme).
    enable: true
    # Version info of NexT after scheme info (vX.X.X).
    version: true

  # 备案信息，如果网站有备案号，可以在这里填写备案号
  # Beian ICP and gongan information for Chinese users. See: http://www.beian.miit.gov.cn, http://www.beian.gov.cn
  beian:
    enable: false
    icp:
    # The digit in the num of gongan beian.
    gongan_id:
    # The full num of gongan beian.
    gongan_num:
    # The icon for gongan beian. See: http://www.beian.gov.cn/portal/download
    gongan_icon_url:
```

然后还可以在 themes\next\layout\_partials的footer.swig里修改，这个我在前面的Footer添加运行时间有说，在这个文件可以修改他们的位置，也可以手动去掉或添加某些东西。如我的：

![16](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122901/20191229014054-350909.png)



> 观看下一篇hexo系列博文(四)



参考：

​	[1](https://blog.csdn.net/weixin_42905141/article/details/102677424)

​	[2](https://zhuanlan.zhihu.com/p/60424755)

