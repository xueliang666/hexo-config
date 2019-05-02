title: Java mail发送邮件示例
author: 学亮
date: 2019-04-28 16:17:23
tags:
---
```
package com.zhangxueliang.demo;

import java.util.*;
import javax.mail.*;
import javax.mail.Message.RecipientType;
import javax.mail.internet.*;
import javax.activation.*;
 
public class SendEmailUtils{
	private static String smtp_host = "smtp.163.com"; // 网易
	private static String username = "zhangxueliang***********@163.com"; // 邮箱账户
	private static String password = "***********"; // 邮箱授权码
	private static String from = "zhangxueliang***********@163.com"; // 使用当前账户
	
	//测试代码：测试邮件能否发送成功
	public static void main(String[] args) {
		sendMail("张学亮在测试邮件玩玩", "邮件正文内容", "zhangxueliang********@163.com");
	}
	
	public static void sendMail(String subject, String content, String to) {
		Properties props = new Properties();
		props.setProperty("mail.smtp.host", smtp_host);
		props.setProperty("mail.transport.protocol", "smtp");
		props.setProperty("mail.smtp.auth", "true");
		Session session = Session.getInstance(props);
		Message message = new MimeMessage(session);
		try {
			message.setFrom(new InternetAddress(from));
			message.setRecipient(RecipientType.TO, new InternetAddress(to));
			message.setSubject(subject);
			message.setContent(content, "text/html;charset=utf-8");
			Transport transport = session.getTransport();
			transport.connect(smtp_host, username, password);
			transport.sendMessage(message, message.getAllRecipients());
			System.out.println("邮件发送成功...");
		} catch (Exception e) {
			e.printStackTrace();
			throw new RuntimeException("邮件发送失败...");
		}
	}
	
}


```
