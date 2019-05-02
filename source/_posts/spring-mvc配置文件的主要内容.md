title: SSM框架整合步骤
author: 学亮
date: 2019-04-28 16:14:37
tags:
---
**SSM框架整合**

- 1、Dao层：

> mybatis整合spring，通过spring管理SqlSessionFactory、mapper代理对象。需要mybatis和spring的整合包。

![](https://i.imgur.com/nNxXoD6.jpg)

- 2、Service层：

> 所有的service实现类都放到spring容器中管理。由spring创建数据库连接池，并有spring管理实务。发布dubbo服务.


![](https://i.imgur.com/LWGMTn7.jpg)


- 3、表现层：

> Springmvc框架，由springmvc管理controller。引用dubbo服务


![](https://i.imgur.com/mtGGUwE.jpg)

*Dao整合*

**创建SqlMapConfig.xml配置文件**

    <?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE configuration
			PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
			"http://mybatis.org/dtd/mybatis-3-config.dtd">
	<configuration>
	
	</configuration>

**Spring整合mybatis**

> 创建applicationContext-dao.xml

    <beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
	xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
	http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd
	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd
	http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.2.xsd">

		<!-- 数据库连接池 -->
		<!-- 加载配置文件 -->
		<context:property-placeholder location="classpath:properties/*.properties" />
		<!-- 数据库连接池 -->
		<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
			destroy-method="close">
			<property name="url" value="${jdbc.url}" />
			<property name="username" value="${jdbc.username}" />
			<property name="password" value="${jdbc.password}" />
			<property name="driverClassName" value="${jdbc.driver}" />
			<property name="maxActive" value="10" />
			<property name="minIdle" value="5" />
		</bean>
		<!-- 让spring管理sqlsessionfactory 使用mybatis和spring整合包中的 -->
		<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
			<!-- 数据库连接池 -->
			<property name="dataSource" ref="dataSource" />
			<!-- 加载mybatis的全局配置文件 -->
			<property name="configLocation" value="classpath:mybatis/SqlMapConfig.xml" />
		</bean>
		<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
			<property name="basePackage" value="com.taotao.mapper" />
		</bean>
	</beans>

> db.properties

    jdbc.driver=com.mysql.jdbc.Driver
	jdbc.url=jdbc:mysql://localhost:3306/taotao?characterEncoding=utf-8
	jdbc.username=root
	jdbc.password=root


> 备注：
Druid是目前最好的数据库连接池，在功能、性能、扩展性方面，都超过其他数据库连接池，包括DBCP、C3P0、BoneCP、Proxool、JBoss DataSource。
Druid已经在阿里巴巴部署了超过600个应用，经过多年多生产环境大规模部署的严苛考验。

*Service整合*

**管理Service**

    <?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
		xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
		xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
		xmlns:dubbo="http://code.alibabatech.com/schema/dubbo" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd
		http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd
		http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.2.xsd">
	
		<context:component-scan base-package="com.taotao.service"></context:component-scan>
	
		<!-- 使用dubbo发布服务 -->
		<!-- 提供方应用信息，用于计算依赖关系 -->
		<dubbo:application name="taotao-manager" />
		<dubbo:registry protocol="zookeeper"
			address="192.168.25.154:2181,192.168.25.154:2182,192.168.25.154:2183" />
		<!-- 用dubbo协议在20880端口暴露服务 -->
		<dubbo:protocol name="dubbo" port="20880" />
		<!-- 声明需要暴露的服务接口 -->
		<dubbo:service interface="com.taotao.service.ItemService" ref="itemServiceImpl" />
	
	</beans>

**事务管理**

> 创建applicationContext-trans.xml

    <beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
	xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
	http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd
	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd
	http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.2.xsd">
		<!-- 事务管理器 -->
		<bean id="transactionManager"
			class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
			<!-- 数据源 -->
			<property name="dataSource" ref="dataSource" />
		</bean>
		<!-- 通知 -->
		<tx:advice id="txAdvice" transaction-manager="transactionManager">
			<tx:attributes>
				<!-- 传播行为 -->
				<tx:method name="save*" propagation="REQUIRED" />
				<tx:method name="insert*" propagation="REQUIRED" />
				<tx:method name="add*" propagation="REQUIRED" />
				<tx:method name="create*" propagation="REQUIRED" />
				<tx:method name="delete*" propagation="REQUIRED" />
				<tx:method name="update*" propagation="REQUIRED" />
				<tx:method name="find*" propagation="SUPPORTS" read-only="true" />
				<tx:method name="select*" propagation="SUPPORTS" read-only="true" />
				<tx:method name="get*" propagation="SUPPORTS" read-only="true" />
			</tx:attributes>
		</tx:advice>
		<!-- 切面 -->
		<aop:config>
			<aop:advisor advice-ref="txAdvice"
				pointcut="execution(* com.taotao.service.*.*(..))" />
		</aop:config>
	</beans>

> Web.xml

    <?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xmlns="http://java.sun.com/xml/ns/javaee" xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
		xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
		id="WebApp_ID" version="2.5">
		<display-name>taotao-manager</display-name>
		<!-- 加载spring容器 -->
		<context-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:spring/applicationContext*.xml</param-value>
		</context-param>
		<listener>
			<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
		</listener>
	</web-app>

*表现层整合*

> Springmvc.xml

    <?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
		xmlns:context="http://www.springframework.org/schema/context"
		xmlns:dubbo="http://code.alibabatech.com/schema/dubbo" 
		xmlns:mvc="http://www.springframework.org/schema/mvc"
		xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
	        http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.2.xsd
	        http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd
	        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd">
	
		<context:component-scan base-package="com.taotao.controller" />
		<mvc:annotation-driven />
		<bean
			class="org.springframework.web.servlet.view.InternalResourceViewResolver">
			<property name="prefix" value="/WEB-INF/jsp/" />
			<property name="suffix" value=".jsp" />
		</bean>
		
		<!-- 引用dubbo服务 -->
		<dubbo:application name="taotao-manager-web"/>
		<dubbo:registry protocol="zookeeper" address="192.168.25.154:2181,192.168.25.154:2182,192.168.25.154:2183"/>	
		<dubbo:reference interface="com.taotao.service.ItemService" id="itemService" />
		
	</beans>

> web.xml

    <?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xmlns="http://java.sun.com/xml/ns/javaee" xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
		xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
		id="WebApp_ID" version="2.5">
		<display-name>taotao-manager-web</display-name>
		<welcome-file-list>
			<welcome-file>login.html</welcome-file>
		</welcome-file-list>
		<!-- 解决post乱码 -->
		<filter>
			<filter-name>CharacterEncodingFilter</filter-name>
			<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
			<init-param>
				<param-name>encoding</param-name>
				<param-value>utf-8</param-value>
			</init-param>
		</filter>
		<filter-mapping>
			<filter-name>CharacterEncodingFilter</filter-name>
			<url-pattern>/*</url-pattern>
		</filter-mapping>
	
	
		<!-- springmvc的前端控制器 -->
		<servlet>
			<servlet-name>taotao-manager</servlet-name>
			<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
			<!-- contextConfigLocation不是必须的， 如果不配置contextConfigLocation， springmvc的配置文件默认在：WEB-INF/servlet的name+"-servlet.xml" -->
			<init-param>
				<param-name>contextConfigLocation</param-name>
				<param-value>classpath:spring/springmvc.xml</param-value>
			</init-param>
			<load-on-startup>1</load-on-startup>
		</servlet>
		<servlet-mapping>
			<servlet-name>taotao-manager</servlet-name>
			<url-pattern>/</url-pattern>
		</servlet-mapping>
	</web-app>

