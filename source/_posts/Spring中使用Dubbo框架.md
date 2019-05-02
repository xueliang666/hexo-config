title: Spring中使用Dubbo框架
author: 学亮
date: 2019-04-28 16:15:55
tags:
---
**Dubbo框架**

*什么是dubbo*

> 随着互联网的发展，网站应用的规模不断扩大，常规的垂直应用架构已无法应对，分布式服务架构以及流动计算架构势在必行，亟需一个治理系统确保架构有条不紊的演进。

![](https://i.imgur.com/RhVWWNP.jpg)

> ![](https://i.imgur.com/5XUjg8u.jpg)

*Dubbo架构*

![](https://i.imgur.com/qrkO7ir.jpg)

![](https://i.imgur.com/GFzqmtA.jpg)

*使用方法*

*Spring配置*



> Dubbo采用全Spring配置方式，透明化接入应用，对应用没有任何API侵入，只需用Spring加载Dubbo的配置即可，Dubbo基于Spring的Schema扩展进行加载。

**单一工程中spring的配置**

    <bean id="xxxService" class="com.xxx.XxxServiceImpl" />
	<bean id="xxxAction" class="com.xxx.XxxAction">
		<property name="xxxService" ref="xxxService" />
	</bean>

**远程服务：**

> 在本地服务的基础上，只需做简单配置，即可完成远程化：

> 将上面的local.xml配置拆分成两份，将服务定义部分放在服务提供方remote-provider.xml，将服务引用部分放在服务消费方remote-consumer.xml。
> 
> 并在提供方增加暴露服务配置<dubbo:service>，在消费方增加引用服务配置<dubbo:reference>。

**发布服务：**

    <!-- 和本地服务一样实现远程服务 -->
	<bean id="xxxService" class="com.xxx.XxxServiceImpl" />
	<!-- 增加暴露远程服务配置 -->
	<dubbo:service interface="com.xxx.XxxService" ref="xxxService" />

**调用服务：**

    <!-- 增加引用远程服务配置 -->
	<dubbo:reference id="xxxService" interface="com.xxx.XxxService" />
	<!-- 和本地服务一样使用远程服务 -->
	<bean id="xxxAction" class="com.xxx.XxxAction">
		<property name="xxxService" ref="xxxService" />
	</bean>

*注册中心*

> 注册中心负责服务地址的注册与查找，相当于目录服务，服务提供者和消费者只在启动时与注册中心交互，注册中心不转发请求，压力较小。使用dubbo-2.3.3以上版本，建议使用zookeeper注册中心。
Zookeeper是Apacahe Hadoop的子项目，是一个树型的目录服务，支持变更推送，适合作为Dubbo服务的注册中心，工业强度较高，可用于生产环境，并推荐使用。

![](https://i.imgur.com/IAApCJ5.jpg)

*框架整合*

**添加dubbo的依赖**

> 加入dubbo相关的jar包。服务层、表现层都添加。

    <!-- dubbo相关 -->
	<dependency>
		<groupId>com.alibaba</groupId>
		<artifactId>dubbo</artifactId>
		<!-- 排除依赖 -->
		<exclusions>
			<exclusion>
				<groupId>org.springframework</groupId>
				<artifactId>spring</artifactId>
			</exclusion>
			<exclusion>
				<groupId>org.jboss.netty</groupId>
				<artifactId>netty</artifactId>
			</exclusion>
		</exclusions>
	</dependency>
	<dependency>
		<groupId>org.apache.zookeeper</groupId>
		<artifactId>zookeeper</artifactId>
	</dependency>
	<dependency>
		<groupId>com.github.sgroschupf</groupId>
		<artifactId>zkclient</artifactId>
	</dependency>

