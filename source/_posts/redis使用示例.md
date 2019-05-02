title: redis使用示例
author: 学亮
date: 2019-04-28 16:12:52
tags:
---
> 代码使用

```
@Autowired
private RedisTemplate<String, String> redisTemplate;
......
// 发送一封激活邮件
// 生成激活码
String activecode = RandomStringUtils.randomNumeric(32);

// 将激活码保存到redis，设置24小时失效
redisTemplate.opsForValue().set(model.getTelephone(), activecode, 24,
		TimeUnit.HOURS);
```

> 配置文件：

```
<!-- 引入redis配置 -->
<import resource="applicationContext-cache.xml"/>
......
<!-- jedis 连接池配置 -->
<bean id="poolConfig" class="redis.clients.jedis.JedisPoolConfig">  
      <property name="maxIdle" value="300" />        
      <property name="maxWaitMillis" value="3000" />  
      <property name="testOnBorrow" value="true" />  
  </bean>  

<!-- jedis 连接工厂 -->
<bean id="redisConnectionFactory"  
      class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory"  
      p:host-name="localhost" p:port="6379" p:pool-config-ref="poolConfig"  
      p:database="0" />  
      
 <!-- spring data 提供 redis模板  -->
 <bean id="redisTemplate" class="org.springframework.data.redis.core.RedisTemplate">  
     <property name="connectionFactory" ref="redisConnectionFactory" /> 
     <!-- 如果不指定 Serializer   -->
     <property name="keySerializer">
         <bean class="org.springframework.data.redis.serializer.StringRedisSerializer" />
     </property>
     <property name="valueSerializer">
     	<bean class="org.springframework.data.redis.serializer.StringRedisSerializer"> 
     	</bean>
     </property> 
 </bean>
```
