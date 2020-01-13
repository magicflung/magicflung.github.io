---
title: 快速搭建博客：Next主题
copyright: true
tags: hexo
categories: hexo
description: 搭建博客系列：更换主题
abbrlink: 66b7033a
date: 2019-12-22 21:13:44
top:
---

## 1. 主题

主题文件都是存放在站点的themes文件下，默认为landscape主题，主题可以从hexo官网下载，每个主题都会有文档。

下载主题有两种方式，一种是下载主题包然后放到themes文件中，而我喜欢用另一种，就是使用GitBash从github上clone下来，可能速度有点慢，自己也可以用第一种方法，去github直接下载。



## 2. 语言

一开始来是英文的，所以得改成中文，打开站点目录下的_config.yml。找到下面的代码：

![1](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20191226222123-835691.png)

在language的值修改为: zh-CN

然后测试下就变中文。



## 3. NexT主题

#### 3.1 下载NexT主题

我使用的是Next主题，比较火，出bug容易找到解决方案，访问速度很快，简洁。

打开GitBash，定位到你的博客站点目录下，或者在你的博客站点目录右键点击GitBash。输入如下指令：

```git
git clone https://github.com/theme-next/hexo-theme-next.git themes/next
```

然后等待下载，最后的themes/next 表示会把克隆的主题放到该目录，具体可以参考  [官方](https://github.com/theme-next/hexo-theme-next)。

#### 3.2 更换主题

在站点目录下的_config.yml文件中更改新下载的主题，把theme后面的值改为next，记得要有空格，这是yml文件得遵循yml语法。

![2](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20191226222142-371141.png)

然后使用之前说的三条指令启动本地测试，访问localhost:4000，就可以看到更换的next主题

NexT主题中还有四个不同风格的主题，我是用Gemini风格，这个可以改，在主题目录下（next）的_config.yml中查询 Schemes，然后下面有四种风格，都试试喜欢哪一种，把不用的注释，只能存在一个风格。

![3](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20191226222143-959399.png)

####  3.3 在菜单栏开启一些跳转页面

还是在主题目录下的_config.yml中。把想要的页面开启，去掉#即可。

![4](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20191226222144-962329.png)

我开启的从上到下分别是：首页、阅读排行（这个是我加的原先没有，后面再说怎么加）、归档、分类、标签、关于我。**可以在menu中调换显示顺序。**

但是这些页面不会自动生成，得我们创建。

比如：**分类页面**，在站点目录打开Git Bash，输入

```git
hexo new page categories
```

然后去站点目录的source/categories的index.md 中，配置：copy过去即可。

```yml
---
title: 分类
type: "categories" # 这要跟主题目录下的_config.yml的那个名称一样
comments: false # 不开启评论
---
```

- **标签页面**



```
hexo new page tags
```

在source/tags的index.md 中

```yml
---
title: 标签 # 默认是显示英文，需要自己改中文
type: "tags"
comments: false
---
```

- **关于页面**



```
hexo new page about
```

在source/about的index.md 中

```yml
title: 关于
date: 2019-11-30 22:52:16
type: "about"

# 然后下面就可以使用Markdown语法介绍自己
```

- **文章阅读排行页面**



```
hexo new page top
```

在source/top的index.md 中，添加：

```yml
---
comments: false
type: "top"
---
<div id="top"></div>
<script src="https://cdn1.lncld.net/static/js/av-core-mini-0.6.4.js"></script>
<script>AV.initialize("PqPgFdX018fr1aVY5hzsSsNI-gzGzoHsz", "3QD9vLXdOV4igzOOalSTkhkc");</script>
<script type="text/javascript">
  var time=0
  var title=""
  var url=""
  var query = new AV.Query('Counter');
  query.notEqualTo('id',0);
  query.descending('time');
  query.limit(1000);
  query.find().then(function (todo) {
    for (var i=0;i<1000;i++){
      var result=todo[i].attributes;
      time=result.time;
      title=result.title;
      url=result.url;
      var content="<p>"+"<font color='#1C1C1C'>"+"【文章"+time+"人观看过】"+"</font>"+"<a href='"+"https://flunggg.cn/"+url+"'>"+title+"</a>"+"</p>";
      document.getElementById("top").innerHTML+=content
    }
  }, function (error) {
    console.log("error");
  });
</script>
```

并且在主题目录下，themes\next\languages 中找到我们最初在网站配置的语言，我是 zh-CN，所以我打开这个文件。在menu中添加 top: 阅读排行。**每次在菜单栏新增就得在这里配置一个中文意思。**

```yml
menu:
  home: 首页
  archives: 归档
  categories: 分类
  top: 阅读排行
  tags: 标签
  about: 关于
  search: 搜索
  schedule: 日程表
  sitemap: 站点地图
  commonweal: 公益 404
```

- **归档页面**



```git
hexo new page top
```

然后source\archives中配置

```yml
---
title: archives
type: "archives"
comments: false
---
```

>  观看下篇系列博文 NexT主题的美化



参考：

[Hexo博客搭建全攻略(二)：NexT主题配置](https://www.jianshu.com/p/d95cff938277)