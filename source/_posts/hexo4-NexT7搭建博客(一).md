---
title: 快速搭建博客：入门
copyright: true
tags: hexo
categories: hexo
description: 搭建博客系列：入门
abbrlink: f1c28309
date: 2019-12-17 23:28:39
top: 
---

### 1.简介

网上有很多hexo+nexT7的教程，很多因为现在版本问题，有些配置完会出错或者不生效。搭建这个博客用了我4天的时间，加上美化。
期间使用nexT5.0和hexo4.0导致访问路径有点问题，出现 http://localhost:4000/archives/20%/ 我不明白咋回事，所以删了两次本地博客。结果发现是版本问题。

**首先在前面说清楚，每做完一步就给我使用三条指令本地测试（下面会说）一下，别傻傻的等着搞完一半甚至全部做完才来测试，要是出错那可是很致命的，最坏的结果当然是重做。如果每次都有测试要是出现bug就可以根据哪步来排除问题。**

而且我后期是使用阿里云服务器来搞的，别的服务器没碰过，但大致是一样的。后面有想搞域名的现在就可以先去搞域名，比如阿里云的万网（我是在这个搞的），腾讯云、百度云等都可以。并且先去备案。当然不备也可以，CDN加速都需要备案才可以申请。先把案备了以后也好办事。

### 2.安装Node和Git

使用hexo需要安装两个前置工具: Node 和 Git。

Node直接默认安装就行，点击下载 [Node](http://nodejs.cn/download/)。安装完不用理。

Git也是直接下载，点击下载 [Git](https://git-scm.com/)。一直默认安装完需要如下操作：

- 如果没有GitHub账号的，自己去注册，有账号的，直接创建一个新的仓库，名为 `yourgithubname.github.io`，其中`yourgithubname`是你的github名称，必须操作。比如我的：/ 前面是我的用户名，后面就是用我的用户名。  
  ![](hexo4-nexT7搭建博客(一)/gitname.png)

- 配置Git的SSH Key，第一次使用Git Bash使用一些配置：
    - 你的github用户名，非昵称，并填写你的github注册邮箱



```git
git config --global user.name "name"
git config --global user.email  "xxx@qq.com"
```

- 配置SSH，可以自己登录，每次部署就不用登录



```js
ssh-keygen -t rsa -C "邮件地址"
```
- - 然后连续3次回车，最终会生成一个文件在用户目录下，打开C盘下的用户目录下你的用户名继续打开，找到`.ssh\id_rsa.pub`文件（像我的 `C:\Users\chast\.ssh`），记事本打开并复制里面的内容，打开你的github主页，右上角点你的小头像进入`Settings -> SSH and GPG keys -> New SSH key`：
  - Title随便命名，把复制的内容粘贴到Key中。

  

  

  ![](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019123020/20191230204837-701017.png)



### 3.安装Hexo
- 然后点开刚刚安装的git，在 Window开始界面会出现一个 Git Bash，点击它，然后在一个磁盘上面创建一个目录，随便名，再使用Git Bash定位到目录中
![](hexo4-nexT7搭建博客(一)/1.png)
- 然后安装Hexo: 默认安装为4.0版本。



```js
npm install -g hexo
```
- 初始化Hexo



```
hexo init
```
- 初始化就如下面这样。

  

  ![](hexo4-nexT7搭建博客(一)/站点目录.png)

- 以后在本地启动博客就使用下面三条指令。



```git
hexo clean // 清除缓存
hexo g  // 生成静态页面
hexo s // 启动本地项目
```
- 然后就可以在浏览器上访问 [localhost:4000](localhost:4000) 来看到最初的博客，如果看到说明成功了。
- 那部署博客到git上,先在站点目录下的_config.yml文件最后一行配置。



```xml
deploy:
  type: git
  repository: https://github.com/yourgitname/yourgitname.github.io.git
  branch: master
```
- 再安装推送插件



```
 npm install hexo-deployer-git --save
```



- 然后使用下面三条命令。 



```git
![主题配置文件css样式](D:\myblog\blog\source\_posts\hexo4-nexT7-6搭建博客（一）\主题配置文件css样式.png)hexo clean // 清除缓存
hexo g  // 生成静态页面
hexo d // 部署
```



### 4.发表文章

```
# 创建文章
hexo new "Hello My Blog"
```

然后会在站点目录下的source文件的_posts文件就会出现刚刚创建的文件，然后用Markdown语法来写作。推荐一个写作的：Typora。

然后在使用上面的三条指令部署到服务器。



### 5.Hexo的目录结构

![目录](hexo4-nexT7搭建博客(一)/目录.png)
- 如果想自定义修改站点主题的内容在：站点目录\themes\next\layout中修改，根据文件名。
- 如果想自己添加css样式，在主题配置文件中把下图的注释去掉，然后去对应的目录创建该文件，然后就在里面添加css样式。![](hexo4-nexT7搭建博客(一)/主题配置文件css样式.png)



### 6.需要了解的一些名词

- 站点目录：就是最开始初始化hexo的那个目录。
- 主题目录：就是站点目录下的themes中的主题文件。比如我用的是NexT主题，那么themes中就会有一个next文件，点进去就是主题目录。
- 因为站点目录和主题目录下都有_config.yml文件，所以别混了。
- 资源文件：站点目录下的source文件，保存我们写的创建的博文，和首页导航栏的跳转页面。
- 模板文件：站点目录下的scaffolds，里面有个post文件，可以修改博文格式，比如想每次创建固定加点什么，就不必每次创建博文自己去添加。



### 7.我的版本

如何查看我的版本，在站点根目录下(hexo初始化的地方)有一个 packge.json ，里面就可以看到使用hexo初始化的结果。

主要注意跟我的hexo和等下要装的主题nexT是否一致，最好一致，可以在初始化后拿我下面的代码去覆盖原代码，然后使用 npm install ，会根据json中的代码重新安装一样的版本。**不过建议别copy我的去安装，不知道会抱什么错，还是一步一步来。hexo版本和next主题版本一致对后面主题的美化有关系。**

**注意：未来应该不会更新，现在主要用的版本是 hexo4.0 和 next7.x，注意看版本**

```json
{
  "name": "hexo-site",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "build": "hexo generate",
    "clean": "hexo clean",
    "deploy": "hexo deploy",
    "server": "hexo server"
  },
  "hexo": {
    "version": "4.0.0"
  },
  "dependencies": {
    "gulp": "^4.0.2",
    "gulp-clean": "^0.4.0",
    "gulp-htmlclean": "^2.7.22",
    "gulp-htmlmin": "^5.0.1",
    "gulp-imagemin": "^6.2.0",
    "gulp-minify-css": "^1.2.4",
    "gulp-uglify": "^3.0.2",
    "hexo": "^4.0.0",
    "hexo-abbrlink": "^2.0.5",
    "hexo-asset-image": "^1.0.0",
    "hexo-autonofollow": "^1.0.1",
    "hexo-baidu-url-submit": "0.0.6",
    "hexo-deployer-git": "^2.1.0",
    "hexo-generator-archive": "^1.0.0",
    "hexo-generator-baidu-sitemap": "^0.1.6",
    "hexo-generator-category": "^1.0.0",
    "hexo-generator-feed": "^2.2.0",
    "hexo-generator-index": "^1.0.0",
    "hexo-generator-search": "^2.4.0",
    "hexo-generator-searchdb": "^1.2.0",
    "hexo-generator-sitemap": "^2.0.0",
    "hexo-generator-tag": "^1.0.0",
    "hexo-neat": "^1.0.4",
    "hexo-renderer-ejs": "^1.0.0",
    "hexo-renderer-marked": "^2.0.0",
    "hexo-renderer-stylus": "^1.1.0",
    "hexo-server": "^1.0.0",
    "hexo-symbols-count-time": "^0.7.0",
    "hexo-wordcount": "^6.0.1"
  },
  "devDependencies": {}
}
```

>  观看下篇系列博文 更换主题

