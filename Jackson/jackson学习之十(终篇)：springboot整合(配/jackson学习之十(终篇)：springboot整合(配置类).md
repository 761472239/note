# jackson学习之十(终篇)：springboot整合(配置类)

## 目录

*   [开始实战](#开始实战)

## 开始实战

1）pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <artifactId>jacksondemo</artifactId>
        <groupId>com.bolingcavalry</groupId>
        <version>1.0-SNAPSHOT</version>
        <relativePath>../pom.xml</relativePath>
    </parent>
    <groupId>com.bolingcavalry</groupId>
    <artifactId>springbootconfigbean</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>springbootconfigbean</name>
    <description>Demo project for Spring Boot with Jackson, configuration from config bean</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <!--不用spring-boot-starter-parent作为parent时的配置-->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.3.3.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <!-- swagger依赖 -->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
        </dependency>
        <!-- swagger-ui -->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>


```

2）配置类JacksonConfig.java，需要ConditionalOnMissingBean注解避免冲突，另外还给实例指定了名称customizeObjectMapper，如果应用中通过Autowired使用此实例，需要指定这个名字，避免报错`"There is more than one bean of 'ObjectMapper' type"`：

```java
@Configuration
public class JacksonConfig {

    @Bean("customizeObjectMapper")
    @Primary
    @ConditionalOnMissingBean(ObjectMapper.class)
    public ObjectMapper getObjectMapper(Jackson2ObjectMapperBuilder builder) {
        ObjectMapper mapper = builder.build();

        // 日期格式
        mapper.setDateFormat(new SimpleDateFormat("yyyy-MM-dd hh:mm:ss"));

        // 美化输出
        mapper.enable(SerializationFeature.INDENT_OUTPUT);

        return mapper;
    }
}

```

3）使用配置好的mapper

```java
@Qualifier("customizeObjectMapper")
@Autowired
ObjectMapper mapper;
```
