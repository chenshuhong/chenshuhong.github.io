---
title: Object.defindProperty使用
date: 2019-03-26 15:39:14
tags: javascript
categories: 前端
---
`Object.defineProperty()`的作用就是直接在一个对象上定义一个新属性，或者修改一个已经存在的属性

```
Object.defineProperty(obj, prop, desc)
```

1. obj 需要定义属性的当前对象

2. prop 当前需要定义的属性名

3. desc 属性描述符


### 属性的特性以及内部属性

javacript 有三种类型的属性

1. 命名数据属性：拥有一个确定的值的属性。这也是最常见的属性
2. 命名访问器属性：通过`getter`和`setter`进行读取和赋值的属性

3. 内部属性：由JavaScript引擎内部使用的属性，不能通过JavaScript代码直接访问到，不过可以通过一些方法间接的读取和设置。比如，每个对象都有一个内部属性`[[Prototype]]`，你不能直接访问这个属性，但可以通过`Object.getPrototypeOf()`方法间接的读取到它的值。虽然内部属性通常用一个双吕括号包围的名称来表示，但实际上这并不是它们的名字，它们是一种抽象操作，是不可见的，根本没有上面两种属性有的那种字符串类型的属性

### 属性描述符

通过Object.defineProperty()为对象定义属性，有两种形式，且不能混合使用，分别为数据描述符，存取描述符，下面分别描述下两者的区别：

#### 数据描述符 --特有的两个属性（value,writable）

```js
let Person = {}
Object.defineProperty(Person, 'name', {
   value: 'jack',
   writable: false // 是否可以改变,默认是false
})
```

```
Person.name = 'csh'
console.log(Person.name) //jack
```

| 属性名       | 属性值    |
| ------------ | --------- |
| value        | undefined |
| get          | undefined |
| set          | undefined |
| writable     | false     |
| enumerable   | false     |
| configurable | false     |

#### 存取描述符 --是由一对 getter、setter 函数功能来描述的属性

`get`：一个给属性提供`getter`的方法，如果没有`getter`则为`undefined`。该方法返回值被用作属性值。默认为`undefined`。
 `set`：一个给属性提供`setter`的方法，如果没有`setter`则为`undefined`。该方法将接受唯一参数，并将该参数的新值分配给该属性。默认值为`undefined`。

```js
let Person = {}
let temp = null
Object.defineProperty(Person, 'name', {
  get: function () {
    return temp
  },
  set: function (val) {
    temp = val
  }
})
```

```
Person.name = 'jack'
console.log(Person.name) //jack
```

#### 数据描述符和存取描述均具有以下描述符

- configrable 

  configrable 描述属性是否配置，以及可否删除

```
let Person = {}
Object.defineProperty(Person, 'name', {
   value: 'jack',
   configurable: false,
   writable:true,
})
delete Person.name //Can't delete property 'name' of #<object>
//configurable为false不能删除
```

```
let Person = {}
Object.defineProperty(Person, 'name', {
   value: 'jack',
   configurable: false,
   writable:true,
})
Object.defineProperty(Person, 'name', {
   value: 'csh',
})//can't redefine property:name,不能重新定义
//configurable为false，writable为false时，不能重新定义
```

```
let Person = {}
Object.defineProperty(Person, 'name', {
   value: 'jack',
   configurable: true,
   writable:false,
})
//虽然不能通过Person.name = 'XXX' 赋值，但可以通过属性定义方式修改
Object.defineProperty(Person,'name',{
    value:'XXX'
})
```

从以上代码运行结果分析总结可知：

> 1. configurable: false 时，不能删除当前属性，且不能重新配置当前属性的描述符(有一个小小的意外：可以把writable的状态由true改为false,但是无法由false改为true),但是在writable: true的情况下，可以改变value的值
>
> 2. configurable: true时，可以删除当前属性，可以配置当前属性所有描述符。

- enumerable

   描述属性是否会出现在for in 或者 Object.keys()的遍历中

  ```
  let Person = {}
  Object.defineProperty(Person,'name',{value:'Jack',enumerable:false})
  Person.gender = 'male'
  Object.defineProperty(Person,'age',{value:'Jack',enumerable:true})
  console.log(Object.keys(Person)) //['gender','age']
  for(let k in Person){console.log(k)} //gender age
  console.log(Person.propertyIsEnumerable('name')) // true
  ```

#### 不变性

- 对象常量

  结合writable: false 和 configurable: false 就可以创建一个真正的常量属性（不可修改，不可重新定义或者删除）

- 禁止扩展

  如果你想禁止一个对象添加新属性并且保留已有属性，就可以使用Object.preventExtensions(...)

  ```
  "use strict";
  var p = {
    name: "jack"
  };
  Object.preventExtensions(p);
  p.gender = "male"; 
  // TypeError: Cannot add property gender, object is not extensible
  console.log(p.gender); 
  ```

   但仍然可以进行属性配置扩展，想也不能进行配置可用使用Object.seal(),会创建一个密封的对象，这个方法实际上会在一个现有对象上调用object.preventExtensions(...)并把所有现有属性标记为configurable:false。

  密封之后不仅不能添加新属性，也不能重新配置或者删除任何现有属性（虽然可以改属性的值）

  想也无法修改值可以用freeze，Object.freeze()会创建一个冻结对象，这个方法实际上会在一个现有对象上调用Object.seal(),并把所有现有属性标记为writable: false,这样就无法修改它们的值。

  这个方法是你可以应用在对象上级别最高的不可变性，它会禁止对于对象本身及其任意直接属性的修改（但是这个对象引用的其他对象是不受影响的）
   你可以深度冻结一个对象，具体方法为，首先这个对象上调用Object.freeze()然后遍历它引用的所有对象，并在这些对象上调用Object.freeze()。但是一定要小心，因为这么做有可能会无意中冻结其他共享对象。
