---
title: js的label使用
date: 2019-03-14 20:44:44
tags: javascript
categories: 前端
---
## label

使用 label 语句可以在代码中添加标签，以便将来使用。以下是 label 语句的语法：

```
label: statement
```

下面是一个示例：

```
start: for (var i=0; i < count; i++) {
	alert(i);
}
```
<!-- more -->
这个例子中定义的 start 标签可以在将来由 break 或 continue 语句引用。加标签的语句一般都要与 for 语句等循环语句配合使用。 如下面的例子所示： 

```js
var num = 0;
outermost:
for (var i=0; i < 10; i++) {
	for (var j=0; j < 10; j++) {
		if (i == 5 && j == 5) {
			break outermost;
		}
		num++;
	}
}
alert(num); //55
```

在这个例子中， outermost 标签表示外部的 for 语句。如果每个循环正常执行 10 次，则 num++语句就会正常执行 100 次。换句话说，如果两个循环都自然结束， num 的值应该是 100。但内部循环中的 break 语句带了一个参数：要返回到的标签。添加这个标签的结果将导致 break 语句不仅会退出内部的 for 语句（即使用变量 j 的循环），而且也会退出外部的 for 语句（即使用变量 i 的循环）。为此，当变量 i 和 j 都等于 5 时， num 的值正好是 55。同样， continue 语句也可以像这样与 label 语句联用，如下面的例子所示： 

```
var num = 0;
outermost:
for (var i=0; i < 10; i++) {
	for (var j=0; j < 10; j++) {
		if (i == 5 && j == 5) {
			continue outermost;
		}
		num++;
	}
}
alert(num); //95
```

在这种情况下， continue 语句会强制继续执行循环——退出内部循环，执行外部循环。当 j 是 5时， continue 语句执行，而这也就意味着内部循环少执行了 5 次，因此 num 的结果是 95。虽然联用 break、 continue 和 label 语句能够执行复杂的操作，但如果使用过度，也会给调试带来麻烦。在此，我们建议如果使用 label 语句，一定要使用描述性的标签，同时不要嵌套过多的循环。 
