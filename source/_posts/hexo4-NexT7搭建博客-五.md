---
title: 快速搭建博客：写作技巧
copyright: true
description: 搭建博客系列：写作技巧，非常方便
abbrlink: 6d171bb6
date: 2019-12-27 22:46:55
tags: hexo
categories: hexo
top: 98
---

## 1. Hexo添加文章时自动打开编辑器

- 首先在站点目录下的scripts目录中创建一个JavaScript脚本文件。如果没有这个scripts目录，则新建一个。

- scripts目录新建的JavaScript脚本文件可以任意取名。



通过这个脚本，我们用其来监听`hexo new`这个动作，并在检测到`hexo new`之后，执行编辑器打开的命令。

- 如果你是windows平台的Hexo用户，则将下列内容写入你的脚本：（直接复制，不用改）



```js
var spawn = require('child_process').exec;
hexo.on('new', function(data){
  spawn('start  "markdown编辑器绝对路径.exe" ' + data.path);
});

```



- 如果你是Mac平台Hexo用户，则将下列内容写入你的脚本：（直接复制，不用改）



```
var exec = require('child_process').exec;
hexo.on('new', function(data){
    exec('open -a "markdown编辑器绝对路径.app" ' + data.path);
});
```



## 2. 新建文章时，在相同目录下创建同名文件夹（便于图片管理）

- 打开站点配置文件`_config.yml`，搜索`post_asset_folder`字段，设置其值为`true`
- 安装hexo-asset-image：`npm install hexo-asset-image --save`
- 此时`hexo new "fileName"`会在`/source/_posts`目录下创建同名的文件夹
- 只需在 md 文件里使用 `![title](图片名.jpg)` ，无需路径名就可以插入图片。但是我们会使用图床，存到这只是为了以后图床失效可以找到图片。



## 3. 图床

图床有很多，比如阿里云，[腾讯云](https://cloud.tencent.com/)，[又拍云](https://www.upyun.com/)等，这些都是可以用的。我使用的是阿里云的OSS来存储图床。

**也可以把图床存在CSDN，从CSDN文章copy过去，但是切记要本地保存，别以后CSDN失效找不到图。**

到[阿里云](https://oss.console.aliyun.com/)，没注册的先去注册一下。然后如下图：

![1](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122811/20191228114909-744979.png)

- 先去购买流量包，不贵，一年40G才9元。



![2](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122811/20191228115059-750472.png)

![3](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122811/20191228115107-460467.png)

- 然后去创建一个Bucket，然后如下填入就创建。



![4](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122811/20191228115459-452228.png)

![5](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122811/20191228115509-645533.png)

- 顺便点开右上角我们的头像，如图：点开AccessKey管理



![6](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122811/20191228115910-637021.png)



- 点开完，出现下面的这个，如果有用户AccessKey就把它的AccessKey ID和Access Key Secret保存到txt，我记得刚创建会自动下载一个文件，该文件就保存这两个键值对。
- 如果没有，则创建该用户，然后就会弹出一个下载框下载一个文件。



![7](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122812/20191228120208-577925.png)

![8](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20200113/20200113174134-81243.png)

- 然后先放着，下面的**自动变图床链接的编译器**要用





## 4. 自动变图床链接的编译器

我是在网上发现一个typora的插件，可以自动把上传的本地连接。

[typora下载](https://www.lanzous.com/i8brcxg)，也可以去[官网](https://typora.io/)，但是官网加载好慢。。

[原作者](https://github.com/Thobian/typora-plugins-win-img)，我fork到[我的github](https://github.com/magicflung/typora-plugins-win-img)，测试了一下，最新版能够用，下面是我的教程：

- 先把该项目下载下来，然后里面有window.html和plugins两个文件。然后我在我的github这样写。



![8](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122812/20191228122715-350912.png)

- 最后，得去配置好我们的图床，在`plugins\image`的upload.js文件，该文件就是可以配置我们的图床。把刚刚在阿里云下载的文件中，两个AccessKeyId和AccessKeySecret的值复制进去。



![9](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122812/20191228121525-979618.png)

- 而BucketDomain需要去阿里云的OSS中的文件管理，先随便上传一张图片，然后点击看看它的链接，如图：



![10](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122812/20191228121712-240809.png)

- 然后把类似`http://flunggg.oss-cn-shenzhen.aliyuncs.com/`复制到BucketDomain去。
- 这个upload.js文件可以自定义配置很多东西，比如我上面限制上传文件大小最多为10MB，不过一般不要这么大，最多4MB左右，或者去压缩一下，推荐一个压缩网站：[tinyjpg](https://tinyjpg.com/)。太大的可以去压缩一下再放到图床。



![11](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20200113/20200113174421-292473.png)



**注意：可能有时对于旧文章想上传，鼠标点一下，等候一下就会上传，如果没有上传则重新本地上传就会自动变为图床链接。并且再改变图片样式时不成功，我想把图片缩小点就不成功。**但是确实方便很多。

参考：

[Hexo添加文章时自动打开编辑器](https://blog.csdn.net/weixin_39345384/article/details/80785373)

