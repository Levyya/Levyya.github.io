---
title: java 知识点
date: 2022-10-11 14:23:19
tags:
- Java
categories:
- 计算机
- Java
---

> 计算机科学领域的任何问题都可以通过增加一个间接的中间层来解决，计算机整个体系从上到下都是按照严格的层次结构设计的



## 设计模式

### 设计模式原则



单例模式

工厂模式

代理模式

装饰者模式

责任链模式

模板模式

策略模式

外观模式



## 框架

### Spring

IOC 和 AOP 常用的注解

Bean 代指那些被IOC容器所管理的对象

AOP 常用的注解

* @Aspect  声明为切面类
* @Before  前置通知
* @AfterReturning  后置通知
* @AfterThrowing  异常通知
* @Around  环绕通知
* @Pointcut  指定切入点表达式





SpringBoot 常用注解

* @SpringBootApplication

  * @Configuration
  * @EnableAutoConfiguration
  * @ComponentScan

* @Autowired

* @Component, @Repository, @Service, @Controller

* @RestController

  * @Controller
  * @ResponseBody

* @Scope

  * singleton
  * prototype
  * request
  * session

* @GetMapping

  * 等价于 @RequestMapping(value="", method=RequestMethod.GET)

* @PathVariable

  * 获取路径参数

* @RequestParam

  * 获取查询参数

* @RequestBody

  * HttpMessageConverter 将请求中的body中的json字符串转换为Java对象

* @ConfigurationProperties

* @Value

* @Valid

  * @NotNull

* @Validated

* @ControllerAdvice

  * ExceptionHandler

* @Entity

* @Table

* @Id

* 

  

  

  



微服务



## 分布式理论

### CAP

C 一致性

A 可用性

P 分区容错性

CP架构：Zookeeper、HBase

AP架构：Eureka、Cassandra

强一致性场景下选择CP

Nacos 既可以CP也可以AP

### BASE

Basically Available

Soft-state

Eventually Consistent

核心思想：即使无法做到强一致性，但每个应用都可以根据自身业务特点，采取适当的方式来使系统达到最终一致性。



软状态：允许系统中的数据存在中间状态

最终一致性具体方式

* 读时修复
* 写时修复
* 异步修复

### 共识算法

Paxos、Raft、ZAB

Paxos

* Basic Paxos
  * 提议者
  * 接受者
  * 学习者
* Multi Paxos

二阶段提交

Raft 算法

拜占庭将军：多位将军如何达成是否要进攻的一致性决定，信使消息可靠但有可能被暗杀。

节点类型

* Leader
* Candidate
* Follower







## Java 基础

### String

* String, StringBuffer, StringBuilder

### 集合

#### HashMap

数据结构



### 锁

synchronized 和 Lock



方法   锁  CPU

sleep   不释放    释放

yield    不释放    释放

join    释放    释放

wait    释放    释放



### JVM

死亡对象判断方法

* 引用计数法
* 可达性分析法
  * GC Roots
    * 虚拟机栈（栈帧中的本地变量表）中引用的对象
    * 本地方法栈（Native方法）中引用的对象
    * 方法区中类静态属性引用的对象
    * 方法区中常量引用的对象
    * 所有被同步锁持有的对象





4 种引用类型

* 强引用
  * 默认，内存不足时报OOM，也不会回收
* 软引用
  * 内存空间不足时回收
  * 和ReferenceQueue 联合使用
* 弱引用
  * 只要被发现就会被回收
* 虚引用
  * 任何适合都可能被回收



垃圾收集算法

* 标记清除算法
* 标记复制算法
* 标记整理算法
* 分代收集算法

垃圾收集器

* Serial

* ParNew

* Parallel Scavenge

* Serial Old

* Parallel Old

* CMS

  * 初始标记
  * 并发标记
  * 重新标记
  * 并发清除（清除未标记的区域）

* G1

  * 步骤
    * 初始标记
    * 并发标记
    * 最终标记
    * 筛选回收

* ZGC

  

类加载过程

* 加载
* 连接
  * 验证
  * 准备
  * 解析
* 初始化



类加载器

* 启动类加载器 Bootstrap ClassLoader
* 拓展类加载器 Extension ClassLoader
* 应用程序类加载器 Application ClassLoader
* 用户自定义类加载器 User Defined ClassLoader



### 异常机制

Throwable

Error & Exception

* Error
  * VirtulMachineError
    * StackOverFlowError
    * OutOfMemoryError
  * AWTError

* Exception
  * IOException
    * EOFException
    * FileNotFoundException
  * RuntimeException
    * ArrithmeticException
    * MissingResourceException
    * ClassNotFoundException
    * NullPointerException
    * IllegalArgumentException
    * ArrayIndexOutOfBoundsException
    * UnknownTypeException



### 多线程

线程池

ThreadPoolExecutor

* corePoolSize
* maximumPoolSize
* workQueue
* keepAliveTime
  * 线程池中的线程数量大于corePoolSize时，如果没有新的任务提交，核心线程外的线程不会立即销毁，而是等待keepAliveTime才会被回收销毁
* unit
* threadFactory
* handler
  * AbortPolicy
  * DiscardPolicy
  * CallerRunsPolicy
  * DiscardOldestPolicy

```java
public class TaskProcessUtil {
    
    private static Map<String, ExecutorService> executors = new ConcurrentHashMap<>();
    
    private static ExecutorService init(String poolName, int poolSize) {
        return new ThreadPollExecutor(pollSize, pollSize, 
                                      0L, TimeUnit.MILLISECONDS, 
                                      new LinkedBlockingQueue<Runnable>(), 
                                      new ThreadFactoryBuilder().setNameFormat("Pool0" + poolName).setDaemon(false).build(), 
                                      new ThreadPoolExecutor.CallerRunsPolicy());
    }
    
    public static ExecutorService getOrIhitExecutors(String poolName, int poolSize) {
        ExecutorService executorService = executors.get(poolName);
        if (null == executorService) {
            synchronized (TaskProcessUtil.class) {
                executorService = init(poolName, poolSize);
                executors.put(poolName, executorService);
            }
        }
        return executorService;
    }
    
    public static void releaseExecutors(String poolName) {
        ExecutorService executorService = executors.remove(poolName);
        if (executorService != null) {
            executorService.shutdown();
        }
    }
}
```









## MySQL

### 索引

**主键索引 & 二级索引**

**聚集索引**：索引和数据放在一起，主键索引就是聚集索引。

优点：查询速度快，查找到索引就能拿到数据

缺点：

1. 依赖于有序数据
2. 更新代价大
   1. 修改数据，需要修改对应的索引

**非聚集索引**：索引和数据分开，二级索引就是非聚集索引

优点：

1. 更新代价小

缺点：

1. 可能需要回表

**覆盖索引**：一个索引覆盖所需要查询的字段的值



### 事务

概念：逻辑上的一组操作，要么都执行，要么都不执行

ACID

A 原子性

C 一致性

I  隔离性

D 持久性

开启事务的方式

```sql
// method 1
BEGIN;  // or START TRANSACTION
SELECT ...;
UPDATE ...;
COMMIT;
// method 2
SET AUTOCOMMIT = 0;
```



### 锁

* 表锁 & 行锁
* 共享锁 & 排他锁
* 意向锁 

* 行锁
* 间隙锁
* 临键锁（Next-key Lock)

### 日志

redo log

属于物理日志，记录”在某个数据页上做了什么修改“，属于InnoDB存储引擎

bin log

* 属于逻辑日志，记录sql语句，属于MySQL Server层

* 功能：数据备份

* 记录格式：statement，row，mixed

undo log

* 发生异常时，对已经执行的操作进行回滚
* 用来保证事务的原子性





**drop, delete, truncate 区别**

* drop 丢弃数据，直接将表删掉
* truncate 清空数据，只删除表中的数据，再插入数据时自增长id又从1开始，在清空表中的数据时候使用
* delete 删除数据，如果不加where，等同于truncate
  * delete from tableName where column=columnName

## 计算机网络

OSI 七层模型

* 应用层
* 表示层
* 会话层
* 传输层
* 网络层
* 数据链路层
* 物理层

TCP/IP 四层模型

* 应用层
  * HTTP, DNS, FTP, Telnet, DHCP, SMTP, POP3, IMAP, SSH
* 传输层
  * TCP, UDP
* 网络层
  * IP, ARP, ICMP, ARP, RIP, OSPF, BGP, NAT
* 网络接口层
  * MAC, CSMA/CD



HTTP 常见状态码

2XX （成功状态码）

* 200 OK：请求被成功处理
* 201 Created
* 202 Accepted
* 203 No Content

3XX （重定向状态码）

* 301 Moved Permanently：资源被永久重定向了
* 302 Found：资源被临时重定向

4XX （客户端错误状态码）

* 400 Bad Request：发送的HTTP请求存在问题
* 401 Unauthorized：未认证
* 403 Forbidden：非法请求
* 404 Not Found：资源未找到
* 409 Conflict

5XX （服务端错误状态码）

* 500 Internal Server Error
* 502 Bad Gateway
* 503 Unavailable



### HTTP 2.0

* 二进制传输
* 头部压缩 （HPACK算法，只发送差异数据）
* 多路复用
* 服务端推送
* 安全性（HTTPS, TLS）

### HTTP 3.0

* QUIC 基于UDP实现





## Zookeeper

应用场景

1. 分布式锁

   通过创建唯一节点获取分布式锁，当获得锁的一方执行完相关代码或者挂掉之后就释放锁

2. 命名服务

   通过Zookeeper的顺序节点生成全局唯一ID

3. 数据发布、订阅

   通过Watcher机制，将数据发布到Zookeeper被监听的节点上，其它机器可通过监听Zookeeper上节点的变化来实现配置的动态更新

如何防止集群脑裂？

Zookeeper 的过半机制，少于等于一半是不能产生leader的

