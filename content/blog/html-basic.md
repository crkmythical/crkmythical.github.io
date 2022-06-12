---
title: html_basic
encrypt: false
enc_pwd: askding
categories:
  - Coding
  - JavaScript
date: 2020-12-20 14:05:39
top:
tags:
layout:
---
编写进度
- [x] 

**HTML(Hyper Text Markup Language)** 是一种用来制作超文本文件的简单标记语言，用HTML编写的超文本文件成为html文件，它能独立于各种操作系统平台。  

[W3School TIY Editor](https://www.w3school.com.cn/tiy/t.asp)

```html
<html>
  <head>
  <!--头部信息-->
  </head>
  <body>
  <!--文件主题--->
  </body>
</html>
```

## 层标签<div>
```html
<div id="" align="" style="" class="">
...
</div>
```

## 文本样式
### 标题与段落
```html
<h1>Typography</h1>
<h2>History</h2>
<p>Thomas identified the victim only as a 32-year-old woman.</p>
<br /> <!-- 换行--->
<hr /><!-- 水平分割线>
```


![html-1](../images/html-1.png)
## 链接
## 超链接
href的写法
```html
<a href="http://www.w3.org/index.html">W3C</a>
<a href="//www.w3.org/index.html">W3C</a>
<a href="mailto:askding@qq.com?Subject=我有疑问">W3C</a>
<a href="ftp://ftp.pku.edu.cn">跳转到FTP</a>
<a href="telnet://10.1.1.1">Telnet到10.1.1.1</a>


<a href="a/b/c.html">相对路径</a>
<a href="../../c.html">相对路径</a>
```
页面内锚点
```html
<p id="test">test</p>
<p><a href="#test">回到test</a></p>
```

链接目标target的写法
```html
<a href="http://www.w3.org" target="_self">W3C(当前窗口打开)</a>
<a href="http://www.w3.org" target="_blank">W3C(新建窗口打开)</a>
```
### 图片链接
```html
<img src="/path/to/img.jpg" alt="替代文字" />
```

## 多媒体

### 动态文字
```html
<marquee direction="left/right/up/down" behavior="scroll/slide/alternate" loop=1 scrollamount=5 scrolldelay=1 bgcolor=“#FFFF55” width=300 height=500> 动态文字. </marquee>

direction: 滚动方向
behavior: 滚动方式
loop: 循环次数
scrollamount: 滚动速度
scrolldelay: 滚动延迟
bgcolor: 背景颜色
width、height:滚动面积
```

### Audio标签
```html
<audio src="path/to/audio_file" autoplay="" controls="" loop=1" >您的浏览器不支持audio标记</audio>
```

### Video标签
```html
<video src="path/to/video_file" autoplay="" width=10 height=10 loop=1 >您的浏览器不支持video标记</video>

```

### 字幕标签
```html
<track src="vat/sintel-en.vtt" srclang="en". kind="captions" label="English captions" />
```



### 多媒体文件
```html
<embed src="/path/to/file" autostart=True/False loop=3 hidden=True/False width=300 height=400></embed>

```
 
### 背景音乐
```html
<bgsound src="/path/to/music.mp3" loop= 1>
```


## 列表
### 定义列表
```html
<dl>
	<dt>名词1<dd>解释名词1
	<dt>名词2<dd>解释名词2
</dl>
```


### 无序列表
```html
<ul type="disc/circle/square">
	<LI>列表项
	<LI>列表项
	<LI>列表项
</ul>

```

### 有序列表
```html
<ol type=1/a/A/i/I start=1 >
	<LI>列表项
	<LI>列表项
	<LI>列表项
</ol>


```

### 菜单列表

```html
<menu>
	<LI>菜单项
	<LI>菜单项
	<LI>菜单项
	<LI>菜单项
</menu>
```


### 目录列表
```html
<dir>
	<LI>文件名
	<LI>文件名
	<LI>文件名
	<LI>文件名
</dir>

```


## 表格
```html
<table width=300 height=300  align="left/center/right" border=1 bordercolor="#FFFF55">
	<caption>表格标题</caption>
	<thead bgcolor align > <!--可选--->
	<tr><!--第一行--->
		<th>表头1</th> <!--第一列--->
		<th>表头2</th> <!--第二列--->
		<th>表头3</th> <!--第三列--->
	</tr>
	<thead>
	<tbody bicolor="" aligin="">  <!--可选--->
	<tr><!--第一行--->
		<td>单元格内的文字</td> <!--第一列--->
		<td>单元格内的文字</td> <!--第二列--->
		<td>单元格内的文字</td> <!--第三列--->
	</tr>
	<tr><!--第二行--->
		<td>单元格内的文字</td>
		<td>单元格内的文字</td>
		<td>单元格内的文字</td>
	</tr>
	<tbody>
	<tfoot bgcolor="" align="" >  <!--可选--->
	<tr><!--第三行--->
		<td>单元格内的文字</td>
		<td>单元格内的文字</td>
		<td>单元格内的文字</td>
	</tr>
	<tfoot>
</table>

```

## 表单
```html
<!--创建表单--->
<form name="form1" method="GET/POST" action="url" enctype="Text/plain | multipart/form-data | application/x-www-form-urlencoded"  target="_self/_blank">
	控件: <input name="控件名" type="控件类型"  >
	文本框: <input name="text1" type="text" size=10 maxlength=200 value="文本默认值" >
	密码框: <input name="password1" type="password" size=10 maxlength=200 value="文本默认值">
	<br />
	单选按钮: <input name="radio1" type="radio" value="单选按钮取值1" checked>选项1
	单选按钮: <input name="radio1" type="radio" value="单选按钮取值2" >选项2
	<br />
	复选项: <input name="checkbox1" type="checkbox" value="复选项的值" checked>项目1
	复选项: <input name="checkbox1" type="checkbox" value="复选项的值" >项目2
	复选项: <input name="checkbox1" type="checkbox" value="复选项的值" checked>项目3
	<br />
	普通按钮: <input name="button1" type="button" value="普通按钮显示值" onclick="function">
	<br />
	提交按钮: <input name="submit1" type="submit" value="提交按钮显示值" >
	重置按钮: <input name="reset1" type="reset" value="重置按钮显示值" >
	图像提交按钮: <input name="image1" type="image" src="path/to/image" value="默认值" >
	隐藏按钮: <input name="hidden1" type="hidden" value="提交参数值">
	文件按钮: <input name="file1" type="file" >
	
<!--菜单列表类型的控件--->
	<!--下拉菜单--->
	证件类型: <select name="cardtype">
		   	<option value="id_card" selected">身份证
			<option value="stu_card" >学生证
			<option value="drive_card">驾驶证
			<option value="other_card">其他证件
		   </select>	
	<!--列表项菜单--->
   对html评价: <select name="content" size=5 multiple>
		  	<option value="M1" selected>很容易
			<option value="M2" >一般
			<option value="M3" >能理解
		  </select>
<!--文本域---->
	留言: <textarea name="textarea1" rows=5 cols=80 >默认显示值</textarea>	

</form>

```



## 框架
### 框架基本结构
```html
<!--水平分割窗口rows--->
<frameset rows="30%,70%" frameborder=0/1 >
	<frame src="path/to/xx.html name="xx" >
	<frame src="path/to/file name="file1" >
</frameset>

<!--垂直分割窗口cols--->
<frameset rows="25%,55%，25%" frameborder=0/1 >
	<frame>
	<frame>
	<frame>
</frameset>


### 浮动框架
```html
<iframe name="" align="" width=12 height=12  src="path/to/xx.html" >
<iframe>
```
















