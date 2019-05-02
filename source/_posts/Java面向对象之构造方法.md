title: Java面向对象之构造方法
author: 学亮
tags:
  - java基础
categories: []
date: 2019-04-28 16:17:00
---
```
package com.zhangxueliang.demo;

/*
 * 构造方法：
 * 		给对象的数据进行初始化
 * 
 * 格式：
 * 		方法名和类名相同
 * 		没有返回值类型，连void都不能写
 * 		没有具体的返回值
 *
 */
public class Student {	
	public Student() {
		System.out.println("这是构造方法");
	}
}

```

```
public class StudentDemo {
	public static void main(String[] args) {
		//如何调用构造方法呢?
		//通过new关键字调用
		//格式：类名 对象名 = new 构造方法(...);
		Student s = new Student();
	}
}
```