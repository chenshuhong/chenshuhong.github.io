---
title: 闭包
date: 2019-02-26 01:55:23
tags: javascript
categories: 前端
---
原文：[https://github.com/mqyqingfeng/Blog/issues/9](https://github.com/mqyqingfeng/Blog/issues/9)

- 从理论角度：所有的函数。因为它们都在创建的时候就将上层上下文的数据保存起来了。哪怕是简单的全局变量也是如此，因为函数中访问全局变量就相当于是在访问自由变量，这个时候使用最外层的作用域。
- 从实践角度：以下函数才算是闭包：
  1. 即使创建它的上下文已经销毁，它仍然存在（比如，内部函数从父函数中返回）
  2. 在代码中引用了自由变量

以下面代码为例，首先分析上下文栈和执行上下文的变化情况

```
var scope = "global scope";
function checkscope(){
    var scope = "local scope";
    function f(){
        return scope;
    }
    return f;
}

var foo = checkscope();
foo();
```

1. 执行全局代码，创建全局执行上下文，全局上下文被压入执行上下文栈

```js
    ECStack = [
        globalContext
    ];
```

2. 全局上下文初始化

```js
    globalContext = {
        VO: [global],
        Scope: [globalContext.VO],
        this: globalContext.VO
    }
```

3. 初始化的同时，checkscope 函数被创建，保存作用域链到函数的内部属性[[scope]]

```js
    checkscope.[[scope]] = [
      globalContext.VO
    ];
```

3. 执行 checkscope 函数，创建 checkscope 函数执行上下文，checkscope 函数执行上下文被压入执行上下文栈

```js
    ECStack = [
        checkscopeContext,
        globalContext
    ];
```

4. checkscope 函数执行上下文初始化：
   1. 复制函数 [[scope]] 属性创建作用域链，
   2. 用 arguments 创建活动对象，
   3. 初始化活动对象，即加入形参、函数声明、变量声明，
   4. 将活动对象压入 checkscope 作用域链顶端。同时 f 函数被创建，保存作用域链到 f 函数的内部属性[[scope]]

```
checkscopeContext = {
        AO: {
            arguments: {
                length: 0
            },
            scope: undefined,
            f: reference to function f(){}
        },
        Scope: [AO, globalContext.VO],
        this: undefined
    }
f.[[scope]] = [
	checkscopeContext.AO,
	globalContext.VO
];    
```

5. checkscope 函数执行完毕，checkscope 执行上下文从执行上下文栈中弹出

```
ECStack = [
        globalContext
    ];
```

6. 执行 f 函数，创建 f 函数执行上下文，f 执行上下文被压入执行上下文栈

```
ECStack = [
        fContext,
        globalContext
    ];
```

7. f 执行上下文初始化，创建变量对象、作用域链、this等

   1. 复制函数 [[scope]] 属性创建作用域链

   1. 用 arguments 创建活动对象

   1. 初始化活动对象，即加入形参、函数声明、变量声明

   1. 将活动对象压入 f 作用域链顶端

```
fContext = {
        AO: {
            arguments: {
                length: 0
            }
        },
        Scope: [AO, checkscopeContext.AO, globalContext.VO],
        this: undefined
    }
```

8. f 函数执行，沿着作用域链查找 scope 值，返回 scope 值
9. f 函数执行完毕，f 函数上下文从执行上下文栈中弹出

```
ECStack = [
        globalContext
    ];
```



f 执行上下文维护了一个作用域链：

```js
fContext = {
    Scope: [AO, checkscopeContext.AO, globalContext.VO],
}
```

对的，就是因为这个作用域链，f 函数依然可以读取到 checkscopeContext.AO 的值，说明当 f 函数引用了 checkscopeContext.AO 中的值的时候，即使 checkscopeContext 被销毁了，但是 JavaScript 依然会让 checkscopeContext.AO 活在内存中，f 函数依然可以通过 f 函数的作用域链找到它，正是因为 JavaScript 做到了这一点，从而实现了闭包这个概念。
