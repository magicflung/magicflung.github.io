---
title: 快速搭建博客：美化主题第二篇
copyright: true
tags: hexo
categories: hexo
description: 接上面一篇，搭建博客系列：美化主题第一篇
abbrlink: 6bbc4ccb
date: 2019-12-24 16:23:44
top: 99
---


## 23. 添加Valine评论

评论系统有很多种，只能选一种。

我自己喜欢用不登录评论，也就是偶尔有人要说一下，但是发现还得登录。这个评论系统就是Valine，下面是教程：

### 23.1. 注册LeanCloud

点击[LeanCloud](https://www.leancloud.cn/)，去注册一个账号，点右上角的控制台就会跳到注册。这里有分为国内和国际版，国内的在2019-10-1之后需要自定义已备案域名才能继续提供服务了，觉得麻烦的同学可以移步去国际版[LeanCloud国际版](https://leancloud.app/)。

### 23.2. 创建应用

注册好之后，进行实名验证、邮箱验证等等。

点击创建应用，输入名称，然后直接创建。

![1](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20191226223250-526177.png)

### 23.3.  设置Web安全域名，填入自己的域名

目的是：为了数据安全。

打开应用，点击 设置 -> 安全中心。看下面，把能访问到你网站的URL全写进去。

![2](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20191226231100-189293.png)

### 23.4. 获取APP ID 和 APP Key

如下打开：

![3](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20191226223310-866021.png)

然后在主题目录的_config.yml 文件查询 valine，然后把上面的两个复制进去

```
# Valine
# For more information: https://valine.js.org, https://github.com/xCss/Valine
valine:
  enable: true # 开启
  appid:  # 添加你的appid
  appkey: # 添加你的appkey
  notify: false # Mail notifier
  verify: false # Verification code
  placeholder: Just go go # Comment box placeholder
  avatar: monsterid # Gravatar style
  guest_info: nick,mail,link # Custom comment header
  pageSize: 10 # Pagination size
  language: # Language, available values: en, zh-cn
  visitor: true # Article reading statistic
  comment_count: true # If false, comment count will only be displayed in post page, not in home page
  recordIP: false # Whether to record the commenter IP
  serverURLs: # When the custom domain name is enabled, fill it in here (it will be detected automatically by default, no need to fill in)
  #post_meta_order: 0
  avatar_cdn: https://www.gravatar.com/avatar/
```

效果图是这样的：

![4](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20191226223312-376598.png)

不过上面的 网址(http://) 我觉得没什么用，就把它去掉了，打开themes\next\layout\_third-party\comments中的valine.swig文件，删掉第三个，我的已经删了：

![5](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20191226231109-15136.png)

也可以选择Gitalk评论系统，这个我就不说了。要的自己百度。



## 24. Vline邮箱评论通知

每当有人在博文下面评论我们的博文，我们就可以使用这个插件通过邮箱知道。

下面这款插件是新款，有几个优点：

- 完善的邮件通知，自定义 SMTP 发件频率和内容不再受限
- 基于 Akismet 的垃圾评论自动标注和过滤
- 评论管理，避免直接操作数据库

效果：

![6](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122716/20191227161440-133731.png)

1.我们还是要借助[Leancloud](https://leancloud.cn/dashboard/login.html#/signup)，打开刚刚我们创建的应用，如图：修改右边的边框内容。这是配置恢复模板。

![7](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20191226231120-940462.png)

> **注**：请注意修改链接为你的`博客`或者`网站首页`。

```html
<p>Hi, {{username}}</p>
<p>
你在 {{appname}} 的评论收到了新的回复，请点击查看：
</p>
<p><a href="你的网址首页链接" style="display: inline-block; padding: 10px 20px; border-radius: 4px; background-color: #3090e4; color: #fff; text-decoration: none;">马上查看</a></p>
```

2.还是在Leancloud中，打开 云引擎 -》设置，把下面代码复制进去：

```
https://github.com/DesertsP/Valine-Admin.git
```

![8](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20191226231201-585025.png)

然后还是在这，点击 部署，如图：

![9](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122623/20191226231521-977193.png)

上面的图我用别人的，因为我自己点开就是下面的图：

![10](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122623/20191226231522-649612.png)



然后等待部署完，比较久。

3.然后还是回到 云引擎 -》 设置 中，如图：找到 自定义环境变量

![11](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122623/20191226231530-750242.png)

需要配置信息：

| 变量名       | 实例                | 说明                                                 |
| ------------ | ------------------- | ---------------------------------------------------- |
| SITE_NAME    | flunggg             | 【必填】网站名称                                     |
| SITE_URL     | https://flunggg.cn/ | 网站地址                                             |
| SMTP_HOST    | smtp.qq.com         | QQ邮箱为这个，其他的自己查                           |
| SMTP_PORT    | 465                 | Https的邮件端口                                      |
| SMTP_USER    | xxxxx@qq.com        | 这里填写QQ邮箱，然后有人评论就会发送到我们这个邮箱里 |
| SMTP_PASS    |                     | 这里添加邮箱申请的SMTP密码                           |
| SENDER_NAME  | xxx博客--评论提醒   | 发送邮件的昵称                                       |
| SENDER_EMAIL |                     | 发送人邮箱，同上面的QQ邮箱一致就行                   |
| ADMIN_URL    |                     | 管理评论后台，这个待会说                             |

4.如何申请SMTP，直接看图，我用的是qq邮箱，你要用别的得去查。开启后把STMP密码复制到这。

![12](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20191226231203-596536.png)

5.设置二级域名后你可以访问评论管理后台。还是在这里的下面找到 Web主机域名

![13](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122623/20191226231540-510138.png)

现在好像已经是默认给你一个，不能改的，然后把这个域名复制到上面环境变量的ADMIN_URL。

我们还需要密码，用户名，需要在这里设置，只需要填写 email、password、username，这三个字段即可，使用 username 或 email 登陆即可。

![14](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20191226231208-314925.png)

![15](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20191226231212-930528.png)

然后重启下实例

![16](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20191226231211-767956.png)

最后访问域名，输入我们刚刚的username或者email，登录。就可以管理我们的评论区啦。



LeanCloud 休眠策略

免费版的 LeanCloud 容器，是有强制性休眠策略的，不能 24 小时运行：

- 每天必须休眠 6 个小时
- 30 分钟内没有外部请求，则休眠。
- 休眠后如果有新的外部请求实例则马上启动（但激活时发送邮件会失败）。

分析了一下上方的策略，如果不想付费的话，最佳使用方案就设置定时器，每天 7 - 23 点每 20 分钟访问一次，这样可以保持每天的绝大多数时间邮件服务是正常的。



## 25. Valine 头像设置

我们可以修改评论的头像，路人的头像有下面几种，默认为第一种

![17](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122901/20191229014500-940138.png)

在主题目录的_config.yml文件查找：valine 中，找到avatar，然后就可以修改上面的图标

```

# Valine
# For more information: https://valine.js.org, https://github.com/xCss/Valine
valine:
  ...
  avatar: monsterid # Gravatar style
  ... 
```

> 如果你修改了头像后发现没有更新，请不要慌张，因为`gravatar.cat.net` 有七天的缓存期，安静的等待吧

然后还可以设置自己的自定义头像，登录 [gravatar](https://cn.gravatar.com/)，注册账号并设置头像，然后在valine评论时，输入我们刚刚注册的邮箱，会显示出我们自定义的头像。我的设置如下：

![18](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122901/20191229014509-84570.png)

## 26. 添加文章阅读次数

这个还是得借助[Leancloud](https://leancloud.cn/dashboard/login.html#/signup) 。

在主题目录的_config.yml 查询 leancloud_visitors

```
![23](D:\myblog\blog\source\_posts\hexo4-NexT7搭建博客(四)\23.png)# Show number of visitors of each article.
# You can visit https://leancloud.cn to get AppID and AppKey.
# AppID and AppKey are recommended to be the same as valine's for counter compatibility.
# Do not enable both `valine.visitor` and `leancloud_visitors`.
leancloud_visitors:
  enable: true # 开启
  app_id: # 填入你的Leancloud的appId
  app_key: # 填入你的Leancloud的appKey
  # Dependencies: https://github.com/theme-next/hexo-leancloud-counter-security
  # If you don't care about security in leancloud counter and just want to use it directly
  # (without hexo-leancloud-counter-security plugin), set `security` to `false`.
  security: false # 默认
  betterPerformance: false # 默认
```

点击刚刚创建的应用（评论那个），点击设置->应用Keys中的Id和Key。复制粘贴到上面那个地方。然后在该应用，有一个叫 存储，点开，然后看到创建Class，创建一个Counter就行。

![9](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122720/20191227202707-993365.png)

![10](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122720/20191227202922-607348.png)

## 27. Url 持久化

我们可以发现 hexo 默认生成的文章地址路径是 `【网站名称／年／月／日／文章名称】`。

这种链接对搜索爬虫是很不友好的，它的 url 结构超过了三层，太深了。

下面我推荐安装 `hexo-abbrlink` 插件：

```
npm install hexo-abbrlink --save
```

然后在站点目录下，查询 permalink

```
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://flunggg.cn/  # 这里我填写我自己的域名，没有域名的先别动
root: /
permalink: archives/:abbrlink.html # 该这个
abbrlink:
  alg: crc32  # 算法：crc16(default) and crc32
  rep: hex    # 进制：dec(default) and hex
```

效果：

![11](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122901/20191229014745-258468.png)



## 28. nofollow 标签的使用

减少出站链接能够有效防止权重分散。简单的说就是，如果A网页上有一个链接指向B网页，但A网页给这个链接加上了 rel="nofollow" 标注，则搜索引擎不把A网页计算入B网页的[反向链接](https://baike.baidu.com/item/反向链接/7535075)。[搜索引擎](https://baike.baidu.com/item/搜索引擎/104812)看到这个标签就可能减少或完全取消链接的投票[权重](https://baike.baidu.com/item/权重/10245966)。（百度百科）

安装插件：

```
npm install hexo-autonofollow --save
```

然后在站点目录下的_config.yml添加

```
# 外部链接优化
nofollow:
    enable: true
    exclude:     # 例外的链接，可将友情链接放置此处
    - 'yousite'
```

这样，例外的链接将不会被加上 nofollow 属性。

## 29. 取消“文章目录”的自动编号

在主题目录下的_config.yml，搜索 toc

```yml
toc:
  enable: true
  # Automatically add list number to toc.
  number: false # 取消自动编号
  # If true, all words will placed on next lines if header width longer then sidebar width.
  wrap: true
  # If true, all level of TOC in a post will be displayed, rather than the activated part of it.
  expand_all: false
  # Maximum heading depth of generated toc.
  max_depth: 6
```

## 30. 背景（不要共存）

### 30.1 雨代码（就是我当前用的这样）

重头戏来了，我第一次发现这个也是惊喜一番。

在主题目录下的`themes\next\source\js\src`中新建一个名为DigitalRain.js文件。然后添加如下代码：

```
window.onload = function(){
    //获取画布对象
    var canvas = document.getElementById("canvas");
    //获取画布的上下文
    var context =canvas.getContext("2d");
    var s = window.screen;
    var W = canvas.width = s.width;
    var H = canvas.height;
    //获取浏览器屏幕的宽度和高度
    //var W = window.innerWidth;
    //var H = window.innerHeight;
    //设置canvas的宽度和高度
    canvas.width = W;
    canvas.height = H;
    //每个文字的字体大小
    var fontSize = 12;
    //计算列
    var colunms = Math.floor(W /fontSize);	
    //记录每列文字的y轴坐标
    var drops = [];
    //给每一个文字初始化一个起始点的位置
    for(var i=0;i<colunms;i++){
        drops.push(0);
    }
    //运动的文字
    var str ="WELCOME TO WWW.ITRHX.COM";
    //4:fillText(str,x,y);原理就是去更改y的坐标位置
    //绘画的函数
    function draw(){
        context.fillStyle = "rgba(238,238,238,.08)";//遮盖层
        context.fillRect(0,0,W,H);
        //给字体设置样式
        context.font = "600 "+fontSize+"px  Georgia";
        //给字体添加颜色
        context.fillStyle = ["#33B5E5", "#0099CC", "#AA66CC", "#9933CC", "#99CC00", "#669900", "#FFBB33", "#FF8800", "#FF4444", "#CC0000"][parseInt(Math.random() * 10)];//randColor();可以rgb,hsl, 标准色，十六进制颜色
        //写入画布中
        for(var i=0;i<colunms;i++){
            var index = Math.floor(Math.random() * str.length);
            var x = i*fontSize;
            var y = drops[i] *fontSize;
            context.fillText(str[index],x,y);
            //如果要改变时间，肯定就是改变每次他的起点
            if(y >= canvas.height && Math.random() > 0.99){
                drops[i] = 0;
            }
            drops[i]++;
        }
    };
    function randColor(){//随机颜色
        var r = Math.floor(Math.random() * 256);
        var g = Math.floor(Math.random() * 256);
        var b = Math.floor(Math.random() * 256);
        return "rgb("+r+","+g+","+b+")";
    }
    draw();
    setInterval(draw,35);
};
```

然后在主题目录`themes\next\layout`下的_layout.swig文件中，在\</html\>上添加如下代码：

```
<!-- 数字雨 -->
<canvas id="canvas" width="1440" height="900" ></canvas>
<script type="text/javascript" src="/js/DigitalRain.js"></script>
```

并且，在主题目录的配置文件_config.yml中查找 custom_file_path

```yml
# Define custom file paths.
# Create your custom files in site directory `source/_data` and uncomment needed files below.
custom_file_path:
  #head: source/_data/head.swig
  #header: source/_data/header.swig
  #sidebar: source/_data/sidebar.swig
  #postMeta: source/_data/post-meta.swig
  #postBodyEnd: source/_data/post-body-end.swig
  #footer: source/_data/footer.swig
  #bodyEnd: source/_data/body-end.swig
  #variable: source/_data/variables.styl
  mixin: source/_data/mixins.styl
  style: source/_data/styles.styl
```

这些文件可以自定义css，只需要开启（把#去掉）就行。然后根据路径创建该文件，或者可以自定义文件路径然后去创建。我在`source/_data/styles.styl`中添加如下代码：

```css
canvas {
  position: fixed;
  right: 0px;
  bottom: 0px;
  min-width: 100%;
  min-height: 100%;
  height: auto;
  width: auto;
  z-index: -1;
}
```



### 30.2 自定义背景图

像上面那样，去开启一个css文件路径，然后添加代码：

```yml
// 添加背景图片
body {
  background: url(/images/background.jpg); // 你给的背景图
  -moz-background-size:100% 100%;
  -webkit-background-size:100% 100%;
  top: 0;
  left: 0;
  z-index: -2;
  background-repeat: no-repeat;
  background-attachment: fixed;
  background-position: 50% 50%;
  background-size: cover;
}
```



### 30.3 其他背景

其他背景都可以在主题目录配置文件_config.yml ，查询 canvas_nest和canvas_ribbon，我就不说了，具体参考官方：[Canvas-nest](https://github.com/hustcc/canvas-nest.js)，[Canvas-ribbon](https://github.com/zproo/canvas-ribbon)。得先安装才能开启。



## 31. 浏览器网页标题恶搞（转，有贴转载地址）

效果：

![12](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122723/20191227230231-60662.png)

![13](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122723/20191227230232-474188.png)

当用户访问你的博客时点击到了其他网页，我们可以恶搞一下网页标题，呼唤用户回来，首先在目录 \Hexo\themes\hexo-theme-spfk\source\js 下新建一个 FunnyTitle.js 文件，在里面填写如下代码：

```js
<!--浏览器搞笑标题-->
 var OriginTitle = document.title;
 var titleTime;
 document.addEventListener('visibilitychange', function () {
     if (document.hidden) {
         $('[rel="icon"]').attr('href', "/img/trhx2.png");
         document.title = 'ヽ(●-`Д´-)ノ你丑你就走！';
         clearTimeout(titleTime);
     }
     else {
         $('[rel="icon"]').attr('href', "/img/trhx2.png");
         document.title = 'ヾ(Ő∀Ő3)ノ你帅就回来！' + OriginTitle;
         titleTime = setTimeout(function () {
             document.title = OriginTitle;
         }, 2000);
     }
 });
```

然后在主题目录`themes\next\layout`下的_layout.swig文件中，在\</html\>上添加如下代码：

```js
<!--浏览器搞笑标题-->
<script type="text/javascript" src="/js/src/FunnyTitle.js"></script>
```



## 32. 一些样式

还是刚刚的主题目录的配置文件_config.yml中查找 custom_file_path

```yml
# Define custom file paths.
# Create your custom files in site directory `source/_data` and uncomment needed files below.
custom_file_path:
  #head: source/_data/head.swig
  #header: source/_data/header.swig
  #sidebar: source/_data/sidebar.swig
  #postMeta: source/_data/post-meta.swig
  #postBodyEnd: source/_data/post-body-end.swig
  #footer: source/_data/footer.swig
  #bodyEnd: source/_data/body-end.swig
  #variable: source/_data/variables.styl
  mixin: source/_data/mixins.styl
  style: source/_data/styles.styl
```

在`source/_data/styles.styl`中添加如下代码：下面的属性都可以在自己的网站按F12来查看要修改哪里，这应该不用我废话了。我的CSS都是复制别人的，自己不会CSS。

```css
// 文章之间的分割线
.posts-expand .post-eof {
  margin: 4em auto 4em;
  background: white;
}
// 标题栏
.site-meta {
// 这些自定义添加
}
// 底部文字
.footer-inner {
  font-size: 17px;
  color:  #262626;
  font-family: 'EB Garamond',"Noto Serif SC",sans-serif;
  font-weight: bold;
}
.fa-heart {
  color: red;
}
.post-meta-item-icon {
  color: red;
}
.footer {
// 这些自定义添加
}
// 修改主体透明度
.main-inner {
  background: #000;
  opacity: 0.90;
  padding-right: 3px;
  padding-left: 3px;
}
//主页文章添加阴影效果
.post {
  margin-top: 30px;
  margin-bottom: 30px;
  padding: 25px;
  -webkit-box-shadow: 0 0 5px rgba(202, 203, 203, .5);
  -moz-box-shadow: 0 0 5px rgba(202, 203, 204, .5);
}
// 移动端
@media (max-width: 767px) {
  .post-block {
    padding-right:10px;
  }
  .posts-expand .post-eof {
    margin: 4em auto 4em;
    background: white;
  }
}
```



## 33. 一些修改

比如，默认这里是很长的，我现在是改了，太长会导致手机端显示不好，所以可以改

- 电脑端：



![14](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122723/20191227235222-622840.png)

- 手机端：



![15](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122723/20191227235317-936841.png)

在主题目录`themes\next\layout\_macro`的post.swig中修改。主要是加个换行。

![16](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122723/20191227235613-926949.png)

搜索：if theme.leancloud_visitors.enable 。然后在这上面添加如下代码：

```html
{#添加换行#}
<br>
```



参考：

[小丁的个人博客](https://tding.top/archives/9a232bbe.html)

[评论邮箱参考](https://cloud.tencent.com/developer/article/1142490)

[Valine官方](https://valine.js.org/avatar.html)

[雨代码，标题恶搞](https://www.itrhx.com/2018/08/27/A04-Hexo-blog-topic-personalization/)