---
title: 前端模块化
date: 2019-03-10 02:35:25
tags: javascript
categories: 前端
---
## 四种模块加载规范

- AMD
- CMD
- CommonJS
- ES6

## AMD
<!-- more -->
> The Asynchronous Module Definition

**AMD 是 RequireJS 在推广过程中对模块定义的规范化产出。**

一个使用requireJs的项目：

目录

```
* project/
    * index.html
    * vender/
        * main.js
        * require.js
        * add.js
        * square.js
        * multiply.js
```

`index.html` 的内容如下：

```html
<!DOCTYPE html>
<html>
    <head>
        <title>require.js</title>
    </head>
    <body>
        <h1>Content</h1>
        <script data-main="vender/main" src="vender/require.js"></script>
    </body>
</html>
```

`data-main="vender/main"` 表示主模块是 `vender` 下的 `main.js`。

`main.js` 的配置如下：

```js
// main.js
require(['./add', './square'], function(addModule, squareModule) {
    console.log(addModule.add(1, 1))
    console.log(squareModule.square(3))
});
```

require 的第一个参数表示依赖的模块的路径，第二个参数表示此模块的内容。

由此可以看出，`主模块`依赖 `add 模块`和 `square 模块`。

我们看下 `add 模块`即 `add.js` 的内容：

```js
// add.js
define(function() {
    console.log('加载了 add 模块');
    var add = function(x, y) {　
        return x + y;
    };

    return {　　　　　　
        add: add
    };
});
```

`requirejs` 为全局添加了 `define` 函数，你只要按照这种约定的方式书写这个模块即可。

那如果依赖的模块又依赖了其他模块呢？

我们来看看`主模块`依赖的 `square 模块`， `square 模块`的作用是求出一个数字的平方，比如输入 3 就返回 9，该模块依赖一个`乘法模块`，该乘法模块即 `multiply.js` 的代码如下：

```js
// multiply.js
define(function() {
    console.log('加载了 multiply 模块')
    var multiply = function(x, y) {　
        return x * y;
    };

    return {　　　　　　
        multiply: multiply
    };
});
```

而 `square 模块`就要用到 `multiply 模块`，其实写法跟 main.js 添加依赖模块一样：

```js
// square.js
define(['./multiply'], function(multiplyModule) {
    console.log('加载了 square 模块')
    return {　　　　　　
        square: function(num) {
            return multiplyModule.multiply(num, num)
        }
    };
});
```

require.js 会自动分析依赖关系，将需要加载的模块正确加载。

如果我们在浏览器中打开 `index.html`，打印的顺序为：

```
加载了 add 模块
加载了 multiply 模块
加载了 square 模块
2
9
```

## CMD

CMD 其实就是 SeaJS 在推广过程中对模块定义的规范化产出

一个使用seaJs的项目：

目录与 requirejs 项目目录相同:

```
* project/
    * index.html
    * vender/
        * main.js
        * require.js
        * add.js
        * square.js
        * multiply.js
```

`index.html` 的内容如下：

```
<!DOCTYPE html>
<html>
<head>
    <title>sea.js</title>
</head>
<body>
    <h1>Content</h1>
    <script src="vender/sea.js"></script>
    <script>
    // 在页面中加载主模块
    seajs.use("./vender/main");
    </script>
</body>

</html>
```

main.js 的内容如下：

```
// main.js
define(function(require, exports, module) {
    var addModule = require('./add');
    console.log(addModule.add(1, 1))

    var squareModule = require('./square');
    console.log(squareModule.square(3))
});
```

add.js 的内容如下：

```
// add.js
define(function(require, exports, module) {
    console.log('加载了 add 模块')
    var add = function(x, y) {　
        return x + y;
    };
    module.exports = {　　　　　　
        add: add
    };
});
```

square.js 的内容如下：

```
define(function(require, exports, module) {
    console.log('加载了 square 模块')
    var multiplyModule = require('./multiply');
    module.exports = {　　　　　　
        square: function(num) {
            return multiplyModule.multiply(num, num)
        }
    };

});
```

multiply.js 的内容如下：

```
define(function(require, exports, module) {
    console.log('加载了 multiply 模块')
    var multiply = function(x, y) {　
        return x * y;
    };
    module.exports = {　　　　　　
        multiply: multiply
    };
});
```

跟第一个例子是同样的依赖结构，即 main 依赖 add 和 square，square 又依赖 multiply。

如果我们在浏览器中打开 `index.html`，打印的顺序为：

```
加载了 add 模块
2
加载了 square 模块
加载了 multiply 模块
9
```

## AMD 与 CMD 的区别

1.AMD 推崇**依赖前置**，CMD 推崇**依赖就近**。看两个项目中的 main.js：

```
// require.js 例子中的 main.js
// 依赖必须一开始就写好
require(['./add', './square'], function(addModule, squareModule) {
    console.log(addModule.add(1, 1))
    console.log(squareModule.square(3))
});
// sea.js 例子中的 main.js
define(function(require, exports, module) {
    var addModule = require('./add');
    console.log(addModule.add(1, 1))

    // 依赖可以就近书写
    var squareModule = require('./square');
    console.log(squareModule.square(3))
});
```

2.对于依赖的模块，AMD 是**提前执行**，CMD 是**延迟执行**。看两个项目中的打印顺序：

```
// require.js
加载了 add 模块
加载了 multiply 模块
加载了 square 模块
2
9
// sea.js
加载了 add 模块
2
加载了 square 模块
加载了 multiply 模块
9
```

AMD 是将需要使用的模块先加载完再执行代码，而 CMD 是在 require 的时候才去加载模块文件，加载完再接着执行。

## CommonJS

AMD 和 CMD 都是用于浏览器端的模块规范，而在服务器端比如 node，采用的则是 CommonJS 规范。

导出模块的方式：

```
var add = function(x, y) {　
    return x + y;
};

module.exports.add = add;
```

引入模块的方式：

```
var add = require('./add.js');
console.log(add.add(1, 1));
```

我们将之前的例子改成 CommonJS 规范：

```
// main.js
var add = require('./add.js');
console.log(add.add(1, 1))

var square = require('./square.js');
console.log(square.square(3));
// add.js
console.log('加载了 add 模块')

var add = function(x, y) {　
    return x + y;
};

module.exports.add = add;
// multiply.js
console.log('加载了 multiply 模块')

var multiply = function(x, y) {　
    return x * y;
};

module.exports.multiply = multiply;
// square.js
console.log('加载了 square 模块')

var multiply = require('./multiply.js');

var square = function(num) {　
    return multiply.multiply(num, num);
};

module.exports.square = square;
```

如果我们执行 `node main.js`，打印的顺序为：

```
加载了 add 模块
2
加载了 square 模块
加载了 multiply 模块
9
```

跟 sea.js 的执行结果一致，也是在 require 的时候才去加载模块文件，加载完再接着执行。

> CommonJS 规范加载模块是同步的，也就是说，只有加载完成，才能执行后面的操作。
>
> 由于 Node.js 主要用于服务器编程，模块文件一般都已经存在于本地硬盘，所以加载起来比较快，不用考虑非同步加载的方式，所以 CommonJS 规范比较适用。
>
> 但是，如果是浏览器环境，要从服务器端加载模块，这时就必须采用非同步模式，因此浏览器端一般采用 AMD 规范。

## ES6

导出模块的方式：

```
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export {firstName, lastName, year};
```

引入模块的方式：

```
import {firstName, lastName, year} from './profile';
```

## ES6 与 CommonJS

它们有两个重大差异。

> 1. CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。
> 2. CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。因为 CommonJS 加载的是一个对象（即module.exports属性），该对象只有在脚本运行完才会生成。而 ES6 模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。

第一个差异：

举个例子：

```
// 输出模块 counter.js
var counter = 3;
function incCounter() {
  counter++;
}
module.exports = {
    counter: counter,
    incCounter: incCounter,
};
// 引入模块 main.js
var mod = require('./counter');

console.log(mod.counter);  // 3
mod.incCounter();
console.log(mod.counter); // 3
```

counter.js 模块加载以后，它的内部变化就影响不到输出的 mod.counter 了。这是因为 mod.counter 是一个原始类型的值，会被缓存。

但是如果修改 counter 为一个引用类型的话：

```
// 输出模块 counter.js
var counter = {
    value: 3
};

function incCounter() {
    counter.value++;
}
module.exports = {
    counter: counter,
    incCounter: incCounter,
};
// 引入模块 main.js
var mod = require('./counter.js');

console.log(mod.counter.value); // 3
mod.incCounter();
console.log(mod.counter.value); // 4
```

value 是会发生改变的。不过也可以说这是 "值的拷贝"，只是对于引用类型而言，值指的其实是引用。

而如果我们将这个例子改成 ES6:

```
// counter.js
export let counter = 3;
export function incCounter() {
  counter++;
}

// main.js
import { counter, incCounter } from './counter';
console.log(counter); // 3
incCounter();
console.log(counter); // 4
```

> ES6 模块的运行机制与 CommonJS 不一样。JS 引擎对脚本静态分析的时候，遇到模块加载命令 import，就会生成一个只读引用。等到脚本真正执行时，再根据这个只读引用，到被加载的那个模块里面去取值。换句话说，ES6 的 import 有点像 Unix 系统的“符号连接”，原始值变了，import 加载的值也会跟着变。因此，ES6 模块是动态引用，并且不会缓存值，模块里面的变量绑定其所在的模块。

## babel

```
// ES6
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export {firstName, lastName, year};
// Babel 编译后
'use strict';

Object.defineProperty(exports, "__esModule", {
  value: true
});
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

exports.firstName = firstName;
exports.lastName = lastName;
exports.year = year;

// ES6
import {firstName, lastName, year} from './profile';
// Babel 编译后
'use strict';

var _profile = require('./profile');
```

babel 只是把 ES6 模块语法转为 CommonJS 模块语法，然而浏览器是不支持这种模块语法的，所以直接跑在浏览器会报错的，如果想要在浏览器中运行，还是需要使用打包工具将代码打包。

## webpack

Babel 将 ES6 模块转为 CommonJS 后， webpack 又是怎么做的打包的呢？它该如何将这些文件打包在一起，从而能保证正确的处理依赖，以及能在浏览器中运行呢？

首先为什么浏览器中不支持 CommonJS 语法呢？

这是因为浏览器环境中并没有 module、 exports、 require 等环境变量。

换句话说，webpack 打包后的文件之所以在浏览器中能运行，就是靠模拟了这些变量的行为。

那怎么模拟呢？

我们以 CommonJS 项目中的 square.js 为例，它依赖了 multiply 模块：

```
console.log('加载了 square 模块')

var multiply = require('./multiply.js');


var square = function(num) {　
    return multiply.multiply(num, num);
};

module.exports.square = square;
```

webpack 会将其包裹一层，注入这些变量：

```
function(module, exports, require) {
    console.log('加载了 square 模块');

    var multiply = require("./multiply");
    module.exports = {
        square: function(num) {
            return multiply.multiply(num, num);
        }
    };
}
```

webpack如何将commonjs打包？

```js
// 自执行函数
(function(modules) {

    // 用于储存已经加载过的模块
    var installedModules = {};

    function require(moduleName) {

        if (installedModules[moduleName]) {
            return installedModules[moduleName].exports;
        }

        var module = installedModules[moduleName] = {
            exports: {}
        };

        modules[moduleName](module, module.exports, require);

        return module.exports;
    }

    // 加载主模块
    return require("main");

})({
    "main": function(module, exports, require) {

        var addModule = require("./add");
        console.log(addModule.add(1, 1))

        var squareModule = require("./square");
        console.log(squareModule.square(3));

    },
    "./add": function(module, exports, require) {
        console.log('加载了 add 模块');

        module.exports = {
            add: function(x, y) {
                return x + y;
            }
        };
    },
    "./square": function(module, exports, require) {
        console.log('加载了 square 模块');

        var multiply = require("./multiply");
        module.exports = {
            square: function(num) {
                return multiply.multiply(num, num);
            }
        };
    },

    "./multiply": function(module, exports, require) {
        console.log('加载了 multiply 模块');

        module.exports = {
            multiply: function(x, y) {
                return x * y;
            }
        };
    }
})
```
