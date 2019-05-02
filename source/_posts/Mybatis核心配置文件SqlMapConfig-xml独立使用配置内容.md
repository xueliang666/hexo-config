title: Mybatis核心配置文件SqlMapConfig.xml独立使用配置内容
author: 学亮
date: 2019-04-28 15:51:08
tags:
---
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- 配置属性 
		先加载内部属性，再加载外部属性，如果有同名属性会覆盖。
	-->
	<properties resource="db.properties">
		<property name="jdbc.driver" value="com.mysql.jdbc.Driver"/>
		<property name="jdbc.url" value="jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8"/>
		<property name="jdbc.username" value="hello"/>
	</properties>
	<!-- 配置pojo别名 -->
	<typeAliases>
		<!-- <typeAlias type="com.zxl.mybatis.po.User" alias="user"/> -->
		<!-- 扫描包的形式创建别名，别名就是类名，不区分大小写 -->
		<package name="com.zxl.mybatis.po"/>
	</typeAliases>
	<!-- 和spring整合后 environments配置将废除-->
	<environments default="development">
		<environment id="development">
		<!-- 使用jdbc事务管理-->
			<transactionManager type="JDBC" />
		<!-- 数据库连接池-->
			<dataSource type="POOLED">
				<property name="driver" value="${jdbc.driver}" />
				<property name="url" value="${jdbc.url}" />
				<property name="username" value="${jdbc.username}" />
				<property name="password" value="${jdbc.password}" />
			</dataSource>
		</environment>
	</environments>
	<!-- 加载mapper文件 -->
	<mappers>
		<!-- resource基于classpath查找 -->
		<!-- <mapper resource="sqlmap/user.xml"/> -->
		<!-- <mapper resource="mapper/mapper.xml"/> -->
		<!-- 根据接口名称加载mapper文件
		要求：1、mapper映射文件和接口在同一个目录下
		    2、mapper映射文件的名称和接口名称一致。
		    3、class就是接口的权限定名
		 -->
		<!-- <mapper class="com.zxl.mybatis.mapper.UserMapper"/> -->
		<!-- 使用扫描包的形式加载mapper文件 -->
		<package name="com.zxl.mybatis.mapper"/>
	</mappers>
</configuration>
```
