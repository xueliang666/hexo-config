title: oracle集合运算
author: 学亮
tags:
  - oracle
categories: []
date: 2019-04-28 16:26:00
---


> union做并集运算：
> 
> ①union集合运算的结果不包括去掉重复记录。



> ②union all集合运算的结果包括重复记录。


![](https://i.imgur.com/dDkWMB2.png)

> intersect进行交集运算


![](https://i.imgur.com/H4NTe7w.png)

> minus进行差集运算



> 在A中去掉B中出现的重复记录后的差集

![](https://i.imgur.com/ZFbrcp3.png)

> 使用差集进行分页查询

![](https://i.imgur.com/9fhO8hr.png)