---
title: Markdown语法整理
comments: true
date: 2016-02-08 08:35:08
tags: Markedown
categories: 文档编辑
id: wdbj-0208083508
mathjax: true
keywords: Markdown语法、Markdown
description: 常用Markdown语法整理
---
<span id='top'></span>
## 前言
{% note info %} 最近看到几个小伙伴都是在用Word来写笔记，就随口问了一下他们知不知道Markdown，他们居然一脸闷逼的看着我说不知道，于是想整理Markdown的语法，以便小伙伴们学习，至于介绍就一句话把，Markdown就是一款清量级的文本标记语言，其宗旨就是易读易写，还可以顺带提神逼格 {% endnote %}
<!--more-->
## 1.空格、换行
### 1.1空格
<span id='nbsp'></span>
　　1.`&ensp;`或`&#8194;`  
　　2.`&emsp;`或`&#8195;`  
　　3.`&nbsp;`或`&#160;`   
　　4.输入法切换到全角,两次空格 
### 1.2换行  
　　1.就像使用html一样可以使用`<br>`标签   
　　2.在上一行末尾输入两个以上空格然后再回车
## 2.倾斜
代码：
```
  *这是倾斜* 
  **这是加粗**
  ***这是加粗倾斜***
  ～～这是删除线～～
```
 效果：  
*这是倾斜*  
**这是加粗**  
***这是加粗倾斜***   
～～这是删除线～～  

## 3.标题
第一种写法：
```
　　这是一个一级标题
=====================

　　这是一个二级标题
---------------------
```
第二种写法：
```
#一级标题
##二级标题
###三级标题
####四级标题
#####五级标题
######六级标题
```

## 4.超链接
### 4.1行内式
语法说明：  
`[链接文字](链接地址 “可选title属性”)`这样的形式。  
链接地址与链接标题前有一个空格。
代码：
```
   欢迎来到[大毛修仙](http://blog.chenjiujiu.com)
   欢迎来到[大毛修仙](http://blog.chenjiujiu.com "大毛的博客")
```
效果：  
　　欢迎来到[大毛修仙](http://blog.chenjiujiu.com)  
　　欢迎来到[大毛修仙](http://blog.chenjiujiu.com "大毛的博客")

### 4.2参考式
参考式一般用在学术论文上面，或者需要果重复使用多次时，它可以让你对链接进行统一的管理  

语法说明：   
参考式链接分为两部分  
文中的写法[链接文字][链接标记]  
在文本的任意位置添加[链接标记]:链接地址 “链接标题”，链接地址与链接标题前有一个空格。
如果链接文字本身可以做为链接标记，你也可以写成[链接文字][] 
[链接文字]：链接地址的形式.

代码：
```
  这是我的博客地址[我的博客][1]
  这是我的github地址[GitHub][]

  [1]:http://blog.chenjiujiu.com "大毛"
  [GitHub]:http://http://blog.leanote.com/freewalk
```
效果：  
　　这是我的博客地址[我的博客][1]  
　　这是我的github地址[GitHub][]

  [1]:http://blog.chenjiujiu.com "大毛"
  [GitHub]:http://http://blog.leanote.com/freewalk

### 4.3自我链接
语法说明：  
　　使用<>把地址包起来即可  

代码：  
```
　<http://blog.chenjiujiu.com>
　<address@example.com>
```
效果：  
　　<http://blog.chenjiujiu.com>  
　　<address@example.com>
### 4.4锚点  
代码：
```
这里是跳转目的<span id='top'></span>
跳转到[前言](#top)
```

效果：  
跳转到[前言](#top)

## 5.列表

可以使用html的ul和ol标签来表示列表，或者以下方式
### 5.1无序列表
语法：  
　　使用* + - 表示无序列表  
代码：

```
* 无序列表1
+ 无序列表2
- 无序列表3
```

效果：    
* 无序列表1  
+ 无序列表2  
- 无序列表3  

### 5.2有序列表
语法：  
　　使用数字加一个英文句号表示有序列表  
代码：
```
1. 有序列表1
2. 有序列表2
3. 有序列表3
```
效果：    
1. 有序列表1  
2. 有序列表2  
3. 有序列表3   

## 6.引用
语法说明：     
　　引用需要在被引用的文本前加上>符号。  
代码：
```
> 这是一句引用的话,
> 这是一句引用的话.
```
效果：  
> 这是一句引用的话,  
> 这是一句引用的话.  

Markdown 也允许你偷懒只在整个段落的第一行最前面加上 >
代码：
```
> 这是一句引用的话,
 这是一句引用的话.
 这是一句引用的话.
```
效果：   
> 这是一句引用的话,  
这是一句引用的话.   
这是一句引用的话.  
### 6.1多层引用嵌套
区块引用可以嵌套（例如：引用内的引用），只要根据层次加上不同数量的 >
代码:
```
> 请问 Markdwon 怎么用？ - 小白  
>> 自己看教程！ - 愤青   
>>> 教程在哪？ - 小白 
```
显示效果：     
> 请问 Markdwon 怎么用？ - 小白  
>> 自己看教程！ - 愤青   
>>> 教程在哪？ - 小白 

## 7.图片
### 7.1 行内式
语法说明：`![图片Alt](图片地址 “图片Title”)`  
代码： 
```
![小和尚](http://blog.chenjiujiu.com/images/pic-markdown/heshan.png  "小和尚")
```
效果：  
![小和尚](http://blog.chenjiujiu.com/images/pic-markdown/heshan.png "小和尚")

### 7.2 参考式
语法说明：  
在文档要插入图片的地方写`![图片Alt][标记]`  
在文档的最后写上[标记]:图片地址 “Title”
代码： 
```
![和尚][heshan]

![heshan]:http://blog.chenjiujiu.com/images/pic-markdown/heshan.png "小和尚"
```
效果：  
![和尚][heshan]  

[heshan]:http://blog.chenjiujiu.com/images/pic-markdown/heshan.png "小和尚" 


## 8.内容目录
　在段落中填写 `[TOC]` 以显示全文内容的目录结构。
## 9.脚注
语法说明：
　　在需要添加注脚的文字后加上脚注名字[^注脚名字],称为加注。  
　　然后在文本的任意位置(一般在最后)添加脚注，脚注前必须有对应的脚注名字。
　　不能直接使用空格，否则无效，请使用[以上](#nbsp) 空格方式
代码： 
```
使用 Markdown[^1]可以效率的书写文档, 直接转换成 HTML[^2]

[^1]:Markdown是一种纯文本标记语言
[^2]:HyperTextMarkupLanguage超文本标记语言
```
效果：  
　使用 Markdown[^1]可以效率的书写文档, 直接转换成 HTML[^2]

[^1]:Markdown是一种纯文本标记语言  
[^2]:HyperTextMarkupLanguage超文本标记语言 

## 10.math数学公式
### 10.1行内公式
语法：用$包裹  
代码：
```
这是一个行内公式$\sum_{i=0}^N\int_{a}^{b}g(t,i)\text{d}t$
```
显示效果：  
这是一个行内公式$\sum_{i=0}^N\int_{a}^{b}g(t,i)\text{d}t$  

### 10.2块级公式
语法：用$$包裹  
代码：
```
这是一个块级公式$$\sum_{i=0}^N\int_{a}^{b}g(t,i)\text{d}t$$
```
显示效果：  
这是一个块级公式$$\sum_{i=0}^N\int_{a}^{b}g(t,i)\text{d}t$$
[math语法参考](https://math.meta.stackexchange.com/questions)


