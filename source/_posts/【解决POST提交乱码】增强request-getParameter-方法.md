title: 【解决POST提交乱码】增强request.getParameter()方法
author: 学亮
date: 2019-04-28 16:12:11
tags:
---
```
package com.zxl.login.filter;

import java.io.UnsupportedEncodingException;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletRequestWrapper;

public class MyHttpServletRequest extends HttpServletRequestWrapper {
	private HttpServletRequest request;
	public MyHttpServletRequest(HttpServletRequest request) {
		super(request);
		this.request = request;
	}
	@Override
	// 增强request.getParameter()方法:
	public String getParameter(String name) {
		// 获得请求方式：
		String method = request.getMethod();
		// 根据get还是post请求进行不同方式的乱码的处理:
		if("GET".equalsIgnoreCase(method)){
			// get方式的请求
			String value = super.getParameter(name);
			try {
				value = new String(value.getBytes("ISO-8859-1"),"UTF-8");
			} catch (UnsupportedEncodingException e) {
				e.printStackTrace();
			}
			return value;
		}else if("POST".equalsIgnoreCase(method)){
			// post方式的请求
			try {
				request.setCharacterEncoding("UTF-8");
			} catch (UnsupportedEncodingException e) {
				e.printStackTrace();
			}
		}
		return super.getParameter(name);
	}
}

```

```
package com.zxl.login.filter;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;

/**
 * Servlet Filter implementation class GenericEncodingFilter
 */
public class GenericEncodingFilter implements Filter {

    /**
     * Default constructor. 
     */
    public GenericEncodingFilter() {
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see Filter#destroy()
	 */
	public void destroy() {
		// TODO Auto-generated method stub
	}

	/**
	 * @see Filter#doFilter(ServletRequest, ServletResponse, FilterChain)
	 */
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
		// 在过滤器中增强request对象，并将增强后的request对象传递给Servlet:
		HttpServletRequest req = (HttpServletRequest) request;
		// 增强req:
		MyHttpServletRequest myReq = new MyHttpServletRequest(req);
		chain.doFilter(myReq, response);
	}

	/**
	 * @see Filter#init(FilterConfig)
	 */
	public void init(FilterConfig fConfig) throws ServletException {
		// TODO Auto-generated method stub
	}

}

```
