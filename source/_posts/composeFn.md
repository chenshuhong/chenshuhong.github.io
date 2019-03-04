---
title: 组合函数
date: 2019-03-04 17:30:51
tags: javascript
categories: 前端
---
## 需求

我们需要写一个函数，输入 'kevin'，返回 'HELLO, KEVIN'。

## 尝试

```
var toUpperCase = function(x) { return x.toUpperCase(); };
var hello = function(x) { return 'HELLO, ' + x; };

var greet = function(x){
    return hello(toUpperCase(x));
};

greet('kevin');
```

还好我们只有两个步骤，首先小写转大写，然后拼接字符串。如果有更多的操作，greet 函数里就需要更多的嵌套，类似于 `fn3(fn2(fn1(fn0(x))))`。

## 优化

试想我们写个 compose 函数：

```
var compose = function(f,g) {
    return function(x) {
        return f(g(x));
    };
};
```

greet 函数就可以被优化为：

```
var greet = compose(hello, toUpperCase);
greet('kevin');
```

利用 compose 将两个函数组合成一个函数，让代码从右向左运行，而不是由内而外运行，可读性大大提升。这便是函数组合。

但是现在的 compose 函数也只是能支持两个参数，如果有更多的步骤呢？我们岂不是要这样做：

```
compose(d, compose(c, compose(b, a)))
```

为什么我们不写一个帅气的 compose 函数支持传入多个函数呢？这样就变成了：

```
compose(d, c, b, a)
```

## compose

我们直接抄袭 underscore 的 compose 函数的实现：

```
function compose() {
    var args = arguments;
    var start = args.length - 1;
    return function() {
        var i = start;
        var result = args[start].apply(this, arguments);
        while (i--) result = args[i].call(this, result);
        return result;
    };
};
```

