title: JVM内存划分
author: 学亮
tags:
  - java基础
categories: []
date: 2019-04-28 16:19:00
---
> Java 程序在运行时，需要在内存中的分配空间。为了提高运算效率，就对空间进行了不同区域的划分，因为每一片区域都有特定的处理数据方式和内存管理方式。
栈 存储局部变量
堆 存储new出来的东西
方法区 存储和方法相关的属性
本地方法区 (和系统相关)
寄存器 (给CPU使用)