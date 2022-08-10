## 1、swagger-ui界面分布

<img src="https://gitee.com/zzursy/blog-image/raw/master/img/9976a728-94f8-4edc-b1cf-325ff1938efc.png" alt="img"  />

## 2、入门案例

- 配置maven依赖

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
```

- SwaggerConfig

```java
/**
 * @Configuration 相等于 @Component
 */
@Configuration
@EnableSwagger2
public class SwaggerConfig {
}

```

- controller

```java
@RestController
public class HelloController {

    @RequestMapping("/request")
    public String req() {
        return "request";
    }
}
```



## 3、配置swagger

1. 配置Docket，进入源码，发现默认构造函数只有一个

   ```java
   public Docket(DocumentationType documentationType) {
       this.apiInfo = ApiInfo.DEFAULT;
       this.groupName = "default";
       this.enabled = true;
       this.genericsNamingStrategy = new DefaultGenericTypeNamingStrategy();
       this.applyDefaultResponseMessages = true;
       this.host = "";
       this.pathMapping = Optional.absent();
       this.apiSelector = ApiSelector.DEFAULT;
       this.enableUrlTemplating = false;
       this.vendorExtensions = Lists.newArrayList();
       this.documentationType = documentationType;
   }
   ```

   

2. 配置ApiINfo，这是默认的配置
   **this.apiInfo = ApiInfo.DEFAULT;**

   ```java
   static {
       DEFAULT = new ApiInfo("Api Documentation", "Api Documentation", 
                             "1.0", "urn:tos", DEFAULT_CONTACT, 
                             "Apache 2.0", "http://www.apache.org/licenses/LICENSE-2.0", new ArrayList());
   }
   ```

   

3. 配置完毕

   ```java
   @Configuration
   @EnableSwagger2
   public class SwaggerConfig {
       /**
        * 进去查看源码，Docket只有一个构造方法，需要穿进去一个 DocumentationType
        * public Docket(DocumentationType documentationType) {
        * this.apiInfo = ApiInfo.DEFAULT;
        * this.groupName = "default";
        * ......
        * this.documentationType = documentationType;
        * }
        *
        * @return
        */
       @Bean
       public Docket docket() {
           return new Docket(DocumentationType.SWAGGER_2)
               .apiInfo(setMyApiInfo());
       }
       /**
        * 配置swagger2 apiInfo信息
        * @return
        */
       private ApiInfo setMyApiInfo() {
           return new ApiInfo("我的Swagger",
                              "Api Documentation",
                              "1.0",
                              "urn:tos",
                              new Contact("rsy", "123", "123@qq.com"),
                              "Apache 2.0",
                              "http://www.baidu.com",
                              new ArrayList());
       }
   }
   ```

4. 运行结果

   

![img](https://gitee.com/zzursy/blog-image/raw/master/img/9b866df5-9ed3-4224-8bfa-c84339b74096.png)



## 4、配置Swagger扫描接口

Docket.select()
接着上面的docket() 讲解

```java
public Docket docket(){
    return new Docket(DocumentationType.SWAGGER_2)
        .apiInfo(apiInfo())
        .select()
        // RequestHandlerSelectors, 配置要扫描的包
        // basePackage(): 指定要扫描的包
        // any(): 扫描全部
        // none(): 都不扫描
        // withClassAnnotation : 扫描类上的注解, 参数是一个注解的反射对象
        // 例如：withClassAnnotation(RestController.class) 只扫描类上有@RestController的生成文档
        // withMethodAnnotation: 扫描方法上的注解, 参数是一个注解的反射对象
        .apis(RequestHandlerSelectors.basePackage("com.kuang.swagger.controller"))
        // paths(): 过滤什么路径
        .paths(PathSelectors.ant("/kuang/**"))
        .build();  //build
}
```

==配置是否启动Swagger（enable 是否启动Swagger， 如果为false， 则Swagger 不能再浏览器中访问）==



## 5、配置环境

问题：我只希望我的Swagger在生产环境中使用，在发布的时候不使用？
解题思路：

- 判断是不是生产环境 flag = false
- **注入enable(flag)**

解题步骤：

1. 先在resources目录下创建两个properties文件，如下图
   ![在这里插入图片描述](https://gitee.com/zzursy/blog-image/raw/master/img/20210504162914.png)

2. 接下来在application.properties文件中激活环境

   ```properties
   spring.profiles.active=dev
   ```

3. 在application-dev.properties中

   ```properties
   server.port=8081
   ```

   

4. 接着上面的docket()，在方法中加入形参Environment获取`application.properties`中的设置的环境

   ```java
   @Bean
   public Docket docket(Environment environment){
       // 设置要显示的Swagger环境
       Profiles profiles = Profiles.of("dev", "test");
       // 通过 environment.acceptsProfiles(profiles) 判断是否处在自己设定的环境当中
       boolean flag = environment.acceptsProfiles(profiles);
       return new Docket(DocumentationType.SWAGGER_2)
           .apiInfo(setMyApiInfo())
           //此处判断是生产环境 flag 就是为true 就可以访问Swagger
           .enable(flag)
           .select()
           //这里只扫描controller包下面的文件，默认的error接口就不会被扫描
           .apis(RequestHandlerSelectors.basePackage("com.example.demo.controller"))
           //.paths(PathSelectors.ant("/kuang/**"))
           .build();  //build
   }
   
   ```

5. 在浏览器上请求http://localhost:8081/swagger-ui.html#/就可以访问Swagger，
   我们可以修改spring.profiles.active为pro
   `spring.profiles.active=pro`
   然后在浏览器上请求http://localhost:8082/swagger-ui.html#/
   会出现😱 Could not render e, see the console.



## 6、swagger2 注解

用于==controller==类上:

| 注解 | 说明           |
| ---- | -------------- |
| @Api | 对请求类的说明 |

用于方法上面 (说明参数的含义):

| 注解                                  | 说明                                                        |
| ------------------------------------- | ----------------------------------------------------------- |
| @ApiOperation                         | 方法的说明                                                  |
| @ApiImplicitParams、@ApiImplicitParam | 方法的参数的说明；@ApiImplicitParams 用于指定单个参数的说明 |

用于方法上面 (返回参数或对象的说明):

| 注解                        | 说明                                                    |
| --------------------------- | ------------------------------------------------------- |
| @ApiResponses、@ApiResponse | 方法返回值的说明 ；@ApiResponses 用于指定单个参数的说明 |

对象类:

| 注解              | 说明                                           |
| ----------------- | ---------------------------------------------- |
| @ApiModel         | 用在 JavaBean 类上，说明 JavaBean 的 用途      |
| @ApiModelProperty | 用在 JavaBean 类的属性上面，说明此属性的的含议 |

### **@API**

@API: 放在 请求的类上, 与 @Controller 并列, 说明类的作用, 如用户模块, 订单类等.

```java
//tags="说明该类的作用"
//value="该参数没什么意义, 所以不需要配置"
//示例：
@API(tags="订单模块")
@Controller
public class OrderController {

}
```

**其它属性配置:**

| 属性名称    | 备注                               |
| ----------- | ---------------------------------- |
| value       | url 的路径值                       |
| tags        | 如果设置这个值、value 的值会被覆盖 |
| description | 对 api 资源的描述                  |
| basePath    | 基本路径                           |

| position | 如果配置多个 Api 想改变显示的顺序位置   |
| -------- | --------------------------------------- |
| produces | 如, “application/json, application/xml” |
| consumes | 如, “application/json, application/xml” |

| protocols      | 协议类型，如: http, https, ws, wss. |
| -------------- | ----------------------------------- |
| authorizations | 高级特性认证时配置                  |
| hidden         | 配置为 true ，将在文档中隐藏        |

### @ApiOperation

方法的说明

```
@ApiOperation:"用在请求的方法上, 说明方法的作用"
    value="说明方法的作用"
    notes="方法的备注说明"
```

### @ApiImplicitParams

`@ApiImplicitParams` ,`@ApiImplicitParam`: 方法参数的说明

1. @ApiImplicitParams: 用在请求的方法上, 包含一组参数说明
2. @ApiImplicitParam: 对单个参数的说明

 

```
name: 参数名
value: 参数的汉字说明, 解释
required: 参数是否必须传
paramType: 参数放在哪个地方
    . header --> 请求参数的获取:@RequestHeader
    . query --> 请求参数的获取:@RequestParam
    . path(用于 restful 接口)--> 请求参数的获取:@PathVariable
dataType: 参数类型, 默认 String, 其它值 dataType="Integer"
defaultValue: 参数的默认值
```

 

```java
@API(tags="用户模块")
@Controller
public class UserController {
    @ApiOperation(value="用户登录",notes="随边说点啥")
    @ApiImplicitParams({
        @ApiImplicitParam(name="mobile",value="手机号",required=true,paramType="form"),
        @ApiImplicitParam(name="password",value="密码",required=true,paramType="form"),
        @ApiImplicitParam(name="age",value="年龄",required=true,paramType="form",dataType="Integer")
    })
    @PostMapping("/login")
    public JsonResult login(@RequestParam String mobile, @RequestParam String password,
                            @RequestParam Integer age){
        //...
        return JsonResult.ok(map);
    }
}
```

### @ApiResponses

@ApiResponses , @ApiResponse: 方法返回值的说明

```
@ApiResponses: 方法返回对象的说明
@ApiResponse: 每个参数的说明
code: 数字, 例如 400
message: 信息, 例如 "请求参数没填好"
response: 抛出异常的类
```

 

```java
@API(tags="用户模块")
@Controller
public class UserController {
    @ApiOperation("获取用户信息")
    @ApiImplicitParams({
        @ApiImplicitParam(paramType="query", name="userId", dataType="String", required=true, value="用户 Id")
    })
    @ApiResponses({
        @ApiResponse(code = 400, message = "请求参数没填好"),
        @ApiResponse(code = 404, message = "请求路径没有或页面跳转路径不对")
    })
    @ResponseBody
    @RequestMapping("/list")
    public JsonResult list(@RequestParam String userId) {
        ...
            return JsonResult.ok().put("page", pageUtil);
    }
}
```

### @ApiModel

@ApiModel: 用于 JavaBean 上面, 表示一个 JavaBean(如: 响应数据) 的信息

@ApiModel: 用于 JavaBean 的类上面, 表示此 JavaBean 整体的信息

(这种一般用在 post 创建的时候, 使用 @RequestBody 这样的场景,

请求参数无法使用 @ApiImplicitParam 注解进行描述的时候 )

### @ApiModelProperty

@ApiModelProperty :  用在 JavaBean 类的属性上面, 说明属性的含义

```java
@ApiModel(description= "返回响应数据")
public class RestMessage implements Serializable{
    @ApiModelProperty(value = "是否成功")
    private boolean success=true;
    @ApiModelProperty(value = "返回对象")
    private Object data;
    @ApiModelProperty(value = "错误编号")
    private Integer errCode;
    @ApiModelProperty(value = "错误信息")
    private String message;
    /* getter/setter 略 */
}
```

## 7、总结

 

```
@Api：用在请求的类上，表示对类的说明
    tags="说明该类的作用，可以在UI界面上看到的注解"
    value="该参数没什么意义，在UI界面上也看到，所以不需要配置"
@Api(tags="APP用户注册Controller")
 
@ApiOperation：用在请求的方法上，说明方法的用途、作用
    value="说明方法的用途、作用"
    notes="方法的备注说明"
@ApiOperation(value="用户注册",notes="手机号、密码都是必输项，年龄随边填，但必须是数字")
 
@ApiImplicitParams：用在请求的方法上，表示一组参数说明
    @ApiImplicitParam：用在@ApiImplicitParams注解中，指定一个请求参数的各个方面
        name：参数名
        value：参数的汉字说明、解释
        required：参数是否必须传
        paramType：参数放在哪个地方
            · header --> 请求参数的获取：@RequestHeader
            · query --> 请求参数的获取：@RequestParam
            · path（用于restful接口）--> 请求参数的获取：@PathVariable
            · body（不常用）
            · form（不常用）    
        dataType：参数类型，默认String，其它值dataType="Integer"       
        defaultValue：参数的默认值
        
 @ApiImplicitParams({
    @ApiImplicitParam(name="mobile",value="手机号",required=true,paramType="form"),
    @ApiImplicitParam(name="password",value="密码",required=true,paramType="form"),
    @ApiImplicitParam(name="age",value="年龄",required=true,paramType="form",dataType="Integer")
})
 
@ApiResponses：用在请求的方法上，表示一组响应
    @ApiResponse：用在@ApiResponses中，一般用于表达一个错误的响应信息
        code：数字，例如400
        message：信息，例如"请求参数没填好"
        response：抛出异常的类
        
@ApiOperation(value = "select1请求",notes = "多个参数，多种的查询参数类型")
@ApiResponses({
    @ApiResponse(code=400,message="请求参数没填好"),
    @ApiResponse(code=404,message="请求路径没有或页面跳转路径不对")
})
    
@ApiModel：用于响应类上，表示一个返回响应数据的信息
            （这种一般用在post创建的时候，使用@RequestBody这样的场景，
            请求参数无法使用@ApiImplicitParam注解进行描述的时候）
    @ApiModelProperty：用在属性上，描述响应类的属性
    
```



```java
import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;

import java.io.Serializable;

@ApiModel(description= "返回响应数据")
public class RestMessage implements Serializable{

    @ApiModelProperty(value = "是否成功")
    private boolean success=true;
    @ApiModelProperty(value = "返回对象")
    private Object data;
    @ApiModelProperty(value = "错误编号")
    private Integer errCode;
    @ApiModelProperty(value = "错误信息")
    private String message;

    /* getter/setter */
}
```

