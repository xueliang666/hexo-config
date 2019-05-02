title: Java爬虫开发案例
author: 学亮
tags:
  - 爬虫
categories: []
date: 2019-04-29 20:08:00
---
> 某新闻网站爬虫开发实战

<!-- more -->

# 某新闻网站爬虫开发实战
* 1、需求说明
	* 1.1、基本情况说明
	* 1.2、需求分析
	* 1.3、功能分析

# 1、 需求说明
### 1.1、基本情况说明
* 网站地址：https://www.huxiu.com/
* 网站类型：新闻（自媒体）通过高质量的文章或者新闻获取更多的用户，从构建商业模式
* 爬虫爬取的范围：爬取首页（包含详情页的连接）、爬取新闻的详情页、


### 1.2、需求分析
* 通过解析首页，得到首页包含的新闻的列表信息
* 为了获取完整的信息，我们需要自动操作分页按钮。
	* 分页请求的url：
````shell
	https://www.huxiu.com/v2_action/article_list
````
	* 分页提交的header
````
Accept:application/json, text/javascript, */*; q=0.01
Accept-Encoding:gzip, deflate
Accept-Language:zh-CN,zh;q=0.8
Connection:keep-alive
Content-Length:80
Content-Type:application/x-www-form-urlencoded; charset=UTF-8
Cookie:SERVERID=6d35b07e250aadfe8b4fbf72aeadecb9|1504659560|1504658827
Host:www.huxiu.com
Origin:https://www.huxiu.com
Referer:https://www.huxiu.com/
User-Agent:Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/50.0.2661.102 Safari/537.36
X-Requested-With:XMLHttpRequest
````


* 分页POST提交的参数
* huxiu_hash_code:353a9683918c807f5f783dc1df116fad 
		用来标识当前用户（浏览器）
		* page:2-----total_page
			当前的分页编码
		* last_dateline:1504534200
			时间
* 分页请求的返回值
```shell
result: 1, msg: "获取成功",…}
data:"<div>这里是数据</div>"
last_dateline:"1504321620"
msg:"获取成功"
result:1
total_page:1585
```

### 1.3、功能分析
* 获取首页的html文档
	* 解析首页的html文档获得article urls
	* 迭代urls，一次请求每个url。
		* 在解析单个新闻详情页，只需要内容
		* 文章对象：标题、正文、作者、发布时间、评论数、点赞数、收藏数
* 获取分页的html文档
	* 发起分页的请求，传递三个参数，有两个变量。
		* 分页编号
			* 第一次分页 startpage=2
			* 第二次分页 startpage=3
		* last_dataline
			* 第一分页时，需要从首页中解析last_datalien
			* 第二次分页时，从上一个分页请求的返回值中获取。
	* 编写for自动执行分页
* 将结果数据保存起来
	* 连接数据库：jdbctemplate


# 2、代码开发
### 2.1、创建maven的项目
* 导入pom依赖
	* httpclient:用来做网络请求
	* jsoup:用来解析html文档
	* spring-jdbc：使用spring jdbctemplate
	* mysql的驱动包：mysql-java-connector.jar
	* 解析json串的包(alibaba fastjson（性能好）,google Gson)
* 如何寻找pom依赖
	* mvnrepository
	* maven.org
* 项目依赖的pom

```shell
  <dependency>
            <groupId>org.apache.httpcomponents</groupId>
            <artifactId>httpclient</artifactId>
            <version>4.5.3</version>
        </dependency>
        <!-- jsoup HTML parser library @ https://jsoup.org/ -->
        <dependency>
            <groupId>org.jsoup</groupId>
            <artifactId>jsoup</artifactId>
            <version>1.10.3</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>4.2.6.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.41</version>
        </dependency>
        <dependency>
            <groupId>c3p0</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.1.2</version>
        </dependency>

        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.31</version>
        </dependency>
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.8.1</version>
        </dependency>
```


# 3、优化爬虫
### 3.1、使用多线程
创建线程的三种方式
* 继承Thread
* 实现runable接口
* 创建有返回值的线程
	* 第一步：实现callable接口，复写call方法，得到一个返回值
	* 第二步：创建FutureTask类，将callable对象传入。
	* 第三步：**使用Thread，运行FutureTask**
	* 第四步：等待10秒
	* 第五步：通过FutureTask.get() 获取返回值
    
### 3.2、使用线程池 Executors
问题：
* 发现使用多线程，如果没有线程池，就会增加系统开销。
	* 网卡的网络资源
	* CPU的计算资源
	* 物理内存 **每创建一个线程要消耗1M的内存**
![](img/2017-09-06_160326.png)
* 循环利用创建好的线程
	* 定时器的两种方式 1）固定周期执行 2）执行完上一次之后再隔一定周期后执行
![](img/2017-09-06_160415.png)

### 3.3、使用线程池优化爬虫

### 3.4、性能优化的思考方向
* 多线程，是提高了访问网络的速度
* 数据库的连接池，是数据库的连接复用。
	* 批量存储
* 解析文档的时候可以优化
---------------------------------
> 总结：
在做系统的优化时，将系统分解成各个模块。比如：网络请求模块，数据处理模块，数据保存模块。然后针对不同的模块进行针对性的优化。






