title: Spring Boot项目开启异步使用多线程完整代码案例
author: 学亮
tags:
  - spring boot
  - 异步
  - 多线程
categories: []
date: 2019-04-29 16:13:00
---
> 本文通过代码来演示如果在spring boot的项目中使用多线程，也就是异步。要异步并不难，我们写的代码天天都在跟异步多线程打交道，容易让人感到迷惑的是异步的底层原理，不仅要会使用，更要熟悉其实现原理，才能更加灵活地在项目中进行运用。



<!-- more -->

> 首先在pom.xml中引入依赖。

````
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.zhangxueliang.demo</groupId>
	<artifactId>springbootdemo</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>springbootdemo</name>
	<description>Demo project for Spring Boot</description>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.1.0.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-redis</artifactId>
		</dependency>

		<dependency>
			<groupId>org.apache.httpcomponents</groupId>
			<artifactId>fluent-hc</artifactId>
			<version>4.5.3</version>
		</dependency>
		<dependency>
			<!-- jsoup HTML parser library @ https://jsoup.org/ -->
			<groupId>org.jsoup</groupId>
			<artifactId>jsoup</artifactId>
			<version>1.10.3</version>
		</dependency>
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>fastjson</artifactId>
			<version>1.2.31</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-jdbc -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-jdbc</artifactId>
		</dependency>
		<!--mysql驱动-->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>8.0.12</version>
			<scope>runtime</scope>
		</dependency>
		<!--集成mybatis-->
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>1.3.1</version>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-jdbc</artifactId>
		</dependency>
		<!--整合freemarker-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-freemarker</artifactId>
		</dependency>
		<!--整合thymleaf-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-thymeleaf</artifactId>
		</dependency>
		<!--整合alibaba easyPOI-->
		<dependency>
			<groupId>cn.afterturn</groupId>
			<artifactId>easypoi-base</artifactId>
			<version>3.2.0</version>
		</dependency>
		<dependency>
			<groupId>cn.afterturn</groupId>
			<artifactId>easypoi-web</artifactId>
			<version>3.2.0</version>
		</dependency>
		<dependency>
			<groupId>cn.afterturn</groupId>
			<artifactId>easypoi-annotation</artifactId>
			<version>3.2.0</version>
		</dependency>
		<!-- lombok可使代码更简洁 -->
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<scope>provided</scope>
		</dependency>
		<!--热部署-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<optional>true</optional>
		</dependency>
		<!--引入AOP依赖-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-aop</artifactId>
		</dependency>
		<!--整合MongoDB-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-mongodb</artifactId>
		</dependency>
		<!--整合rabbitmq-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-amqp</artifactId>
		</dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
        </dependency>
		<!--使用@Slf4j注解-->
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
		</dependency>
		<!--爬虫相关依赖-->
		<!-- jsoup HTML parser library @ https://jsoup.org/ -->
		<dependency>
			<groupId>org.jsoup</groupId>
			<artifactId>jsoup</artifactId>
			<version>1.11.2</version>
		</dependency>
		<!--整合POI-->
		<!--<dependency>
			<groupId>org.apache.poi</groupId>
			<artifactId>poi</artifactId>
			<version>RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.apache.poi</groupId>
			<artifactId>poi-ooxml</artifactId>
			<version>RELEASE</version>
		</dependency>-->

		<dependency>
			<groupId>org.apache.poi</groupId>
			<artifactId>poi</artifactId>
			<version>3.15</version>
		</dependency>
		<dependency>
			<groupId>org.apache.poi</groupId>
			<artifactId>poi-ooxml-schemas</artifactId>
			<version>3.15</version>
		</dependency>
		<dependency>
			<groupId>org.apache.poi</groupId>
			<artifactId>poi-ooxml</artifactId>
			<version>3.15</version>
		</dependency>

		<!-- webMagic start -->
		<dependency>
			<groupId>us.codecraft</groupId>
			<artifactId>webmagic-core</artifactId>
			<version>0.7.3</version>
		</dependency>
		<dependency>
			<groupId>us.codecraft</groupId>
			<artifactId>webmagic-extension</artifactId>
			<version>0.7.3</version>
		</dependency>
		<dependency>
			<groupId>us.codecraft</groupId>
			<artifactId>webmagic-extension</artifactId>
			<version>0.7.3</version>
			<exclusions>
				<exclusion>
					<groupId>org.slf4j</groupId>
					<artifactId>slf4j-log4j12</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
		<!-- webMagic end -->

        <!--引入log工程jar-->
		<!--<dependency>
			<groupId>com.fengyuncx.log</groupId>
			<artifactId>fy-log</artifactId>
			<version>0.0.1-SNAPSHOT</version>
			<scope>system</scope>
			<systemPath>${project.basedir}/src/main/resources/lib/fy-log-0.0.1-SNAPSHOT.jar</systemPath>
		</dependency>-->
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<!--打包时将引入的本地jar打进去-->
				<configuration>
					<includeSystemScope>true</includeSystemScope>
				</configuration>
			</plugin>
		</plugins>
	</build>


</project>

````

> 在启动类上添加@EnableAsync注解，开启异步.注意：如果在配置类上添加了该注解，启动类上就无需添加。

````
@SpringBootApplication
//@EnableAsync//开启异步
public class ZxlDemoApplication {

	public static void main(String[] args) {
		SpringApplication.run(ZxlDemoApplication.class, args);
	}

}
````

> 编写多线程配置类

````
/**
 * 线程配置类
 */
@Configuration
@EnableAsync
public class AsyncTaskConfig implements AsyncConfigurer {

    // ThredPoolTaskExcutor的处理流程
    // 当池子大小小于corePoolSize，就新建线程，并处理请求
    // 当池子大小等于corePoolSize，把请求放入workQueue中，池子里的空闲线程就去workQueue中取任务并处理
    // 当workQueue放不下任务时，就新建线程入池，并处理请求，如果池子大小撑到了maximumPoolSize，就用RejectedExecutionHandler来做拒绝处理
    // 当池子的线程数大于corePoolSize时，多余的线程会等待keepAliveTime长时间，如果无请求可处理就自行销毁

    @Override
    public Executor getAsyncExecutor() {
        ThreadPoolTaskExecutor taskExecutor = new ThreadPoolTaskExecutor();
        taskExecutor.setCorePoolSize(50);// 核心线程数
        taskExecutor.setMaxPoolSize(50);// 最大线程数
        taskExecutor.setQueueCapacity(200);// 等待队列
        taskExecutor.setThreadNamePrefix("zxl_taskExecutor-");//线程名称前缀
        taskExecutor.initialize();
        return taskExecutor;
    }

    @Override
    public AsyncUncaughtExceptionHandler getAsyncUncaughtExceptionHandler() {
        return null;
    }
}
````

> 编写线程任务执行类

````
package com.zhangxueliang.demo.springbootdemo.async;

import java.util.Random;
import java.util.concurrent.Future;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.scheduling.annotation.Async;
import org.springframework.scheduling.annotation.AsyncResult;
import org.springframework.stereotype.Service;

/**
 * 线程执行任务类
 */
@Service
public class AsyncTaskService {

    private Logger logger = LoggerFactory.getLogger(AsyncTaskService.class);

    Random random = new Random();// 默认构造方法

    @Async
    // 表明是异步方法
    // 无返回值
    public void executeAsyncTask(String msg) {
        System.out.println(Thread.currentThread().getName()+"开启新线程执行" + msg);
    }

    @Async
    public Future<String> doReturn(int i){
        logger.info(">>>>>>>>>>>>>>线程名>>>>>>>>>>>>>>"+Thread.currentThread().getName());
        try {
            // 这个方法需要调用500毫秒
            Thread.sleep(500);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        // 消息汇总
        return new AsyncResult<>(String.format("这个是第{%s}个异步调用的证书", i));
    }


    /**
     * 异常调用返回Future
     * @return
     * @throws InterruptedException
     */
    @Async
    public Future<Long> asyncInvokeReturnFuture() throws InterruptedException {
        long start = System.currentTimeMillis();
        Thread.sleep(5000);
        Future<Long> future = new AsyncResult<Long>((System.currentTimeMillis()-start));// Future接收返回值，这里是String类型，可以指明其他类型
        return future;
    }

    @Async
    public Future<Long> asyncInvokeReturnFuture2() throws InterruptedException {
        long start = System.currentTimeMillis();
        Thread.sleep(3500);
        Future<Long> future = new AsyncResult<Long>((System.currentTimeMillis()-start));// Future接收返回值，这里是String类型，可以指明其他类型
        return future;
    }

    @Async
    public Future<Long> asyncInvokeReturnFuture3() throws InterruptedException {
        long start = System.currentTimeMillis();
        Thread.sleep(2500);
        Future<Long> future = new AsyncResult<Long>((System.currentTimeMillis()-start));// Future接收返回值，这里是String类型，可以指明其他类型
        return future;
    }
}
````

> 编写测试类，也就是调用多线程任务。

````
@RunWith(SpringRunner.class)
@SpringBootTest
public class ZxlTest {
	private Logger logger = LoggerFactory.getLogger(ZxlTest.class);
	@Autowired
    private AsyncTaskService asyncTaskService;
	@Test
    public void testAsync(){
//        String msg="测试测试";
//        System.out.println(Thread.currentThread().getName()+":"+msg);
//        asyncTaskService.executeAsyncTask(msg);
        try {
            long start = System.currentTimeMillis();
            Future<Long> future = asyncTaskService.asyncInvokeReturnFuture();
            Future<Long> future2 = asyncTaskService.asyncInvokeReturnFuture2();
            Future<Long> future3 = asyncTaskService.asyncInvokeReturnFuture3();
            Long l1 = future.get();//5000
            Long l2 = future2.get();//3500
            Long l3 = future3.get();//2500
            System.out.println(Thread.currentThread().getName()+"*************************************");
            System.out.println(Thread.currentThread().getName()+(l1+l2+l3 ));
            System.out.println(Thread.currentThread().getName()+"-------------------------------------");
            System.out.println(Thread.currentThread().getName()+(System.currentTimeMillis()-start));
            System.out.println(Thread.currentThread().getName()+"+++++++++++++++++++++++++++++++++++++");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
````










