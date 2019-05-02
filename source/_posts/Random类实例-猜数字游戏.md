title: Random类实例--猜数字游戏
author: 学亮
tags:
  - 常用api
categories: []
date: 2019-04-28 16:20:00
---
![这里写图片描述](https://img-blog.csdn.net/20180916175602143?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2E3NzIzMDQ0MTk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
```
package com.zhangxueliang.demo;

import java.util.Random;
import java.util.Scanner;

public class RandomTest {
	public static void main(String[] args) {
		// 系统产生一个随机数1-100之间的。
		Random r = new Random();
		int number = r.nextInt(100) + 1;

		while(true){
			// 键盘录入我们要猜的数据
			Scanner sc = new Scanner(System.in);
			System.out.println("请输入你要猜的数字(1-100)：");
			int guessNumber = sc.nextInt();
	
			// 比较这两个数据(用if语句)
			if (guessNumber > number) {
				System.out.println("你猜的数据" + guessNumber + "大了");
			} else if (guessNumber < number) {
				System.out.println("你猜的数据" + guessNumber + "小了");
			} else {
				System.out.println("恭喜你,猜中了");
				break;
			}
		}
	}
}

```