---
title: hexo Next主题配置教程
comments: true
mathjax: false
date: 2017-09-12 10:31:48
tags: hexo
id: rjaz-0912103148
categories: 软件安装
keywords: next配置、hexo配置、next自定义、next美化
description: 最详细的Next主题配置教程
---
## 前言
{% note info %} 依照惯例来一个前言语，上一篇文章中写了hexo的入门，但是hexo默认的主题啊设置啊大大不满足我们的需求，那我们就来进行一些自定义的配置吧{% endnote %}
<!--more-->

博主用的是[next最新版](https://github.com/theme-next/hexo-theme-next)  
官方主题[themes](https://hexo.io/themes/)   
就用next6.0为例，其它主题操作都大同小异 
next基础配置有文档，都是中文的比较详细，我这就不再重复了   
[nexT中文文档](http://theme-next.iissnan.com/third-party-services.html)

## 1.在右上角或者左上角fork me on github
1. 实现效果图:  
![fork-me-on-github](http://blog.chenjiujiu.com/images/pic-hexo/fork-me-on-github.png)  
2. 挑选样式:  
点击[这里](https://blog.github.com/2008-12-19-github-ribbons/)或者[这里](http://tholman.com/github-corners/)挑选自己喜欢的样式，并复制代码
3. 实现方法:
然后粘贴刚才复制的代码到`themes/next/layout/_layout.swig`文件中(放在`<div class="headband"></div>`的下面)，并把href改为你的github地址  
![代码插入](http://blog.chenjiujiu.com/images/pic-hexo/forke-me-pic.png)

## 2.点击出现桃心效果
效果图:  
![点击桃心效果图](http://blog.chenjiujiu.com/images/pic-hexo/click-love-show.gif)  
实现方法:  
1. 打下这个[网址](http://7u2ss1.com1.z0.glb.clouddn.com/love.js)复制页面的代码  

2. 在`/themes/next/source/js/src`里面新建love.js文件并且将刚刚的代码粘贴进去，然后保存。

3. 然后打开`\themes\next\layout\_layout.swig`文件,在末尾（在前面引用会出现找不到的bug）添加以下代码：
```
<!-- 页面点击小红心 -->
<script type="text/javascript" src="/js/src/love.js"></script>
```
## 3.修改文章底部标签符号
效果：
![底部标签图](http://blog.chenjiujiu.com/images/pic-hexo/link-show.png)  
方法：  
修改模板`/themes/next/layout/_macro/post.swig`，搜索 `rel="tag">#`，将`#`换成`<i class="fa fa-tag"></i>`

## 4.文章末尾统一添加“本文结束”标记
效果:  
![底部结束图](http://blog.chenjiujiu.com/images/pic-hexo/end-show.png)  
方法：  
1. 在路径 `\themes\next\layout\_macro` 中新建 `passage-end-tag.swig`文件,并添加以下内容：
```
<div>
    {% if not is_index %}
        <div style="text-align:center;color: #ccc;font-size:14px;">-------------本文结束<i class="fa fa-paw"></i>感谢您的阅读-------------</div>
    {% endif %}
</div>
```
2. 打开`\themes\next\layout\_macro\post.swig`文件，在post-body 之后， post-footer 之前添加如下代码（post-footer之前两个DIV）
```
<div>
  {% if not is_index %}
    {% include 'passage-end-tag.swig' %}
  {% endif %}
</div>
```
3. 然后打开主题配置文件（_config.yml),在末尾添加：
```
# 文章末尾添加“本文结束”标记
passage_end_tag:
  enabled: true
```

## 5.修改打赏字体不闪动
修改文件`next/source/css/_common/components/post/post-reward.styl`，然后注释其中的效果`wechat:hover`和`alipay:hover`，如下：
```
/* 注释文字闪动效果
 #wechat:hover p{
    animation: roll 0.1s infinite linear;
    -webkit-animation: roll 0.1s infinite linear;
    -moz-animation: roll 0.1s infinite linear;
}
 #alipay:hover p{
   animation: roll 0.1s infinite linear;
    -webkit-animation: roll 0.1s infinite linear;
    -moz-animation: roll 0.1s infinite linear;
}
*/
```

## 6.切换中文
nexT5.0 打开主题配置文件，最开始设置字段language值为zh-Hans 
nexT6.0 打开主题配置文件，最开始设置字段language值为zh-CN  

注意：6.0 的中文模式下如果页面设置了摘要，则在点击阅读全文是控制台会抱一个错，原因是因为地址栏的中文'更多'字样被编码成url了，解决办法就是进入`themes/next/languages`中吧zh-CN文件的`more: 更多`改为`more: more`

## 7.站点配置文件翻译
```
# Site <站点信息>
title: <标题>
subtitle: <副标题>
description: <描述>
keywords: <关键字>
author: <作者名字>
language: <语言>
timezone: <时区 默认您的电脑时区>

# URL <网址>
## 如果您的网站存放在子目录中，例如 http://yoursite.com/blog，则请将您的 url 设为 http://yoursite.com/blog 并把 root 设为 /blog/。
url: <网址>
root: <根目录 >
permalink: <生成文件路径>
permalink_defaults: <生成文件缺省路径>


source_dir: source   <源文件夹，这个文件夹用来存放内容。>
public_dir: public   <公共文件夹，这个文件夹用于存放生成的站点文件。>
tag_dir: tags   <标签文件夹>
archive_dir: archives   <归档文件夹>
category_dir: categories   <分类文件夹>
code_dir: downloads/code    <nclude code 文件夹>
i18n_dir: :lang   <国际化（i18n）文件夹>
skip_render:   <跳过指定文件的渲染，您可使用 glob 表达式来匹配路径。>
  
# Writing <文章>
new_post_name: :title.md   < 新建文章默认文件名>
default_layout: post   < 默认布局>
titlecase: false   < Transform title into titlecase>
external_link: true   < 在新标签中打开一个外部链接，默认为true>
filename_case: 0   <转换文件名，1代表小写；2代表大写；默认为0，意思就是创建文章的时候，是否自动帮你转换文件名，默认就行，意义不大。>
render_drafts: false   <是否渲染_drafts目录下的文章，默认为false>
post_asset_folder: false   <启动 Asset 文件夹>
relative_link: false   <把链接改为与根目录的相对位址，默认false>
future: true   <显示未来的文章，默认false>
highlight:   <代码块的设置> 
  enable: true  <代码设置开关>
  line_number: true <显示行数>
  auto_detect: false <自动高亮某些单词>
  tab_replace:  <多余空格替换>
  
  
# Home page setting <主页设置>
index_generator:
  path: ''  <博客索引页的根路径。(默认= ")>
  per_page: 10  <首页显示的文章数。(0 =禁用分页)>
  order_by: -date <文章顺序。(按日期顺序递减)>
  
# Category & Tag   <分类和标签的设置>
default_category: <默认分类>
category_map:   <分类地图>
tag_map:   <标签地图>

# Date / Time format <时间日期>
date_format: YYYY-MM-DD <日期格式>
time_format: HH:mm:ss   <时间格式>

# Pagination <分页>
per_page: 10   <每页显示的文章量 (0 = 关闭分页功能)>
pagination_dir: page   <分页目录>

# Extensions <主题>
theme: next  <主题名字>   

# Deployment <部署>
deploy:
  - type: <类型>
    repo: <仓储url>
    branch: <分支>
    ignore_pattern: <忽略文件>
    extend_dirs: <添加一些扩展目录来发布>
    ignore_hidden: <是否忽略隐藏文件的发布>
```

## 8.主题配置文件翻译


```
cache:
  enable: true <是否开启缓存>

favicon: <站点图标 可以放在hexo文件夹下的/source里>
  small: /images/favicon.ico
  medium: /images/avatar.png
  apple_touch_icon: /images/avatar.png
  safari_pinned_tab: /images/logo.svg
  #android_manifest: /images/manifest.json
  #ms_browserconfig: /images/browserconfig.xml

rss: <rss不设置 需要安装插件>

footer:
  since: 2015 <网站时间 从xx开始 类似 2015-2016>
  icon: <年份和版权之间的图标>
    name: user <图标名字>
    animated: false <图标动画>
    color: "#808080" <图标颜色>
    
  copyright: <版权信息>
  
  powered: <默认的 hexo链接开关>
    enable: false
    version: false
    
  theme: <默认的 主题链接开关>
    enable: false
    version: false

menu: #菜单路径设置 如果hexo在二级目录放置要去掉/
  home: /
  archives: /archives <归档>
  tags: /tags <标签>
  categories: /categories  <分类>
  about: /about <关于我>
  commonweal: /404.html  <公益404>
  #sitemap: /sitemap.xml <站点地图>

menu_settings:<菜单设置>
  icons: true   <图标开关>
  badges: true  <数量徽章开关>

# Schemes<主题设置>
#scheme: Muse
#scheme: Mist
#scheme: Pisces
scheme: Gemini

<边栏设置>
# Posts / Categories / Tags in sidebar.
site_state: true

# Social Links  <其它链接>
  GitHub: https://github.com/Chenjiujiu || github
  E-Mail: mailto:cjiujiu@icloud.com || envelope
  #Google: https://plus.google.com/yourname || google
  #Twitter: https://twitter.com/yourname || twitter
  #FB Page: https://www.facebook.com/yourname || facebook
  #VK Group: https://vk.com/yourname || vk
  #StackOverflow: https://stackoverflow.com/yourname || stack-overflow
  #YouTube: https://youtube.com/yourname || youtube
  #Instagram: https://instagram.com/yourname || instagram
  #Skype: skype:yourname?call|chat || skype

social_icons: <其它链接图标>
  enable: true  <其它链接开关>
  icons_only: false <是否只显示图标>
  transition: false <过渡动画>
  exturl: false 

# Blog rolls <友情链接>
links_icon: link    <友情链接图标>
links_title: 天帝宝库   <友情链接显示文字>
#links_layout: block
links_layout: inline
links:  <友情链接url>
  优设: http://www.uisdc.com/
  张鑫旭: http://www.zhangxinxu.com/
  Web前端导航: http://www.alloyteam.com/nav/
  前端书籍资料: http://www.36zhen.com/t?id=3448
  百度前端技术学院: http://ife.baidu.com/
  google前端开发基础: http://wf.uisdc.com/cn/

# Sidebar Avatar <边栏头像>
avatar: /images/avatar.png  <头像文件地址>

<边栏文章目录>
toc:
  enable: true  <开关>
  number: false <是否显示目录数>
  wrap: false   <单词是否断行>

# Creative Commons 4.0 International License.<版权协议>
# http://creativecommons.org/
# Available: by | by-nc | by-nc-nd | by-nc-sa | by-nd | by-sa | zero
#creative_commons: by-nc-sa
#creative_commons:

sidebar:
  position: right   <边栏位置>
  display: post     <什么时候显示>
  #display: always
  #display: hide
  #display: remove
 
  offset: 12    <边栏偏移>

  # Back to top in sidebar (only for Pisces | Gemini).
  b2t: false <回到顶部>

  # Scroll percent label in b2t button.
  scrollpercent: false

  # Enable sidebar on narrow view (only for Muse | Mist).
  onmobile: false


<摘要设置>
scroll_to_more: true <滚动显示更多>
save_scroll: false  <页面缓存滚动的位置>
excerpt_description: true   <自动从描述截取>
<自动截取摘要高度>
auto_excerpt:
  enable: false
  length: 150

<编辑时间设置>
post_meta:
  item_text: true   <时间文本>
  created_at: true  <显示创建时间>
  updated_at:   
    enabled: false  <显示更新时间>
    another_day: true
  categories: true

# Post wordcount display settings
# Dependencies: https://github.com/theme-next/hexo-symbols-count-time
symbols_count_time:
  separated_meta: true
  item_text_post: true
  item_text_total: false
  awl: 4
  wpm: 275

codeblock: <代码块设置>
  border_radius:   <代码块圆角> 
  copy_button:  
    enable: false   <显示复制按钮>
    show_result: false  <显示结果>

# Wechat Subscriber
#wechat_subscriber: 
  #enabled: true    <文章页脚微信订阅开关>
  #qcode: /path/to/your/wechatqcode ex. /uploads/wechat-qcode.jpg   <二维码地址>
  #description: ex. subscribe to my blog by scanning my public wechat account   <文字说明>

# Reward    <文章页脚打赏开关>
reward_comment: <打赏文字>
wechatpay: <微信打赏图片>
#alipay: <支付宝打赏图片>
#bitcoin: <比特币打赏图片>

<热门文章设置>
# Related popular posts
# Dependencies: https://github.com/tea3/hexo-related-popular-posts
related_posts:
  enable: false
  title: # custom header, leave empty to use the default one
  display_in_home: false
  params:
    maxCount: 5
    #PPMixingRate: 0.0
    #isDate: false
    #isImage: false
    #isExcerpt: false
    
<页脚版权信息>
# Declare license on posts
post_copyright: 
  enable: true
  license: <a href="https://creativecommons.org/licenses/by-nc-sa/4.0/" rel="external nofollow" target="_blank">CC BY-NC-SA 4.0</a>
```