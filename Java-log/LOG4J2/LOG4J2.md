# LOG4J2

## Log4j2简介

Apache Log4j 2是对Log4j的升级，它比其前身Log4j 1.x提供了重大改进，并提供了Logback中可用的许多改进，同时修复了Logback架构中的一些问题。被誉为是目前最优秀的Java日志框架。

**SLF4j + Log4j2 的组合，是市场上最强大的日志功能实现方式，绝对是未来的主流趋势。**

## 入门案例

引入依赖：

log4j 2.x版本(核心依赖)

```groovy
implementation group: 'org.apache.logging.log4j', name: 'log4j-api', version: '2.17.1'
implementation group: 'org.apache.logging.log4j', name: 'log4j-core', version: '2.17.1'
```

## 配置SLF4J使用

入门案例：

- 单纯的使用Log4j2 就是 门面+实现；

- Log4j2和Log4j提供了相同的日志级别输出；

配置文件：

- Log4j2是参考logback，配置文件也是xml格式，默认加载类路径下（resources）的log4j2.xml文件。

根标签：

- `<configuration> </configuration>`

日志框架本身的相关设置：

- 在根标签加入  status="debug"，设置日志框架本身的输出级别；

- monitorInterval="5" ，自动加载配置文件的间隔时间，不低于5s；

- ` <configuration status="debug" monitorInterval="5"> </configuration>`

虽然log4j2也是日志门面，但是现在市场的主流趋势仍然是slf4j

所以我们仍然需要使用slf4j作为日志门面，搭配log4j2强大的日志实现功能，进行日志的相关操作

接下来我们配置的就是当今市场上的最强大，最主流的日志使用搭配方式：

slf4j+log4j2：

1. 导入slf4j的日志门面
2. 导入log4j2的适配器
3. 导入log4j2的日志门面
4. 导入log4j2的日志实现

执行原理：
slf4j门面调用的是log4j2的门面，再由log4j2的门面调用log4j2的实现

```groovy
// slf4j的日志门面
implementation 'org.slf4j:slf4j-api:1.7.36'
// slf4j和
log4j的适配器
implementation 'org.apache.logging.log4j:log4j-slf4j-impl:2.17.1'
// log4j2的日志门面
implementation 'org.apache.logging.log4j:log4j-api:2.17.1'
// log4j2的实现
implementation 'org.apache.logging.log4j:log4j-core:2.17.1'

implementation 'com.lmax:disruptor:3.4.2'
```

## 配置文件log4j2.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<configuration>
    <properties>
        <property name="logDir">/Users/rsy</property>
    </properties>
    <Appenders>
        <Console name="AConsole" target="SYSTEM_ERR"></Console>
        <File name="fileAppender" fileName="${logDir}/log4j2.log">
            <PatternLayout pattern="[%-5level] %d{yyyy-MM-dd HH:mm:ss.SSS} %m%n"></PatternLayout>
        </File>
        <!--
            按照指定规则来拆分日志文件

            fileName:日志文件的名字
            filePattern：日志文件拆分后文件的命名规则
                        $${date:yyyy-MM-dd}：根据日期当天，创建一个文件夹
                                    例如：2021-01-01这个文件夹中，记录当天的所有日志信息（拆分出来的日志放在这个文件夹中）
                                          2021-01-02这个文件夹中，记录当天的所有日志信息（拆分出来的日志放在这个文件夹中）
                        roll_log-%d{yyyy-MM-dd-HH-mm}-%i.log
                        为文件命名的规则：%i表示序号，从0开始，目的是为了让每一份文件名字不会重复
        -->
        <RollingFile name="rollingFile" fileName="${logDir}/roll_log.log"
                     filePattern="${logDir}/$${date:yyyy-MM-dd}/roll_log-%d{yyyy-MM-dd-HH-mm}-%i.log">

            <!-- 日志消息格式 -->
            <PatternLayout
                    pattern="[%-5level] %d{yyyy-MM-dd HH:mm:ss.SSS} %m%n"/>

            <Policies>

                <!-- 在系统启动时，触发拆分规则，产生一个日志文件 -->
                <OnStartupTriggeringPolicy/>

                <!-- 按照文件的大小进行拆分 -->
                <SizeBasedTriggeringPolicy size="10KB"/>

                <!-- 按照时间节点进行拆分 拆分规则就是filePattern-->
                <TimeBasedTriggeringPolicy/>

            </Policies>

            <!-- 在同一目录下，文件的个数限制，如果超出了设置的数值，则根据时间进行覆盖，新的覆盖旧的规则-->
            <DefaultRolloverStrategy max="30"/>

        </RollingFile>
        <!-- 配置异步日志 -->
<!--        <Async name="myAsync">-->
<!--            &lt;!&ndash; 将控制台输出做异步的操作 &ndash;&gt;-->
<!--            <AppenderRef ref="AConsole"/>-->
<!--        </Async>-->
        
    </Appenders>
    <Loggers>
        <!-- 自定义logger，让自定义的logger为异步logger -->
        <!--
 
            includeLocation="false"
            表示去除日志记录中的行号信息，这个行号信息非常的影响日志记录的效率（生产中都不加这个行号）
            严重的时候可能记录的比同步的日志效率还有低
 
            additivity="false"
            表示不继承rootlogger
 
        -->
        <!-- 只有在这个包结构下的文件，打印的日志才会异步输出 -->
        <AsyncLogger name="com.rty" level="trace"
                     includeLocation="false" additivity="false">
            <!-- 将控制台输出consoleAppender，设置为异步打印 -->
            <AppenderRef ref="AConsole"/>
        </AsyncLogger>
        <Root level="trace">
            <!--<AppenderRef ref="AConsole"></AppenderRef>-->
<!--            <AppenderRef ref="myAsync"></AppenderRef>-->
            <AppenderRef ref="AConsole"></AppenderRef>
        </Root>
    </Loggers>
</configuration>
```

## 其他相关

[log4j2.xml配置日志写入数据库_moon-CSDN博客_log4j2 数据库](https://blog.csdn.net/qq_23543983/article/details/80687643?spm=1001.2101.3001.6650.5&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-5.pc_relevant_aa&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-5.pc_relevant_aa&utm_relevant_index=9)
