title: 【Spring MVC】@RequestParam注解参数绑定
author: 学亮
tags:
  - Spring MVC
categories: []
date: 2019-04-29 20:47:00
---
> 使用@RequestParam注解进行参数绑定。

<!-- more -->

> web.xml

````
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns="http://xmlns.jcp.org/xml/ns/javaee" 
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee 
	http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" 
	id="WebApp_ID" version="3.1">
	<!-- 定义Spring MVC的前端控制器 -->
  <servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>
        org.springframework.web.servlet.DispatcherServlet
    </servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>/WEB-INF/springmvc-config.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <!-- 让Spring MVC的前端控制器拦截所有请求 -->
  <servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
  
  <!-- 编码过滤器 -->
  <filter>
		<filter-name>characterEncodingFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
 </filter>
	<filter-mapping>
		<filter-name>characterEncodingFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
	
</web-app>
````

> springmvc-config.xml

````
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:mvc="http://www.springframework.org/schema/mvc"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc-4.2.xsd     
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context-4.2.xsd">
        
    <!-- spring可以自动去扫描base-pack下面的包或者子包下面的java文件，
    	如果扫描到有Spring的相关注解的类，则把这些类注册为Spring的bean -->
    <context:component-scan base-package="org.zhangxueliang.controller"/>
    
    <!-- 视图解析器  -->
     <bean id="viewResolver"
          class="org.springframework.web.servlet.view.InternalResourceViewResolver"> 
        <!-- 前缀 -->
        <property name="prefix">
            <value>/WEB-INF/content/</value>
        </property>
        <!-- 后缀 -->
        <property name="suffix">
            <value>.jsp</value>
        </property>
    </bean>
    
</beans>
````

> UserController.java

````
package org.zhangxueliang.controller;

import java.util.ArrayList;
import java.util.List;
import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.zhangxueliang.domain.User;



// Controller注解用于指示该类是一个控制器，可以同时处理多个请求动作
@Controller
// RequestMapping可以用来注释一个控制器类，此时，所有方法都将映射为相对于类级别的请求, 
// 表示该控制器处理所有的请求都被映射到 value属性所指示的路径下
@RequestMapping(value="/user")
public class UserController{
	
	// 静态List<User>集合，此处代替数据库用来保存注册的用户信息
	private static List<User> userList;
	
	// UserController类的构造器，初始化List<User>集合
	public UserController() {
		super();
		userList = new ArrayList<User>();
	}

	// 静态的日志类LogFactory
	private static final Log logger = LogFactory
            .getLog(UserController.class);

	// 该方法映射的请求为http://localhost:8080/context/user/register，该方法支持GET请求
	@RequestMapping(value="/register",method=RequestMethod.GET)
	 public String registerForm() {
		 logger.info("register GET方法被调用...");
		 // 跳转到注册页面
	     return "registerForm";
	 }
	
	// 该方法映射的请求为http://localhost:8080/RequestMappingTest/user/register，该方法支持POST请求
	 @RequestMapping(value="/register",method=RequestMethod.POST)
	 // 将请求中的loginname参数的值赋给loginname变量,password和username同样处理
	 public String register(
			 @RequestParam("loginname") String loginname,
			 @RequestParam("password") String password,
			 @RequestParam("username") String username) {
		 logger.info("register POST方法被调用...");
		 // 创建User对象
		 User user = new User();
		 user.setLoginname(loginname);
		 user.setPassword(password);
		 user.setUsername(username);
		 // 模拟数据库存储User信息
		 userList.add(user);
		 // 跳转到登录页面
	     return "loginForm";
	 }
	 
	// 该方法映射的请求为http://localhost:8080/RequestMappingTest/user/login
	 @RequestMapping("/login")
	 public String login(
			// 将请求中的loginname参数的值赋给loginname变量,password同样处理
			 @RequestParam("loginname") String loginname,
			 @RequestParam("password") String password,
			 Model model) {
		 logger.info("登录名:"+loginname + " 密码:" + password);
		 // 到集合中查找用户是否存在，此处用来模拟数据库验证
		 for(User user : userList){
			 if(user.getLoginname().equals(loginname) 
					 && user.getPassword().equals(password)){
				 model.addAttribute("user",user);
				 return "welcome";
			 }
		 }
	     return "loginForm";
	 }

}


````

> registerForm.jsp

````
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>注册页面</title>
</head>
<body>
<h3>注册页面</h3>
<form action="register" method="post">
     <table>
         <tr>
         	<td><label>登录名: </label></td>
             <td><input type="text" id="loginname" name="loginname" ></td>
         </tr>
         <tr>
         	<td><label>密码: </label></td>
             <td><input type="password" id="password" name="password"></td>
         </tr>
         <tr>
         	<td><label>真实姓名: </label></td>
             <td><input type="text" id="username" name="username" ></td>
         </tr>
         <tr>
             <td><input id="submit" type="submit" value="注册"></td>
         </tr>
     </table>
</form>
</body>
</html>
````

> loginForm.jsp

````
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>登录页面</title>
</head>
<body>
<h3>登录页面</h3>
<form action="login" method="post">
     <table>
         <tr>
         	<td><label>登录名: </label></td>
             <td><input type="text" id="loginname" name="loginname" ></td>
         </tr>
         <tr>
         	<td><label>密码: </label></td>
             <td><input type="password" id="password" name="password"></td>
         </tr>
         <tr>
             <td><input id="submit" type="submit" value="登录"></td>
         </tr>
     </table>
</form>
</body>
</html>
````

> welcome.jsp

````
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>测试@RequestMapping注解</title>
</head>
<body>
<!-- 页面可以访问Controller传递传递出来的模型User对象 -->
欢迎[${requestScope.user.username }]登陆
<br>
</body>
</html>
````










