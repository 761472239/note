# Spring Boot 之 spring.factories

## 目录

*   [一、创建配置文件](#一创建配置文件)

*   [二、使用bean](#二使用bean)

## 一、创建配置文件

META-INF/spring.factories

```.properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=com.example.Person
```

如果是多行

```.properties
# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.jiuqi.nr.dataentry.config.DataEntryConfig,\
com.jiuqi.nr.dataentry.config.DataEntryPlugInUnitConfig,\
com.jiuqi.nr.dataentry.config.InterceptorConfig
```

这个时候已经将bean注入到了容器中

## 二、使用bean

在组件中直接使用

```java
@Autowired
private Person person;
```
