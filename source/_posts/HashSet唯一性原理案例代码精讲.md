title: HashSet唯一性原理案例代码精讲
author: 学亮
tags:
  - HashSet
categories: []
date: 2019-04-29 17:19:00
---
> Set体系的集合:
    A:存入集合的顺序和取出集合的顺序不一致
    B:没有索引
    C:存入集合的元素没有重复







<!-- more -->

**HashSet使用 & 唯一性原理**

````
public class HashSetDemo2 {
	public static void main(String[] args) {
		//创建集合对象
		HashSet<Student> hs = new HashSet<Student>();
		//创建元素对象
		Student s = new Student("zhangsan",18);
		Student s2 = new Student("lisi",19);
		Student s3 = new Student("lisi",19);
		//添加元素对象
		hs.add(s);
		hs.add(s2);
		hs.add(s3);
		//遍历集合对象
		for (Student student : hs) {
			System.out.println(student);
		}
		 
	}
		 
}

````

**HashSet唯一性原理**

````
规则:新添加到HashSet集合的元素都会与集合中已有的元素一一比较
	首先比较哈希值(每个元素都会调用hashCode()产生一个哈希值)
	如果新添加的元素与集合中已有的元素的哈希值都不同,新添加的元素存入集合
	如果新添加的元素与集合中已有的某个元素哈希值相同,此时还需要调用equals(Object obj)比较
	如果equals(Object obj)方法返回true,说明新添加的元素与集合中已有的某个元素的属性值相同,那么新添加的元素不存入集合
	如果equals(Object obj)方法返回false, 说明新添加的元素与集合中已有的元素的属性值都不同, 那么新添加的元素存入集合

````

**代码案例**

````
/*
 *	使用HashSet存储自定义对象并遍历 	
 *	通过查看源码发现：
 *				HashSet的add()方法，首先会使用当前集合中的每一个元素和新添加的元素进行hash值比较，
 *				如果hash值不一样，则直接添加新的元素
 *				如果hash值一样，比较地址值或者使用equals方法进行比较
 *				比较结果一样，则认为是重复不添加
 *				所有的比较结果都不一样则添加
 */
public class HashSetDemo2 {
	public static void main(String[] args) {
		//创建集合对象
		HashSet<Student> hs = new HashSet<Student>();
		//创建元素对象
		Student s = new Student("zhangsan",18);
		Student s2 = new Student("lisi",19);
		Student s3 = new Student("lisi",19);
		//添加元素对象
		hs.add(s);
		hs.add(s2);
		hs.add(s3);
		//遍历集合对象
		for (Student student : hs) {
			System.out.println(student);
		}
		 
	}
		 
}

class Student {
	String name;
	int age;
	
	public Student(String name,int age) {
		this.name = name;
		this.age = age;
	}

	@Override
	public String toString() {
		return "Student [name=" + name + ", age=" + age + "]";
	}

	@Override
	public boolean equals(Object obj) {
		//System.out.println("-------------------");
		Student s = (Student)obj;//向下转型，可以获取子类特有成员
		
		//比较年龄是否相等，如果不等则返回false
		if(this.age != s.age) {
			return false;
		}
		
		//比较姓名是否相等，如果不等则返回false
		if(!this.name.equals(s.name)) {
			return false;
		}
		
		//默认返回true，说明两个学生是相等的
		return true;
	}
	
	@Override
	public int hashCode() {
		return 1;
	}
	
}

````
**hashCode()方法优化**

> 如果让hashCode()方法返回一个固定值,那么每个新添加的元素都要调用equals(Object obj)方法比较,那么效率较低
   只需要让不同属性的值的元素产生不同的哈希值,那么就可以不再调用equals方法比较提高效率


**代码案例**

````
public class Person {
	String name;
	int age;
	
	public Person(String name,int age) {
		this.name = name;
		this.age = age;
	}

	@Override
	public String toString() {
		return "Person [name=" + name + ", age=" + age + "]";
	}

	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + age;
		result = prime * result + ((name == null) ? 0 : name.hashCode());
		return result;
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
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
	
	/*
	@Override
	public int hashCode() {
		
		 * 我们发现当hashCode方法永远返回整数1时，所有对象的hash值都是一样的，
		 * 有一些对象他的成员变量完全不同，但是他们还需要进行hash和equals方法的比较，
		 * 如果我们可以让成员变量不同的对象，他们的hash值也不同，这就可以减少一部分equals方法的比较
		 * 从而可以提高我们程序的效率
		 * 
		 * 可以尝试着让hashCode方法的返回值和对象的成员变量有关
		 * 可以让hashCode方法返回所有成员变量之和，
		 * 让基本数据类型直接想加，然后引用数据类型获取hashCode方法返回值后再相加（boolean不可以参与运算）
		 * 
		 
		//return age;
		return age + name.hashCode();
	}
	
	@Override
	public boolean equals(Object obj) {
		System.out.println("-------------");
		
		//提高效率
		if(this == obj) {
			return true;
		}
		
		//提高健壮性
		if(this.getClass() != obj.getClass()) {
			return false;
		}
		
		
		
		//向下转型
		Person p = (Person)obj;
		
		if(!this.name.equals(p.name)) {
			return false;
		}
		
		if(this.age != p.age) {
			return false;
		}
		
		return true;
		
	}*/
}

````

**代码测试案例**

````
public class HashSetDemo3 {
	public static void main(String[] args) {
		//创建集合对象
		HashSet<Person> hs = new HashSet<Person>();
		//创建元素对象
		Person p = new Person("zhangsan",18);
		Person p2 = new Person("lisi",18);
		Person p3 = new Person("lisi",18); 
       
		//添加元素对象
		hs.add(p);
		hs.add(p2);
		hs.add(p3);
		//遍历集合对象
		for (Person person : hs) {
			System.out.println(person);
		}
	}
}

````



