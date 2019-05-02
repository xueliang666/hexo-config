title: System类的常用方法及类字段
author: 学亮
date: 2019-04-29 14:05:07
tags:
---
> System 类包含一些有用的类字段和方法。它不能被实例化。


<!-- more -->


**成员方法**

````
  static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length) :
     从src源数组的srcPos索引开始,复制length个元素
	  从destPost位置开始将这些元素放至到dest数组中
  static long currentTimeMillis() 
      返回以毫秒为单位的当前时间
  static void exit(int status) 
      终止当前正在运行的 Java 虚拟机
  static void gc()  
	运行垃圾回收器

````

**代码示例**

````
/*
 * System:包含一些有用的类字段和方法。它不能被实例化
 * static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)  
 * static long currentTimeMillis()  
 * static void exit(int status) 
   static void gc()  
 * 
 */
public class SystemDemo {
	public static void main(String[] args) {
		//method();
		//method2();
		//method3();
		
		//static void gc()  
		//Demo d = new Demo();
		new Demo();
		System.gc();
	}

	private static void method3() {
		//static void exit(int status) :终止虚拟机
		
		for (int i = 0; i < 100000; i++) {
			System.out.println(i);
			if(i == 100) {
				System.exit(0);
			}
		}
	}

	private static void method2() {
		/*
		 *  static long currentTimeMillis() :以毫秒值返回当前系统时间
		 *  这个毫秒的时间是相对时间，相对于1970-1-1 00:00:00 ： 0
		 *  1970-1-1 00:00:01 : 1000
		 *  1970-1-1 00:01:00: 1000 * 60
		 *  1970-1-1 01:00:00: 1000 * 60 * 60
		 *  1000毫秒 = 1秒
		 *  
		 */
		//System.out.println(System.currentTimeMillis());
		
		
		long start = System.currentTimeMillis();
		for (int i = 0; i < 100000; i++) {
			System.out.println(i);
		}
		long end = System.currentTimeMillis();
		System.out.println(end - start);
	}

	private static void method() {
		/*
		 * static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)  
		 * 复制数组
		 * 参数1：源数组
		 * 参数2：源数组的起始索引位置
		 * 参数3：目标数组
		 * 参数4：目标数组的起始索引位置
		 * 参数5：指定接受的元素个数
		 */
		int[] src = {1,2,3,4,5};
		int[] dest = new int[5];
		System.arraycopy(src, 2, dest, 4, 3);
		
		for (int i = 0; i < dest.length; i++) {
			System.out.print(dest[i]);
		}
	}
}

class Demo {
	@Override
	protected void finalize() throws Throwable {
		System.out.println("我被回收了");
	}
}

````
