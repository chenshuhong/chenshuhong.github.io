---
title: 执行上下文
date: 2019-02-24 20:58:00
tags: javascript
categories: 前端
---
## 词法作用域

首先了解下词法作用域—函数的作用域在函数定义的时候就决定了

```javascript
var value = 1;

function foo() {
    console.log(value);
}

function bar() {
    var value = 2;
    foo();
}

bar();
```

执行 foo 函数，先从 foo 函数内部查找是否有局部变量 value，如果没有，就根据书写的位置，查找上面一层的代码，也就是 value 等于 1，所以结果会打印 1。

## 执行上下文

JavaScript 引擎并非一行一行地分析和执行程序，而是一段一段地分析执行。当执行一段可执行代码（全局代码，函数代码，eval代码）的时候，会进行一个“准备工作”，这里的“准备工作”，让我们用个更专业一点的说法，就叫做"执行上下文(execution context)"。执行上下文栈（Execution context stack，ECS）来管理执行上下文。

对于每个执行上下文，都有三个重要属性

- 变量对象(Variable object，VO)
- 作用域链(Scope chain)
- this

## 变量对象

变量对象是与执行上下文相关的数据作用域，存储了在上下文中定义的变量和函数声明。

因为不同执行上下文下的变量对象稍有不同，所以我们来聊聊全局上下文下的变量对象和函数上下文下的变量对象。

### 全局上下文

全局上下文中的变量对象就是全局对象

### 函数上下文

在函数上下文中，我们用活动对象(activation object, AO)来表示变量对象。

活动对象和变量对象其实是一个东西，只是变量对象是规范上的或者说是引擎实现上的，不可在 JavaScript 环境中访问，只有到当进入一个执行上下文中，这个执行上下文的变量对象才会被激活，所以才叫 activation object 呐，而只有被激活的变量对象，也就是活动对象上的各种属性才能被访问。

活动对象是在进入函数上下文时刻被创建的，它通过函数的 arguments 属性初始化。arguments 属性值是 Arguments 对象。

### 执行过程

具体看文章[https://github.com/mqyqingfeng/Blog/issues/5](https://github.com/mqyqingfeng/Blog/issues/5)

总结：

1. 全局上下文的变量对象初始化是全局对象
2. 函数上下文的变量对象初始化只包括 Arguments 对象
3. 在进入执行上下文时会给变量对象添加形参、函数声明、变量声明等初始的属性值
4. 在代码执行阶段，会再次修改变量对象的属性值

## 作用域链

具体看文章[https://github.com/mqyqingfeng/Blog/issues/6](https://github.com/mqyqingfeng/Blog/issues/6)

## this

this一般有几种调用场景
var obj = {a: 1, b: function(){console.log(this);}}
1、作为对象调用时，指向该对象 obj.b(); // 指向obj
2、作为函数调用, var b = obj.b; b(); // 指向全局window
3、作为构造函数调用 var b = new Fun(); // this指向当前实例对象
4、作为call与apply调用 obj.b.apply(object, []); // this指向当前的object

## 整合

```js
var scope = "global scope";
function checkscope(){
    var scope = "local scope";
    function f(){
        return scope;
    }
    return f();
}
checkscope();
```

执行过程如下

1. 执行全局代码，创建全局执行上下文，全局上下文被压入执行上下文栈

```js
    ECStack = [
        globalContext
    ];
```

1. 全局上下文初始化

```js
    globalContext = {
        VO: [global],
        Scope: [globalContext.VO],
        this: globalContext.VO
    }
```

1. 初始化的同时，checkscope 函数被创建，保存作用域链到函数的内部属性[[scope]]

```js
    checkscope.[[scope]] = [
      globalContext.VO
    ];
```

1. 执行 checkscope 函数，创建 checkscope 函数执行上下文，checkscope 函数执行上下文被压入执行上下文栈

```js
    ECStack = [
        checkscopeContext,
        globalContext
    ];
```

1. checkscope 函数执行上下文初始化：

   1. 复制函数 [[scope]] 属性创建作用域链，
   2. 用 arguments 创建活动对象，
   3. 初始化活动对象，即加入形参、函数声明、变量声明，
   4. 将活动对象压入 checkscope 作用域链顶端。
   5. 同时 f 函数被创建，保存作用域链到 f 函数的内部属性[[scope]]

   ```js
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
   ```

2. 执行 f 函数，创建 f 函数执行上下文，f 函数执行上下文被压入执行上下文栈

   ```
   ECStack = [
           fContext,
           checkscopeContext,
           globalContext
       ];
   ```

3. f 函数执行上下文初始化, 以下跟第 5 步相同：

   1. 复制函数 [[scope]] 属性创建作用域链
   2. 用 arguments 创建活动对象
   3. 初始化活动对象，即加入形参、函数声明、变量声明
   4. 将活动对象压入 f 作用域链顶端

   ```js
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

1. f 函数执行，沿着作用域链查找 scope 值，返回 scope 值

2. f 函数执行完毕，f 函数上下文从执行上下文栈中弹出

   ```
   ECStack = [
           checkscopeContext,
           globalContext
       ];
   ```

3. checkscope 函数执行完毕，checkscope 执行上下文从执行上下文栈中弹出

   ```
   ECStack = [
           globalContext
       ];
   ```
