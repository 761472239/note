# JsonIgnoreProperties和JsonIgnore

## 目录

*   [问题](#问题)

*   [解决问题](#解决问题)

## 问题

springboot项目中定义了很多类，我们在rest返回中直接返回或者在返回对象中使用这些类，spring已经使用jackson自动帮我们完成这些的to json。

但是有时候自动转的json内容太多，或者格式不符合我们的期望，因此需要调整类的to json过程，或者说希望自定义类的json过程。

前端后端的传入对象的属性不一致的情况下使用。

## 解决问题

`@JsonIgnoreProperties `、`@JsonIgnore` 、`@JsonFormat`。

*   @JsonIgnore  忽略一个字段，可以用在变量或者Getter方法上，用在Setter方法时，和变量效果一样。这个注解一般用在我们要忽略的字段上。

*   @JsonIgnoreProperties 忽略一堆字段，将这个注解写在类上之后，就会忽略类中不存在的字段。

    *   `@JsonIgnoreProperties(ignoreUnknown = true)`

    *   这个注解还可以指定要忽略的字段，例如 `@JsonIgnoreProperties({ “password”, “secretKey” })`

*   @JsonFormat 格式转换，例如对于Date类型字段，如果不适用JsonFormat默认在rest返回的是long，如果我们使用`@JsonFormat(timezone = “GMT+8”, pattern = “yyyy-MM-dd HH:mm:ss”)`，就返回`"2018-11-16 22:58:15"`

> **官方文档：**[**JsonIgnoreProperties (Jackson-annotations 2.6.0 API)**](https://fasterxml.github.io/jackson-annotations/javadoc/2.6/com/fasterxml/jackson/annotation/JsonIgnoreProperties.html "JsonIgnoreProperties (Jackson-annotations 2.6.0 API)")

```java
@Data
@JsonIgnoreProperties(value = {"fullName", "comment"})
public class User {
    private String id;
    private String name;
    private String fullName;
    private String comment;
    private String mail;

    @JsonIgnore
    private String address;

    @JsonFormat(timezone = "GMT+8", pattern = "yyyy-MM-dd HH:mm:ss")
    private Date regDate;

    private Date reg2Date;
}
```

[参考博客：Spring Boot程序中@JsonIgnoreProperties与@JsonIgnore基本使用\_russle的专栏-CSDN博客\_jsonignoreproperties](https://blog.csdn.net/russle/article/details/84147236 "参考博客：Spring Boot程序中@JsonIgnoreProperties与@JsonIgnore基本使用_russle的专栏-CSDN博客_jsonignoreproperties")
