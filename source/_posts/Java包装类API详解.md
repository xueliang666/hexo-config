title: Java包装类API详解
author: 学亮
tags:
  - java基础
  - 包装类
categories: []
date: 2019-04-29 14:54:00
---
> 在实际程序使用中，程序界面上用户输入的数据都是以字符串类型进行存储的。而程序开发中，我们需要把字符串数据，根据需求转换成指定的基本数据类型，如年龄需要转换成int类型，考试成绩需要转换成double类型等。那么，想实现字符串与基本数据之间转换怎么办呢？
Java中提供了相应的对象来解决该问题，基本数据类型对象包装类：java将基本数据类型值封装成了对象。封装成对象有什么好处？可以提供更多的操作基本数值的功能。
8种基本类型对应的包装类中需要注意int对应的是Integer，char对应的Character，其他6个都是基本类型首字母大写即可。



<!-- more -->

````
/*
 * 需求：判断一个数是否符合int类型的范围
 * 由于基本数据类型只能做一些简单的操作和运算，所以Java为我们封装了基本数据类型，为每种基本数据类型提供了包装类
 * 包装类就是封装了基本数据类型的类，为我们提供了更多复杂的方法和一些变量
 * 
 * byte		Byte
 * short	Short
 * char		Character
 * int		Integer
 * long		Long
 * float	Float
 * double	Double
 * boolean	Boolean
 * 
 * Integer:
 * 		String --- int
 * 			方式1：int intValue()
 * 			方式2： static int parseInt(String s) 
 * 		int --- String
 * 			方式1： + ""
 * 			方式2：String toString()
 * 
 * 构造方法：
 * 		Integer(int value) 
 * 		Integer(String s) 

 
 */
public class IntegerDemo {
	public static void main(String[] args) {
		/*int n = 10;
		if(n >= Math.pow(-2, 31) && n <= Math.pow(2, 31) -1) {
			System.out.println("符合");
		}
		else {
			System.out.println("不符合");
		}*/
		
		 
		Integer i = new Integer("10");
		System.out.println(i);
		
	 
		int a = i.intValue();
		System.out.println(a + 10 );
		
 
		int b = Integer.parseInt("20");
		System.out.println(b + 30);
		
		
		 
		Integer i2 = new Integer(40);
		String s = i2.toString();
		System.out.println(s);
		
		 
		String s2 = Integer.toString(50);
		System.out.println(s2);
		
	}
}

````
**包装类的自动拆箱与自动装箱**

> 在需要的情况下，基本类型与包装类型可以通用。有些时候我们必须使用引用数据类型时，可以传入基本数据类型。
比如：
	基本类型可以使用运算符直接进行计算，但是引用类型不可以。而基本类型包装类作为引用类型的一种却可以计算，原因在于，Java”偷偷地”自动地进行了对象向基本数据类型的转换。
	相对应的，引用数据类型变量的值必须是new出来的内存空间地址值，而我们可以将一个基本类型的值赋值给一个基本类型包装类的引用。原因同样在于Java又”偷偷地”自动地进行了基本数据类型向对象的转换。
自动拆箱：对象转成基本数值
自动装箱：基本数值转成对象

````
/*
 * JDK1.5特性：自动装箱和拆箱
 * 
 */
public class IntegerDemo2 {
	public static void main(String[] args) {
		//Integer i = new Integer(10);
		
		//自动装箱
		//相当于： Integer i = new Integer(10);
		//Integer i = 10;
		
		//自动拆箱
		//相当于 int a = i.intValue();
		//Integer i = 10;
		//int a = i;
		
		Integer i = 10;
		Integer i2 = 20;
		Integer i3 = i + i2;
		/*
		 * Integer i3 = new Integer(i.intValue() + i2.intValue());
		 * 
		 */
		
		ArrayList list = new ArrayList();
		list.add(1);//自动装箱，list.add(new Integer(1));
	}
}

````


