---
title: 选择器优先级
date: 2019-03-06 01:39:42
tags: css
categories: 前端
---
# css选择符分类

1.标签选择器(如：body,div,p,ul,li)

2.类选择器(如：class="head",class="head_logo")

3.ID选择器(如：id="name",id="name_txt")
<!-- more -->
4.全局选择器(如：*号)

5.后代选择器(如：.head .head_logo,注意两选择器用空格键分开)  

6..分组选择器 div,span,img {color:Red} 即具有相同样式的标签分组显示

7.子选择器(如：div>p ,带大于号>)

8.伪类选择器(如：就是链接样式,a元素的伪类，4种不同的状态：link、visited、active、hover。)

9.伪元素选择器(如：img:after)

10.CSS 相邻兄弟选择器器 (如：h1+p,带加号+)

...还有很多

# css优先级

当两个规则都作用到了同一个html元素上时，如果定义的属性有冲突，那么应该用谁的值的，CSS有一套优先级的定义。

**不同级别**

1. 在属性后面使用 !important 会覆盖页面内任何位置定义的元素样式。
2. 作为style属性写在元素内的样式
3. id选择器
4. 类选择器
5. 标签选择器
6. 通配符选择器
7. 浏览器自定义或继承

​      **总结排序：!important > 行内样式>ID选择器 > 类选择器 > 标签 > 通配符 > 继承 > 浏览器默认属性**

同一级别中后写的会覆盖先写的样式

有个简单的计算方法（权值实际并不是按十进制，用数字表示只是说明思想，一万个class也不如一个id权值高）

- 内联样式表的权值为 1000
- ID 选择器的权值为 100
- Class 类选择器的权值为 10
- HTML 标签选择器的权值为 1

## **CSS选择器的解析原则**

CSS选择器的解析是从右往左的，这样的好处是尽早的过滤掉一些无关的样式规则和元素 。原因:[https://blog.csdn.net/jinboker/article/details/52126021](https://blog.csdn.net/jinboker/article/details/52126021)


