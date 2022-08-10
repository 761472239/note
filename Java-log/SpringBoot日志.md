## 入门案例

SpringBoot日志的级别，默认是Info

logback的风格输出（默认使用的是logback的日志实现）

Spring配置文件

```properties
logging.level.com.bjpowernode=trace
logging.pattern.console=%d{yyyy-MM-dd} [%level] -%m%n
# 日志输出的目录
logging.file.path=D:/test/springbootlog
```



```
如果是需要配置日志拆分等相对高级的功能
            那么application.properties就达不到需要了
            需要使用日志实现相应的配置文件
 
            例如我们现在使用的是logback日志实现
            那么就需要在类路径resources下，配置logback.xml
 
            由于log4j2性能的强大
            当今市场上越来越多的项目选择使用slf4j+log4j2的组合
            springboot默认使用的是slf4j+logback的组合
            我们可以将默认的logback替换成为log4j2
 
            1.启动器依赖，间接的依赖logback
                所以需要将之前的环境中，logback的依赖去除掉
            2.添加log4j2依赖
            3.将log4j2的配置文件log4j2.xml导入到类路径resources下面
```

