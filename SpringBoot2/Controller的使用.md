## @Controller 处理http请求

```java
@Controller
public class HelloController {

    @RequestMapping(value="/hello",method= RequestMethod.GET)
    public String sayHello(){
        return "hello";
    }
}
```

如果直接使用`@Controller`这个注解，当运行该SpringBoot项目后，在浏览器中输入：local:8080/hello,会得到如下错误提示：  

![image-20210707122008767](https://gitee.com/zzursy/blog-image/raw/master/img/20210707122008.png)

出现这种情况的原因在于：没有使用模版。即@Controller 用来响应页面，@Controller必须配合模版来使用。spring-boot 支持多种模版引擎包括：

1. Thymeleaf 官网推荐
2. FreeMarker
3. Groovy
4. Velocity
5. JSP 不推荐

本文以Thymeleaf为例介绍使用模版，具体步骤如下：

1. 

```apl
implementation "org.springframework.boot:spring-boot-starter-thymeleaf"
```
2. 

```java
@Controller
public class HelloController {

    @RequestMapping("/hello")
    public String hello() {
        return "hello";
    }
}
```

3. 在resources目录的templates目录下添加一个hello.html文件
4. 再次运行此项目之后，在浏览器中输入：`localhost:8080/hello`，就可以看到`html`的结果了。



> 因此，我们就直接使用@RestController注解来处理http请求来，这样简单的多。

## @RestController

Spring4之后新加入的注解，原来返回json需要@ResponseBody和@Controller配合。

即@RestController是@ResponseBody和@Controller的组合注解。

```java
@RestController
public class HelloController {

    @RequestMapping(value="/hello",method= RequestMethod.GET)
    public String sayHello(){
        return "hello";
    }
}
```



与下面代码作用相同

```java
@Controller
@ResponseBody
public class HelloController {

    @RequestMapping(value="/hello",method= RequestMethod.GET)
    public String sayHello(){
        return "hello";
    }
}
```



![image-20210707144355383](https://gitee.com/zzursy/blog-image/raw/master/img/20210707144355.png)

> **注意：不管加不加模板引擎都是，返回的是JSON数据**





