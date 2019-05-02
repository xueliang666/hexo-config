title: Math工具类常用API使用案例
author: 学亮
tags:
  - Math工具类
categories: []
date: 2019-04-29 11:57:00
---
> 对基础API的使用能够熟练掌握，能极大提高开发效率。有些知识是很简单，但并不是我们不学习和掌握它们的借口，越是简单的东西，在关键时刻越是能起到至关重要的作用，就好比我们花很长时间解决的一个BUG，结果到头来竟是少打了一个;所致，是不是很恼火。
<!-- more -->
> Math工具类常用API：

```java
@Test
    public void testMathUtil(){
        //Math:包含了一些基本的数学运算方法
        //static double PI
        System.out.println("PI的值："+Math.PI);

        //static double abs(double a)  :返回绝对值
        System.out.println("返回绝对值："+Math.abs(15));
        System.out.println("返回绝对值："+Math.abs(-10));

        //static double ceil(double a) 天花板   向上取整
        System.out.println("向上取整："+Math.ceil(1.2));
        System.out.println("向上取整："+Math.ceil(1.6));
        //static double floor(double a)  地板  向下取整
        System.out.println("向下取整："+Math.floor(1.2));
        System.out.println("向下取整："+Math.floor(1.6));

        //static long round(double a)  ：四舍五入
        System.out.println("四舍五入："+Math.round(1.2));
        System.out.println("四舍五入："+Math.round(1.6));

        //static double max(double a, double b)
        System.out.println("比较3和4谁最大==> "+Math.max(3, 4));

        //static double pow(double a, double b) :返回第一个参数的第二个参数次幂
        System.out.println("返回第一个参数的第二个参数次幂==> "+Math.pow(3, 2));

        //static double random() :返回一个随机数，大于零且小于一
        System.out.println("返回一个随机数，大于零且小于一==> "+Math.random());
        String s = Math.random() + "";
        System.out.println("生成一个随机数只保留2位小数==> "+s.substring(0, s.lastIndexOf(".") + 3));

    }
```