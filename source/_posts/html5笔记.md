---
title: html5笔记
comments: true
mathjax: false
date: 2017-05-16 02:44:16
tags: html5
id: bj-0516024416
categories: html笔记
keywords: html5笔记、html5、html
description: html5学习笔记
---
## 前言
{% note info %} HTML5的出现的目的是想要解决目前web上存在的各种问题，包括但不限于以下 1.Web浏览器之间的兼容性很低。2.文档结构不够明确。3.Web应用程序的功能受到限制 {% endnote %}

<!--more-->
## 1 全局属性
全局属性就是指可以对任何元素都使用的属性
### 1.1 contentEditable属性
是由微软开发，功能是允许用户编辑元素的内容，boole值类型,true元素允许编译,可继承
```html
<ul contenteditable="true">
  <li>这个列表是可编辑的</li>
  <li>此时isContenteditable为true</li>
</ul>
```
### 1.2 designMode属性
用来设置整个页面是否可编辑，只能在js中修改,值为on/off
```javascript
document.designMode='on';
```

### 1.3 hidden属性
通知浏览器不渲染该元素，boole值类型
```html
<ul hidden="hidden">
  <li>这个列表是不可见的</li>
</ul>
```

### 1.4 spellcheck属性
html针对`input`和`textarea`设计的，对输入文本进行语法检查
```html
<input type="text" spellcheck="true">
<!--此时如果input中输入单词有误会有虚线显示-->
```

### 1.5 tabindex属性
用户连续按下table键时候光标的顺序    
下面示例连按table键光标会依次在1342的顺序了,当设置为-1时不能通过table获取到焦点
默认只有链接或者表单元素能使用`table`获取焦点，我们可以使用此属性来改变  
```html
<a href="#" tabindex="1">1</a>
<a href="#" tabindex="4">2</a>
<a href="#" tabindex="2">3</a>
<a href="#" tabindex="3">4</a>
```

## 2 新增的元素
1. 新增的结构元素：   
section、article、aside、header、hgroup、footer、nav、figure
2. 新增的其它元素：   
video、audio、embed、mark、progress、meter、time、ruby、rt、rp、wbr、canvas、command、details、datalist、datagrid、keygen、output、source、menu
3. 新增的input元素类型：  
email、url、number、range、Date Pickers、data、time、datetime、datetime-local、month、week、search、tel、color、

## 3 表单元素新增属性
### 3.1 表单内元素的form属性
可以把表单内的属性书写在任何位置，然后通过该元素指定一个form属性，属性值为该表单的id，来声明该元素从属指定表单
```html
<form id="testform">
	<input type="text">
</form>
<input type="text" form="testform">
```

### 3.2 表单内元素的formaction属性
可以为所有的提交按钮增加不同的formaction属性，使单击不同的按钮时可以将表单提交到不同的页面
```html
<form action="test01.php">
	<input type="submit" formaction="test02.php">
	<input type="submit" formaction="test03.php">
</form>
```

### 3.3 表单内元素的formmethod属性
可以使用formmethod属性对不同的按钮设置不同提交方法
```html
<form action="test01.php">
	<input type="submit" formmethod="get">
	<input type="submit" formmethod="post">
</form>
```
### 3.4 表单内元素的formenctype属性
可以使用formenctype属性对表单元素指定不同的编码方式
```html
<form>
	<input type="text" formenctype="application/x-www-form-urlencoded">
	<input type="text" formenctype="multipart/form-data">
	<input type="text" formenctype="text/plain">
</form>
```

### 3.5 表单内元素的formtarget属性
可以使用formtarget属性对表单元素指定不同的打开方式
```html
<form action="test01.php">
	<input type="submit" formtarget="_blank">
	<input type="submit" formtarget="_self">
</form>
```

### 3.6 表单内元素的autofocus属性
设置autodocus属性，页面打开时，该控件自动获得焦点
```html
<input type="text" autofocus>
```
### 3.7 表单内元素的required属性
在提交时如果元素中的内容为空白，则不允许提交，同时浏览器中显示信息提示文字
```html
<form>
	<input type="text" required="required">
</form>
```
### 3.8 表单内元素的labels属性
所有可使用标签的表单元素等定义了一个labels属性，属性值是有何NodeList对象，
代表钙元素所绑定的标签元素label的集合
```html
<input type="text" id="demo">
<label for="demo">1</label>
<label for="demo">2</label>
demo.labels为两个标签的集合
```
### 3.9 标签的control属性
可以在标签内部放置一个表单元素，并且通过该标签的control属性来访问该表单元素
```html
<label id="demo">
	<input type="text">
</label>
可以使用demo.control来找到input元素
```
## 4 文本框新增属性
### 4.1 placeholder属性
输入提示
```html
<input type="text" placeholder="请输入文字">
```
### 4.2 list属性
配合datalist使用，为文本框增加一个list属性list值为datalist的id，在输入时出现下拉提示，提示内容为datalist
```html
<form>
  <input type="text" list="demo">
  <datalist id="demo">
    <option value="html">html</option>
    <option value="css">css</option>
    <option value="js">js</option>
  </datalist>
</form>
```
### 4.3 AutoComplete属性
在表单上添加`AutoComplete='off'`来关闭浏览器自动记录输入过的表单值的功能，默认是`on`的，来提高表单安全性

### 4.4 pattern属性
对表单元素使用pattern属性，属性值为正则表达式，来检查表单输入是否符合格式，不匹配则不允许提交
```html
<input type="text" pattern="[a-z]{6}"> 必须是6个小写字母
```

### 4.5 SelectionDirection属性
针对input于textarea元素，当用户在其中使用鼠标选择文字是后，可以通过该属性来或者用户选取的方向，正向选取或不选取时值为forward，当用户反向选取是值为backward

### 4.6 复选框的indeterminate属性
通过js脚本设置复选框indeterminate属性，为复选框设置"尚未明确是否选取"的状态
```html
<input type="checkbox"  name="12" value="1">
<input type="checkbox" name="12" value="2">
<script>
	var inp=document.getElementsByTagName('input')[0];
	inp.indeterminate=true
</script>
```

### 4.7 表单验证
使用html的表单元素类型可自动进行表单验证，可以添加novalidate="true"取消表单默认验证，自己来处理结果，表单的checkValidity存储验证结果，boole类型
```html
<form novalidate="true">
  <input type="email">
</form>
此时使用input的checkValidity的值来判断是否符合格式
```

## 5 增强页面元素
### 5.1 figure、figcaption
figure表示网页上的一个独立的内容，删除后对网页无影响  
figcaption是figure元素的标题，需要放在figure元素内部，只能有一个
```html
<figure>
  <figcaption>标题</figcaption>
  <img src="./img.jpg">
</figure>
```