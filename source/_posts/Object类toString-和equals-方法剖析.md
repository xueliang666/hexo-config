title: Object类toString()和equals()方法剖析
author: 学亮
date: 2019-04-29 13:59:01
tags:
---
> Object类是Java语言中的根类，即所有类的父类。它中描述的所有方法子类都可以使用。所有类在创建对象的时候，最终找的父类就是Object。


<!-- more -->

**toString()方法**

> 由于toString方法返回的结果是内存地址，而在开发中，经常需要按照对象的属性得到相应的字符串表现形式，因此也需要重写它。

````
/*
 * String toString()  : 返回该对象的字符串表示
 * 		 return getClass().getName() + "@" + Integer.toHexString(hashCode());
 * 		 getClass():返回一个字节码对象
 * 		 Integer.toHexString():返回指定参数的十六进制字符串形式
 * 		 hashCode()：返回该对象的哈希码值（内部地址）

 * boolean equals(Object obj)  
 * 
 */
public class ObjectDemo {
	public static void main(String[] args) {
		Student s = new Student();
		s.name = "zhangsan";
		s.age = 18;
		System.out.println(s.toString());//com.zhangxueliang.Student@737951b0
		System.out.println(s);//说明我们输出一个对象就是默认输出这个对象的toString()方法
	}
}

class Student extends Object {
	String name;
	int age;
	/*
	public String toString() {
		return name + "@" + age;
	}
	*/
	
	@Override
	public String toString() {
		return "Student [name=" + name + ", age=" + age + "]";
	}
	
	
}

````

**equals()方法**

> equals方法，用于比较两个对象是否相同，它其实就是使用两个对象的内存地址在比较。Object类中的equals方法内部使用的就是==比较运算符。
在开发中要比较两个对象是否相同，经常会根据对象中的属性值进行比较，也就是在开发经常需要子类重写equals方法根据对象的属性值进行比较。


````
/*
 * boolean equals(Object obj)  
 * 		使用==来比较两个对象是否相等，则比较地址值是否相等
 */
public class ObjectDemo2 {
	public static void main(String[] args) {
		Person p = new Person("zhangsan",18);
		Person p2 = new Person("zhangsan",19);
		
		//boolean flag = p.equals(p2);
		
		boolean flag = p.equals(new ArrayList());
		System.out.println(flag);
	}
}

class Person {
	String name;
	int age;
	
	public Person(String name,int age) {
		this.name = name;
		this.age = age;
	}


	@Override
	public boolean equals(Object obj) {
		//提高效率
		if (this == obj)
			return true;
		
		if (obj == null)
			return false;
		//提高健壮性
		if (getClass() != obj.getClass())
			return false;
		
		//向下转型
		Person other = (Person) obj;
		
		if (age != other.age)
			return false;
		if (name == null) {
			if (other.name != null)
				return false;
		} else if (!name.equals(other.name))
			return false;
		return true;
	}
	
	
	/*@Override
	public boolean equals(Object o) {
		//提高效率 当前对象和传递进来的对象地址值一样，则不用比较成员
		if(this == o) {
			return true;
		}
		
		//提高代码的健壮性
		if(this.getClass() != o.getClass()) {
			return false;
		}
		
		//向下转型
		Person other = (Person) o;
		
		if(!this.name.equals(other.name)) {
			return false;
		}
		
		if(this.age != other.age) {
			return false;
		}
		
		return true;
	}*/
}

````

