---
title: hexo-Travis CI 持续集成部署hexo
comments: true
mathjax: false
date: 2017-09-14 21:22:47
tags: hexo
id: rjaz-0914212247
categories: 软件安装
keywords: hexo持续集成、Travis CI、hexo部署、hexo源码管理
description: 使用Travis CI持续集成部署hexo到github
---
## 前言
{% note info %} 刚把博客迁移到github上，但是hexo只是把编译之后的文件发布到github上，博客原文件只是保存在本地机器上，不方便切换机器以及博客源码的备份，网上找了几种办法，至于为什么最后还是用持续集成做且看下面分析 {% endnote %}

<!--more-->

## hexo源码备份管理
bolg源码管理方法大概有这几种把  
1. 使用网盘来管理;但是每次换电脑都要下载一遍，每次写完了也需要上传来更新.

2. 使用github来管理; hexo本来就是搭在github上，打在云虚拟机上另外再说，使用github来管理好处不用再多说了，具体用法就是把源文件push到博客的分支上，用分支来保存源文件，master分支来保存编译后的静态页面；

3. 使用最新'hexo-deployer-git'，由于没有发布在npm上，所以只能从git上安装,支持源码和博客的部署,代码如下
```
deploy:
  - type: git
    repo: git@github.com:<username>/<username>.github.io.git
    branch: master
  - type: git
    repo: git@github.com:<username>/<username>.github.io.git
    branch: src
    extend_dirs: /
    ignore_hidden: false
    ignore_pattern:
        public: .
```
但是发现一个问题，首次部署时候正常，但是之后部署就回吧主题文件themes也发布到master分支上。除非你每次部署的时候的手动删除.deploy文件夹,猜测原因是因为源码部署吧themes添加到了.deploy文件夹种，导致下一次会把主题文件部署上，如果在忽略文件种把themes文件忽略的话源码的提交也会忽略themes文件，暂时没找到解决办法，如有施主知道如何解决麻烦在下方留言给贫道，这种写法没法中间添加操作，所以看第四种吧。

4. 由于hexo生成出来的静态页面带有很多的空格，那么久需要我们对生成的文件来进行压缩，压缩之后再发布以及上传源码，一系列操作每次写完博客还是挺烦的

```
hexo clean；
git g；
gulp；
hexo d；
git add .
git commit --all -m 'updata'
git push
```
这个世界不愧是懒人驱动的啊!使用持续集成来吧操作简化，本地只需要负责提交更新到github，由travis得虚拟机来完成一些列的清空，压缩，重新编译，以及部署的操作，下面就开始具体介绍了

## 1.什么是Travis CI
Travis CI 是一个的开源持续集成构建项目，它与jenkins的区别在于采用了yaml格式，不需要再本地搭建服务器，并且是高度集成 GitHub 的，所以对于开源项目还是非常友好的。
在github上，添加travis ci，当添加的项目有code push的时候，会推送通知到travis，随之根据设置好的脚本文件来执行一系列的程序。

## 2.准备条件
有一个github账号并且搭建好了hexo的静态页面

## 3.注册Travis CI
第一步总是很容易的,打开[Travis CI](https://travis-ci.org)官网,注册或者直接是哟github第三方登录
## 4.添加Travis CI管理的项目
在右上角你的账户名点击进入 profile,在这可以看到你的github里面的项目,选择是需要进行持续集成的项目
![addprofile](https://blog.chenjiujiu.com/images/pic-hexo/addprofile.png)
如果没有看到你的项目你可以是有左边de在Repositories tab页点击Sync now同步你的github项目。选中项目将默认的off改变为on开启项目的持续集成。  
![addprofile](https://blog.chenjiujiu.com/images/pic-hexo/openfile.png)  
如果没有你的项目那么应该是还没同步过来点击同步按钮来实现同步
![addprofile](https://blog.chenjiujiu.com/images/pic-hexo/syncgithub.png)  
然后点击设置,勾选下面者这几项  
![addprofile](https://blog.chenjiujiu.com/images/pic-hexo/travis.png)  
到此 Travis虽然读取到了github文件但是却没权限进行修改和push,我们需要为travis添加一个Access Token


## 5.创建一个新的Access Token
在Github的setting页面，左侧面板选择Developer settings然后Personal access tokens, 右上角点击Generate new token。生成token时候需要确定访问scope，这里我们选择第一个repo即可。重要：生成的token只有第一次可见，一定要保存下来备用。
![createtoken](https://blog.chenjiujiu.com/images/pic-hexo/createtoken.png)

## 6.加密Token
### (可选操作：除非你对于Travis CI保存你的密钥不信任，才需要做，否则可以直接跳过省事)

1. 准备Travis命令行工具，需要依赖ruby环境。对于Windows环境，可以使用[这里](https://rubyinstaller.org/downloads/)的安装包，安装完成后可用`ruby -v`检查。  
请安装2.4.x版本。  
![ruby](https://blog.chenjiujiu.com/images/pic-hexo/ruby-v.png)
**注意:**不要安装2.5版本。暂时不支持。会报错:  
```
ERROR: Error installing travis: The last version of ffi (>= 1.3.0) to support your Ruby & RubyGems was 1.9.18. Try installing it with gem install ffi -v 1.9.18 and then running the current command again ffi requires Ruby version < 2.5, >= 2.0. The current ruby version is 2.5.0.
```

2. 安装命令行工具，参考[这里](https://github.com/travis-ci/travis.rb#installation)官方文档：  
```
gem install travis
```
安装慢的可以使用国内镜像安装 [Ruby China](http://gems.ruby-china.org)
3. 验证travis
使用travis -v来验证travis是否安装成功，
如果显示版本号则表示安装成功，否则安装失败，重复1，2

4. 登录加密token 
输入travis login，会提示你需要输入github用户名和密码，

```
$ travis login
We need your GitHub login to identify you.
This information will not be sent to Travis CI, only to api.github.com.
The password will not be displayed.
Try running with --github-token or --auto if you don't want to enter your password anyway.
Username: xxx@xxx.xxx
Password for xxx@xxx.xxx: 
Successfully logged in as demo!

```
登陆成功后，开始加密，代码如下：

```
travis encrypt -r <github name>/<github repo> GH_Token=XXX
```
加密成功会生成一串秘钥，例如：  
```
Please add the following to your .travis.yml file:
  secure: "OrEeqU0z6GJdC6Sx/XI7AMiQ8NM9GwPpZkVDq6cBHcD6OlSppkSwm6JvopTR\newLDTdtbk/dxKurUzwTeRbplIEe9DiyVDCzkeEiJGfgfq7woh+GRo+q6+UIWLE\n3nowpI9AzXt7iB
```
把输出的secure:"xxxx"复制保存，

## 7.添加token
把获取到的token或者加密后的token设置到travis上，如下name地方随便输入一个token名字，value地方输入你的token或者加密后的token
![addtoken](https://blog.chenjiujiu.com/images/pic-hexo/addtoken.png)

## 8.配置gulpfile
因为hexo编译出来的html有大量空白，并且css，js文件都没有进行枷锁，为了性能，使用gulp来对文件进行压缩，当然你可以使用其他方法，由于gulp是在travis虚拟机上运行，本地就没必要再安装了，当然你感兴趣想试试就随便了，配置gulofil.js文件
```
var gulp = require('gulp');
var cleancss = require('gulp-clean-css');
var uglify = require('gulp-uglify');
var htmlmin = require('gulp-htmlmin');
var htmlclean = require('gulp-htmlclean');
var imagemin = require('gulp-imagemin');

 // 压缩 public 目录 css
 gulp.task('minify-css', function() {
  return gulp.src('./public/**/*.css')
      .pipe(cleancss())
      .pipe(gulp.dest('./public'));
});
// 压缩 public 目录 html
gulp.task('minify-html', function() {
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
// 压缩 public/js 目录 js
gulp.task('minify-js', function() {
  return gulp.src('./public/**/*.js')
      .pipe(uglify())
      .pipe(gulp.dest('./public'));
});

  // 压缩图片任务
    // 在命令行输入 gulp images 启动此任务
    gulp.task('images', function () {
      // 1. 找到图片
      gulp.src('./images/**/*.*')
      // 2. 压缩图片
          .pipe(imagemin({
              progressive: true
          }))
      // 3. 另存图片
          .pipe(gulp.dest('./images'))
  });

  // 执行 gulp 命令时执行的任务
  gulp.task('default', [
      'minify-html','minify-css','minify-js','images'
  ]);
```
然后在package.json文件中的dependencies内添加gulp运行依赖的包名
```
  "dependencies": {
  //添加的文件依赖
    "gulp": "^3.9.1",
    "gulp-clean-css": "^3.9.4",
    "gulp-htmlclean": "^2.7.22",
    "gulp-htmlmin": "^4.0.0",
    "gulp-imagemin": "^4.1.0",
    "gulp-uglify": "^3.0.0",
  //end
    "hexo": "^3.2.0",
    "hexo-generator-archive": "^0.1.4",
    "hexo-generator-category": "^0.1.3",
    "hexo-generator-index": "^0.2.0",
    "hexo-generator-tag": "^0.2.0",
    "hexo-renderer-ejs": "^0.3.0",
    "hexo-renderer-marked": "^0.3.0",
    "hexo-renderer-stylus": "^0.3.1",
    "hexo-server": "^0.2.0"
  }
```
这样虚拟机在执行npm init的时候救会把gulp所依赖的包安装上

## 9.配置.travis.yml
在博客根目录创建文件.travis.yml,文件种写travis执行的程序代码,可以参考我的
```
language: node_js  #使用的语言
node_js: stable  #版本

cache:
    apt: true
    directories:
        - node_modules # 设置缓存不经常更改的内容
before_install:
    - export TZ='Asia/Beijin' # 更改时区
install:
  - npm install  #安装hexo及插件
  - npm install -g gulp #安装gulp
script:
  - hexo clean  #清除原先的静态文件
  - hexo g && gulp #重新生成并压缩
after_script:
  - cd ./public
  - git init
  - git config user.name "chenjiujiu" 
  - git config user.email "cjiujiu@icloud.com"  
  - git add .
  - git commit -m "Travis CI Auto Builder at `date +"%Y-%m-%d %H:%M"`"  # 提交记录包含时间 跟上面更改时区配合
  - git push --force --quiet "https://${ci}@${GH_REF}" master:master  #ci
  是在Travis中配置环境变量的名称
branches:
  only:
    - src
env:
 global:
   - GH_REF: github.com/chenjiujiu/chenjiujiu.github.io.git  
    #设置GH_REF，注意更改yourname
```
## 10.到这就差不多结束了

在本地修改文件然后使用git命令提交到github源码仓库
```
git add .
git commit -am "测试"
git push -u origin master:src
```
等待几分钟后，travis会自动把public里面的静态文件push到master分支。blog成功部署
你可以可以使用travis把博客部署到多个地方，在.travis.yml后继续添加命令即可

## 后话
持续集成有点忙，毕竟需要在虚拟机中clone你的github仓库，然后安装插件，再编译运行，然后把结果push回github，但是这个不影响博客发布了，在本地使用hexo s -debug 测试好 就好了，没那么急这一两分钟上线