---
title: css_basic 
encrypt: false 
enc_pwd: askding 
categories:
- Coding
- JavaScript 
date: 2020-12-20 17:31:07 
top: 
tags: 
layout:
---

编写进度

- \[x\]

**CSS(Cascading Style Sheet)** 层叠样式表,是一种用来为结构化文档（如 HTML 文档或 XML 应用）添加样式（字体、间距和颜色等）的计算机语言。

[W3School TIY Editor](https://www.w3school.com.cn/tiy/t.asp)

CSS引入方式

```html
<link rel="stylesheet" type="text/css" href="path/to/xx.css" />
```

CSS规则：

```css
selector {
    style_name1: value1;
    style_name2: value2;
    ...
}
```

## CSS基本样式

四种基础选择器

- **派生选择器** 通过依据元素在其位置的上下文关系来定义样式，可以使标记更加简洁

```css
li strong {
    font-style: italic;
    font-weight: normal;
  }
```

- **id选择器** id 选择器可以为标有特定 id 的 HTML 元素指定特定的样式。以 “#” 来定义

```css
#pid {
  color: #00755f;
}
```

- **类选择器** 类可以为特定class属性的HTML元素指定特定的样式，以一个点号(.)定义

```css
.divclass {
  color: red;
}
```

- **属性选择器** 对带有指定属性的 HTML 元素设置样式。

```css
[title]
{
color:red;
}
```

### 背景

重要属性

| 属性  | 描述  |
| --- | --- |
| background-attachment | 背景图像是否固定或者随着页面的其余部分滚动 |
| background-color | 设置元素的背景颜色 |
| background-image | 把图片设置为背景 |
| background-position | 设置背景图片的起始位置 |
| background-repeat | 设置背景图片是否及如何重复 |

CSS3 背景

| 属性  | 描述  |
| --- | --- |
| background-size | 规定背景图片的尺寸 |
| background-origin | 规定背景图片的定位区域 |
| background-clip | 规定背景的绘制区域 |

```css
/* 把python.png设置为背景图片，但是不平铺 */
body {
  background-image: url("python.png");
  background-repeat: no-repeat;
}
```

### 文本

**重要属性**

| 属性  | 描述  |
| --- | --- |
| `color` | 文本颜色 |
| `direction` | 文本方向 |
| `line-height` | 行高  |
| `letter-spacing` | 字符间距 |
| `text-align` | 对齐元素中的文本 |
| `text-decoration` | 向文本添加修饰 |
| `text-indent` | 缩进元素中文本的首行 |
| `text-transform` | 元素中的字母 |
| `unicode-bidi` | 设置文本方向 |
| `white-space` | 元素中空白的处理方式 |
| `word-spacing` | 字间距 |

### 链接

**`CSS`链接的四种状态**

- `a:link` –普通的、未被访问的链接
- `a:visited` –用户已访问的链接
- `a:hover` –鼠标指针位于链接的上方
- `a:active` –链接被点击的时刻

**注意事项**

- `a:hover`必须位于`a:link`和`a:visited`之后
- `a:active`必须位于`a:hover`之后

```css
a:link {color:#FF0000;}		/* 未被访问的链接 */
a:visited {color:#00FF00;}	/* 已被访问的链接 */
a:hover {color:#FF00FF;}	/* 鼠标指针移动到链接上 */
a:active {color:#0000FF;}	/* 正在被点击的链接 */
```

### 列表

| 属性  | 描述  |
| --- | --- |
| [list-style](https://www.w3school.com.cn/cssref/pr_list-style.asp) | 简写属性。用于把所有用于列表的属性设置于一个声明中。 |
| [list-style-image](https://www.w3school.com.cn/cssref/pr_list-style-image.asp) | 将图象设置为列表项标志。 |
| [list-style-position](https://www.w3school.com.cn/cssref/pr_list-style-position.asp) | 设置列表中列表项标志的位置。 |
| [list-style-type](https://www.w3school.com.cn/cssref/pr_list-style-type.asp) | 设置列表项标志的类型。 |
| marker-offset |     |

```css
ul li {
    list-style-image : url(xxx.gif)
}
```

### 表格

| 属性  | 描述  |
| --- | --- |
| [border-collapse](https://www.w3school.com.cn/cssref/pr_tab_border-collapse.asp) | 设置是否把表格边框合并为单一的边框。 |
| [border-spacing](https://www.w3school.com.cn/cssref/pr_tab_border-spacing.asp) | 设置分隔单元格边框的距离。 |
| [caption-side](https://www.w3school.com.cn/cssref/pr_tab_caption-side.asp) | 设置表格标题的位置。 |
| [empty-cells](https://www.w3school.com.cn/cssref/pr_tab_empty-cells.asp) | 设置是否显示表格中的空单元格。 |
| [table-layout](https://www.w3school.com.cn/cssref/pr_tab_table-layout.asp) | 设置显示单元、行和列的算法。 |

```css
table, th, td
  {
  border: 1px solid blue;
  }
```

### 轮廓

| 属性  | 描述  | CSS |
| --- | --- | --- |
| [outline](https://www.w3school.com.cn/cssref/pr_outline.asp) | 在一个声明中设置所有的轮廓属性。 | 2   |
| [outline-color](https://www.w3school.com.cn/cssref/pr_outline-color.asp) | 设置轮廓的颜色。 | 2   |
| [outline-style](https://www.w3school.com.cn/cssref/pr_outline-style.asp) | 设置轮廓的样式。 | 2   |
| [outline-width](https://www.w3school.com.cn/cssref/pr_outline-width.asp) | 设置轮廓的宽度。 | 2   |

```css
#p1 {
  outline-color: #ff704d;
  outline-style: groove;
  outline-width: 10px;
}
```

## CSS盒子模型

![css-1.png](https://www.escapelife.site/images/web-css-rumen-jichu-3.png)

### 内边距padding

**内边距**

- `padding`属性定义元素边框与元素内容之间的空白区域
- `padding`属性接受长度值或百分比值，但不允许使用负值
- 按照**上、右、下、左**的顺序分别设置各边的内边距

**内边距的四个属性**

- `padding-top`
- `padding-right`
- `padding-bottom`
- `padding-left`

```css
h1 {
  padding-top: 10px;
  padding-right: 0.25em;
  padding-bottom: 2ex;
  padding-left: 20%;
  }

```

### 边框border

**边框**

- 元素的边框(`border`)是围绕元素内容和内边距的一条或多条线
- 设置`border`属性可以规定元素边框的样式、宽度和颜色

**边框样式**

- `border-style`设置样式

**边框宽度**

- `border-width`需要设置边框的宽度
- `border-top-width`
- `border-right-width`
- `border-bottom-width`
- `border-left-width`

**边框颜色**

- `border-color`需要设置边框的颜色
- `border-top-color`
- `border-right-color`
- `border-bottom-color`
- `border-left-color`

```css
td {
  border-style: solid;
  border-width: 15px 5px 15px 5px;
}

border-style: dashed;
border-top-width: 15px;
border-right-width: 5px;
border-bottom-width: 15px;
border-left-width: 5px;
border-color: blue rgb(25%, 35%, 45%) #909090 red;
```

### 外边距margin

**外边距**

- 外边距就是围绕在内容框的区域，默认为透明的区域
- 同样，外边距也接受任何长度的单位、百分数，与内边距很相似
- `margin`的默认值是`0`，但是一般浏览器都会预定提供样式
- 对称复制

**外边距的四个属性**

- `margin-top`
- `margin-right`
- `margin-bottom`
- `margin-left`

```css
h2 {
  margin-top: 20px;
  margin-right: 30px;
  margin-bottom: 30px;
  margin-left: 20px;
  }

```

### 外边距合并

外边距合并指的是，当两个垂直外边距相遇时，它们将形成一个外边距。合并后的外边距的高度等于两个发生合并的外边距的高度中的较大者。

当一个元素出现在另一个元素上面时，第一个元素的下外边距与第二个元素的上外边距会发生合并。请看下图：

![](https://www.w3school.com.cn/i/ct_css_margin_collapsing_example_1.gif)

当一个元素包含在另一个元素中时（假设没有内边距或边框把外边距分隔开），它们的上和/或下外边距也会发生合并。请看下图：

![](https://www.w3school.com.cn/i/ct_css_margin_collapsing_example_2.gif)

## CSS定位

定位的基本思想很简单，它允许你定义元素框相对于其正常位置应该出现的位置，或者相对于父元素、另一个元素甚至浏览器窗口本身的位置。

### 定位机制

**CSS 有三种基本的定位机制**

- 普通流
- 元素按照其在`HTML`中的位置顺序决定排布的过程。
- 浮动
- 浮动的框可以向左或向右移动，直到它的外边缘碰到包含框或另一个浮动框的边框为止。
- 绝对定位:
- 绝对定位使元素的位置与文档流无关，因此不占据空间。这一点与相对定位不同，相对定位实际上被看作普通流定位模型的一部分，因为元素的位置相对于它在普通流中的位置。

**定位属性**

- `position`，将元素放在一个静态的、相对的、绝对的或固定的位置
- 通过对 `top`,`left`,`right`,`bottom` 这四个属性的赋值让元素向对应的方向偏移
- `overflow`设置元素溢出其区域发生的事情
- `clip` 设置元素的显示形状，多用于图片
- `vertical-align`设置元素的垂直对其方式
- `z-index`设置元素的堆叠顺序

### 相对定位

设置为相对定位的元素框会偏移某个距离。元素仍然保持其未定位前的形状，它原本所占的空间仍保留。

```css
#box_relative {
  position: relative;
  left: 30px;
  top: 20px;
}
```

### 绝对定位

设置为绝对定位的元素框从文档流完全删除，并相对于其包含块定位，包含块可能是文档中的另一个元素或者是初始包含块。  
元素原先在正常文档流中所占的空间会关闭，就好像该元素原来不存在一样。 元素定位后生成一个块级框，而不论原来它在正常流中生成何种类型的框。

```css
#box_relative {
  position: absolute;
  left: 30px;
  top: 20px;
}
```

### 浮动

浮动的框可以向左或向右移动，直到它的外边缘碰到包含框或另一个浮动框的边框为止。  
由于浮动框不在文档的普通流中，所以文档的普通流中的块框表现得就像浮动框不存在一样。

**`float`属性可以赋值如下**

- `left：`元素向左浮动
- `right：`元素向右浮动
- `none：`不浮动
- `inherit：`从父级继承浮动的属性
- `clear：`主要用于去掉向各方向的浮
```
