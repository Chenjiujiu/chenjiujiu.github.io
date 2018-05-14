---
title: Hexo 从零开始搭建静态博客
comments: true
mathjax: false
date: 2017-09-11 01:31:18
tags: hexo
id: rjaz-0911013118
categories: 软件安装
keywords: 个人博客搭建、hexo教程、next主题配置、hexo自定义
description: 基于hexo搭建个人静态博客基础教程
---

## 前言
{% note info %}折腾了几天把博客从Wordpress转到hexo 也从自己的空间转到了Giuhub，其中遇到了各种各样的问题，在此记录一下基本安装流程{% endnote %}
<!--more-->
## 1.hexo简单介绍
开发者：  
Hexo是一个开源的静态博客生成器,用node.js开发,作者是台湾[tommy351](https://zespia.tw)   

是什么：  
她是一个快速、简洁且高效的博客框架。hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页  
官网：[hexo](https://hexo.io)  
api:[hexo文档](https://hexo.io/api/)  
## 2.准备工具
1. 安装[node](https://nodejs.org/zh-cn/)
2. 安装[git](https://nodejs.org/zh-cn/)
3. 注册一个[github](https://github.com)账号  
还没有安装node或者git的小伙伴可以点击链接查看安装指南，在这就不重复介绍了

## 3.下载和安装hexo
使用npm下载并全局安装：
```
    npm install -g hexo
```
hexo的Github地址[heox](https://github.com/hexojs/hexo)  
1. 创建一个文件夹，（不要使用中文名字，会出各种各样的问题，你懂得）   

2. 打开命令窗口cd进入刚刚创建的文件夹,window推荐使用git Bash  
如果你的文件夹路径比较深的话，window可以在目标文件夹内按住shift然后单击鼠标右键，在此处打开命令窗口mac进入文件夹复制文件路径然后把文件路径粘贴到cd 命令之后即可 

3. 运行`hexo init` 如果没有报错的话就已经搭建成功了，是不是很简单啊

4. hexo文件目录：
- node_modules：是依赖包
- public：存放的是生成的页面
- scaffolds：命令生成文章等的模板
- source：用命令创建的各种文章
- themes：主题
- _config.yml：整个博客的配置
- db.json：存放一些统计数据之类的
- package.json：项目所需模块项目的配置信息

## 4.运行查看
搭建成功后你就可以在本地查看你的blog了，继续在命令窗口输入一下命令 注释不要
```
hexo clean  //清空之前生成的文件
hexo generate //编译生成静态网页
hexo server //启动本地服务器
```
没报错的话会提示你会看到这句  
`INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.`
接着就是打开浏览器输入localhost:4000 来查看你的blog
![hexo](http://blog.chenjiujiu.com/images/pic-hexo/hexo-themes.jpeg)

## 5.更换主题
hexo 默认主题是landscape，你可以在hexo文件内的themes文件夹内看到这个主题  
更换主题,博主采用的是[nexT6.0](https://github.com/theme-next/hexo-theme-next) 主题,简约风格计较适合程序员,集成多个插件。爽的不要不要的,可以根据自己喜好来寻找更换也可以自己制作,这里提供官网的[主题](https://hexo.io/themes/)链接,Github上也提供了大量主题  
 
### 1.1下载方式
两种方式，下载主题文件或者从github上克隆

#### 1.1.1直接下载
直接下载主题文件包，解压到根目录下的/themes文件内，下载一半文件名都带版本号，最好是把文件夹版本号去掉

#### 1.1.2克隆
以next为例，其它主题操作都大同小异  
命令行输入以下代码
```
$ cd hexo
$ git clone https://github.com/theme-next/hexo-theme-next themes/next
```

## 6.部署
blog当然不能只有本次查看了，所以需要部署到线上这里提供两种部署方式，github和云虚拟主机

### 6.1部署到Github
Github每个账户有一个免费的静态站点不用白不用

#### 6.1.1 准备账号
 首先需要有github账号，没有的注册一个
 
#### 6.1.2 创建仓储
新建一个仓储![新建仓储](http://blog.chenjiujiu.com/images/pic-hexo/newRepository.png)
用Github名字.github.io的格式来命名你的仓储，格式必须是这样![仓储名字](http://blog.chenjiujiu.com/images/pic-hexo/newRepository2.png)

#### 6.1.4配置git信息
配置Github基本信息，具体请在本站右边搜索-Github简单入门

#### 6.1.5安装部署插件
由于hexo现在版本吧部署插件分开了，所以需要单独安装部署插件,很简单
```
依旧是使用npm安装，速度慢可以使用cnpm
npm install hexo-deployer-git --save
```
你也可以从github上安装最新版本  
```
$ npm install git+ssh://git@github.com:hexojs/hexo-deployer-git.git --save
```
地址:[hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)  
建议安装新版本，后续部署源码可以简便一下

#### 6.1.6配置项目文件 
用编辑器打开你的blog项目，修改_config.yml文件的一些配置(冒号之后都是有一个半角空格的)：把地址更换成你刚刚新建的仓储地址，建议是用来git协议来传输
```
deploy:
  type: git
  repo: https://github.com/YourgithubName/YourgithubName.github.io.git
  branch: master
  
以下是使用git协议
deploy:
  type: git
  repo: git@github.com:YourgithubName/YourgithubName.github.io.git
  branch: master
```

#### 6.1.7开始部署代码
命令行输入以下命令来完成部署
```
hexo clean 
hexo generate
hexo deploy
```
#### 6.1.8查看部署结果
在浏览器中输入`http://yourgithubname.github.io`就可以看到你的个人博客啦，是不是很兴奋！

### 6.2部署到BCH
需要有一个云虚拟主机
#### 6.2.1安装插件
依旧是相同的 先安装部署插件
```
依旧是使用npm安装，速度慢可以使用cnpm
npm install hexo-deployer-ftpsync --save 
```
带上github地址 [hexo-deployer-ftpsync](https://github.com/hexojs/hexo-deployer-ftpsync)
#### 6.2.2配置项目文件 
用编辑器打开你的blog项目，修改_config.yml文件的一些配置(冒号之后都是有一个半角空格的)：
```
deploy:
  type: ftpsync
  host: <host>            #主机地址
  user: <user>            #用户名
  pass: <password>        #密码
  remote: [remote]        #上传到空间的指定目录。比如/public_html/。默认为/
  port: [port]            #端口，默认为21
  ignore: [ignore]        #忽略的文件
  connections: [connections]  #使用的连接数，默认1
  verbose: [true|false]   #显示调试信息，默认false
```
需要注意的是:hexo-deployer-ftpsync插件并非直接上传生成的静态网站，而是将remote指向的目录的目录结构同步成与本地public文件完全相同的目录结构。也就是说如果你的虚拟主机上存有一些其他文件的话也将被全部删除。所以必要时，可以填写ignore键的值,或者使用子目录.
ignore格式：   
ignore: ['dir1','dir2','file1.htm']  
#### 6.2.3预览
输入你的虚拟主机对应的域名就可以查看到你不blog了 

## 7.源文件备份
如果你是下载了最新版本的部署插件的话则特别简单
只需要在部署参数这样设置即可
```
deploy:
  - type: git
    repo: git@github.com:yougithubname/yougithubname.github.io.git
    branch: master
  - type: git
    repo: git@github.com:yougithubname/yougithubname.github.io.git
    branch: src
    extend_dirs: /
    ignore_hidden: false
    ignore_pattern:
        public: .
```

## 8.域名绑定
如果是部署在github上可以使用github给的域名或者绑定自己的,域名方法如下  
1. 在你的域名提供商处给你的域名添加一个CNAME解析，解析地址是`yougithubname.github.io`

2. 然后到你博客 根目录/source 目录下创建一个新文件CNAME 
在里面写上你自己的域名，比如我的是blog.chenjijiu.com，就直接在CNAME文件中写上这个地址就好了。
注意：CNAME需要大写，文件无后缀名，怎么添加无后缀名的文件自行百度吧
3. 然后执行以下`hexo g`,`hexo d`，让后访问你自己的地址就可以跳转到博客了。  
`hexo g`就是`hexo generate`的简写，相同的`hexo d`是`hexo deploy`的简写
          
 
