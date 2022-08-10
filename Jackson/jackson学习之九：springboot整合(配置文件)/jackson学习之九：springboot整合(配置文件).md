# jackson学习之九：springboot整合(配置文件)

## 目录

*   [开始实战](#开始实战)

*   [添加配置文件](#添加配置文件)

## 开始实战

1）因为该项目工程为jacksondemo子工程，pom.xml配置如下，需要注意的是parent不能使用spring-boot-starter-parent，而是通过dependencyManagement节点来引入springboot依赖：

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
    <artifactId>springbootproperties</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>springbootproperties</name>
    <description>Demo project for Spring Boot</description>

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

2）启动类

```java
package com.bolingcavalry.springbootproperties;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SpringbootpropertiesApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootpropertiesApplication.class, args);
    }
}

```

3）swagger

```java
package com.bolingcavalry.springbootproperties;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.service.Contact;
import springfox.documentation.service.Tag;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration
@EnableSwagger2
public class SwaggerConfig {

    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .tags(new Tag("JsonPropertySerializationController", "JsonProperty相关测试"))
                .select()
                // 当前包路径
                .apis(RequestHandlerSelectors.basePackage("com.bolingcavalry.springbootproperties.controller"))
                .paths(PathSelectors.any())
                .build();
    }

    //构建 api文档的详细信息函数,注意这里的注解引用的是哪个
    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                //页面标题
                .title("SpringBoot整合Jackson(基于配置文件)")
                //创建人
                .contact(new Contact("程序员欣宸", "https://github.com/zq2599/blog_demos", "zq2599@gmail.com"))
                //版本号
                .version("1.0")
                //描述
                .description("API 描述")
                .build();
    }
}

```

4）序列化和反序列化用到的Bean类，可见使用了JsonProperty属性来设置序列化和反序列化时的json属性名，field0字段刻意没有get方法，是为了验证JsonProperty的序列化能力：

```java
package com.bolingcavalry.springbootproperties.bean;

import com.fasterxml.jackson.annotation.JsonProperty;
import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;

import java.util.Date;

@ApiModel(description = "JsonProperty注解测试类")
public class Test {

    @ApiModelProperty(value = "私有成员变量")
    @JsonProperty(value = "json_field0", index = 1)
    private Date field0 = new Date();

    public void setField0(Date field0) {
        this.field0 = field0;
    }

    @ApiModelProperty(value = "来自get方法的字符串")
    @JsonProperty(value = "json_field1", index = 0)
    public String getField1() {
        return "111";
    }

    @Override
    public String toString() {
        return "Test{" +
                "field0=" + field0 +
                '}';
    }
}

```

5）测试用的Controller代码如下，很简单只有两个接口，serialization返回序列化结果，deserialization接受客户端请求参数，反序列化成实例，通过toString()来检查反序列化的结果，另外，还通过Autowired注解从spring容器中将ObjectMapper实例直接拿来用：

```java
@RestController
@RequestMapping("/jsonproperty")
@Api(tags = {"JsonPropertySerializationController"})
public class JsonPropertySerializationController {

    private static final Logger logger = LoggerFactory.getLogger(JsonPropertySerializationController.class);

    @Autowired
    ObjectMapper mapper;

    @ApiOperation(value = "测试序列化", notes = "测试序列化")
    @RequestMapping(value = "/serialization", method = RequestMethod.GET)
    public Test serialization() throws JsonProcessingException {

        Test test = new Test();
        logger.info(mapper.writeValueAsString(test));

        return test;
    }

    @ApiOperation(value = "测试反序列化", notes="测试反序列化")
    @RequestMapping(value = "/deserialization",method = RequestMethod.PUT)
    public String deserialization(@RequestBody Test test) {
        return test.toString();
    }
}

```

## 添加配置文件

1）application.yml

```yaml
spring:
  jackson:
    # 日期格式化
    date-format: yyyy-MM-dd HH:mm:ss
    # 序列化相关
    serialization:
      # 格式化输出
      indent_output: true
      # 忽略无法转换的对象
      fail_on_empty_beans: true
    # 反序列化相关
    deserialization:
      # 解析json时，遇到不存在的属性就忽略
      fail_on_unknown_properties: false
    # 设置空如何序列化
    defaultPropertyInclusion: NON_EMPTY
    parser:
      # 允许特殊和转义符
      allow_unquoted_control_chars: true
      # 允许单引号
      allow_single_quotes: true

```
