# jackson学习之二：jackson-core

## 目录

*   [关于jackson-core](#关于jackson-core)

*   [创建父子工程](#创建父子工程)

*   [新增子工程beans](#新增子工程beans)

*   [JsonFactory线程安全吗?](#jsonfactory线程安全吗)

*   [新建子工程core实战](#新建子工程core实战)

## 关于jackson-core

1.  低阶API库，提供流式解析工具JsonParser，流式生成工具JsonGenerator。

2.  日常的序列化和反序列化处理中，最常用的是jackson-annotations和jackson-databind，而jackson-core由于它提供的API过于基础，我们大多数情况下是用不上的。

3.  尽管jackson-databind负责序列化和反序列化处理，但它的底层实现是调用了jackson-core的API；

## 创建父子工程

创建名为jacksondemo的maven工程，这是个父子结构的工程，其pom.xml内容如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <properties>
        <java.version>1.8</java.version>
    </properties>
    <groupId>com.bolingcavalry</groupId>
    <artifactId>jacksondemo</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>
    <modules>
        <module>core</module>
        <module>beans</module>
        <module>databind</module>
    </modules>
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>com.fasterxml.jackson.core</groupId>
                <artifactId>jackson-databind</artifactId>
                <version>2.11.0</version>
                <scope>compile</scope>
            </dependency>
            <dependency>
                <groupId>org.slf4j</groupId>
                <artifactId>slf4j-log4j12</artifactId>
                <version>1.7.25</version>
                <scope>compile</scope>
            </dependency>
            <dependency>
                <groupId>commons-io</groupId>
                <artifactId>commons-io</artifactId>
                <version>2.7</version>
                <scope>compile</scope>
            </dependency>
            <dependency>
                <groupId>org.apache.commons</groupId>
                <artifactId>commons-lang3</artifactId>
                <version>3.10</version>
                <scope>compile</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
</project>

```

## 新增子工程beans

1）在父工程jscksondemo下新增名为beans的子工程，这里面是一些常量和Pojo类。

2）增加定义常量的类Constant.java

```java
package com.bolingcavalry.jacksondemo.beans;

public class Constant {
    /**
     * 该字符串的值是个网络地址，该地址对应的内容是个JSON
     */
    public final static String TEST_JSON_DATA_URL = "https://raw.githubusercontent.com/zq2599/blog_demos/master/files/twitteer_message.json";
    /**
     * 用来验证反序列化的JSON字符串
     */
    public final static String TEST_JSON_STR = "{\n" +
            "  \"id\":1125687077,\n" +
            "  \"text\":\"@stroughtonsmith You need to add a \\\"Favourites\\\" tab to TC/iPhone. Like what TwitterFon did. I can't WAIT for your Twitter App!! :) Any ETA?\",\n" +
            "  \"fromUserId\":855523, \n" +
            "  \"toUserId\":815309,\n" +
            "  \"languageCode\":\"en\"\n" +
            "}";
    /**
     * 用来验证序列化的TwitterEntry实例
     */
    public final static TwitterEntry TEST_OBJECT = new TwitterEntry();
    /**
     * 准备好TEST_OBJECT对象的各个参数
     */
    static {
        TEST_OBJECT.setId(123456L);
        TEST_OBJECT.setFromUserId(101);
        TEST_OBJECT.setToUserId(102);
        TEST_OBJECT.setText("this is a message for serializer test");
        TEST_OBJECT.setLanguageCode("zh");
    }
}

```

3）增加一个Pojo，对应的是一条推特消息。

```java
package com.bolingcavalry.jacksondemo.beans;
/**
 * @Description: 推特消息bean
 * @author: willzhao E-mail: zq2599@gmail.com
 * @date: 2020/7/4 16:24
 */
public class TwitterEntry {
    /**
     * 推特消息id
     */
    long id;
    /**
     * 消息内容
     */
    String text;    /**
     * 消息创建者
     */
    int fromUserId;
    /**
     * 消息接收者
     */
    int toUserId;
    /**
     * 语言类型
     */
    String languageCode;    public long getId() {
        return id;
    }    public void setId(long id) {
        this.id = id;
    }    public String getText() {
        return text;
    }    public void setText(String text) {
        this.text = text;
    }    public int getFromUserId() {
        return fromUserId;
    }    public void setFromUserId(int fromUserId) {
        this.fromUserId = fromUserId;
    }    public int getToUserId() {
        return toUserId;
    }    public void setToUserId(int toUserId) {
        this.toUserId = toUserId;
    }    public String getLanguageCode() {
        return languageCode;
    }    public void setLanguageCode(String languageCode) {
        this.languageCode = languageCode;
    }    public TwitterEntry() {
    }    public String toString() {
        return "[Tweet, id: "+id+", text='"+text+"', from: "+fromUserId+", to: "+toUserId+", lang: "+languageCode+"]";
    }
}

```

4）以上就是准备工作了，接下来开始实战jackson-core

## JsonFactory线程安全吗?

1）JsonFactory是否是线程安全的，这是编码前要弄清楚的问题，因为JsonParser和JsonGenerator的创建都离不开JsonFactory。

2）如下图红框所示，jackson官方文档中明确指出JsonFactory是线程安全的，可以放心的作为全局变量给多线程同时使用。

3）官方文档地址：[http://fasterxml.github.io/jackson-core/javadoc/2.11/](http://fasterxml.github.io/jackson-core/javadoc/2.11/ "http://fasterxml.github.io/jackson-core/javadoc/2.11/")

## 新建子工程core实战

1）新建子工程core，pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>jacksondemo</artifactId>
        <groupId>com.bolingcavalry</groupId>
        <version>1.0-SNAPSHOT</version>
        <relativePath>../pom.xml</relativePath>
    </parent>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.bolingcavalry</groupId>
    <artifactId>core</artifactId>
    <name>core</name>
    <description>Demo project for jackson core use</description>
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>8</source>
                    <target>8</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
    <dependencies>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
        </dependency>
        <dependency>
            <groupId>commons-io</groupId>
            <artifactId>commons-io</artifactId>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
        </dependency>
        <dependency>
            <groupId>com.bolingcavalry</groupId>
            <artifactId>beans</artifactId>
            <version>${project.version}</version>
        </dependency>
    </dependencies>
</project>

```

2）新建StreamingDemo类，这里面是调用jackson-core的API进行序列化和反序列化的所有demo，如下：

**jackson低阶方法的使用**

```java
public class StreamingDemo {

    private static final Logger logger = LoggerFactory.getLogger(StreamingDemo.class);

    JsonFactory jsonFactory = new JsonFactory();

    /**
     * 该字符串的值是个网络地址，该地址对应的内容是个JSON
     */
    final static String TEST_JSON_DATA_URL = "https://raw.githubusercontent.com/zq2599/blog_demos/master/files/twitteer_message.json";

    /**
     * 用来验证反序列化的JSON字符串
     */
    final static String TEST_JSON_STR = "{\n" +
            "  \"id\":1125687077,\n" +
            "  \"text\":\"@stroughtonsmith You need to add a \\\"Favourites\\\" tab to TC/iPhone. Like what TwitterFon did. I can't WAIT for your Twitter App!! :) Any ETA?\",\n" +
            "  \"fromUserId\":855523, \n" +
            "  \"toUserId\":815309,\n" +
            "  \"languageCode\":\"en\"\n" +
            "}";

    /**
     * 用来验证序列化的TwitterEntry实例
     */
    final static TwitterEntry TEST_OBJECT = new TwitterEntry();

    /**
     * 准备好TEST_OBJECT对象的各个参数
     */
    static {
        TEST_OBJECT.setId(123456L);
        TEST_OBJECT.setFromUserId(101);
        TEST_OBJECT.setToUserId(102);
        TEST_OBJECT.setText("this is a message for serializer test");
        TEST_OBJECT.setLanguageCode("zh");
    }
```

**反序列化测试(JSON -> Object)，入参是JSON字符串**

```java
/**
     * 反序列化测试(JSON -> Object)，入参是JSON字符串
     *
     * @param json JSON字符串
     * @return
     * @throws IOException
     */
    public TwitterEntry deserializeJSONStr(String json) throws IOException {

        JsonParser jsonParser = jsonFactory.createParser(json);

        if (jsonParser.nextToken() != JsonToken.START_OBJECT) {
            jsonParser.close();
            logger.error("起始位置没有大括号");
            throw new IOException("起始位置没有大括号");
        }

        TwitterEntry result = new TwitterEntry();

        try {
            // Iterate over object fields:
            while (jsonParser.nextToken() != JsonToken.END_OBJECT) {

                String fieldName = jsonParser.getCurrentName();

                logger.info("正在解析字段 [{}]", jsonParser.getCurrentName());

                // 解析下一个
                jsonParser.nextToken();

                switch (fieldName) {
                    case "id":
                        result.setId(jsonParser.getLongValue());
                        break;
                    case "text":
                        result.setText(jsonParser.getText());
                        break;
                    case "fromUserId":
                        result.setFromUserId(jsonParser.getIntValue());
                        break;
                    case "toUserId":
                        result.setToUserId(jsonParser.getIntValue());
                        break;
                    case "languageCode":
                        result.setLanguageCode(jsonParser.getText());
                        break;
                    default:
                        logger.error("未知字段 '" + fieldName + "'");
                        throw new IOException("未知字段 '" + fieldName + "'");
                }
            }
        } catch (IOException e) {
            logger.error("反序列化出现异常 :", e);
        } finally {
            jsonParser.close(); // important to close both parser and underlying File reader
        }

        return result;
    }
```

**序列化测试(Object -> JSON)**

```java
/**
     * 序列化测试(Object -> JSON)
     *
     * @param twitterEntry
     * @return 由对象序列化得到的JSON字符串
     */
    public String serialize(TwitterEntry twitterEntry) throws IOException {
        String rlt = null;
        ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
        JsonGenerator jsonGenerator = jsonFactory.createGenerator(byteArrayOutputStream, JsonEncoding.UTF8);

        try {
            jsonGenerator.useDefaultPrettyPrinter();

            jsonGenerator.writeStartObject();
            jsonGenerator.writeNumberField("id", twitterEntry.getId());
            jsonGenerator.writeStringField("text", twitterEntry.getText());
            jsonGenerator.writeNumberField("fromUserId", twitterEntry.getFromUserId());
            jsonGenerator.writeNumberField("toUserId", twitterEntry.getToUserId());
            jsonGenerator.writeStringField("languageCode", twitterEntry.getLanguageCode());
            jsonGenerator.writeEndObject();
        } catch (IOException e) {
            logger.error("序列化出现异常 :", e);
        } finally {
            jsonGenerator.close();
        }

        // 一定要在
        rlt = byteArrayOutputStream.toString();

        return rlt;
    }


```

**最后的测试**

```java
    public static void main(String[] args) throws Exception {

        StreamingDemo streamingDemo = new StreamingDemo();

        // 执行一次对象转JSON操作
        logger.info("********************执行一次对象转JSON操作********************");
        String serializeResult = streamingDemo.serialize(TEST_OBJECT);
        logger.info("序列化结果是JSON字符串 : \n{}\n\n", serializeResult);

        // 用本地字符串执行一次JSON转对象操作
        logger.info("********************执行一次本地JSON反序列化操作********************");
        TwitterEntry deserializeResult = streamingDemo.deserializeJSONStr(TEST_JSON_STR);
        logger.info("\n本地JSON反序列化结果是个java实例 : \n{}\n\n", deserializeResult);

    }
```

**输出日志**

```xml
2021-10-04 13:44:49 INFO  StreamingDemo:184 - ********************执行一次对象转JSON操作********************
2021-10-04 13:44:49 INFO  StreamingDemo:186 - 序列化结果是JSON字符串 : 
{
  "id" : 123456,
  "text" : "this is a message for serializer test",
  "fromUserId" : 101,
  "toUserId" : 102,
  "languageCode" : "zh"
}


2021-10-04 13:44:49 INFO  StreamingDemo:189 - ********************执行一次本地JSON反序列化操作********************
2021-10-04 13:44:49 INFO  StreamingDemo:81 - 正在解析字段 [id]
2021-10-04 13:44:49 INFO  StreamingDemo:81 - 正在解析字段 [text]
2021-10-04 13:44:49 INFO  StreamingDemo:81 - 正在解析字段 [fromUserId]
2021-10-04 13:44:49 INFO  StreamingDemo:81 - 正在解析字段 [toUserId]
2021-10-04 13:44:49 INFO  StreamingDemo:81 - 正在解析字段 [languageCode]
2021-10-04 13:44:49 INFO  StreamingDemo:191 - 
本地JSON反序列化结果是个java实例 : 
[Tweet, id: 1125687077, text='@stroughtonsmith You need to add a "Favourites" tab to TC/iPhone. Like what TwitterFon did. I can't WAIT for your Twitter App!! :) Any ETA?', from: 855523, to: 815309, lang: en]

```

3）上述代码可见JsonParser负责将JSON解析成对象的变量值，核心是循环处理JSON中的所有内容。

4）JsonGenerator负责将对象的变量写入JSON的各个属性，这里是开发者自行决定要处理哪些字段。

5）不论是JsonParser还是JsonGenerator，大家都可以感觉到工作量很大，需要开发者自己动手实现对象和JSON字段的关系映射，实际应用中不需要咱们这样辛苦的编码，jackson的另外两个库(annonation的databind)已经帮我们完成了大量工作，上述代码只是揭示最基础的jackson执行原理。

6）以上就是jackson最底层的工作原理。
