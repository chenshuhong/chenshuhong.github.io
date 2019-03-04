---
title: each的模拟实现
date: 2019-03-04 10:35:37
tags: javascript
categories: 前端
---
## each遍历模拟

```
each(object, [callback])
```

回调函数拥有两个参数：第一个为对象的成员或数组的索引，第二个为对应变量或内容。

如果需要退出 each 循环可使回调函数返回 false，其它返回值将被忽略。

<!-- more -->

```
// 遍历数组
each( [0,1,2], function(i, n){
    console.log( "Item #" + i + ": " + n );
});
// Item #0: 0
// Item #1: 1
// Item #2: 2

// 遍历对象
each({ name: "John", lang: "JS" }, function(i, n) {
    console.log("Name: " + i + ", Value: " + n);
});
// Name: name, Value: John
// Name: lang, Value: JS

$.each( [0, 1, 2, 3, 4, 5], function(i, n){
    if (i > 2) return false;
    console.log( "Item #" + i + ": " + n );
});

// Item #0: 0
// Item #1: 1
// Item #2: 2
```

## 第一版

```
// 第一版
function each(obj, callback) {
    var length, i = 0;

    if ( isArrayLike(obj) ) {
        length = obj.length;
        for ( ; i < length; i++ ) {
            callback(i, obj[i])
        }
    } else {
        for ( i in obj ) {
            callback(i, obj[i])
        }
    }

    return obj;
}
```

## 终止循环

把：

```
callback(i, obj[i])
```

替换成：

```
if (callback(i, obj[i]) === false) {
    break;
}
```

轻松实现中止循环的功能。

## this

我们在实际的开发中，我们有时会在 callback 函数中用到 this，先举个不怎么恰当的例子：

```
// 我们给每个人添加一个 age 属性，age 的值为 18 + index
var person = [
    {name: 'kevin'},
    {name: 'daisy'}
]
$.each(person, function(index, item){
    this.age = 18 + index;
})

console.log(person)
```

这个时候，我们就希望 this 能指向当前遍历的元素，然后给每个元素添加 age 属性。

指定 this，我们可以使用 call 或者 apply，其实也很简单：

我们把：

```
if (callback(i, obj[i]) === false) {
    break;
}
```

替换成：

```
if (callback.call(obj[i], i, obj[i]) === false) {
    break;
}
```

## 最终版

所以最终的 each 源码为：

```
function each(obj, callback) {
    var length, i = 0;

    if (isArrayLike(obj)) {
        length = obj.length;
        for (; i < length; i++) {
            if (callback.call(obj[i], i, obj[i]) === false) {
                break;
            }
        }
    } else {
        for (i in obj) {
            if (callback.call(obj[i], i, obj[i]) === false) {
                break;
            }
        }
    }

    return obj;
}
```
