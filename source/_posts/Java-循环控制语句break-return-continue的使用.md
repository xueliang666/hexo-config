title: Java 循环控制语句break/return/continue的使用
author: 学亮
tags:
  - java基础
categories: []
date: 2019-04-28 16:21:00
---
```
package com.zhangxueliang.demo;

public class BreakDemo {
	public static void main(String[] args) {
//		break;
		
		for(int x=1; x<=10; x++) {
			if(x == 3) {
//				break;
//				continue;
				return;
			}
			System.out.println("HelloWorld"+x);
		}
	}
}
```