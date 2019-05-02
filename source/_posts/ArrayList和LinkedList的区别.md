title: ArrayList和LinkedList的区别
author: 学亮
tags:
  - LinkedList
  - ArrayList
  - java集合
categories: []
date: 2019-04-29 16:56:00
---
> List子体系特点：
A:有序的（存储和读取的顺序是一致的） 
B:有整数索引 
C:允许重复的






<!-- more -->

**List的特有功能**

````
void add(int index, E element) :将元素添加到index索引位置上
E get(int index) :根据index索引获取元素
E remove(int index) :根据index索引删除元素
E set(int index, E element):将index索引位置的的元素设置为element

````

**List代码案例**

````
/*
 * List:
 * 		有序的（存储和读取的顺序是一致的）
 * 		有整数索引
 * 		允许重复的
 * 
 * List的特有功能：
 * 			void add(int index, E element)  
 * 			E get(int index)  
 * 			E remove(int index)  
 *  		E set(int index, E element)
 *  
 * 增删改查
 */
public class ListDemo {
	public static void main(String[] args) {
		//创建的列表对象
		List list = new ArrayList();
		
		//void add(int index, E element)  : 在指定索引位置添加指定元素
		list.add(0, "hello");
		list.add(0, "world");
		list.add(1, "java");
		
		//E get(int index)  :根据索引返回元素
		/*System.out.println(list.get(0));
		System.out.println(list.get(1));
		System.out.println(list.get(2));*/
		//System.out.println(list.get(3));
		
		/*for (int i = 0; i < list.size(); i++) {
			System.out.println(list.get(i));
		}*/
		
		//E remove(int index)  : 删除指定元素并返回
		
		//System.out.println(list.remove(5));
		
		//E set(int index, E element) : 将指定索引位置的元素替换为指定元素，并将原先的元素返回
		System.out.println(list.set(0, "android"));
		
		System.out.println(list);
	}
}

````

**LinkedList特有功能**

````
LinkedList底层使用的是链表结构,因此增删快,查询相对ArrayList较慢
void addFirst(E e) :向链表的头部添加元素
 void addLast(E e):向链表的尾部添加元素
 E getFirst():获取链头的元素,不删除元素
 E getLast():获取链尾的元素,不删除元素
 E removeFirst():返回链头的元素并删除链头的元素
 E removeLast():返回链尾的元素并删除链尾的元素

````

**LinkedList代码案例**

````
/*
 * List的常用子类：
 * 		ArrayList
 * 			底层是数组结构，查询快，增删慢
 * 		LinkedList
 * 			底层结构是链表，查询慢，增删快
 * 
 * 如何选择使用不同的集合？
 * 		如果查询多，增删少，则使用ArrayList
 * 		如果查询少，增删多，则使用LinkedList
 * 		如果你不知道使用什么，则使用ArrayList
 * 
 * LinkedList的特有功能：
 * 			void addFirst(E e)  
 * 			void addLast(E e) 
 			E getFirst()  
 			E getLast()  
 			E removeFirst() 
 			E removeLast() 
 * 				
 */
public class LinkedListDemo {
	public static void main(String[] args) {
			LinkedList list = new LinkedList();
			list.add("hello");
			list.add("world");
		
			//void addFirst(E e)  :将元素添加到索引为0的位置
 			//void addLast(E e) ：将元素添加到索引为size()-1的位置
			list.addFirst("java");
			list.addLast("android");
			
 			//E getFirst()  :获取索引为0的元素
 			//E getLast()  ：获取索引为size()-1的元素
			//System.out.println(list.getFirst());
			//System.out.println(list.getLast());
			
 			//E removeFirst() :删除索引为0的元素并返回
 			//E removeLast() ：删除索引为size()-1的元素并返回
			System.out.println(list.removeFirst());
			System.out.println(list.removeLast());

			System.out.println(list);
	}
}

````

**代码案例**

````
/*
 * 需求：定义一个方法，返回指定列表中指定元素的索引位置
 * 
 * 判断元素是否存在
 * 
 */
public class ListTest {
	public static void main(String[] args) {
		List list = new ArrayList();
		list.add("hello");
		list.add("world");
		list.add("java");
		
		//int index = index(list,"php");
		//System.out.println(index);
		
		//boolean flag = contains(list, "php");
		//System.out.println(flag);
		
		boolean flag = list.contains("php");
		System.out.println(flag);
	}
	
	public static int index(List list,Object other) {
		for(int x = 0;x < list.size();x++) {
			//获取列表中的元素
			Object obj = list.get(x);
			//使用列表中的元素和指定的元素进行比较
			if(obj.equals(other)) {
				return x;
			}
		}
		//查找不到指定的元素
		return -1;
	}
	
	public static boolean contains(List list,Object other) {
		//获取指定元素在指定列表中的索引位置
		int index = index(list,other);
		//如果索引位置大于等于0，则认为元素存在，否则不存在
		if(index >= 0) {
			return true;
		}
		else {
			return false;
		}
	}
	
	
	
}

````










