## Logback简介

Logback是由`log4j`创始人设计的又一个开源日志组件。

Logback当前分成三个模块：`logback-core`,`logback- classic`和`logback-access`。

logback-core是其它两个模块的基础模块。

logback-classic是log4j的一个改良版本。

此外logback-classic完整实现SLF4J API。使你可以很方便地更换成其它日志系统如log4j或JDK14 Logging。

logback-access访问模块与Servlet容器集成提供通过Http来访问日志的功能。

## Logback中的组件

- Logger：日志的记录器，主要用于存放日志对象，也可以定义日志类型、级别。
- Appender：用于指定日志输出的目的地，目的地可以是控制台、文件、数据库等等。 
- Layout：负责把事件转化为字符串，格式化日志信息的输出。
    - 在Logback中Layout对象被封装在encoder中
    - 使用的encoder其实就是Layout

## Logback的配置文件

Logback提供了3种配置文件

logback.groovy 

logback-test.xml 

logback.xml

如果都不存在则采用默认的配置

## Logback的日志输出格式

日志输出格式：
  `%-10level`  级别 案例为设置10个字符，左对齐
  `%d{yyyy-MM-dd HH:mm:ss.SSS}` 日期
  `%c`  当前类全限定名
  `%M`  当前执行日志的方法
  `%L`  行号
  `%thread` 线程名称
  `%m`或者`%msg`   信息
  `%n`  换行

## 案例实现

- 引入依赖

```groovy
implementation group: 'org.slf4j', name: 'slf4j-api', version: '1.7.36'
testImplementation group: 'ch.qos.logback', name: 'logback-classic', version: '1.2.10'
```

实际使用的还是slf4j

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
```

## 日志级别

Logback有5种日志级别：

trace < debug < info < warn < error

默认是debug，所以trace的信息不会输出。

## 配置文件

详细的配置说明：[初探Logback：学会看懂Logback配置文件 - 云+社区 - 腾讯云](https://cloud.tencent.com/developer/article/1532105)

Logback配置文件的使用，在resources下面，创建一份配置文件，命名为logback.xml

一切配置都是在根标签中进行操作的

```xml
<configuration>
</configuration>
```

通用属性

```xml
<property name="" value=""></property>
<property name="pattern" value="[%-5level] %d{yyyy-MM-dd HH:mm:ss} %c %M %L %thread %m%n"></property>
<property name="pattern1" value="[%-5level]%d{yyyy-MM-dd HH:mm:ss.SSS}%c%M%L%thread%m%n"></property>
```

所谓配置文件中的通用属性是为了让接下来的配置更加方便引用

通过以`${name}`的形式，方便的取得value值，通过取得的value值可以做文件的其他配置而使用

配置文件Appender

```xml
<!-- 配置文件的appender 普通文件-->
<appender name="fileAppender" class="ch.qos.logback.core.FileAppender">
  <!-- 引入文件位置 -->
  <file>${logDir}/logback.log</file>
  <!-- 设置输出格式 -->
  <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
    <pattern>${pattern}</pattern>
  </encoder>
</appender>
```

配置ConsoleAppender

```xml
<!-- 配置控制台appender -->
<appender name="consoleAppender"
          class="ch.qos.logback.core.ConsoleAppender">
  <!--
            表示对于日志输出目标的配置
            默认：System.out 表示以黑色字体输出日志
            设置：System.err 表示以红色字体输出日志
   -->
  <target>
    System.err
  </target>
  			<!--
            配置日志输出格式
            手动配置格式的方式
            直接引入上述的通用属性即可
        -->
  <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
    <!-- 格式引用通用属性配置 -->
    <pattern>${pattern}</pattern>
  </encoder>
</appender>
```

配置HtmlAppender

```xml
<!-- 配置文件的appender html文件 -->
<appender name="htmlFileAppender" class="ch.qos.logback.core.FileAppender">

  <file>${logDir}/logback.html</file>
  <encoder class="ch.qos.logback.core.encoder.LayoutWrappingEncoder">
    <layout class="ch.qos.logback.classic.html.HTMLLayout">
      <pattern>${pattern1}</pattern>
    </layout>
  </encoder>

</appender>
```

配置RollingFileAppender

```xml
<!-- 配置文件的appender 可拆分归档的文件 -->
<appender name="roll" class="ch.qos.logback.core.rolling.RollingFileAppender">

  <!-- 输入格式 -->
  <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
    <pattern>${pattern}</pattern>
  </encoder>
  <!-- 引入文件位置 -->
  <file>${logDir}/roll_logback.log</file>

  <!-- 指定拆分规则 -->
  <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">

    <!-- 按照时间和压缩格式声明文件名 压缩格式gz -->
    <fileNamePattern>${logDir}/roll.%d{yyyy-MM-dd}.log%i.gz
    </fileNamePattern>

    <!-- 按照文件大小来进行拆分 -->
    <maxFileSize>1KB</maxFileSize>

  </rollingPolicy>

</appender>
```

使用过滤器，配置ConsoleAppender

```xml
<!-- 配置控制台的appender 使用过滤器 -->
<appender name="consoleFilterAppender" class="ch.qos.logback.core.ConsoleAppender">

  <target>
    System.err
  </target>

  <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
    <pattern>${pattern}</pattern>
  </encoder>

  <!-- 配置过滤器 -->
  <filter class="ch.qos.logback.classic.filter.LevelFilter">

    <!-- 设置日志的输出级别 -->
    <level>ERROR</level>

    <!-- 高于level中设置的级别，则打印日志 -->
    <onMatch>ACCEPT</onMatch>

    <!-- 低于level中设置的级别，则屏蔽日志 -->
    <onMismatch>DENY</onMismatch>
    
  </filter>

</appender>
```

配置异步任务

```xml
<!-- 配置异步日志 -->
<appender name="asyncAppender" class="ch.qos.logback.classic.AsyncAppender">
  <appender-ref ref="consoleAppender"/>
</appender>
```

配置logger和rooter标签

```xml
<!--
        日志记录器
        配置root logger
        level：配置日志级别

        可以同时配置多个appender，做日志的多方向输出
    -->
<root level="ALL">
  <!-- 引入appender -->
  <!--<appender-ref ref="roll"/>-->
  <!--<appender-ref ref="consoleFilterAppender"/>-->
  <!--<appender-ref ref="consoleAppender"/>-->
  <appender-ref ref="asyncAppender"/>
</root>

<!--
        additivity="false"
        表示不继承rootlogger，这种用法只针对某个目录下的日志输出
    -->
<logger name="com.bjpowernode" level="info" additivity="false">
  <!-- 在自定义logger中配置appender -->
  <appender-ref ref="consoleAppender"/>
</logger>
```

