title: Java方法案例--比较三个数中的最大值
author: 学亮
date: 2019-04-28 16:19:01
tags:
---
```
package com.zhangxueliang.demo;

import java.util.Scanner;

public class MethodTest2 {
	// 返回三个数中的最大值
	public static float getMax(float a, float b, float c) {
		if (a > b) {
			if (a > c) {
				return a;
			} else {
				return c;
			}
		} else {
			if (b > c) {
				return b;
			} else {
				return c;
			}
		}
	}
	
	public static void main(String[] args) {
		//创建对象
		Scanner sc = new Scanner(System.in);
		
		//接收数据
		System.out.println("请输入第一个数据：");
		float a = sc.nextFloat();
		
		System.out.println("请输入第二个数据：");
		float b = sc.nextFloat();
		
		System.out.println("请输入第三个数据：");
		float c = sc.nextFloat();
		
		//调用方法
		float max = getMax(a,b,c);
		System.out.println("max:"+max);
	}
}

```