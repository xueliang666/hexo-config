title: Java监听器实现在线人数统计案例
author: 学亮
date: 2019-04-28 15:50:27
tags:
---
```
package com.zxl.listener;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;

public class OnlineCountServletContextListener implements ServletContextListener {

	public void contextDestroyed(ServletContextEvent arg0) {
		// TODO Auto-generated method stub
		
	}

	public void contextInitialized(ServletContextEvent sce) {
		sce.getServletContext().setAttribute("ccount", 0);
	}

}

```

```
package com.zxl.listener;

import javax.servlet.http.HttpSession;
import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;

public class OnlineCountHttpSessionListener implements HttpSessionListener {

	public void sessionCreated(HttpSessionEvent hse) {
		//上线
		HttpSession session = hse.getSession();
		System.out.println(session.getId()+"上线了。。。");
		Integer ccount = (Integer) session.getServletContext().getAttribute("ccount");
		ccount++;
		session.getServletContext().setAttribute("ccount", ccount);
	}

	public void sessionDestroyed(HttpSessionEvent hse) {
		//离线
		HttpSession session = hse.getSession();
		System.out.println(session.getId()+"离线了。。。");
		Integer ccount = (Integer) session.getServletContext().getAttribute("ccount");
		ccount--;
		session.getServletContext().setAttribute("ccount", ccount);
	}

}

```


```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>统计在线人数</title>
</head>
<body>
<h1>在线人数:${ccount}</h1>
</body>
</html>
```


```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
  <display-name>filter_listener</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  <!-- <listener>
  	<listener-class>com.zxl.listener.MyServletRequestListener</listener-class>
  </listener> -->
  <listener>
  	<listener-class>com.zxl.listener.OnlineCountServletContextListener</listener-class>
  </listener>
  <listener>
  	<listener-class>com.zxl.listener.OnlineCountHttpSessionListener</listener-class>
  </listener>
</web-app>
```
