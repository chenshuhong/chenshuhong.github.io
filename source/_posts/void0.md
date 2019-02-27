---
title: javascript中"return obj === void 0"这种写法的原因和好处
date: 2019-02-27 16:44:21
tags: javascript
categories: 前端
---
void其实是javascript中的一个函数，接受一个参数，返回值永远是undefined
```js
void 0
void (0)
void "hello"
void (new Date())
// always return undefined
```
**直接使用undefined不是更好吗，直观**
1. 使用void 0比使用undefined能够减少3个字节。虽然这是个优势，个人但感觉意义不大，牺牲了可读性和简单性。
```js
"undefined".length //9
"void 0".length //6
```
2. undefined并不是javascript中的保留字，我们可以使用undefined作为变量名字，然后给它赋值。在ES5之前，window下的undefined是可以被重写的，于是导致了某些极端情况下使用undefined会出现一定的差错。
 所以，用void 0是为了防止undefined被重写而出现判断不准确的情况。不过目前浏览器基本都无法被重写了，所以现在用void 0 代替 undefined意义不大
