title: Java中的return语句使用总结
author: 学亮
tags:
  - java基础
categories: []
date: 2019-04-28 16:21:00
---
```
package com.zhangxueliang.demo;

public class TestReturn {
	public static void main(String args[]) { 
	       TestReturn t = new TestReturn(); 
	        //t.test1(); 
//	        System.out.println(t.test2()); 
	       //String test2 = t.test2();
	       //System.out.println(test2);
	       test3("1");
	    } 

	public static void test3(String str) {
        if ("1".equals(str)) {
            System.out.println("输入的参数是1");
            return ;
        }
        if ("2".equals(str)) {
            System.out.println("输入的参数是2");
            return;
        }
        if ("3".equals(str)) {
            System.out.println("输入的参数是3");
            return;
        }
        System.out.println("你输入的不是123`````````");
        // ...这里可以写不满足上述条件的处理

    }
		
	    /** 
	     * 无返回值类型的return语句测试 
	     */ 
	    public void test1() { 
	        System.out.println("---------无返回值类型的return语句测试--------"); 
	        for (int i = 1; ; i++) { 
	            if (i == 4) return; 
	            System.out.println("i = " + i); 
	        }
	    } 

	    /** 
	     * 有返回值类型的return语句测试 
	     * @return String 
	     */ 
	    public String test2(){ 
	        System.out.println("---------有返回值类型的return语句测试--------"); 
	        return "返回一个字符串"; 
	    } 
}

```