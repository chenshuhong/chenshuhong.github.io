---
title: clone
date: 2019-02-27 17:20:08
tags: javascript
categories: 前端
---
## 浅拷贝

### 数组

slice、concat 返回一个新数组的特性来实现拷贝。

```
var arr = ['old', 1, true, null, undefined];

var new_arr = arr.concat();

new_arr[0] = 'new';

console.log(arr) // ["old", 1, true, null, undefined]
console.log(new_arr) // ["new", 1, true, null, undefined]
var new_arr = arr.slice(); //["old", 1, true, null, undefined]
```
<!-- more -->
但是如果数组嵌套了对象或者数组的话，比如：

```
var arr = [{old: 'old'}, ['old']];

var new_arr = arr.concat();

arr[0].old = 'new';
arr[1][0] = 'new';

console.log(arr) // [{old: 'new'}, ['new']]
console.log(new_arr) // [{old: 'new'}, ['new']]
```

如果数组元素是基本类型，就会拷贝一份，互不影响，而如果是对象或者数组，就会只拷贝对象和数组的引用，这样我们无论在新旧数组进行了修改，两者都会发生变化。

我们把这种复制引用的拷贝方法称之为浅拷贝，与之对应的就是深拷贝，深拷贝就是指完全的拷贝一个对象，即使嵌套了对象，两者也相互分离，修改一个对象的属性，也不会影响另一个。

### 对象

- object.assign
- 展开运算符（...）

### 通用方法

```
var shallowCopy = function(obj) {
    // 只拷贝对象
    if (typeof obj !== 'object') return;
    // 根据obj的类型判断是新建一个数组还是对象
    var newObj = obj instanceof Array ? [] : {};
    // 遍历obj，并且判断是obj的属性才拷贝
    for (var key in obj) {
        if (obj.hasOwnProperty(key)) {
            newObj[key] = obj[key];
        }
    }
    return newObj;
}
```



## 深拷贝

### JSON.parse(JSON.stringify(obj));

简单暴力，但是

- 具有循环引用的对象时，报错
- 当值为函数、`undefined`、或`symbol`时，无法拷贝

### 通用方法

```
var deepCopy = function(obj) {
    if (typeof obj !== 'object') return;
    var newObj = obj instanceof Array ? [] : {};
    for (var key in obj) {
        if (obj.hasOwnProperty(key)) {
            newObj[key] = (obj[key]&&typeof obj[key] === 'object') ? deepCopy(obj[key]) : obj[key];
        }
    }
    return newObj;
}
```


