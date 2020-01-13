---
title: 快速搭建博客：优化博客
copyright: true
description: 搭建博客系列：优化博客
abbrlink: f41853c4
date: 2019-12-28 16:39:23
tags: hexo
categories: hexo
top:
---

## 1. 域名绑定

### 1.1 GitHub仓库的域名绑定

- 如果一直打算把博客部署在GitHub上，默认就是部署在这里，那么我们来绑定一下域名。

首先先去买个域名，[阿里云的万网](https://wanwang.aliyun.com/)（我是在这个搞的），[腾讯云](https://dnspod.cloud.tencent.com/)、[百度云](https://cloud.baidu.com/product/bcd.html)等都可以，这我在前面很早就有说。

打开你部署在github的博客仓库，然后点击Settings，看到Repository name，复制下面那个。

![1](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122816/20191228165411-448783.png)

在阿里云点开你的域名解析：我配置的是@，也就是www.xxx.cn和xxx.cn都可以访问，你可以自己按自己的方式。

![2](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122816/20191228165615-800176.png)

![](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122816/20191228165621-963532.png)

然后点击左边那个蓝色按钮，添加记录。

![3](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122816/20191228165625-682867.png)

然后再回到刚刚的github你博客仓库的setting中，往下拉。

![4](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122816/20191228165756-585323.png)

最后，在自己本地的站点目录的source文件中，创建一个CNAME文件，然后在里面填上我们的域名。因为刚开始在github那边填上我们的域名，就会在仓库生成一个CNAME文件，而我们本地博客没有，每次上传时都会把它删了，所以得在本地创建一个这样的文件。

![5](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122817/20191228170240-899062.png)

访问试试成功了没有。

### 1.2 阿里云的域名绑定

这个最简单了，提前是自己要有阿里云的服务器或者别的服务器。然后去获取服务器的公网IP地址。比如：我的阿里云服务器的公网。

![6](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122817/20191228170757-391535.png)

然后回到刚刚域名解析那边，如图：

![7](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122817/20191228171050-107982.png)

如果服务器没有装nginx或者tomcat的话，访问不到什么，所以看下一步。

## 2. 将博客部署到阿里云服务器

这一步我走了很多坑。有时候是githooks不起作用，有时候是nginx问题。

### 2.1 本地电脑连接阿里云

[阿里云入门](https://blog.csdn.net/weixin_41800884/article/details/103148518)

[阿里云本地SSH连接](https://blog.csdn.net/weixin_41800884/article/details/103148850)

这里额外补充，可以下载一个[WinSCP](https://winscp.net/eng/docs/lang:chs)的可视连接程序。

### 2.2 安装nginx

最好跟我安装一样版本的nginx。看下面的博文，也有某些nginx出错的解决方法。

[安装nginx](https://blog.csdn.net/weixin_41800884/article/details/103545128)

然后访问公网ip看看有没有出现什么，有出现nginx的首页就说明成功。

### 2.3 博客部署到阿里云服务器

#### 2.3.1 原理

在本地编辑文本，然后使用Git远程部署到VPS的Git仓库。`hexo d`命令实际上只deploy了本地的public文件夹，Git Hooks实际上就是当Git仓库收到最新的push时，将Git仓库接受到的内容复制到VPS上的网站目录内。相当于完成了手动将public文件夹复制到VPS的网站根目录里。

下面这张是用别人的图，该原理就是这样：[原地址](https://segmentfault.com/a/1190000016106584)

![8](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122819/20191228193136-962690.png)

#### 2.3.2 安装配置Git

连接上我们的远程阿里云服务器，然后执行：

```shell
sudo yum install git
```

等待下载，查看是否成功：

```shell
git --version
```

#### 2.3.3 创建git用户以及配置免密登录

- 创建一个名为git的用户。不是我们github上的那个git用户。



```shell
adduser git
```

假如没有提示输入密码,则

```shell
passwd git
```

输入自定义密码，以后登录该名为git的用户要用的。

- 然后，赋给git用户sudo权限



```shell
chmod 740 /etc/sudoers
vim /etc/sudoers
```

找到如下内容：

```shell
# User privilege specification
root    ALL=(ALL:ALL) ALL
```

然后在root下面添加：

```shell
# User privilege specification
root    ALL=(ALL:ALL) ALL
git     ALL=(ALL:ALL) ALL
```

然后按下ESC键，加上 ; 再输入wq保存退出。这应该不用我说了，vim编译器的指令。

然后修改回文件的权限：

```shell
chmod 440 /etc/sudoers
```

- 切换到git用户



```shell
su git						// 切换到git用户
cd /home/git				// 切换到git用户目录
mkdir -p repo/myblog.git	// 创建git仓库文件夹，以myblog.git为例
cd repo/myblog.git			// 切换到repo目录,进入仓库目录
git init --bare				// 使用--bare参数初始化为裸仓库，这样创建的仓库不包含工作区
```

创建网站目录shell

```shell
cd /home/git 				// 切换到git用户目录
mkdir -p hexo/blog			// 创建目录，用来存放博客的静态文件
```

- 配置ssh



回到我们的本地主机，假设你电脑已经早有配置git密匙，在桌面右键点开Git Bash，输入：

```git
cat ~/.ssh/id_rsa.pub
```

然后就会出现你的git密匙，复制，然后回到远程阿里云服务器，输入：

```
cd /home/git				// 切换到git用户目录
mkdir .ssh				//创建.ssh目录
cd .ssh
sudo vim authorized_keys
```

然后对着vim打开的黑框右键一下，就把刚刚复制的密匙复制过去了，但是注意：有可能在最前面有可能会漏掉几个字符，自己填上去。最后

```
按ESC，然后按分号键; 输入wq
```

![9](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122819/20191228195944-185026.png)

- 确认文件夹的权限



```
sudo chown git:git -R /home/hexo/blog
sudo chown git:git -R /home/git/myblog.git
#然后查看这两个文件夹是不是都属于git:git
ll /home/git/
ll /var/www/
```

- 然后,到这里,就先验证vps是否能用git用户ssh免密登陆.在本地windows打开git bash,输入命令登陆

```
ssh git@你的服务器的公网ip或者域名
```

首次出现要输入yes。如果有出现要输入密码就成功了，不成功再找我，我在这也走了很多坑。

- 然后再配置免密码登录



```shell
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

#### 2.3.4 配置Nginx

来到Nginx的目录下的默认配置文件：

```
cd /etc/nginx/conf.d/
vim default.conf
```

如果出现这个，别理，直接按Enter。

![10](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/2019122820/20191228201653-975400.png)

然后就出现下面这个： 改两处就行。

```
server {
    listen  80; # 这里就是要在阿里云开放80端口
    server_name  flunggg.cn www.flunggg.cn;  # 默认是你的公网，如果有域名填写你的域名，因为我域名解析是@，所以有www和无www都可以，全配上去。
    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;
    return 301 https://$server_name$request_uri; # 以后转https用的
    location / {
        root   /home/git/hexo/blog/; # 改这里
        index  index.html index.htm;
    }

    error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```

然后保存退出。

**这里教一个强制保存退出的，就是在wq加个！。**

```
按ESC，然后按分号键; 输入wq!
```

这是http的，如果要配置https，加SSL证书看这里，现在都是https的网站我建议配上好。

[给博客配置上https](https://blog.csdn.net/weixin_41800884/article/details/103545130)

我现在用的是https，也就是有SSL证书，http和https的两个配置文件可以共存，就是需要做http的强制跳转，也就是我上面代码那句。

#### 2.3.5 配置Git Hooks

简称钩子，我们在本地推送时，即会部署到服务器，也会因为git hooks部署到github存储。

但是我照着网上的资料配置，没有用。所以我换另一种方式。在下一步。

#### 2.3.6 本地配置

最后还需要在站点目录配置文件_config.yml中，找到下面代码：在repo中配置两个，这样就可以推送到两个地方。最后一个type: baidu_url_submitter先别理。主要改的就是repo那三行代码。

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
- type: git
  repo: 
    github: git@github.com:你的github名/你的github名.github.io.git
    vps: git@你的域名或者服务器公网IP:/home/git/repo/myblog.git
  branch: master
- type: baidu_url_submitter
```

最后就可以用你的域名或者公网ip访问博客了，上面配置的github也是可以访问的，但是只拿它来做个存储。



## 3. 使用gulp压缩资源

在站点目录右键打开Git Bash，输入：

```
npm install -d --save gulp gulp-clean gulp-htmlclean gulp-htmlmin gulp-minify-css gulp-uglify
```

等待下载安装，然后在站点目录下创建一个文件：gulpfile.js。然后添上：

```js
var gulp = require('gulp');

//Plugins模块获取
var minifycss = require('gulp-minify-css');
var uglify = require('gulp-uglify');
var htmlmin = require('gulp-htmlmin');
var htmlclean = require('gulp-htmlclean');

//压缩css
gulp.task('minify-css',  async function () {
return gulp.src('./public/**/*.css')
.pipe(minifycss())
.pipe(gulp.dest('./public'));
});

//压缩html
gulp.task('minify-html',  async function () {
return gulp.src('./public/**/*.html')
.pipe(htmlclean())
.pipe(htmlmin({
removeComments: true,
minifyJS: true,
minifyCSS: true,
minifyURLs: true,
}))

.pipe(gulp.dest('./public'))
});

//压缩js 不压缩min.js
gulp.task('minify-js',  async function () {
return gulp.src(['./public/**/*.js', '!./public/**/*.min.js'])
.pipe(uglify())
.pipe(gulp.dest('./public'));
});

// 压缩 public/images 目录内图片(Version>3)
gulp.task('minify-images', async function (done) {
    gulp.src('./public/images/**/*.*')
        .pipe(imagemin([
            imagemin.gifsicle({interlaced: true}),
            imagemin.jpegtran({progressive: true}),
            imagemin.optipng({optimizationLevel: 5}),
            imagemin.svgo({
                plugins: [
                    {removeViewBox: true},
                    {cleanupIDs: false}
                ]
            })
        ]))
        .pipe(gulp.dest('./public/images'));
    done();
});
// 开始任务
gulp.task('default', gulp.parallel('minify-html', 'minify-css',  async function() {
  // Do something after a, b, and c are finished.
}));

```

然后以后每次推送得使用四条指令

```
hexo clean && hexo g && gulp && hexo d
```

### 3.1 一键部署

在`package.json`中加入如下`script`，看清楚，需要一个逗号分隔{}

```
,
"scripts": {
  "push": "hexo clean && hexo g && gulp && hexo d"
}
```


然后在部署的时候只需要运行

```
npm run push
```



参考：

[hexo部署到vps](https://ihaoming.top/archives/c9c37af.html)