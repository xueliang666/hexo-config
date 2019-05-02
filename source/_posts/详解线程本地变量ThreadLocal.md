title: 详解线程本地变量ThreadLocal
author: 学亮
tags:
  - ThreadLocal
categories: []
date: 2019-04-30 10:54:00
---
> 并发应用的一个关键地方就是共享数据。如果你创建一个类对象，实现Runnable接口，然后多个Thread对象使用同样的Runnable对象，全部的线程都共享同样的属性。这意味着，如果你在一个线程里改变一个属性，全部的线程都会受到这个改变的影响。
有时，你希望程序里的各个线程的属性不会被共享。 Java 并发 API提供了一个很清楚的机制叫本地线程变量即ThreadLocal。
模拟ThreadLocal类实现：线程范围内的共享变量，每个线程只能访问他自己的，不能访问别的线程。

<!-- more -->

**一、本地线程变量使用场景**

> 并发应用的一个关键地方就是共享数据。如果你创建一个类对象，实现Runnable接口，然后多个Thread对象使用同样的Runnable对象，全部的线程都共享同样的属性。这意味着，如果你在一个线程里改变一个属性，全部的线程都会受到这个改变的影响。
有时，你希望程序里的各个线程的属性不会被共享。 Java 并发 API提供了一个很清楚的机制叫本地线程变量即ThreadLocal。
模拟ThreadLocal类实现：线程范围内的共享变量，每个线程只能访问他自己的，不能访问别的线程。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190430104311538.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2E3NzIzMDQ0MTk=,size_16,color_FFFFFF,t_70)


**二、对ThreadLocal的理解**

> ThreadLocal，很多地方叫做线程本地变量，也有些地方叫做本地线程变量，其实意思差不多。
ThreadLocal和本地线程没有半毛钱关系，更不是一个特殊的Thread，它只是一个线程的局部变量(其实就是一个Map用于存储每一个线程的变量副本，Map中元素的Key为线程对象，而Value对应线程的变量副本),ThreadLocal会为每个使用该变量的线程提供独立的变量副本，所以每一个线程都可以独立地改变自己的副本，而不会影响其它线程所对应的副本。
对于多线程资源共享的问题，同步机制(Synchronized)采用了“以时间换空间”的方式，而ThreadLocal采用了“以空间换时间”的方式。前者仅提供一份变量，让不同的线程排队访问，而后者为每一个线程都提供了一份变量，因此可以同时访问而互不影响。
官方对ThreadLocal的描述：
> 
> 
> 
> 
> 
>  1、每个线程都有自己的局部变量
> 每个线程都有一个独立于其他线程的上下文来保存这个变量，一个线程的本地变量对其他线程是不可见的（有前提，后面解释） 2、独立于变量的初始化副本
> ThreadLocal可以给一个初始值，而每个线程都会获得这个初始化值的一个副本，这样才能保证不同的线程都有一份拷贝。
> 3、状态与某一个线程相关联  ThreadLocal
> 不是用于解决共享变量的问题的，不是为了协调线程同步而存在，而是为了方便每个线程处理自己的状态而引入的一个机制，理解这点对正确使用ThreadLocal至关重要。
> 通过ThreadLocal存取的数据，总是与当前线程相关，也就是说，JVM
> 为每个运行的线程，绑定了私有的本地实例存取空间，从而为多线程环境常出现的并发访问问题提供了一种隔离机制。 我们还是先来看一个例子：


```
class ConnectionManager {
   
        private static Connection connect = null;

        public static Connection openConnection() {
            if(connect == null){
                connect = DriverManager.getConnection();
            }
            return connect;
        }

        public static void closeConnection() {
            if(connect!=null)
                connect.close();
        }
    }
```

>  　　假设有这样一个数据库链接管理类，这段代码在单线程中使用是没有任何问题的，但是如果在多线程中使用呢？很显然，在多线程中使用会存在线程安全问题：第一，这里面的2个方法都没有进行同步，很可能在openConnection方法中会多次创建connect；第二，由于connect是共享变量，那么必然在调用connect的地方需要使用到同步来保障线程安全，因为很可能一个线程在使用connect进行数据库操作，而另外一个线程调用closeConnection关闭链接。
> 
> 　　所以出于线程安全的考虑，必须将这段代码的两个方法进行同步处理，并且在调用connect的地方需要进行同步处理。
> 
> 　　这样将会大大影响程序执行效率，因为一个线程在使用connect进行数据库操作的时候，其他线程只有等待。
> 
> 　　那么大家来仔细分析一下这个问题，这地方到底需不需要将connect变量进行共享？事实上，是不需要的。假如每个线程中都有一个connect变量，各个线程之间对connect变量的访问实际上是没有依赖关系的，即一个线程不需要关心其他线程是否对这个connect进行了修改的。
> 
> 　　到这里，可能会有朋友想到，既然不需要在线程之间共享这个变量，可以直接这样处理，在每个需要使用数据库连接的方法中具体使用时才创建数据库链接，然后在方法调用完毕再释放这个连接。比如下面这样：


```
class ConnectionManager {
         
        private  Connection connect = null;
         
        public Connection openConnection() {
            if(connect == null){
                connect = DriverManager.getConnection();
            }
            return connect;
        }
         
        public void closeConnection() {
            if(connect!=null)
                connect.close();
        }
    }
     
     
    class Dao{
        public void insert() {
            ConnectionManager connectionManager = new ConnectionManager();
            Connection connection = connectionManager.openConnection();
             
            //使用connection进行操作
             
            connectionManager.closeConnection();
        }
    }

```

> 　　这样处理确实也没有任何问题，由于每次都是在方法内部创建的连接，那么线程之间自然不存在线程安全问题。但是这样会有一个致命的影响：导致服务器压力非常大，并且严重影响程序执行性能。由于在方法中需要频繁地开启和关闭数据库连接，这样不尽严重影响程序执行效率，还可能导致服务器压力巨大。
> 
> 　　那么这种情况下使用ThreadLocal是再适合不过的了，因为ThreadLocal在每个线程中对该变量会创建一个副本，即每个线程内部都会有一个该变量，且在线程内部任何地方都可以使用，线程之间互不影响，这样一来就不存在线程安全问题，也不会严重影响程序执行性能。
> 
> 　　但是要注意，虽然ThreadLocal能够解决上面说的问题，但是由于在每个线程中都创建了副本，所以要考虑它对资源的消耗，比如内存的占用会比不使用ThreadLocal要大。

**三、深入解析ThreadLocal类**

> 在上面谈到了对ThreadLocal的一些理解，那我们下面来看一下具体ThreadLocal是如何实现的。
> 先了解一下ThreadLocal类提供的几个方法：

```
    public T get() { }
    public void set(T value) { }
    public void remove() { }
    protected T initialValue() { }
    
    ```

> get()方法是用来获取ThreadLocal在当前线程中保存的变量副本； set()用来设置当前线程中变量的副本；
> remove()用来移除当前线程中变量的副本；
> initialValue()是一个protected方法，一般是用来在使用时进行重写的，它是一个延迟加载方法；
> 
> ThreadLocal是如何为每个线程创建变量的副本的：
> 1、在每个线程Thread内部有一个ThreadLocal.ThreadLocalMap类型的成员变量threadLocals，这个threadLocals就是用来存储实际的变量副本的，Key为当前ThreadLocal变量，value为变量副本（即T类型的变量）。
> 2、初始时，在Thread里面，threadLocals为空，当通过ThreadLocal变量调用get()方法或者set()方法，就会对Thread类中的threadLocals进行初始化，并且以当前ThreadLocal变量为键值，以ThreadLocal要保存的副本变量为value，存到threadLocals。
> 3、在当前线程里面，如果要使用副本变量，就可以通过get方法在threadLocals里面查找。
> 下面通过一个例子来证明通过ThreadLocal能达到在每个线程中创建变量副本的效果：

```
public class Test {
        ThreadLocal<Long> longLocal = new ThreadLocal<Long>();
        ThreadLocal<String> stringLocal = new ThreadLocal<String>();
    	public void set() {
            longLocal.set(Thread.currentThread().getId());
            stringLocal.set(Thread.currentThread().getName());
        }
        public long getLong() {
            return longLocal.get();
        }
        public String getString() {
            return stringLocal.get();
        }
        public static void main(String[] args) throws InterruptedException {
            final Test test = new Test();
            test.set();
            System.out.println(test.getLong());
            System.out.println(test.getString());
            Thread thread1 = new Thread(){
                public void run() {
                    test.set();
                    System.out.println(test.getLong());
                    System.out.println(test.getString());
                };
            };
            thread1.start();
            thread1.join();
            System.out.println(test.getLong());
            System.out.println(test.getString());
        }
    }
    ```
 　　

> 这段代码的输出结果为：
>
　　![在这里插入图片描述](https://img-blog.csdnimg.cn/20190430105117932.jpg)

> 从这段代码的输出结果可以看出，在main线程中和thread1线程中，longLocal保存的副本值和stringLocal保存的副本值都不一样。最后一次在main线程再次打印副本值是为了证明在main线程中和thread1线程中的副本值确实是不同的。
> 
> 总结一下：
> 
> 　　1）实际的通过ThreadLocal创建的副本是存储在每个线程自己的threadLocals中的；
> 
> 　　2）为何threadLocals的类型ThreadLocalMap的键值为ThreadLocal对象，因为每个线程中可有多个threadLocal变量，就像上面代码中的longLocal和stringLocal；

 

**四.ThreadLocal的应用场景**

>    最常见的ThreadLocal使用场景为：用来解决数据库连接、Session管理，多线程单例模式访问；
> 
> 订单处理包含一系列操作：减少库存量、增加一条流水台账、修改总账，这几个操作要在同一个事务中完成，通常也即同一个线程中进行处理，如果累加公司应收款的操作失败了，则应该把前面的操作回滚，否则，提交所有操作，这要求这些操作使用相同的数据库连接对象，而这些操作的代码分别位于不同的模块类中。
> 银行转账包含一系列操作：
> 把转出帐户的余额减少，把转入帐户的余额增加，这两个操作要在同一个事务中完成，它们必须使用相同的数据库连接对象，转入和转出操作的代码分别是两个不同的帐户对象的方法。
> 
>    我们先看一个简单的例子：

```
public class ThreadLocalTest {
    
      //创建一个Integer型的线程本地变量
        public static final ThreadLocal<Integer> local = new ThreadLocal<Integer>() {
            @Override
            protected Integer initialValue() {
                return 0;
            }
        };
        public static void main(String[] args) throws InterruptedException {
            Thread[] threads = new Thread[5];
            for (int j = 0; j < 5; j++) {       
               threads[j] = new Thread(new Runnable() {
                    @Override
                    public void run() {
                        //获取当前线程的本地变量，然后累加5次
                        int num = local.get();
                        for (int i = 0; i < 5; i++) {
                            num++;
                        }
                        //重新设置累加后的本地变量
                        local.set(num);
                        System.out.println(Thread.currentThread().getName() + " : "+ local.get());
     		    }
                }, "Thread-" + j);
            }
     		for (Thread thread : threads) {
                thread.start();
            }
        }
    }
```
> 运行后结果：
```
Thread-0 : 5
Thread-4 : 5
Thread-2 : 5
Thread-1 : 5
Thread-3 : 5
```
> 我们看到，每个线程累加后的结果都是5，各个线程处理自己的本地变量值，线程之间互不影响。
> 
> 如：数据库连接：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190430105210757.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2E3NzIzMDQ0MTk=,size_16,color_FFFFFF,t_70)

> Session连接：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190430105233990.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2E3NzIzMDQ0MTk=,size_16,color_FFFFFF,t_70)





**五、ThreadLocal使用的一般步骤**
 

> 1、在多线程的类（如ThreadDemo类）中，创建一个ThreadLocal对象threadXxx，用来保存线程间需要隔离处理的对象xxx。
> 2、在ThreadDemo类中，创建一个获取要隔离访问的数据的方法getXxx()，在方法中判断，若ThreadLocal对象为null时候，应该new()一个隔离访问类型的对象，并强制转换为要应用的类型。
> 3、在ThreadDemo类的run()方法中，通过getXxx()方法获取要操作的数据，这样可以保证每个线程对应一个数据对象，在任何时刻都操作的是这个对象。
> 相关链接请参考：http://my.oschina.net/xianggao/blog/392440?fromerr=nKHw4fBT
