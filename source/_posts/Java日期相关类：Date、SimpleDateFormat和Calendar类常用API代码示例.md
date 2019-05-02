title: Java日期相关类：Date、SimpleDateFormat和Calendar类常用API代码示例
author: 学亮
date: 2019-04-29 14:33:37
tags:
---
> Date: 表示特定的瞬间，精确到毫秒，他可以通过方法来设定自己所表示的时间，可以表示任意的时间


<!-- more -->

**Date类的构造方法**

> Date() ：创建的是一个表示当前系统时间的Date对象
Date(long date) ：根据"指定时间"创建Date对象


````
/*
 * Date: 表示特定的瞬间，精确到毫秒，他可以通过方法来设定自己所表示的时间，可以表示任意的时间
 * System.currentTimeMillis():返回的是当前系统时间，1970-1-1至今的毫秒数
 * 
 * 构造方法：
 * 		Date() ：创建的是一个表示当前系统时间的Date对象
		Date(long date) ：根据"指定时间"创建Date对象
 
 */
public class DateDemo {
	public static void main(String[] args) {
		//Date()
		//Date d = new Date();
		//System.out.println(d);//Thu Aug 26 14:17:28 CST 2049
		//System.out.println(d.toLocaleString());
		
		//Date(long date) 
		Date d2 = new Date(1000 * 60 * 60 * 24);//时区 有时差
		System.out.println(d2.toLocaleString());

	}
}

````

**Date类的常用方法**

> void setTime(long time)  
long getTime()


````
import java.util.Date;

/*
 * Date的常用用方法
		毫秒值 --- Date
			设置
			返回值是void，参数long
			void setTime(long time)  
			Date(long date)
		Date --- 毫秒值
			获取
			返回long，无参数
			long getTime()  
 */
public class DateDemo2 {
	public static void main(String[] args) {
		Date d = new Date();//默认当前系统时间
		//d.setTime(1000 * 60 * 60 * 24 * 2);
		System.out.println(d.toLocaleString());
		System.out.println(d.getTime());//172800000
		
		
		d.setTime(172800000L);
		System.out.println(d.toLocaleString());
	}
}

````

**DateFormat类 & SimpleDateFormat**

> DateFormat 是日期/时间格式化子类的抽象类，它以与语言无关的方式格式化并解析日期或时间。日期/时间格式化子类（如 SimpleDateFormat类）允许进行格式化（也就是日期 -> 文本）、解析（文本-> 日期）和标准化。
我们通过这个类可以帮我们完成日期和文本之间的转换。
继续阅读API，DateFormat 可帮助进行格式化并解析任何语言环境的日期。对于月、星期，甚至日历格式（阴历和阳历），其代码可完全与语言环境的约定无关。


**DateFormat&SimpleDateFormat的常用方法**

> 要格式化一个当前语言环境下的日期也就是日期 -> 文本），要通过下面的方法来完成。DateFormat是抽象类，我们需要使用其子类SimpleDateFormat来创建对象。
A:SimpleDateFormat构造方法


````
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

/*
 * SimpleDateFormat:
 * 		格式化：
 * 			Date --- String
 * 			2049-8-26 2049年8月26日
 * 			String format(Date date) 
 * 		解析：
 * 			String --- Date
 * 			"2049-8-26"
 * 			Date parse(String source) 
 * 
 * 构造方法：
 * 		SimpleDateFormat() ：使用默认的模式进行对象的构建
 * 		SimpleDateFormat(String pattern) ：使用的指定的模式进行对象的构建
 * 
 * 注意：Exception in thread "main" java.text.ParseException: Unparseable date: "49年9月26日  下午1:29"
 * 	    解析的字符串，模式必须和构建对象的模式一样
 *
 */
public class SimpleDateFormatDemo {
	public static void main(String[] args) throws ParseException {
		//method();
		//method2();
		//使用指定的模式进行对象的构建
		//1999年9月1日 10:10:10
		//4个小姨2个大美眉和2个小弟弟
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
		
		//格式化
		Date date = new Date();
		String s = sdf.format(date);
		System.out.println(s);//2049年08月26日 13:39:12

		
		//解析
		Date d = sdf.parse("2049年08月26日 13:39:12");
		System.out.println(d.toLocaleString());		

	}

	private static void method2() throws ParseException {
		//使用指定的模式进行对象的构建
		//1999年9月1日
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日");
		//格式化
		Date date = new Date();
		String s = sdf.format(date);
		System.out.println(s);//2049年08月26日
		
		//解析
		Date d = sdf.parse("2049年08月26日");
		System.out.println(d.toLocaleString());
	}

	private static void method() throws ParseException {
		//使用默认模式进行对象的构建
		SimpleDateFormat sdf = new SimpleDateFormat();
		//创建日期对象
		Date date = new Date();
		
		//格式化 把日期对象转换成字符串
		String s = sdf.format(date);
		System.out.println(s);//49-8-26 下午1:29
		
		//解析 把字符串转换成日期对象
		Date d = sdf.parse("49年9月26日  下午1:29");
		System.out.println(d.toLocaleString());
	}

}

````

**Calendar类**
> Calendar是日历类，在Date后出现，替换掉了许多Date的方法。该类将所有可能用到的时间信息封装为静态成员变量，方便获取。
Calendar为抽象类，由于语言敏感性，Calendar类在创建对象时并非直接创建，而是通过静态方法创建，将语言敏感内容处理好，再返回子类对象，如下：
Calendar类静态方法
Calendar c = Calendar.getInstance();  //返回当前时间

**Calendar类常用方法**
````
根据日历的规则，为给定的日历字段添加或减去指定的时间量
abstract void add(int field,int amount);

返回给定日历字段的值
int get(int field)

使用默认时区和语言环境获得一个日历
static Calendar getInstance()

返回一个表示此Calendar时间值
Date getTime()

将给定的日历字段设置为给定值
void set(int field,int value)
````

**代码示例**

````
import java.util.Calendar;

/*
 * Calendar：日历，提供了一些操作年月日时的方法
 * 
 * 获取
 * 修改
 * 添加
 * 
 * 
 */
public class CalendarDemo {
	public static void main(String[] args) {
		//static Calendar getInstance()  
		Calendar c = Calendar.getInstance();
		
		//void set(int field, int value) ：把指定的字段修改成指定的值
		//c.set(Calendar.DAY_OF_MONTH, 20);
		
		//void add(int field, int amount): 在指定的字段上加上指定的值
		c.add(Calendar.DAY_OF_MONTH, -1);
		
		//int get(int field) // 返回给定日历字段的值
		//public static final int YEAR 1 
		//System.out.println(Calendar.YEAR);
		
		//int year = c.get(1);
		int year = c.get(Calendar.YEAR);
		int month = c.get(Calendar.MONTH) + 1;
		int day = c.get(Calendar.DAY_OF_MONTH);
		
		
		System.out.println(year + "年" + month + "月" + day + "日");
		 
	}
}

````











