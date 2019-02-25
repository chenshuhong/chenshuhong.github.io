---
title: 防抖&节流
date: 2019-02-25 09:55:11
tags: javascript
categories: javascript
---
如何防止频繁的事件触发，比如：

1. window 的 resize、scroll

1. mousedown、mousemove
2. keyup、keydown

为了解决这个问题，一般有两种解决方案：

1. debounce 防抖
2. throttle 节流

## 防抖

防抖的原理就是：你尽管触发事件，但是我一定在事件触发 n 秒后才执行，如果你在一个事件触发的 n 秒内又触发了这个事件，那我就以新的事件的时间为准，n 秒后才执行，总之，就是要等你触发完事件 n 秒内不再触发事件，我才执行。

```js
// 第一版
function debounce(func, wait) {
    var timeout;
    return function () {
        clearTimeout(timeout)
        timeout = setTimeout(func, wait);
    }
}
```

缺点：

- func中this指向window
- 事件对象 event 丢失

```js
// 第二版
function debounce(func, wait) {
    var timeout;
	var args = arguments;
    return function () {
        var context = this;

        clearTimeout(timeout)
        timeout = setTimeout(function(){
            func.apply(context,args)
        }, wait);
    }
}
```

优化下，让他支持立即执行

```js
// 第四版
function debounce(func, wait, immediate) {

    var timeout;

    return function () {
        var context = this;
        var args = arguments;

		if (immediate && !timeout) {
            fn.apply(context, args)
        }
        if (timeout) clearTimeout(timeout)
        timeout = setTimeout(() => {
            fn.apply(context, args)
        }, wait)
    }
}
```

增加取消

```js
// 第四版
function debounce(func, wait, immediate) {

    var timeout;

    var debounced = function () {
        var context = this;
        var args = arguments;

		if (immediate && !timeout) {
            fn.apply(context, args)
        }
        if (timeout) clearTimeout(timeout)
        timeout = setTimeout(() => {
            fn.apply(context, args)
        }, wait)
    }
    
    debounced.cancel = function() {
        clearTimeout(timeout);
        timeout = null;
    };
    
    return debounced;
}
```

## 节流

节流的原理很简单：

如果你持续触发事件，每隔一段时间，只执行一次事件。

根据首次是否执行以及结束后是否执行，效果有所不同，实现的方式也有所不同。
我们用 leading 代表首次是否执行，trailing 代表结束后是否再执行一次。

关于节流的实现，有两种主流的实现方式，一种是使用时间戳，一种是设置定时器。

### 使用时间戳

```js
// 第一版
function throttle(func, wait) {
    var context, args;
    var previous = 0;

    return function() {
        var now = +new Date();
        context = this;
        args = arguments;
        if (now - previous > wait) {
            func.apply(context, args);
            previous = now;
        }
    }
}
```

会立刻执行，停止触发后没有办法再执行事件

### 定时器

```js
// 第二版
function throttle(func, wait) {
    var timeout;
	var context, args;
    return function() {
        context = this;
        args = arguments;
        if (!timeout) {
            timeout = setTimeout(function(){
                timeout = null;
                func.apply(context, args)
            }, wait)
        }

    }
}
```

不会立即执行，但停止触发后会再执行一次

### 综合

```
// 第三版
function throttle(func, wait) {
    var timeout, context, args, result;
    var previous = 0;

    var later = function() {
        previous = +new Date();
        timeout = null;
        func.apply(context, args)
    };

    var throttled = function() {
        var now = +new Date();
        //下次触发 func 剩余的时间
        var remaining = wait - (now - previous);
        context = this;
        args = arguments;
         // 如果没有剩余的时间了或者你改了系统时间
        if (remaining <= 0 || remaining > wait) {
            if (timeout) {
                clearTimeout(timeout);
                timeout = null;
            }
            previous = now;
            func.apply(context, args);
        } else if (!timeout) {
            timeout = setTimeout(later, remaining);
        }
    };
    return throttled;
  
}
```

### 可配置头尾,加入取消

leading：false 表示禁用第一次执行
trailing: false 表示禁用停止触发的回调

```js
// 第四版
function throttle(func, wait, options) {
    var timeout, context, args, result;
    var previous = 0;
    if (!options) options = {};

    var later = function() {
        previous = options.leading === false ? 0 : new Date().getTime();
        timeout = null;
        func.apply(context, args);
        if (!timeout) context = args = null;
    };

    var throttled = function() {
        var now = new Date().getTime();
        if (!previous && options.leading === false) previous = now;
        var remaining = wait - (now - previous);
        context = this;
        args = arguments;
        if (remaining <= 0 || remaining > wait) {
            if (timeout) {
                clearTimeout(timeout);
                timeout = null;
            }
            previous = now;
            func.apply(context, args);
            if (!timeout) context = args = null;
        } else if (!timeout && options.trailing !== false) {
            timeout = setTimeout(later, remaining);
        }
    };
    throttled.cancel = function() {
    	clearTimeout(timeout);
    	previous = 0;
    	timeout = null;
	}
    return throttled;
}
```

`leading：false` 和 `trailing: false` 不能同时设置。



