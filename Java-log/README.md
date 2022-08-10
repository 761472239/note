# Java-log

# 日志框架

## 日志框架的作用

1. 控制日志输出的内容和格式。

2. 控制日志输出的位置。

3. 日志文件相关的优化，如异步操作、归档、压缩..

4. 日志系统的维护

5. 面向接口开发 – 日志的门面

## 流行的日志框架

- JUL 
    - java原生日志框架（java util logging）

- Log4j
    - Apache的一个开源项目
- LogBack
    - 由Log4j之父做的另一个开源项目，可靠灵活
- Log4j2
    - Log4j官方的第二个版本，各个方面都是与Logback及其相似，具有插件式结构、配置文件优化等特征，Spring Boot1.4版本以后就不再支持log4j，所以第二个版本营运而生

- JCL
- SLF4J

## 日志门面和日志框架的区别

1. 日志框架技术 JUL、Logback、Log4j、Log4j2

> 用来方便有效地记录日志信息

 

2. 日志门面技术 JCL、SLF4j

为什么要使用日志门面技术：

> 每一种日志框架都有自己单独的API，要使用对应的框架就要使用对应的API，这就大大的增加了应用程序代码对于日志框架的耦合性。
>
> 我们使用了日志门面技术之后，对于应用程序来说，无论底层的日志框架如何改变，应用程序不需要修改任意一行代码，就可以直接上线了。



## 学习博客

1. [Log.properties配置详解 - 小写K - 博客园](https://www.cnblogs.com/lowerCaseK/p/Log_properties.html)

## 学习笔记

1. JUL

2. [Log4j的学习和使用](Log4j%E7%9A%84%E5%AD%A6%E4%B9%A0%E5%92%8C%E4%BD%BF%E7%94%A8/Log4j%E7%9A%84%E5%AD%A6%E4%B9%A0%E5%92%8C%E4%BD%BF%E7%94%A8.md)
3. JCL
4. [SLF4J的学习和使用](SLF4J%E7%9A%84%E5%AD%A6%E4%B9%A0%E5%92%8C%E4%BD%BF%E7%94%A8/SLF4J%E7%9A%84%E5%AD%A6%E4%B9%A0%E5%92%8C%E4%BD%BF%E7%94%A8.md)

2. [LOG4J2](LOG4J2/LOG4J2.md)

6. [Logback](Logback学习笔记.md)
7. [SpringBoot日志框架](SpringBoot日志.md)

## 项目地址
[java-log](../../../../java-log)
