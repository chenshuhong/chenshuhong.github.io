---
title: css3动画
date: 2019-02-28 16:34:25
tags: css
categories: 前端
---
**原文[https://juejin.im/post/5a424a796fb9a045023be66c](https://juejin.im/post/5a424a796fb9a045023be66c)**

## transition

`transition`允许css的属性值在一定的时间区间内平滑地**过渡**。这种效果可以在鼠标单击、获得焦点、被点击或对元素任何改变中触发，并圆滑地以动画效果改变CSS的属性值。

```
transition ：transition-property || transition-duration || transition-timing-function || transition-delay;
```

`transition`主要包含四个属性值：执行变换的属性：`transition-property`，变换延续的时间：`transition-duration`，在延续时间段，变换的速率变化：`transition-timing-function`，变换延迟时间：`transition-delay`。

###  transition-property

```
transition-property: none || all || property;
```

`transition-property`是用来指定当元素其中一个属性改变时执行`transition`效果。

none: 没有属性会获得过渡效果；

all: 所有属性都将获得过渡效果,**也是其默认值**；

property: 定义应用过渡效果的 CSS 属性名称列表，列表以逗号分隔。

### transition-duration

```
transition-duration: time;
```

`transition-duration`是用来指定元素 转换过程的持续时间，取值time为数值，单位为s（秒）或者ms(毫秒)，其默认值是0，也就是变换时是即时的。

###  transition-timing-function

```
transition-timing-function: linear || ease || ease-in || ease-out || ease-in-out || cubic-
bezier(n,n,n,n);
```

| 值                            | 描述                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| linear                        | 规定以相同速度开始至结束的过渡效果。                         |
| ease                          | 规定慢速开始，然后变快，然后慢速结束的过渡效果。             |
| ease-in                       | 规定以慢速开始的过渡效果。                                   |
| ease-out                      | 规定以慢速结束的过渡效果。                                   |
| ease-in-out                   | 规定以慢速开始和结束的过渡效果。                             |
| cubic-bezier(*n*,*n*,*n*,*n*) | 在 cubic-bezier 函数中定义自己的值。可能的值是 0 至 1 之间的数值。 |

### transition-delay

`transition-delay`是用来指定一个动画开始执行的时间，也就是说当改变元素属性值后多长时间开始执行`transition`效果，其取值time为数值，单位为s（秒）或者ms(毫秒)， 默认大小是"0"，也就是变换立即执行，没有延迟。

### transitionend 

transitionend 事件在 CSS 完成过渡后触发。

**注意：** 如果过渡在完成前移除，例如 CSS [transition-property](http://www.runoob.com/cssref/css3-pr-transition-property.html) 属性被移除，过渡事件将不被触发。

```
// Safari 3.1 到 6.0 代码
document.getElementById("myDIV").addEventListener("webkitTransitionEnd", myFunction);

// 标准语法
document.getElementById("myDIV").addEventListener("transitionend", myFunction);
```



### 注意

- `transition`需要明确知道，开始状态和结束状态的具体数值，才能计算出中间状态。比如，height从0px变化到100px，`transition`可以算出中间状态。但是，`transition`没法算出0px到auto的中间状态，也就是说，如果开始或结束的设置是height: auto，那么就不会产生动画效果。

- `transition`需要事件触发，所以没法在网页加载时自动发生。

- `transition`是一次性的，不能重复发生，除非一再触发。

## animation

不同于`transition`只能定义首尾两个状态，`animation`可以定义任意多的关键帧，因而能实现更复杂的动画效果。

```
animation: animation-name || animation-duration || animation-timing-function || animation-delay || animation-iteration-count || animation-direction
```

`animation`主要包含六个属性，具体含义如下：

| 值                          | 描述                                         |
| --------------------------- | -------------------------------------------- |
| *animation-name*            | 规定需要绑定到选择器的 keyframe 名称。。     |
| *animation-duration*        | 规定完成动画所花费的时间，以秒或毫秒计。     |
| *animation-timing-function* | 规定动画的速度曲线。                         |
| *animation-delay*           | 规定在动画开始之前的延迟，默认值为0。        |
| *animation-iteration-count* | 规定动画应该播放的次数，默认值为1。          |
| *animation-direction*       | 规定是否应该轮流反向播放动画，默认值是正向。 |

### keyframe

`@keyframes` 让开发者通过指定动画中特定时间点必须展现的关键帧样式（或者说停留点）来控制CSS动画的中间环节。这让开发者能够控制动画中的更多细节而不是全部让浏览器自动处理。

要使用关键帧, 先创建一个带名称的`@keyframes`规则，以便后续使用 `animation-name`这个属性来调用指定的`@keyframes`. 每个`@keyframes` 规则包含多个关键帧，也就是一段样式块语句，每个关键帧有一个百分比值作为名称，代表在动画进行中，在哪个阶段触发这个帧所包含的样式。

关键帧的编写顺序没有要求，最后只会根据百分比按由小到大的顺序触发。

```
@keyframes animationname {keyframes-selector {css-styles;}}
```

具体含义如下：

| 值                   | 描述                                                         |
| -------------------- | ------------------------------------------------------------ |
| *animationname*      | 必需。定义动画的名称。                                       |
| *keyframes-selector* | 必需。动画时长的百分比。合法的值：0-100%from（与 0% 相同）to（与 100% 相同） |
| *css-styles*         | 必需。一个或多个合法的 CSS 样式属性。                        |

```css
.one {
        width: 100px;
        height: 100px;
        margin: 200px auto;
        background-color: #cd4a48;
        position: relative;
        animation: moveHover 5s ease-in-out 0.2s;

    }


    @keyframes moveHover {
        0% {
            top: 0px;
            left: 0px;
            background: #cd4a48;
        }
        50% {
            top: 200px;
            left: 200px;
            background:#A48992;
        }
        100% {
            top: 350px;
            left:350px;
            background: #FFB89A;
        }
    }
```

### animation-fill-mode

动画结束以后，会立即从结束状态跳回到起始状态。如果想让动画保持在结束状态，需要使用`animation-fill-mode`属性。

```
animation-fill-mode: none || backwards || both
```

- none：默认值，回到动画没开始时的状态。

- forwards：当动画完成后，保持最后一个属性值（在最后一个关键帧中定义）。

- backwards：在 `animation-delay`所指定的一段时间内，在动画显示之前，应用开始属性值（在第一个关键帧中定义）。

- both: 根据`animation-direction`轮流应用forwards和backwards规则。

## transform

`transform`就是变形，主要包括**旋转rotate**、**扭曲skew**、**缩放scale**和**移动translate**以及**矩阵变形matrix**。

```
transform: none || transform-functions
```

none:表示不进么变换；transform-function表示一个或多个变换函数，以空格分开；换句话说就是我们同时对一个元素进行transform的多种属性操作，例如rotate、scale、translate三种。

主要的transform-function变换函数如下：

### translate

| 值                       | 描述                            |
| ------------------------ | ------------------------------- |
| translate(*x*,*y*)       | 定义 2D 转换。                  |
| translate3d(*x*,*y*,*z*) | 定义 3D 转换。                  |
| translateX(*x*)          | 定义转换，只是用 X 轴的值。     |
| translateY(*y*)          | 定义转换，只是用 Y 轴的值。     |
| translateZ(*z*)          | 定义 3D 转换，只是用 Z 轴的值。 |

### scale

| 值                   | 描述                                  |
| -------------------- | ------------------------------------- |
| scale(*x*,*y*)       | 定义 2D 缩放转换。                    |
| scale3d(*x*,*y*,*z*) | 定义 3D 缩放转换。                    |
| scaleX(*x*)          | 通过设置 X 轴的值来定义缩放转换。     |
| scaleY(*y*)          | 通过设置 Y 轴的值来定义缩放转换。     |
| scaleZ(*z*)          | 通过设置 Z 轴的值来定义 3D 缩放转换。 |

### rotate

| 值                            | 描述                             |
| ----------------------------- | -------------------------------- |
| rotate(*angle*)               | 定义 2D 旋转，在参数中规定角度。 |
| rotate3d(*x*,*y*,*z*,*angle*) | 定义 3D 旋转。                   |
| rotateX(*angle*)              | 定义沿着 X 轴的 3D 旋转。        |
| rotateY(*angle*)              | 定义沿着 Y 轴的 3D 旋转。        |
| rotateZ(*angle*)              | 定义沿着 Z 轴的 3D 旋转。        |

### skew

| 值                        | 描述                               |
| ------------------------- | ---------------------------------- |
| skew(*x-angle*,*y-angle*) | 定义沿着 X 和 Y 轴的 2D 倾斜转换。 |
| skewX(*angle*)            | 定义沿着 X 轴的 2D 倾斜转换。      |
| skewY(*angle*)            | 定义沿着 Y 轴的 2D 倾斜转换。      |

### transform-origin

以上变化的默认参照点是元素的中心点，不过可以通过`transform-origin`设置元素的参照点。

```
transform-origin: X || Y || Z
```

其中X，Y，Z对应三维坐标，X，Y，Z的值可以是em，px。此外，X，Y可以是百分值，其中X也可以是字符参数值left，center，right。Y和X一样除了百分值外还可以设置字符值top，center，bottom。


