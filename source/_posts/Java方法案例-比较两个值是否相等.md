title: Java方法案例--比较两个值是否相等
author: 学亮
date: 2019-04-28 16:19:22
tags:
---
```
package com.zhangxueliang.demo;

import java.util.Scanner;

@SuppressWarnings("all")
public class MethodTest {
	//比较两个数是否相等
	public static boolean compare(int a,int b){
		if(a==b){
			return true;
		}else {
			return false;
		}
	}
	
	public static void main(String[] args) {
		//创建对象
		Scanner sc = new Scanner(System.in);
		
		//接收数据
		System.out.println("请输入第一个数据:");
		int a = sc.nextInt();
		
		System.out.println("请输入第二个数据:");
		int b = sc.nextInt();
		
		//调用方法
		boolean flag = compare(a,b);
		System.out.println("flag:"+flag);
	}
}

```