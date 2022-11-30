---
title: Java 错题集
date: 2022-07-30
tags: Java
categories: 
- 计算机
- Java
---

From 牛客网

1）判断对错。List，Set，Map都继承自继承Collection接口。

A：错。Map是顶层接口。

2）如果运行以上jsp文件，地址栏的内容为

```
<html>  

     <head><title> 跳转  </title> </head> 
​     <body>  
​         <jsp:forward page="index.htm"/>     
​     </body>
 </html> 
```

A: http://127.0.0.1:8080/myjsp/forward.jsp

forward和redirect是最常问的两个问题

* forward，服务器获取跳转页面内容传给用户，用户地址栏不变

* redirect，是服务器向用户发送转向的地址，redirect后地址栏变成新的地址

3) 下面这条语句一共创建了多少个对象：

```
String s="welcome"+"to"+360;
```

A: 1; 因为字符串字面量拼接操作是在Java编译器编译期间就执行了，也就是说编译器编译时，直接把"java"、"and"和"python"这三个字面量进行"+"操作得到一个"javaandpython" 常量，并且直接将这个常量放入字符串池中。

4）ResultSet中记录行的第一列索引为？

A: 1

5) 关于下面的一段代码，以下哪些说法是正确的：

```java
public static void main(String[] args) {
    String a = new String("myString");
    String b = "myString";
    String c = "my" + "String";
    String d = c;
    System.out.print(a == b);
    System.out.print(a == c);
    System.out.print(b == c);
    System.out.print(b == d);
}
```

A: A D ; a 指向堆内存，b, c, d指向常量池。

6) 在J2EE中，使用Servlet过滤器，需要在web.xml中配置（）元素

A: <filter\>, <filter-mapping\>



7) 下面哪段程序能够正确的实现了GBK编码字节流到UTF-8编码字节流的转换：

```java
new String(new byte[]{}, "GBK").getBytes("UTF-8");
```

