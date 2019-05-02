title: Collections工具类常用API
author: 学亮
tags:
  - Collections
categories: []
date: 2019-04-29 17:22:00
---
> Collection和Collections有什么区别？
 * 		Collection是集合体系的最顶层，包含了集合体系的共性
 * 		Collections是一个工具类，方法都是用于操作Collection







<!-- more -->

````
/*
 * Collections：
 * 面试题：Collection和Collections有什么区别？
 * 		Collection是集合体系的最顶层，包含了集合体系的共性
 * 		Collections是一个工具类，方法都是用于操作Collection
 * 
 */
public class CollectionsDemo {
	public static void main(String[] args) {
		//static void swap(List list, int i, int j) :将指定列表中的两个索引进行位置互换
		List<Integer> list = new ArrayList<Integer>();
		list.add(1);
		list.add(4);
		Collections.swap(list, 0, 1);
		
		System.out.println(list);
	
	}

	private static void method6() {
		//static void  sort(List<T> list) :按照列表中元素的自然顺序进行排序
		List<Integer> list = new ArrayList<Integer>();
		list.add(1);
		list.add(4);
		list.add(3);
		list.add(2);
		
		Collections.sort(list);
		System.out.println(list);
	}

	private static void method5() {
		//static void shuffle(List list):傻否，随机置换  
		List<Integer> list = new ArrayList<Integer>();
		list.add(1);
		list.add(2);
		list.add(3);
		list.add(4);
		Collections.shuffle(list);
		System.out.println(list);
	}

	private static void method4() {
		//static void reverse(List list)  :反转
		List<Integer> list = new ArrayList<Integer>();
		list.add(1);
		list.add(2);
		list.add(3);
		list.add(4);
		
		Collections.reverse(list);
		System.out.println(list);
	}

	private static void method3() {
		//static void fill(List list, Object obj) :使用指定的对象填充指定列表的所有元素
		List<String> list = new ArrayList<String>();
		list.add("hello");
		list.add("world");
		list.add("java");
		System.out.println(list);
		
		Collections.fill(list, "android");
		
		System.out.println(list);
	}

	private static void method2() {
		//static void copy(List dest, List src) :是把源列表中的数据覆盖到目标列表
		//注意：目标列表的长度至少等于源列表的长度
		//创建源列表
		List<String> src = new ArrayList<String>();
		src.add("hello");
		src.add("world");
		src.add("java");
		
		//创建目标列表
		List<String> dest = new ArrayList<String>();
		dest.add("java");
		dest.add("java");
		dest.add("java");
		dest.add("java");
		Collections.copy(dest, src);
		System.out.println(dest);
	}

	private static void method() {
		//static int  binarySearch(List list, Object key) 使用二分查找法查找指定元素在指定列表的索引位置 
		List<Integer> list = new ArrayList<Integer>();
		list.add(1);
		list.add(2);
		list.add(3);
		list.add(4);
		
		int index = Collections.binarySearch(list, 4);
		System.out.println(index);
	}
}

````