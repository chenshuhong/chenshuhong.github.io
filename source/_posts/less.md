---
title: less使用
date: 2019-03-14 20:44:51
tags: 
- css
- less
categories: 前端
---
## 变量

```
@width: 10px;
@height: @width + 10px;

#header {
  width: @width;
  height: @height;
}
```

编译后

```
#header {
  width: 10px;
  height: 20px;
}
```

## 混合（MIXINS）

```
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}

#menu a {
  color: #111;
  .bordered();
}

.post a {
  color: red;
  .bordered();
}
```

编译后

```
#menu a {
  color: #111;
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}

.post a {
  color: red;
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
```

如果.bordered下有个.test类，你还可以.bordered.test()引用他

## 嵌套

```
#header {
  color: black;
  .navigation {
    font-size: 12px;
  }
  .logo {
    width: 300px;
  }
}
```

编译后

```
#header {
  color: black;
}
#header .navigation {
  font-size: 12px;
}
#header .logo {
  width: 300px;
}
```

当用到伪类选择器时用&

```
.clearfix {
  display: block;
  zoom: 1;

  &:after {
    content: " ";
    display: block;
    font-size: 0;
    height: 0;
    clear: both;
    visibility: hidden;
  }
}
```

当设计到样式表规则比如@media时，他会冒泡寻找规则

```
.component {
  width: 300px;
  @media (min-width: 768px) {
    width: 600px;
    @media  (min-resolution: 192dpi) {
      background-image: url(/img/retina2x.png);
    }
  }
  @media (min-width: 1280px) {
    width: 800px;
  }
}
```

编译成

```
.component {
  width: 300px;
}
@media (min-width: 768px) {
  .component {
    width: 600px;
  }
}
@media (min-width: 768px) and (min-resolution: 192dpi) {
  .component {
    background-image: url(/img/retina2x.png);
  }
}
@media (min-width: 1280px) {
  .component {
    width: 800px;
  }
}
```

## 操作符

`+`, `-`, `*`, `/` 能用在数字，颜色，变量操作上,以最左边出现的单位为主，如果单位转换不了，无视右边单位，*和/单位无效

```
// numbers are converted into the same units
@conversion-1: 5cm + 10mm; // result is 6cm
@conversion-2: 2 - 3cm - 5mm; // result is -1.5cm

// conversion is impossible
@incompatible-units: 2 + 5px - 3cm; // result is 4px

// example with variables
@base: 5%;
@filler: @base * 2; // result is 10%
@other: @base + @filler; // result is 15%
@base: 2cm * 3mm; // result is 6cm
@color: #224488 / 2; //results in #112244
background-color: #112244 + #111; // result is #223355
```

## 方法

less内置了很多方法

```
@base: #f04615;
@width: 0.5;

.class {
  width: percentage(@width); // returns `50%`
  color: saturate(@base, 5%); 
  background-color: spin(lighten(@base, 25%), 8);
}
```

## Map

```
#colors() {
  primary: blue;
  secondary: green;
}

.button {
  color: #colors[primary];
  border: 1px solid #colors[secondary];
}
```

编译成

```
.button {
  color: blue;
  border: 1px solid green;
}
```

## 作用域

```
@var: red;

#page {
  @var: white;
  #header {
    color: @var; // white
  }
}
```

具有变量提升

```
@var: red;

#page {
  #header {
    color: @var; // white
  }
  @var: white;
}
```

## 导入

导入后的文件变量还有存在，不会像js模块化一样访问不了，默认后缀是less

```
@import "library"; // library.less
@import "typo.css";
```


