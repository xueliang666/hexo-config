title: Nodejs--require优先从缓存加载
author: 学亮
tags:
  - Nodejs
categories: []
date: 2019-05-01 23:03:00
---
>  优先从缓存加载
* 由于 在 a 中已经加载过 b 了
* 所以这里不会重复加载
* 可以拿到其中的接口对象，但是不会重复执行里面的代码
* 这样做的目的是为了避免重复加载，提高模块加载效率

<!--more-->

> a.js

```
console.log('a.js 被加载了')
var fn = require('./b')

console.log(fn)

```

> b.js

```
console.log('b.js 被加载了')

module.exports = function () {
  console.log('hello bbb')
}

```

> main.js

```
require('./a')

// 优先从缓存加载
// 由于 在 a 中已经加载过 b 了
// 所以这里不会重复加载
// 可以拿到其中的接口对象，但是不会重复执行里面的代码
// 这样做的目的是为了避免重复加载，提高模块加载效率
var fn = require('./b')

console.log(fn)

```