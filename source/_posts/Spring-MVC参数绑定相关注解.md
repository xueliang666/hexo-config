title: Spring MVC参数绑定相关注解
author: 学亮
tags:
  - Spring MVC
  - 参数绑定
categories: []
date: 2019-04-29 21:24:00
---
> Spring MVC参数绑定相关注解：
@PathVariable
@RequestHeader
@CookieValue

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

> DataBindingController.java

````
package org.zhangxueliang.controller;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.CookieValue;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestHeader;
import org.springframework.web.bind.annotation.RequestMapping;

// Controller注解用于指示该类是一个控制器，可以同时处理多个请求动作
@Controller
public class DataBindingController{

	// 静态的日志类LogFactory
	private static final Log logger = LogFactory
            .getLog(DataBindingController.class);

	// 测试@PathVariable注解
	// 该方法映射的请求为http://localhost:8080/DataBindingTest/pathVariableTest/{userId}
	@RequestMapping(value="/pathVariableTest/{userId}")
	 public void pathVariableTest(
			 @PathVariable() Integer userId) {
		 logger.info("通过@PathVariable获得数据： " + userId);
	 }
	
	// 测试@RequestHeader注解
	// 该方法映射的请求为http://localhost:8080/DataBindingTest/requestHeaderTest
	@RequestMapping(value="/requestHeaderTest")
	 public void requestHeaderTest(
			 @RequestHeader("User-Agent") String userAgent,  
		     @RequestHeader(value="Accept") String[] accepts) {
		 logger.info("通过@requestHeaderTest获得数据： " + userAgent);
		 for(String accept : accepts){
			 logger.info(accept);
		 }
	 }
	
	// 测试@CookieValue注解
	@RequestMapping(value="/cookieValueTest")
	 public void cookieValueTest(
			 @CookieValue(value="JSESSIONID", defaultValue="") String sessionId) {
		 logger.info("通过@requestHeaderTest获得数据： " + sessionId);
	 }


}



````

> SessionAttributesController.java

````
package org.zhangxueliang.controller;


import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.SessionAttributes;
import org.zhangxueliang.domain.User;

// Controller注解用于指示该类是一个控制器，可以同时处理多个请求动作
@Controller
// 将Model中的属性名为user的放入HttpSession对象当中
@SessionAttributes("user")
public class SessionAttributesController{

	// 静态的日志类LogFactory
	private static final Log logger = LogFactory
            .getLog(SessionAttributesController.class);
	
	// 该方法映射的请求为http://localhost:8080/DataBindingTest/{formName}
	@RequestMapping(value="/{formName}")
	 public String loginForm(@PathVariable String formName){
		// 动态跳转页面
		return formName;
	}

	// 该方法映射的请求为http://localhost:8080/DataBindingTest/login
	@RequestMapping(value="/login")
	 public String login(
			 @RequestParam("loginname") String loginname,
			 @RequestParam("password") String password,
			 Model model ) {
		 // 创建User对象，装载用户信息
		 User user = new User();
		 user.setLoginname(loginname);
		 user.setPassword(password);
		 user.setUsername("admin");
		 // 将user对象添加到Model当中
		 model.addAttribute("user",user);
		 return "welcome";
	 }
	
	

}


````

> User.java

````
package org.zhangxueliang.domain;

import java.io.Serializable;

// 域对象，实现序列化接口
public class User implements Serializable{
	
	// 私有字段
	private String loginname;
	private String password;
	private String username;
	
	// 公共构造器
	public User() {
		super();
	}
	// set/get方法
	public String getLoginname() {
		return loginname;
	}
	public void setLoginname(String loginname) {
		this.loginname = loginname;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	
}

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
<h3>测试@SessionAttributes注解</h3>
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
<title>测试@SessionAttributes注解</title>
</head>
<body>
访问request作用范围域中的user对象：${requestScope.user.username }<br>
<br>
</body>
</html>
````
