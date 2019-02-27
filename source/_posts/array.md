---
title: 数组
date: 2019-02-26 10:07:02
tags: javascript
categories: 前端
---
原文:[https://juejin.im/post/5b0903b26fb9a07a9d70c7e0](https://juejin.im/post/5b0903b26fb9a07a9d70c7e0)

## 创建

```js
// 字面量方式:
// 这个方法也是我们最常用的，在初始化数组的时候 相当方便
var a = [3, 11, 8];  // [3,11,8];
// 构造器:
// 实际上 new Array === Array,加不加new 一点影响都没有。
var a = Array(); // [] 
var a = Array(3); // [,,] 
var a = Array(3,11,8); // [ 3,11,8 ]
```

## ES6 Array.of() 

返回由所有参数值组成的数组,如果没有参数，就返回一个空数组。Array.of() 出现的目的是为了解决上述构造器因参数个数不同，导致的行为有差异的问题。

```js
let a = Array.of(3, 11, 8); // [3,11,8]
let a = Array.of(3); // [3]
```

## ES6 Arrary.from()

用于将两类对象转为真正的数组（不改变原对象，返回新的数组）。

第一个参数(必需):要转化为真正数组的对象。

第二个参数(可选): 类似数组的map方法，对每个元素进行处理，将处理后的值放入返回的数组。

第三个参数(可选): 用来绑定this。

```js
    // 1. 对象拥有length属性
    let obj = {0: 'a', 1: 'b', 2:'c', length: 2};
    let arr = Array.from(obj); // ['a','b'];
    // 2. 部署了 Iterator接口的数据结构 比如:字符串、Set、NodeList对象
    let arr = Array.from('hello'); // ['h','e','l','l','o']
    let arr = Array.from(new Set(['a','b'])); // ['a','b']
```

## 方法

数组原型提供了非常多的方法，这里分为三类来讲，一类会改变原数组的值，一类是不会改变原数组，以及数组的遍历方法。

### 改变原数组的方法(9个):

```
let a = [1,2,3];
ES5:
a.splice()/ a.sort() / a.pop()/ a.shift()/  a.push()/ a.unshift()/a.reverse()
ES6:
a.copyWithin() / a.fill
```

对于这些能够改变原数组的方法，要注意避免在循环遍历中改变原数组的选项，比如: 改变数组的长度，导致遍历的长度出现问题。

#### splice() 添加/删除数组元素

splice() 方法**向/从数组中添加/删除**项目，然后返回被删除的项目,array.splice(index,howmany,item1,.....,itemX)

参数

1. index：必需。整数，规定添加/删除项目的位置，使用负数可从数组结尾处规定位置。
2. howmany：可选。要删除的项目数量。如果设置为 0，则不会删除项目。
3. item1, ..., itemX： 可选。向数组添加的新项目。

返回值：如果有元素被删除,返回包含被删除项目的新数组。

```js
let a = [1, 2, 3, 4, 5, 6, 7];
let item = a.splice(0, 3); // [1,2,3]
console.log(a); // [4,5,6,7]
// 从数组下标0开始，删除3个元素
let item = a.splice(-1, 3); // [7]
// 从最后一个元素开始删除3个元素，因为最后一个元素，所以只删除了7

let a = [1, 2, 3, 4, 5, 6, 7];
let item = a.splice(0,3,'添加'); // [1,2,3]
console.log(a); // ['添加',4,5,6,7]
// 从数组下标0开始，删除3个元素，并添加元素'添加'
let b = [1, 2, 3, 4, 5, 6, 7];
let item = b.splice(-2,3,'添加1','添加2'); // [6,7]
console.log(b); // [1,2,3,4,5,'添加1','添加2']
// 从数组最后第二个元素开始，删除3个元素，并添加两个元素'添加1'、'添加2'

let a = [1, 2, 3, 4, 5, 6, 7];
let item = a.splice(0,0,'添加1','添加2'); // [] 没有删除元素，返回空数组
console.log(a); // ['添加1','添加2',1,2,3,4,5,6,7]
let b = [1, 2, 3, 4, 5, 6, 7];
let item = b.splice(-1,0,'添加1','添加2'); // [] 没有删除元素，返回空数组
console.log(b); // [1,2,3,4,5,6,'添加1','添加2',7] 在最后一个元素的前面添加两个元素
```

1. 数组如果元素不够，会删除到最后一个元素为止
2. 操作的元素，包括开始的那个元素
3. 可以添加很多个元素
4. 添加是在开始的元素前面添加的

#### sort() 数组排序

 sort()方法对数组元素进行排序，并返回这个数组。参数可选: 规定排序顺序的比较函数。默认情况下sort()方法没有传比较函数的话，默认按字母升序，如果元素不是字符串的话，会调用`toString()`方法将元素转化为字符串的Unicode位点，然后再比较字符。

```
// 字符串排列 看起来很正常
var a = ["Banana", "Orange", "Apple", "Mango"];
a.sort(); // ["Apple","Banana","Mango","Orange"]
// 数字排序的时候 因为转换成Unicode字符串之后，有些数字会比较大会排在后面 这显然不是我们想要的
var	a = [10, 1, 3, 20,25,8];
console.log(a.sort()) // [1,10,20,25,3,8];
```

**比较函数**

sort的比较函数有两个默认参数，要在函数中接收这两个参数，这两个参数是数组中两个要比较的元素，通常我们用 a 和 b 接收两个将要比较的元素：

- 若比较函数返回值<0，那么a将排到b的前面;
- 若比较函数返回值=0，那么a 和 b 相对位置不变；
- 若比较函数返回值>0，那么b 排在a 将的前面；

**sort排序常见用法**：

1. 数组元素为数字的升序、降序:

   ```
    var array =  [10, 1, 3, 4,20,4,25,8];
    // 升序 a-b < 0   a将排到b的前面，按照a的大小来排序的 
    // 比如被减数a是10，减数是20  10-20 < 0   被减数a(10)在减数b(20)前面   
    array.sort(function(a,b){
      return a-b;
    });
    console.log(array); // [1,3,4,4,8,10,20,25];
    // 降序 被减数和减数调换了  20-10>0 被减数b(20)在减数a(10)的前面
    array.sort(function(a,b){
      return b-a;
    });
    console.log(array); // [25,20,10,8,4,4,3,1];
   ```

2. 数组多条件排序

   ```
    var array = [{id:10,age:2},{id:5,age:4},{id:6,age:10},{id:9,age:6},{id:2,age:8},{id:10,age:9}];
        array.sort(function(a,b){
            if(a.id === b.id){// 如果id的值相等，按照age的值降序
                return b.age - a.age
            }else{ // 如果id的值不相等，按照id的值升序
                return a.id - b.id
            }
        })
     // [{"id":2,"age":8},{"id":5,"age":4},{"id":6,"age":10},{"id":9,"age":6},{"id":10,"age":9},{"id":10,"age":2}] 
   ```

3. 自定义比较函数，天空才是你的极限

类似的：**运用好返回值，我们可以写出任意符合自己需求的比较函数**

```
    var array = [{name:'Koro1'},{name:'Koro1'},{name:'OB'},{name:'Koro1'},{name:'OB'},{name:'OB'}];
    array.sort(function(a,b){
        if(a.name === 'Koro1'){// 如果name是'Koro1' 返回-1 ，-1<0 a排在b的前面
            return -1
        }else{ // 如果不是的话，a排在b的后面
          return 1
        }
    })
    // [{"name":"Koro1"},{"name":"Koro1"},{"name":"Koro1"},{"name":"OB"},{"name":"OB"},{"name":"OB"}]
```

#### pop，shift，push，unshift

pop() 方法删除一个数组中的最后的一个元素，并且返回这个元素。无参数

shift()方法删除数组的第一个元素，并返回这个元素。无参数

push() 方法可向数组的末尾添加一个或多个元素，并返回新的长度。参数: item1, item2, ..., itemX ,要添加到数组末尾的元素

unshift() 方法可向数组的开头添加一个或更多元素，并返回新的长度。参数: item1, item2, ..., itemX ,要添加到数组开头的元素

```
let  a =  [1,2,3];
let item = a.pop();  // 3
console.log(a); // [1,2]

let  a =  [1,2,3];
let item = a.shift();  // 1
console.log(a); // [2,3]

let  a =  [1,2,3];
let item = a.push('末尾');  // 4
console.log(a); // [1,2,3,'末尾']

let  a =  [1,2,3];
let item = a.unshift('开头');  // 4
console.log(a); // ['开头',1,2,3]
```

#### reverse

reverse() 方法用于颠倒数组中元素的顺序。

```
let  a =  [1,2,3];
a.reverse();  
console.log(a); // [3,2,1]
```

#### ES6: copyWithin() 

 在当前数组内部，将指定位置的成员复制到其他位置,并返回这个数组。

```
array.copyWithin(target, start = 0, end = this.length)
```

三个参数都是数值，如果不是，会自动转为数值.

1. target（必需）：从该位置开始替换数据。如果为负值，表示倒数。
2. start（可选）：从该位置开始读取数据，默认为 0。如果为负值，表示倒数。
3. end（可选）：到该位置前停止读取数据，默认等于数组长度。使用负数可从数组结尾处规定位置。

```
// -2相当于3号位，-1相当于4号位
        [1, 2, 3, 4, 5].copyWithin(0, -2, -1)
        // [4, 2, 3, 4, 5]
        var a=['OB1','Koro1','OB2','Koro2','OB3','Koro3','OB4','Koro4','OB5','Koro5']
        // 2位置开始被替换,3位置开始读取要替换的 5位置前面停止替换
        a.copyWithin(2,3,5)
        // ["OB1","Koro1","Koro2","OB3","OB3","Koro3","OB4","Koro4","OB5","Koro5"] 
```

1. 第一个参数是开始被替换的元素位置
2. 要替换数据的位置范围:从第二个参数是开始读取的元素，在第三个参数前面一个元素停止读取
3. 数组的长度不会改变
4. **读了几个元素就从开始被替换的地方替换几个元素**

#### ES6: fill() 

使用给定值，填充一个数组。

参数:

第一个元素(必须): 要填充数组的值

第二个元素(可选): 填充的开始位置,默认值为0

第三个元素(可选)：填充的结束位置，默认是为`this.length`

```
['a', 'b', 'c'].fill(7)
// [7, 7, 7]
['a', 'b', 'c'].fill(7, 1, 2)
// ['a', 7, 'c']
```

### 不改变原数组的方法(8个):

```js
ES5：
slice、join、toLocateString、toStrigin、cancat、indexOf、lastIndexOf、
ES7：
includes
```

#### slice

方法返回一个从开始到结束（不包括结束）选择的数组的一部分浅拷贝到一个新数组对象，且原数组不会被修改。

```js
array.slice(begin, end);
```

begin(可选): 索引数值,接受负值，从该索引处开始提取原数组中的元素,默认值为0。

end(可选):索引数值(不包括),接受负值，在该索引处前结束提取原数组元素，默认值为数组末尾(包括最后一个元素)。

新数组是浅拷贝的，**元素是简单数据类型，改变之后不会互相干扰**。如果是**复杂数据类型(对象,数组)的话，改变其中一个，另外一个也会改变**。

```
let a= [{name:'OBKoro1'}];
    let b=a.slice();
    console.log(b,a); // [{"name":"OBKoro1"}]  [{"name":"OBKoro1"}]
    // a[0].name='改变原数组';
    // console.log(b,a); // [{"name":"改变原数组"}] [{"name":"改变原数组"}]
    // b[0].name='改变拷贝数组',b[0].koro='改变拷贝数组';
    //  [{"name":"改变拷贝数组","koro":"改变拷贝数组"}] [{"name":"改变拷贝数组","koro":"改变拷贝数组"}]
```

#### join

 join() 方法用于把数组中的所有元素通过指定的分隔符进行分隔放入一个字符串，返回生成的字符串。

```
array.join(str)
```

str(可选): 指定要使用的分隔符，默认使用逗号作为分隔符。

```
let a= ['hello','world'];
let str=a.join(); // 'hello,world'
let str2=a.join('+'); // 'hello+world'
```

`join()/toString()`方法在数组元素是数组的时候，会将里面的数组也调用`join()/toString()`,如果是对象的话，对象会被转为`[object Object]`字符串。

```
let a= [['OBKoro1','23'],'test'];
let str1=a.join(); // OBKoro1,23,test
let b= [{name:'OBKoro1',age:'23'},'test'];
let str2 = b.join(); // [object Object],test
// 对象转字符串推荐JSON.stringify(obj);
```

#### toLocaleString

返回一个表示数组元素的字符串。该字符串由数组中的每个元素的 toLocaleString() 返回值经调用 join() 方法连接（由逗号隔开）组成。

调用数组的`toLocaleString`方法，数组中的每个元素都会调用自身的`toLocaleString`方法，对象调用对象的`toLocaleString`,Date调用Date的`toLocaleString`。

```
let a=[{name:'OBKoro1'},23,'abcd',new Date()];
let str=a.toLocaleString(); // [object Object],23,abcd,2018/5/28 下午1:52:20 
```

#### toString

 toString() 方法可把数组转换为由逗号链接起来的字符串。该方法的效果和join方法一样，都是用于数组转字符串的，但是与join方法相比没有优势，也不能自定义字符串的分隔符，因此不推荐使用。

**值得注意的是**：当数组和字符串操作的时候，js 会调用这个方法将数组自动转换成字符串

```
let b= [ 'toString','演示'].toString(); // toString,演示
let a= ['调用toString','连接在我后面']+'啦啦啦'; // 调用toString,连接在我后面啦啦啦
```

#### cancat

方法用于合并两个或多个数组，返回一个新数组。

```
var newArr =oldArray.concat(arrayX,arrayX,......,arrayX)
```

arrayX（必须）：该参数可以是具体的值，也可以是数组对象。可以是任意多个。

```
let a = [1, 2, 3];
let b = [4, 5, 6];
//连接两个数组
let newVal=a.concat(b); // [1,2,3,4,5,6]
// 连接三个数组
let c = [7, 8, 9]
let newVal2 = a.concat(b, c); // [1,2,3,4,5,6,7,8,9]
// 添加元素
let newVal3 = a.concat('添加元素',b, c,'再加一个'); 
// [1,2,3,"添加元素",4,5,6,7,8,9,"再加一个"]
// 合并嵌套数组  会浅拷贝嵌套数组
let d = [1,2 ];
let f = [3,[4]];
let newVal4 = d.concat(f); // [1,2,3,[4]]
```

**ES6扩展运算符...合并数组**：

因为ES6的语法更简洁易懂，所以现在合并数组我大部分采用`...`来处理，`...`运算符可以实现`cancat`的每个栗子，且更简洁和具有高度自定义数组元素位置的效果。

```
let a = [2, 3, 4, 5]
let b = [ 4,...a, 4, 4]
console.log(a,b); //  [2, 3, 4, 5] [4,2,3,4,5,4,4]
```

#### indexOf

返回在数组中可以找到一个给定元素的第一个索引，如果不存在，则返回-1。

array.indexOf(searchElement,fromIndex)

searchElement(必须):被查找的元素

fromIndex(可选):开始查找的位置(不能大于等于数组的长度，返回-1)，接受负值，默认值为0。

数组的indexOf搜索跟字符串的indexOf不一样,数组的indexOf使用严格相等`===`搜索元素，即**数组元素要完全匹配**才能搜索成功。

**注意**：indexOf()不能识别`NaN`,因为NaN === NaN 为false

```
let a=['啦啦',2,4,24,NaN]
console.log(a.indexOf('啦'));  // -1 
console.log(a.indexOf('NaN'));  // -1 
console.log(a.indexOf('啦啦')); // 0
```

数组去重

```
var array = [1, 1, '1'];

function unique(array) {
    var res = [];
    for (var i = 0, len = array.length; i < len; i++) {
        var current = array[i];
        if (res.indexOf(current) === -1) {
            res.push(current)
        }
    }
    return res;
}

console.log(unique(array));
```



#### lastIndexOf

方法返回指定元素,在数组中的最后一个的索引，如果不存在则返回 -1。（从数组后面往前查找）

arr.lastIndexOf(searchElement,fromIndex)

searchElement(必须): 被查找的元素

fromIndex(可选): 逆向查找开始位置，默认值数组的长度-1，即查找整个数组。

关于fromIndex有三个规则:

1. 正值。如果该值大于或等于数组的长度，则整个数组会被查找。
2. 负值。将其视为从数组末尾向前的偏移。(比如-2，从数组最后第二个元素开始往前查找)
3. 负值。其绝对值大于数组长度，则方法返回 -1，即数组不会被查找。

```
let a=['OB',4,'Koro1',1,2,'Koro1',3,4,5,'Koro1']; // 数组长度为10
 // let b=a.lastIndexOf('Koro1',4); // 从下标4开始往前找 返回下标2
 // let b=a.lastIndexOf('Koro1',100); //  大于或数组的长度 查找整个数组 返回9
 // let b=a.lastIndexOf('Koro1',-11); // -1 数组不会被查找
 let b=a.lastIndexOf('Koro1',-9); // 从第二个元素4往前查找，没有找到 返回-1
```

#### ES7 includes()

返回一个布尔值，表示某个数组是否包含给定的值

array.includes(searchElement,fromIndex=0)

searchElement(必须):被查找的元素

fromIndex(可选):默认值为0，参数表示搜索的起始位置，接受负值。正值超过数组长度，数组不会被搜索，返回false。负值绝对值超过长数组度，重置从0开始搜索。

**includes方法是为了弥补indexOf方法的缺陷而出现的:**

1. indexOf方法不能识别`NaN`

2. indexOf方法检查是否包含某个值不够语义化，需要判断是否不等于`-1`，表达不够直观

3. ```
   let a=['OB','Koro1',1,NaN];
   // let b=a.includes(NaN); // true 识别NaN
   // let b=a.includes('Koro1',100); // false 超过数组长度 不搜索
   // let b=a.includes('Koro1',-3);  // true 从倒数第三个元素开始搜索 
   // let b=a.includes('Koro1',-100);  // true 负值绝对值超过数组长度，搜索整个数组
   ```

### 遍历方法(12个):

```
ES5：
forEach、every 、some、 filter、map、reduce、reduceRight、
ES6：
find、findIndex、keys、values、entries
```

- 关于遍历的效率(从快到慢)
  - for 与 do while
  - forEach map every (这3个不相上下,可认为运行速度差不多)
  - $.each
  - $(e).each
  - for in
- 尽量不要在遍历的时候，修改后面要遍历的值
- 尽量不要在遍历的时候修改数组的长度（删除/添加）

#### forEach

按顺序为数组中含有效值的每一项执行一次回调函数。

array.forEach(function(currentValue, index, arr), thisValue)

function(必须): 数组中每个元素需要调用的函数。

```
// 回调函数的参数
1. currentValue(必须),数组当前元素的值
2. index(可选), 当前元素的索引值
3. arr(可选),数组对象本身
```

thisValue(可选):  当执行回调函数时this绑定对象的值，默认值为`undefined`

**关于forEach()你要知道**：

- 无法中途退出循环，只能用`return`退出本次回调，进行下一次回调。
- 它总是返回 undefined值,即使你return了一个值。

**下面类似语法同样适用这些规则**

```
1.对于空数组是不会执行回调函数的
2.对于已在迭代过程中删除的元素，或者空元素会跳过回调函数
3.遍历次数再第一次循环前就会确定，再添加到数组中的元素不会被遍历。
4.如果已经存在的值被改变，则传递给 callback 的值是遍历到他们那一刻的值。
```

```
let a = [1, 2, ,3]; // 最后第二个元素是空的，不会遍历(undefined、null会遍历)
let obj = { name: 'OBKoro1' };
let result = a.forEach(function (value, index, array) { 
	a[3] = '改变元素';
	a.push('添加到尾端，不会被遍历')
	console.log(value, 'forEach传递的第一个参数'); // 分别打印 1 ,2 ,改变元素
	console.log(this.name); // OBKoro1 打印三次 this绑定在obj对象上
	// break; // break会报错
	return value; // return只能结束本次回调 会执行下次回调
	console.log('不会执行，因为return 会执行下一次循环回调')
}, obj);
console.log(result); // 即使return了一个值,也还是返回undefined
// 回调函数也接受箭头函数写法
```

#### every 

方法用于检测数组所有元素是否都符合函数定义的条件

array.every(function(currentValue, index, arr), thisValue)

参数意义同上

方法返回值规则:

1. 如果数组中检测到**有一个元素不满足，则整个表达式返回 false**，且剩余的元素不会再进行检测。
2. 如果所有元素**都满足条件，则返回 true**

```
function isBigEnough(element, index, array) { 
	return element >= 10; // 判断数组中的所有元素是否都大于10
}
let result = [12, 5, 8, 130, 44].every(isBigEnough);   // false
let result = [12, 54, 18, 130, 44].every(isBigEnough); // true
// 接受箭头函数写法 
[12, 5, 8, 130, 44].every(x => x >= 10); // false
[12, 54, 18, 130, 44].every(x => x >= 10); // true
```

#### some

数组中的是否有满足判断条件的元素

array.some(function(currentValue, index, arr), thisValue)

参数意义同上

方法返回值规则：

1. 如果**有一个元素满足条件，则表达式返回true**, 剩余的元素不会再执行检测。
2. 如果**没有满足条件的元素，则返回false**。

```
function isBigEnough(element, index, array) {
	return (element >= 10); //数组中是否有一个元素大于 10
}
let result = [2, 5, 8, 1, 4].some(isBigEnough); // false
let result = [12, 5, 8, 1, 4].some(isBigEnough); // true
```

#### filter 

返回一个新数组, 其包含通过所提供函数实现的测试的所有元素。

let new_array = arr.filter(function(currentValue, index, arr), thisArg)

参数意义同上

```
let a = [32, 33, 16, 40];
let result = a.filter(function (value, index, array) {
	return value >= 18; // 返回a数组中所有大于18的元素
});
console.log(result,a);// [32,33,40] [32,33,16,40]
```

数组去重

```
var array = [1, 2, 1, 1, '1'];

function unique(array) {
    var res = array.filter(function(item, index, array){
        return array.indexOf(item) === index;
    })
    return res;
}

console.log(unique(array));
```

扩展，你用es6 Set去重

```
var array = [1, 2, 1, 1, '1'];

function unique(array) {
   return Array.from(new Set(array));
}

//更简化
function unique(array) {
    return [...new Set(array)];
}

console.log(unique(array)); // [1, 2, "1"]
```



#### map 

创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。

let new_array = arr.map(function(currentValue, index, arr), thisArg)

参数意义同上

```
let a = ['1','2','3','4'];
let result = a.map(function (value, index, array) {
  return value + '新数组的新元素'
});
console.log(result, a); 
// ["1新数组的新元素","2新数组的新元素","3新数组的新元素","4新数组的新元素"] ["1","2","3","4"]
```

#### reduce 

reduce() 方法对累加器和数组中的每个元素（从左到右）应用一个函数，最终合并为一个值。

array.reduce(function(total, currentValue, currentIndex, arr), initialValue)

参数：

function(必须): 数组中每个元素需要调用的函数。

```
// 回调函数的参数
1. total(必须)，初始值, 或者上一次调用回调返回的值
2. currentValue(必须),数组当前元素的值
3. index(可选), 当前元素的索引值
4. arr(可选),数组对象本身
```

initialValue(可选): 指定第一次回调 的第一个参数。

回调第一次执行时:

- 如果 initialValue 在调用 reduce 时被提供，那么第一个 total 将等于 initialValue，此时 currentValue 等于数组中的第一个值；
- 如果 initialValue 未被提供，那么 total 等于数组中的第一个值，currentValue 等于数组中的第二个值。此时如果数组为空，那么将抛出 TypeError。
- 如果数组仅有一个元素，并且没有提供 initialValue，或提供了 initialValue 但数组为空，那么回调不会被执行，数组的唯一值将被返回。

```
 // 数组求和 
let sum = [0, 1, 2, 3].reduce(function (a, b) {
	return a + b;
}, 0);
// 6
// 将二维数组转化为一维 将数组元素展开
let flattened = [[0, 1], [2, 3], [4, 5]].reduce(
	(a, b) => a.concat(b),
	[]
);
// [0, 1, 2, 3, 4, 5]
```

#### reduceRight 

这个方法除了与reduce执行方向相反外，其他完全与其一致，请参考上述 reduce 方法介绍。

#### ES6：find()& findIndex() 

find()定义：用于找出第一个符合条件的数组成员，并返回该成员，如果没有符合条件的成员，则返回undefined。

findIndex()定义：返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1。

这两个方法

语法：

```
let new_array = arr.find(function(currentValue, index, arr), thisArg)
let new_array = arr.findIndex(function(currentValue, index, arr), thisArg)
```

function(必须): 数组中每个元素需要调用的函数。

```
// 回调函数的参数
1. currentValue(必须),数组当前元素的值
2. index(可选), 当前元素的索引值
3. arr(可选),数组对象本身
```

thisValue(可选):  当执行回调函数时this绑定对象的值，默认值为`undefined`

这两个方法都可以识别`NaN`,弥补了`indexOf`的不足.

```
// find
let a = [1, 4, -5, 10].find((n) => n < 0); // 返回元素-5
let b = [1, 4, -5, 10,NaN].find((n) => Object.is(NaN, n));  // 返回元素NaN
// findIndex
let a = [1, 4, -5, 10].findIndex((n) => n < 0); // 返回索引2
let b = [1, 4, -5, 10,NaN].findIndex((n) => Object.is(NaN, n));  // 返回索引4
```

#### ES6 keys()&values()&entries() 遍历键名、遍历键值、遍历键名+键值

三个方法都返回一个新的 Array Iterator 对象，对象根据方法不同包含不同的值。

```
for (let index of ['a', 'b'].keys()) {
	console.log(index);
}
// 0
// 1
    
for (let elem of ['a', 'b'].values()) {
	console.log(elem);
}
// 'a'
// 'b'
    
for (let [index, elem] of ['a', 'b'].entries()) {
	console.log(index, elem);
}
// 0 "a"
// 1 "b"
```

在`for..of`中如果遍历中途要退出，可以使用`break`退出循环。

如果不使用`for...of`循环，可以手动调用遍历器对象的next方法，进行遍历:

```
    let letter = ['a', 'b', 'c'];
    let entries = letter.entries();
    console.log(entries.next().value); // [0, 'a']
    console.log(entries.next().value); // [1, 'b']
    console.log(entries.next().value); // [2, 'c']
```

# 求数组的最大值和最小值

### Math.max([value1[,value2, ...]])

- 如果有任一参数不能被转换为数值，则结果为 NaN。
- max 是 Math 的静态方法，所以应该像这样使用：Math.max()，而不是作为 Math 实例的方法 (简单的来说，就是不使用 new )
- 如果没有参数，则结果为 `-Infinity` (注意是负无穷大)

```
Math.max(true, 0) // 1
Math.max(true, '2', null) // 2
Math.max(1, undefined) // NaN
Math.max(1, {}) // NaN
var min = Math.min(); //Infinity
var max = Math.max(); //-Infinity
console.log(min > max);
```

### 原始方法

```js
var arr = [6, 4, 1, 8, 2, 11, 23];

var result = arr[0];
for (var i = 1; i < arr.length; i++) {
    result =  Math.max(result, arr[i]);
}
console.log(result);
```

### reduce

```js
var arr = [6, 4, 1, 8, 2, 11, 23];

function max(prev, next) {
    return Math.max(prev, next);
}
console.log(arr.reduce(max));
```

### 排序

```js
var arr = [6, 4, 1, 8, 2, 11, 23];

arr.sort(function(a,b){return a - b;});
console.log(arr[arr.length - 1])
```

### eval

```js
var arr = [6, 4, 1, 8, 2, 11, 23];

var max = eval("Math.max(" + arr + ")");
console.log(max)
```

### apply

```js
var arr = [6, 4, 1, 8, 2, 11, 23];
console.log(Math.max.apply(null, arr))
```

### ES6

```js
var arr = [6, 4, 1, 8, 2, 11, 23];
console.log(Math.max(...arr))
```

## 数组扁平化

数组的扁平化，就是将一个嵌套多层的数组 array (嵌套可以是任何层数)转换为只有一层的数组。

举个例子，假设有个名为 flatten 的函数可以做到数组扁平化，效果就会如下：

```
var arr = [1, [2, [3, 4]]];
console.log(flatten(arr)) // [1, 2, 3, 4]
```

### 递归

```js
// 方法 1
var arr = [1, [2, [3, 4]]];

function flatten(arr) {
    var result = [];
    for (var i = 0, len = arr.length; i < len; i++) {
        if (Array.isArray(arr[i])) {
            result = result.concat(flatten(arr[i]))
        }
        else {
            result.push(arr[i])
        }
    }
    return result;
}


console.log(flatten(arr))
```

### reduce

```js
// 方法2
var arr = [1, [2, [3, 4]]];

function flatten(arr) {
    return arr.reduce(function(prev, next){
        return prev.concat(Array.isArray(next) ? flatten(next) : next)
    }, [])
}

console.log(flatten(arr))
```

### ...

```js
// 方法4
var arr = [1, [2, [3, 4]]];

function flatten(arr) {

    while (arr.some(item => Array.isArray(item))) {
        arr = [].concat(...arr);
    }

    return arr;
}

console.log(flatten(arr))
```

# 在数组中查找指定元素

### 实现findIndex

```js
function findIndex(array, predicate, context) {
    for (var i = 0; i < array.length; i++) {
        if (predicate.call(context, array[i], i, array)) return i;
    }
    return -1;
}

console.log(findIndex([1, 2, 3, 4], function(item, i, array){
    if (item == 3) return true;
})) // 2
```

### 实现findLastIndex

```js
function findLastIndex(array, predicate, context) {
    var length = array.length;
    for (var i = length - 1; i >= 0; i--) {
        if (predicate.call(context, array[i], i, array)) return i;
    }
    return -1;
}

console.log(findLastIndex([1, 2, 3, 4], function(item, index, array){
    if (item == 1) return true;
})) // 0
```

### createIndexFinder

findIndex 和 findLastIndex 其实有很多重复的部分，如何精简冗余的内容

```js
function createIndexFinder(dir) {
    return function(array, predicate, context) {

        var length = array.length;
        var index = dir > 0 ? 0 : length - 1;

        for (; index >= 0 && index < length; index += dir) {
            if (predicate.call(context, array[index], index, array)) return index;
        }

        return -1;
    }
}

var findIndex = createIndexFinder(1);
var findLastIndex = createIndexFinder(-1);
```

### sortedIndex

在一个排好序的数组中找到 value 对应的位置，保证插入数组后，依然保持有序的状态。

假设该函数命名为 sortedIndex，效果为：

```js
sortedIndex([10, 20, 30], 25); // 2	
```

```js
// 第一版
function sortedIndex(array, obj) {

    var low = 0, high = array.length;

    while (low < high) {
        var mid = Math.floor((low + high) / 2);
        if (array[mid] < obj) low = mid + 1;
        else high = mid;
    }

    return high;
};

console.log(sortedIndex([10, 20, 30, 40, 50], 35)) // 3
```

现在的方法虽然能用，但通用性不够，比如我们希望能处理这样的情况：

```js
// stooges 配角 比如 三个臭皮匠 The Three Stooges
var stooges = [{name: 'stooge1', age: 10}, {name: 'stooge2', age: 30}];

var result = sortedIndex(stooges, {name: 'stooge3', age: 20}, function(stooge){
    return stooge.age
});

console.log(result) // 1
```

```js
// 第二版
function cb(func, context) {
       if (context === void 0) return func;
       return function() {
           return func.apply(context, arguments);
      };
}
function sortedIndex(array, obj, iteratee, context) {

    iteratee = cb(iteratee, context)

    var low = 0, high = array.length;
    while (low < high) {
        var mid = Math.floor((low + high) / 2);
        if (iteratee(array[mid]) < iteratee(obj)) low = mid + 1;
        else high = mid;
    }
    return high;
};
```

