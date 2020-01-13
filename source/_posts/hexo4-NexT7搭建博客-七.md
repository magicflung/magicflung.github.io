---
title: 快速搭建搭建博客：收录
copyright: true
tags: hexo
categories: hexo
description: 搭建博客系列：百度，必应，谷歌收录。
abbrlink: 6b104b4e
date: 2019-12-29 00:02:39
top:
---

我们必须把我们的网站推送到搜索引擎那， 不然别人除了输入我们的域名，是没法发现我们的博文。

如何查看我的网站是否被收录：

```
site:你的网站
比如我的：site:flunggg.cn
```

如果出现文章就有，没有就是没有。

## 1. 站点地图

站点地图即[sitemap](https://link.jianshu.com/?t=https://baike.baidu.com/item/sitemap/6241567?fr=aladdin)， 是一个页面，上面放置了网站上需要搜索引擎抓取的所有页面的链接。站点地图可以告诉搜索引擎网站上有哪些可供抓取的网页，以便搜索引擎可以更加智能地抓取网站。



## 2. 生成站点地图

安装百度和Google的站点地图生成插件：

```git
npm install hexo-generator-baidu-sitemap --save
npm install hexo-generator-sitemap --save
```

然后来到站点目录配置文件`_config.yml`，在下面添加：

```yml
# 站点地图
sitemap: 
  path: sitemap.xml
baidusitemap:
  path: baidusitemap.xml
```

然后重新推送到服务器，在访问如下URL

```
https://你的域名/sitemap.xml
https://你的域名/baidusitemap.xml
```

看看有没有出现代码。有的话就成功。

## 3.百度收录

### 3.1 百度站长

通过百度站长平台进行链接提交，增加网站的索引量。先去注册并登录：[百度站长平台](https://ziyuan.baidu.com/?castk=LTE%3D)

然后如下图：

![1](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122900/20191229001024-355538.png)

![2](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122900/20191229001031-449605.png)

![3](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122900/20191229001037-231974.png)

我选择的是`https://`，这根据你前面是否添加SSL证书来选择。并且我使用的是不带www的，看个人。然后到第三步，我喜欢用CNAME验证，就是到域名解析那添加个映射。

![4](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122900/20191229003649-19150.png)

### 3.2 链接提交

百度站长平台的链接提交方式分为自动提交和手动提交两种，此处只讲自动提交，手动提交按照要求操作即可。

#### 3.2.1 主动推送

主动推送最为快速的提交方式，是被百度收录最快的推送方式。主动推送可以通过安装插件实现：

```
npm install hexo-baidu-url-submit --save
```

然后到站点目录配置文件`_config.yml`，添加：

```yml
# 主动推送百度，被百度收录
baidu_url_submit:
  count: 10 # 提交最新的10个链接
  host:  # 百度站长平台中注册的域名
  token:  # 秘钥，百度站长平台 > 推送接口 > 接口调用地址中token字段
  path: baidu_urls.txt # 文本文档的地址， 新链接会保存在此文本文档里，不用改
```

![5](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122902/20191229025916-585899.png)

其次，记得查看`_config.yml`文件中url的值， 必须包含是百度站长平台注册的域名， 比如:我的

```
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://flunggg.cn/
root: /
permalink: archives/:abbrlink.html
```

最后，加入新的deployer:

```
deploy:
- type: git ## 这是我原来的deployer
  repo:
  branch:
- type: baidu_url_submitter ## 添加这里内容即可
```

其主动推送的实现原理如下：

- 新链接的产生， `hexo generate` 会产生一个文本文件，里面包含最新的链接
- 新链接的提交， `hexo deploy` 会从上述文件中读取链接，提交至百度搜索引擎

#### 3.2.2 自动推送

可以在`themes\next\layout\_third-party`中看到一个baidu-push.swig文件，这应该是现在next版本自带的。所以就不用配置了。如果没有则，在此目录下创建一个名为：baidu-push.swig文件。然后添加：

```js
{%- if theme.baidu_push %}
  <script{{ pjax }}>
    (function(){
      var bp = document.createElement('script');
      var curProtocol = window.location.protocol.split(':')[0];
      bp.src = (curProtocol === 'https') ? 'https://zz.bdstatic.com/linksubmit/push.js' : 'http://push.zhanzhang.baidu.com/push.js';
      var s = document.getElementsByTagName("script")[0];
      s.parentNode.insertBefore(bp, s);
    })();
  </script>
{%- endif %}
```

#### 3.3.3 sitemap提交

也叫手动推送。把下面的代码：

```
https://你的域名/sitemap.xml
https://你的域名/baidusitemap.xml
```

复制粘贴到这里，然后提交。我的结果就在下面，显示成功。

![6](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122900/20191229003550-704359.png)

注意：百度站长的HTTPS认证先不理，我当初配置完，总是HTTPS认证失败，就是因为站内还有http的链接，但是做了http转htpps也不能。现在突然看到成功了，也许是在服务器那边把http给关了。并且百度站长收录网站很慢很慢，大概半个月后才收录网站。而且我的过了一个星期才有推送数据。刚开始怎么推送都没有。现在才有。但是还是没看到我的文章。

![7](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122900/20191229003645-587488.png)

## 4. 谷歌收录

提交谷歌搜索引擎比较简单，在提交之前，我们依然可以使用 `site:域名` 查看网站是否被收录，谷歌爬虫的确是强大，即使没有提交过，我现在也可以看到我的文章。牛逼！！不像某某推送这么久没效果。

![8](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122900/20191229005043-463018.png)

进入[谷歌站长](https://www.google.com/webmasters/#?modal_active=none)，登录你的谷歌账号。然后如下操作，要是第一次就弹出一个让你输入域名的提示框，就直接输入你要收录的网站域名就行。

![9](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019123117/20191231170655-456946.png)



![10](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122900/20191229005435-133975.png)

我是选择第一个。然后需要等待一天。

然后还需要验证，下面的图我是借来的，因为我已经验证好了。复制那条TXT记录，然后到域名解析那里，使用TXT来映射。

![11](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122901/20191229010850-311092.png)

![12](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122901/20191229011040-65121.png)

你也可以下载个HTML文件然后放在站点目录下的source中，然后推送到服务器。

输入完就不用去理了。第二天会自动收录。

## 5. 必应收录

必应收录也是很简单，点击[必应站长](https://www.bing.com/webmaster/home/mysites)。先注册登录，必应收录有两种方式，一种使用刚刚谷歌导入过去，第二种是就是自己添加URL

![13](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122901/20191229011600-898298.png)

然后就不用理，不过必应也收录也比较慢，到现在才收录我的首页而已，但是比百度快。

