---
title: MySQL语法使用
date: 2022-08-06 20:19:57
tags:
- MySQL
categories:
- 计算机
- MySQL
---

[MySQL doc]([MySQL :: MySQL Documentation](https://dev.mysql.com/doc/))

## 数据类型

整型

* tinyint
* smallint
* mediumint
* int
* bigint
* int(M)

浮点型

* float
* double

定点数

* decimal

字符串

* char 定长字符串，速度快，但浪费空间
* varchar 变长字符串，速度慢，但节省空间

* blob 二进制字符串，字节字符串
* text 字符字符串
* binary
* varbinary

日期时间类型

* datetime 8B
* date 3B
* timestamp 4B
* time 3B
* year 1B

枚举

* enum
* set

## 建表规范

```sql
/* 建表规范 */ ------------------
    -- Normal Format, NF
    - 每个表保存一个实体信息
    - 每个具有一个ID字段作为主键
    - ID主键 + 原子表
    -- 1NF, 第一范式
    字段不能再分，就满足第一范式。
    -- 2NF, 第二范式
    满足第一范式的前提下，不能出现部分依赖。
    消除复合主键就可以避免部分依赖。增加单列关键字。
    -- 3NF, 第三范式
    满足第二范式的前提下，不能出现传递依赖。
    某个字段依赖于主键，而有其他字段依赖于该字段。这就是传递依赖。
    将一个实体信息的数据放在一个表内实现。
```



## 索引

一种数据结构，它为**表中**的**行**提供快速查找功能，通常通过形成表示特定列或一组**列**的所有值的树结构（**B 树）。**

`InnoDB`表始终具有表示**主键**的**聚集索引**。它们还可以在一列或多列上定义一个或多个**二级索引**。根据其结构，二级索引可以分类为**部分**索引、**列**索引或**复合**索引。

索引是**查询**性能的一个关键方面。数据库架构师设计表、查询和索引，以允许快速查找应用程序所需的数据。理想的数据库设计在可行的情况下使用**覆盖索引**;查询结果完全从索引计算，而不读取实际的表数据。每个**外键**约束还需要一个索引，以有效地检查**父**表和**子**表中是否存在值。

尽管 B 树索引是最常见的，但**哈希索引**使用不同类型的数据结构，如在存储引擎和**自适应哈希索引**中。**R 树**索引用于多维信息的空间索引。



覆盖索引：就是包含了所有查询字段 (where,select,order by,group by 包含的字段) 的索引



## 锁

意向锁的东东来快速判断是否可以对某个表使用表锁。

- **意向共享锁（Intention Shared Lock，IS 锁）**：事务有意向对表中的某些加共享锁（S 锁），加共享锁前必须先取得该表的 IS 锁。
- **意向排他锁（Intention Exclusive Lock，IX 锁）**：事务有意向对表中的某些记录加排他锁（X 锁），加排他锁之前必须先取得该表的 IX 锁。

三种行锁

- **记录锁（Record Lock）** ：也被称为记录锁，属于单个行记录上的锁。
- **间隙锁（Gap Lock）** ：锁定一个范围，不包括记录本身。
- **临键锁（Next-key Lock）** ：Record Lock+Gap Lock，锁定一个范围，包含记录本身。记录锁只能锁住已经存在的记录，为了避免插入新记录，需要依赖间隙锁。

InnoDB 的默认隔离级别 RR（可重读）是可以解决幻读问题发生的，主要有下面两种情况：

- **快照读**（一致性非锁定读） ：由 MVCC 机制来保证不出现幻读。
- **当前读** （一致性锁定读）： 使用 Next-Key Lock 进行加锁来保证不出现幻读。







#### [176. 第二高的薪水](https://leetcode.cn/problems/second-highest-salary/)

* ifnull
* limit
* offset

```sql
select ifnull(
    (select distinct salary
    from employee
    order by salary desc
    limit 1 offset 1), null) SecondHighestSalary;
```

#### [184. 部门工资最高的员工](https://leetcode.cn/problems/department-highest-salary/)

* 元组in

```sql
select d.name Department, e.name Employee, e.salary Salary
from employee e
left join Department d on e.departmentId=d.id
where (e.departmentId, e.salary) in (
    select departmentId, max(salary)
    from employee
    group by departmentId
    order by salary
);
```



```sql
# 监控
show engine innodb status;
# 查看表属性
show table status from shop like 't%' \G;
```

基本用法

```sql
show databases;
use database;
show tables;
```



1. 创建InnoDB表

   ```sql
   create table t1 (id int auto_increment, name char(20), primary key(id)) engine=InnoDB;
   ```

   

2. 