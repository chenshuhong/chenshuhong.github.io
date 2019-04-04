---
title: css生成竖条的几种方式
date: 2019-03-21 19:01:49
tags:
categories:
---
先假定所有div的样式

div{
	position:relative;
	width:200px;
	height:60px;
	line-height:60px;
	margin:20px;
	background:#ddd;
	text-align:center;
}
<!-- more -->
## border实现

html

```
<div class="border">border实现</div>
```

css

```
.border{
	border-left:5px solid deeppink;
}
```

缺点就是border的宽高无法调整

## 使用伪元素

html

```
<div class="pesudo">伪元素实现</div>
```

css

```
.pesudo::after{
	content:"";
	width:10px;
	height:50px;
	position:absolute;
	top:5px;
	left:0;
	background:deeppink;
}
```

这里的高度，位置都可以随意调节

## box-shadow

先了解下box-shadow

**语法**

```
box-shadow: h-shadow v-shadow blur spread color inset;
```

box-shadow 向框添加一个或多个阴影。该属性是由逗号分隔的阴影列表，每个阴影由 2-4 个长度值、可选的颜色值以及可选的 inset 关键词来规定。省略长度的值是 0。

| 值         | 描述                                     |
| ---------- | ---------------------------------------- |
| *h-shadow* | 必需。水平阴影的位置。允许负值。         |
| *v-shadow* | 必需。垂直阴影的位置。允许负值。         |
| *blur*     | 可选。模糊距离。                         |
| *spread*   | 可选。阴影的尺寸。                       |
| *color*    | 可选。阴影的颜色。请参阅 CSS 颜色值。    |
| inset      | 可选。将外部阴影 (outset) 改为内部阴影。 |

```
<div class="boxShadow">外 box-shadow 实现</div>
```

```
.boxShadow{
	margin-left:25px;
	box-shadow:-5px 0px 0 0 deeppink;
}
```

这里不要模糊距离和模糊尺寸，然后设置水平阴影，-5表示阴影块向左偏移5px，为了调整，所以div必须先右移动5px

当使用内部阴影时，与外阴影相反

```
.insertBoxShadow{
	box-shadow:inset 5px 0px 0 0 deeppink;
}
```

## drop-shadow

`drop-shadow` 是 CSS3 新增滤镜 `filter` 中的其中一个滤镜，也可以生成阴影，不过它的数值参数个数只有 3 个，比之 box-shadow 少一个。

```
.filterDropShadow{
	margin-left:25px;
	-webkit-filter:drop-shadow(-5px 0 0 deeppink);
}
```

## 渐变 linearGradient

语法

```
background: linear-gradient([<point> || <angle>,]? <stop>, <stop> [, <stop>]*);
```

其共有三个参数，第一个参数表示线性渐变的方向，top 是从上到下、left 是从左到右，如果定义成 left top，那就是从左上角到右下角。第二个和第三个参数分别是起点颜色和终点颜色。你还可以在它们之间插入更多的参数，表示多种颜色的渐变

```
.linearGradient{
	background-image:linear-gradient(90deg, deeppink 0px, deeppink 5px, transparent 5px);
}
```

表示从左到右，0-5px是从deeppink到deeppink的渐变，及纯色块，5px-5px是deeppink到透明的渐变，及不存在的色块，剩下的5px到最右没指定渐变结束颜色值，则是透明色块

## outline实现

个用的比较少，outline （轮廓）是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。这个方法算是下下之选。

```
.outline{
	margin-left:25px;
	height:50px;
	outline:5px solid deeppink;
}
.outline:after{
	position:absolute;
	content:"outline实现";
	top:-5px;
	bottom:-5px;
	right:-5px;
	left:0;
	background:#ddd;
}
```



<iframe height="265" style="width: 100%;" scrolling="no" title="css竖条实现" src="//codepen.io/chenshuhong/embed/GewJyW/?height=265&theme-id=0&default-tab=css,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/chenshuhong/pen/GewJyW/'>css竖条实现</a> by 陈树鸿
  (<a href='https://codepen.io/chenshuhong'>@chenshuhong</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>


