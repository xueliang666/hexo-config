title: HashMap集合精讲
author: 学亮
tags:
  - hashmap
categories: []
date: 2019-04-28 16:24:00
---
**Map接口概述**

> Map接口和Collection接口下的集合，存储数据的形式是不同的。
> **A:Collection中的集合**，元素是孤立存在的，理解为单身。向集合中存储元素采用一个个元素的方式存储。
> **B:Map中的集合**，元素是成对存在的，理解为夫妻。每个元素由键与值两部分组成的，通过键可以找到值。
> 
> size()--集合中元素的个数。
> 
> put(key,value)--存值。
> 
> get(key)--通过key来获取value.
> containsKey(key)--判断Map集合中是否包含键为key的键值对。
> containsValue(value)--判断Map集合中是否包含值为value的键值对。
> isEmpty()--判断Map集合中是否没有任何键值对。
> clear()--清空Map集合中所有的键值对。
> remove(key)--根据key的值来删除Map中的键值对。

**案例代码**

```
package com.zhangxueliang.demo;

import java.util.HashMap;
import java.util.Map;

public class MapDemo01 {
    public static void main(String[] args) {
        Map<String,String> map = new HashMap<String, String>();
        System.out.println(map.put("beida001","张三"));
        System.out.println(map.put("beida002","李四"));
        System.out.println(map.put("beida001","王五"));
        System.out.println(map.put("beida002","赵六"));

//        map.clear();
//        System.out.println("map的元素个数为："+map.size());

//        System.out.println(map.remove("beida002"));
        System.out.println("map的元素个数为："+map.size());

//        System.out.println(map.containsKey("beida003"));

//        System.out.println(map.containsValue("王五"));
//        map.remove("beida001");
//        System.out.println(map.isEmpty());

        System.out.println(map.get("beida001"));
    }
}

```
**案例代码--获取Map中的key和value**

```
public class MapDemo01 {
    public static void main(String[] args) {
        Map<String,String> map = new HashMap<String, String>();
        map.put("beida001","张三");
        map.put("beida002","李四");
        map.put("beida003","王五");

        Set<String> keys = map.keySet();
        for (String key:keys) {
            System.out.println(key);
        }

        Collection<String> values = map.values();
        for(String value:values){
            System.out.println("**********"+value+"**********");
        }
    }
}
```
**案例代码--Map集合的两种遍历方式**

```
public class MapDemo01 {
    public static void main(String[] args) {
        Map<String,String> map = new HashMap<String, String>();
        map.put("beida001","张三");
        map.put("beida002","李四");
        map.put("beida003","王五");
        
        //Map集合的第一种遍历方式
        Set<String> keys = map.keySet();
        for (String key:keys){
            String value = map.get(key);
            System.out.println("键："+key+"----"+"值："+value);
        }

        //Map集合的第二种遍历方式
        Set<Map.Entry<String, String>> entrys = map.entrySet();
        for (Map.Entry<String, String> entry:entrys){
            String key = entry.getKey();
            String value = entry.getValue();
            System.out.println("键："+key+"----"+"值："+value);
        }

    }
}
```