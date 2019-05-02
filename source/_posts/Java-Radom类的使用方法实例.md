title: Java Radom类的使用方法实例
author: 学亮
tags:
  - 常用API
categories: []
date: 2019-04-28 16:21:00
---
```
package com.zhangxueliang.demo;

import java.util.Random;

public class RandomDemo {
	public static void main(String[] args) {
		// 创建对象
		Random r = new Random();

		for (int x = 0; x < 10; x++) {
			// 获取随机数
			int number = r.nextInt(10);
			// 输出随机数
			System.out.println("number:" + number);
		}
		System.out.println("--------------------");

		// 如何获取到一个1-100之间的随机数呢?
		int i = r.nextInt(100) + 1;
		System.out.println("i:" + i);
	}
}

```