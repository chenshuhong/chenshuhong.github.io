---
title: BFC
date: 2019-03-04 03:07:17
tags: css
categories: 前端
---
## 什么是BFC

**块级格式化上下文**，是一个独立的渲染区域，让处于 BFC 内部的元素与外部的元素相互隔离，使内外元素的定位不会相互影响。

## 生成规则（满足其一即可）

- float的值不是none。
- position的值为absolute或fixed
- display的值是inline-block、table-cell、flex、table-caption或者inline-flex
- overflow的值不是visible
- 根元素
<!-- more -->
## 约束规则

- 属于同一个BFC的块盒与行盒（行盒由一行中所有的内联元素所组成）都会垂直的沿着其父元素的边框排列
- 属于同一个 BFC 的两个相邻 Box 的 margin 会发生重叠
- 在BFC中，每一个盒子的左外边缘（margin-left）会触碰到容器的左边缘(border-left)（对于从右到左的格式来说，则触碰到右边缘）。 (子元素 absolute 除外)
- BFC 的区域不会与 float 的元素区域重叠（？）
- 计算 BFC 的高度时，浮动子元素也参与计算（？）
- 文字层不会被浮动层覆盖，环绕于周围（？）

## BFC常见应用

### 解决普通文档流块元素的外边距折叠问题

### BFC清除浮动

### 阻止普通文档流元素被浮动元素覆盖

### 自适应两栏布局
