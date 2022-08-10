# SLF4J的学习和使用

## 入门案例

引入依赖

```groovy
// 核心依赖
implementation group: 'org.slf4j', name: 'slf4j-api', version: '1.7.36'
// SLF4J 自带的简单实现
testImplementation group: 'org.slf4j', name: 'slf4j-simple', version: '1.7.36'
```

```
在没有任何其他日志实现框架集成的基础之上
slf4j使用的就是自带的框架slf4j-simple
slf4j-simple也必须以单独依赖的形式导入进来
```

在没有其他日志实现框架集成的基础之上，slf4j使用的是自带的`slf4j-simpl`，`slf4j-simpl`也必须要引入。

```java
如果不导入这个simple，就会报错
// SLF4J: No SLF4J providers were found.
// SLF4J: Defaulting to no-operation (NOP) logger implementation
// SLF4J: See http://www.slf4j.org/codes.html#noProviders for further details.
```

```java
import org.junit.jupiter.api.Test;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Test02 {
    @Test
    public void test() {
        Logger logger = LoggerFactory.getLogger(Test02.class);
        logger.trace("trace信息");
        logger.debug("debug信息");
        logger.info("info信息");
        logger.warn("warn信息");
        logger.error("error信息");
    }
}
```

> 注意这里引入的是`org.slf4j.*`

## 使用占位符

```java
logger.info("学生信息-姓名："+name+"；年龄："+age);
logger.info("学生信息-姓名：{}，年龄：{}",new Object[]{name,age});
logger.info("学生信息-姓名：{}，年龄：{}",name,age);
```

## 异常信息的处理

```java
logger.info("XXX类中的XXX方法出现了异常，请及时关注信息");
//e是引用类型对象，不能根前面的{}做有效的字符串拼接
logger.info("具体错误是：{}",e);
//我们不用加{},直接后面加上异常对象e即可
logger.info("具体错误是：",e);
```

## 绑定其他日志框架

![img](image/concrete-bindings-20220224112110846.png)

集成其他日志实现之前
观察官网图

SLF4J日志门面，共有3种情况对日志实现进行绑定

1. 在没有绑定任何日志实现的基础之上，日志是不能够绑定实现任何功能的
        值得大家注意的是，通过我们刚刚的演示，slf4j-simple是slf4j官方提供的
        使用的时候，也是需要导入依赖，自动绑定到slf4j门面上
        如果不导入，slf4j 核心依赖是不提供任何实现的
2. logback和simple（包括nop）
        都是slf4j门面时间线后面提供的日志实现，所以API完全遵循slf4j进行的设计
        那么我们只需要导入想要使用的日志实现依赖，即可与slf4j无缝衔接
        值得一提的是nop虽然也划分到实现中了，但是他是指不实现日志记录（后续课程）
3. log4j和JUL
        都是slf4j门面时间线前面的日志实现，所以API不遵循slf4j进行设计
        通过适配桥接的技术，完成的与日志门面的衔接

## 绑定logback

**试着将logback日志框架集成进来**

```groovy
implementation group: 'org.slf4j', name: 'slf4j-api', version: '1.7.36'
//    testImplementation group: 'ch.qos.logback', name: 'logback-classic', version: '1.3.0-alpha14'
```



测试1：
    在原有slf4j-simple日志实现的基础上，又集成了logback
    通过测试，日志是打印出来了 java.lang.ClassNotFoundException: aaa
    通过这一句可以发现SLF4J: Actual binding is of type [org.slf4j.impl.SimpleLoggerFactory]
    虽然集成了logback，但是我们现在使用的仍然是slf4j-simple
    事实上只要出现了这个提示
    SLF4J: Class path contains multiple SLF4J bindings.
    在slf4j环境下，证明同时出现了多个日志实现
    如果先导入logback依赖，后导入slf4j-simple依赖
    那么默认使用的就是logback依赖
    如果有多个日志实现的话，默认使用先导入的实现

测试2：
    将slf4j-simple注释掉
    只留下logback，那么slf4j门面使用的就是logback日志实现
    值得一提的是，这一次没有多余的提示信息
    所以在实际应用的时候，我们一般情况下，仅仅只是做一种日志实现的集成就可以了

通过这个集成测试，我们会发现虽然底层的日志实现变了，但是源代码完全没有改变
这就是日志门面给我们带来最大的好处
在底层真实记录日志的时候，不需要应用去做任何的了解
应用只需要去记slf4j的API就可以了
值得一提的是，我们虽然底层使用的是log4j做的打印，但是从当前代码使用来看
我们其实使用的仍然是slf4j日志门面，至于日志是log4j打印的（或者是logback打印的）
都是由slf4j进行操作的。

## 绑定slf4j-nop

​	使用slf4j-nop，表示不记录日志
​	在我们使用slf4j-nop的时候
​	首先还是需要导入实现依赖
​	这个实现依赖，根据我们之前所总结出来的日志日志实现种类的第二种
​	与logback和simple是属于一类的
​	通过集成依赖的顺序而定
​	所以如果想要让nop发挥效果，禁止所有日志的打印
​	那么就必须要将slf4j-nop的依赖放在所有日志实现依赖的上方

```groovy
implementation group: 'org.slf4j', name: 'slf4j-api', version: '1.7.36'
//    testImplementation group: 'org.slf4j', name: 'slf4j-nop', version: '2.0.0-alpha6'
```



## 绑定log4j

由于log4j是在slf4j之前产生的日志框架实现，并没有遵循slf4j的API规范。之前绑定的logback是之后产生的日志实现。所以要绑定log4j，需要绑定的一个适配器`slf4j-log4j12`，然后导入log4j。**(注意别忘了log4j的配置文件)**

```groovy
implementation group: 'org.slf4j', name: 'slf4j-api', version: '1.7.36'
testImplementation group: 'org.slf4j', name: 'slf4j-log4j12', version: '1.7.36'
```

**测试结果：**

> log4j:WARN No appenders could be found for logger (com.bjpowernode.slf4j.test01.SLF4JTest01).
> log4j:WARN Please initialize the log4j system properly.
> log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.

虽然日志信息没有打印出来，那么根据警告信息可以得出：

使用了log4j日志实现框架

提示appender没有加载，需要在执行日志之前做相应的加载工作（初始化）

我们可以将log4j的配置文件导入使用

测试结果为log4j的日志打印，而且格式和级别完全是遵循log4j的配置文件进行的输出

## log4j项目替换为slf4j+logback的组合

需求：

假设我们项目一直以来使用的是log4j日志框架，但是随着技术和需求的更新换代，log4j已然不能够满足我们系统的需求，我们现在就需要将系统中的日志实现重构为 slf4j+logback的组合，在不触碰java源代码的情况下，将这个问题给解决掉

1. 首先将所有关于其他日志实现和门面依赖全部去除，仅仅只留下log4j的依赖。测试的过程中，只能使用log4j相关的组件
2. 此时需要将日志替换为slf4j+logback
    我们既然不用log4j了，就将log4j去除
    将slf4j日志门面和logback的日志实现依赖加入进来
    这样做，没有了log4j环境的支持，编译报错

这个时候就需要使用桥接器来做这个需求了
桥接器解决的是项目中日志的重构问题，当前系统中存在之前的日志API，可以通过桥接转换到slf4j的实现

桥接器的使用步骤：

1. 去除之前旧的日志框架依赖
2. 添加slf4j提供的桥接组件

```xml
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>
    log4j相关的桥接器
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>log4j-over-slf4j</artifactId>
        <version>1.7.25</version>
    </dependency>
```

桥接器加入后，代码编译就不报错了。

测试：

日志信息输出	，输出格式为logback
证明了现在使用的确实是slf4j门面+logback实现

在重构之后，就会为我们造成这样一种假象，使用的明明是log4j包下的日志组件资源
但是真正日志的实现，却是使用slf4j门面+logback实现
这就是桥接器给我们带来的效果

注意：

​	在桥接器加入之后，适配器就没有必要加入了
​	桥接器和适配器不能同时导入依赖
​	桥接器如果配置在适配器的上方，则运行报错，不同同时出现
​	桥接器如果配置在适配器的下方，则不会执行桥接器，没有任何的意义

