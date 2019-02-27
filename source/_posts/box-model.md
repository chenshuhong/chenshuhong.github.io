---
title: 盒模型
date: 2019-02-22 02:51:21
categories: 前端
tags: css
---
页面渲染时，dom 元素所采用的 **布局模型**。

盒模型的组成，由里向外content,padding,border,margin。

盒模型是有两种标准的，一个是标准模型，一个是IE模型。

{% asset_img box-model.png %}
{% asset_img ie盒子模型.webp %}

在标准模型中，盒模型的宽高只是内容（content）的宽高，而在IE模型中盒模型的宽高是内容(content)+填充(padding)+边框(border)的总宽高。

CSS3 的属性 box-sizing可设置这两种模型

```
/* 标准模型 */
box-sizing:content-box;

 /*IE模型*/
box-sizing:border-box;

inherit: 规定应从父元素继承 box-sizing 属性的值
```
