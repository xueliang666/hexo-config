title: Mybatis DAO开发--Mapper动态代理开发方式
author: 学亮
tags:
  - mybatis
categories: []
date: 2019-04-29 19:50:00
---
> Mybatis DAO开发--Mapper动态代理开发方式

<!-- more -->

**第一步：jar包**
> 创建lib目录，引入相应的jar包，本节课用到的案例引入的jar包就是spring整合mybatis要用到的全部jar包。

**第二步：配置文件**

> 第一个：是SqlMapConfig.xml配置文件。它是mybatis的入口，也就是核心配置文件。

> 第二个：映射配置文件。

> 第三个：数据库配置文件。

> 第四个：log4j日志配置文件。

````
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
	        <!-- <typeAlias type="cn.nwtxxb.mybatis.po.User" alias="user"/> -->
	        <!-- 扫描包的形式创建别名，别名就是类名，不区分大小写 -->
	        <package name="com.nwtxxb.po"/>
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
	        <!--<mapper resource="sqlmap/user.xml"/>-->
	        <!-- <mapper resource="mapper/mapper.xml"/> -->
	        <!-- 根据接口名称加载mapper文件
	        要求：1、mapper映射文件和接口在同一个目录下
	            2、mapper映射文件的名称和接口名称一致。
	            3、class就是接口的权限定名
	         -->
	        <!-- <mapper class="cn.nwtxxb.mybatis.mapper.UserMapper"/> -->
	        <!-- 使用扫描包的形式加载mapper文件 -->
	        <package name="com.nwtxxb.mapper"/>
	    </mappers>
	</configuration>
````
**第三步：编写PO类及数据库建库建表**

````
    public class User {
	    private int id;
	    private String username;// 用户姓名
	    private String sex;// 性别
	    private Date birthday;// 生日
	    private String address;// 地址
	    public int getId() {
	        return id;
	    }
	}
	
````

**第四步：进行Mapper接口开发**

````
    public interface UserMapper {
	    User getUserById(int id);
	    List<User> getUserByName(String username);
	    void insertUser(User user);
	}
````

**第五步：编写Mapper映射文件**

````

    <?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE mapper
	        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
	<!-- namespace是命名空间，作用sql语句的隔离，后面还有重要作用 
	#{}作用就是占位符，相当于jdbc的“?”
	parameterType：查询的参数类型
	resultType：查询结果的数据类型，如果是pojo应该给全路径。
	-->
	<!-- mapper代理的开发规则：
		1、namespace必须是接口的全限定名com.nwtxxb.mapper.UserMapper
		2、Statementid必须和接口的方法名称一致getUserById
		3、接口方法的参数类型要和parameterType要一致
		4、接口方法的返回值类型要和resultType一致
	 -->
	<mapper namespace="com.nwtxxb.mapper.UserMapper">
	    <!-- 别名不区分大小写 -->
	    <select id="getUserById" parameterType="int" resultType="USer">
	        SELECT * FROM `user` WHERE id=#{id};
	    </select>
	
	    <!-- 如果查询结果返回list， resultType设置为list中一个元素的数据类型
	        ${}字符串拼接指令
	    -->
	    <select id="getUserByName" parameterType="string" resultType="com.nwtxxb.po.User">
	        SELECT * FROM `user` WHERE username LIKE '%${value}%'
	    </select>
	    <!-- 参数时候pojo时，#{}中的名称就是pojo的属性 -->
	    <insert id="insertUser" parameterType="com.nwtxxb.po.User">
	        <!-- keyProperty：对于pojo的主键属性 
	            resultType:对应主键的数据类型
	            order：是在insert语句执行之前或者之后。
	            如果使用uuid做主键，应该先生成主键然后插入数据，此时应该使用Before
	        -->
	        <selectKey keyProperty="id" resultType="int" order="AFTER">
	            SELECT LAST_INSERT_ID()
	        </selectKey>
	        INSERT into user (username,birthday,sex,address)
	        values (#{username}, #{birthday}, #{sex}, #{address})
	    </insert>
	</mapper>
````
