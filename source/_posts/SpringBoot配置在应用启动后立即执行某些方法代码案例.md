title: SpringBoot配置在应用启动后立即执行某些方法代码案例
author: 学亮
date: 2019-04-28 16:13:42
tags:
---
>springboot给我们提供了两种“开机启动”方式：ApplicationRunner和CommandLineRunner。

>这两种方法提供的目的是为了满足，在项目启动的时候立刻执行某些方法。我们可以通过实现ApplicationRunner和CommandLineRunner，来实现，他们都是在SpringApplication 执行之后开始执行的。

>CommandLineRunner接口可以用来接收字符串数组的命令行参数，ApplicationRunner 是使用ApplicationArguments 用来接收参数的。

**CommandLineRunner ：**

````
@Component
public class MyCommandLineRunner implements CommandLineRunner{

    @Override
    public void run(String... var1) throws Exception{
        System.out.println("This will be execute when the project was started!");
    }
}
````
**ApplicationRunner ：**

````
@Component
public class MyApplicationRunner implements ApplicationRunner {
    @Override
    public void run(ApplicationArguments var1) throws Exception{
        System.out.println("MyApplicationRunner class will be execute when the project was started!");
    }
}
````

> 这两种方式的实现都很简单，直接实现了相应的接口就可以了。
> 
> 记得在类上加@Component注解。
> 
> 如果想要指定启动方法执行的顺序，可以通过实现org.springframework.core.Ordered接口或者使用org.springframework.core.annotation.Order注解来实现。
> 
> 以ApplicationRunner 为例来分别实现。

**Ordered接口：**
````
@Component
public class MyApplicationRunner implements ApplicationRunner,Ordered{


    @Override
    public int getOrder(){
        return 1;//通过设置这里的数字来知道指定顺序
    }

    @Override
    public void run(ApplicationArguments var1) throws Exception{
        System.out.println("MyApplicationRunner1!");
    }
}
````
**Order注解实现方式：**
````
@Component
@Order(value = 1)
public class MyApplicationRunner implements ApplicationRunner{

    @Override
    public void run(ApplicationArguments var1) throws Exception{
        System.out.println("MyApplicationRunner1!");
    }

}
````
