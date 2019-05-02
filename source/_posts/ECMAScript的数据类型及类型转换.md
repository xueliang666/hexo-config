title: ECMAScript的数据类型及类型转换
author: 学亮
tags:
  - javascript
categories: []
date: 2019-04-28 16:23:00
---
>5种简单数据类型：undifined / null / boolean / number / string。
>还有一种复杂的数据类型：Object.

**typeof操作符**

```
var message = "hello world";
console.log(typeof message);   //string
alert(typeof 100);				//number
```

**数值转换**

> 有三个函数可以实现向数值的转换：Number()  parseInt()  parseFloat()

**①Number()函数**

```
var num1 = Number("hello world");		//NaN
var num2 = Number("");					//0
var num3 = Number("0000123");			//123
var num4 = Number(true);				//1
```

**②ParseInt()函数**

```
var num1 = parseInt("123456abc");		//123456
var num2 = parseInt("");				//NaN
var num3 = parseInt("0xA");				//10
var num4 = parseInt("22.5");			//22
var num5 = parseInt("070");				//56
var num6 = parseInt("70");				//70
var num7 = parseInt("oxf");				//15
```

**③parseFloat()函数**

```
var num1 = parseFloat("123abc");		//123
var num2 = parseFloat("0xA");			//0
var num3 = parseFloat("22.5");			//22.5
var num4 = parseFloat("22.34.56");		//22.34
var num5 = parseFloat("0908.5");		//908.5
var num6 = parseFloat("3.125e7");		//31250000
```

**转换为字符串**

```
var age = 18;
var ageAsString = age.toString();		//""

var flag = true;
var flagAsString = flag.toString();		//"true"

var vv = "hello";
var ww = vv + "world";					//"helloworld"
```



