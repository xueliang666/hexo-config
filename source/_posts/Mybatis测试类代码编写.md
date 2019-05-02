title: Mybatis测试类代码编写
author: 学亮
date: 2019-04-28 15:51:46
tags:
---
```
package test;

import java.io.IOException;
import java.io.InputStream;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Before;
import org.junit.Test;

import com.zxl.mybatis.mapper.UserMapper;
import com.zxl.mybatis.po.User;


public class UserMapperTest {
	private SqlSessionFactory sqlSessionFactory=null;
	@Before
	public void init() throws Exception{
		SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
		InputStream is = Resources.getResourceAsStream("SqlMapConfig.xml");
		sqlSessionFactory = sqlSessionFactoryBuilder.build(is);
	}
	@Test
	public void test1(){
		SqlSession sqlSession = sqlSessionFactory.openSession();
		UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
		User user = userMapper.getUserById(10);
		System.out.println(user);
	}
}

```
