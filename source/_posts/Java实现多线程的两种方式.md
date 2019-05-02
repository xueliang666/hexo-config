title: Java实现多线程的两种方式
author: 学亮
tags:
  - 多线程
categories: []
date: 2019-04-30 16:48:00
---
>  认识多线程之前，我们先要了解几个关于多线程有关的概念。
A:进程：进程指正在运行的程序。确切的来说，当一个程序进入内存运行，即变成一个进程，进程是处于运行过程中的程序，并且具有一定独立功能。
B:线程：线程是进程中的一个执行单元，负责当前进程中程序的执行，一个进程中至少有一个线程。一个进程中是可以有多个线程的，这个应用程序也可以称之为多线程程序。
C:简而言之：一个程序运行后至少有一个进程，一个进程中可以包含多个线程
什么是多线程呢？即就是一个程序中有多个线程在同时执行。


<!--more-->

**线程实现方式**

### 继承Thread类 ###

> A:创建线程的步骤：
1.定义一个类继承Thread。
2.重写run方法。
3.创建子类对象，就是创建线程对象。
4.调用start方法，开启线程并让线程执行，同时还会告诉jvm去调用run方法


**代码案例**

```
public class MyThread extends Thread {
	@Override
	public void run() {
		for (int i = 0; i < 100; i++) {
			System.out.println(getName() + ":" + i);
		}
	}
}

```

*测试*

```
/*
 * 多线程的实现方式：
 * 		方式1：一种方法是将类声明为 Thread 的子类。该子类应重写 Thread 类的 run 方法。接下来可以分配并启动该子类的实例
 * 
 * Thread
 * 		String getName()      返回该线程的名称。 
 * 		void   setName(String name) 改变线程名称，使之与参数 name 相同。
 * 
 * 
 * CPU执行程序的随机性
 */
public class ThreadDemo2 {
	public static void main(String[] args) {
		//创建线程实例
		MyThread mt = new MyThread();
		//修改线程名字
		mt.setName("张三");
		
		//启动线程
		mt.start();
		
		//创建线程实例
		MyThread mt2 = new MyThread();
		mt2.setName("老王");
		
		//启动线程
		mt2.start();
	}
}

```

### 实现Runnable接口 ###

> 创建线程的另一种方法是声明实现 Runnable 接口的类。该类然后实现 run 方法。然后创建Runnable的子类对象，传入到某个线程的构造方法中，开启线程。
为何要实现Runnable接口，Runable是啥玩意呢？继续API搜索。
查看Runnable接口说明文档：Runnable接口用来指定每个线程要执行的任务。包含了一个 run 的无参数抽象方法，需要由接口实现类重写该方法。


*创建线程的步骤。*
> 1、定义类实现Runnable接口。
2、覆盖接口中的run方法。。
3、创建Thread类的对象
4、将Runnable接口的子类对象作为参数传递给Thread类的构造函数。
5、调用Thread类的start方法开启线程。


**代码案例**

```
public class MyThread2 implements Runnable {
	int num;
	
	public MyThread2(int num) {
		this.num = num;
	}
	
	@Override
	public void run() {
		for (int i = 0; i < 100; i++) {
			//Thread t = Thread.currentThread();
			//System.out.println(t.getName() + ":" + i);
			
			//链式编程
			System.out.println(Thread.currentThread().getName() + ":" + i + num);
		}
	}

}

```