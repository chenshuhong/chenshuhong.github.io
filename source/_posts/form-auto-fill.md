---
title: 防止浏览器自动填充表单
date: 2019-02-26 14:42:46
tags: 
- html
- 奇淫技巧
categories: 前端
---
对于带`type='password'`的form表单，浏览器会根据你是否记住密码来自动填充，有时候这种行为是噩梦，一是安全，二是有可能错误填充，并不是所有的带`type='password'`都是登录表单，有可能是注册或者是修改密码，这种情况，就需要防止自动填充
```html
<input type='password' autocomplete="new-password"/>
```
更多方式见stackoverflow
[https://stackoverflow.com/questions/2530/how-do-you-disable-browser-autocomplete-on-web-form-field-input-tag](https://stackoverflow.com/questions/2530/how-do-you-disable-browser-autocomplete-on-web-form-field-input-tag)
