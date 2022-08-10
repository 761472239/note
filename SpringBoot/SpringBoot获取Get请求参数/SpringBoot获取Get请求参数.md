# SpringBoot获取Get请求参数

## 目录

*   [Get请求](#get请求)

    *   [一、参数直接在路径中](#一参数直接在路径中)

    *   [二、参数在？后面](#二参数在后面)

        *   [1、参数正常传递](#1参数正常传递)

        *   [2、参数没有传递](#2参数没有传递)

        *   [3、参数是map](#3参数是map)

        *   [4、参数是数组](#4参数是数组)

    *   [三、参数是对象](#三参数是对象)

        *   [1、基本用法](#1基本用法)

        *   [2、指定参数前缀](#2指定参数前缀)

        *   [3、构造多个对象接收参数](#3构造多个对象接收参数)

# Get请求

## 一、参数直接在路径中

使用@PathVariable

1）假设请求地址是如下这种 RESTful 风格，hangge 这个参数值直接放在路径里面：

`http://localhost:8080/hello/hangge`

2）Controller 可以这么获取该参数：&#x20;

```java
@RestController
public class HelloController {
    @GetMapping("/hello/{name}")
    public String hello(@PathVariable("name") String name) {
        return "获取到的name是：" + name;
    }
}
```

## 二、参数在？后面

### 1、参数正常传递

1）地址如下

`http://localhost:8080/hello?name=hangge`

2）获取参数

```java
@RestController
public class HelloController {
    @GetMapping("/hello")
    public String hello(@RequestParam("name") String name) {
        return "获取到的name是：" + name;
    }
}
```

### 2、参数没有传递

1）地址如下

`http://localhost:8080/hello`

2）获取参数

```java
@RestController
public class HelloController {
    @GetMapping("/hello")
    public String hello(@RequestParam(name = "name", required = false) String name) {
        return "获取到的name是：" + name;
    }
}
```

3）也可以指定默认值

```java
@RestController
public class HelloController {
    @GetMapping("/hello")
    public String hello(@RequestParam(name = "name", defaultValue = "xxx") String name) {
        return "获取到的name是：" + name;
    }
}
```

### 3、参数是map

1）地址如下

`http://localhost:8080/hello?name=hangge&age=11`

2）获取参数

```java
@RestController
public class HelloController {
    @GetMapping("/hello")
    public String hello(@RequestParam Map<String, Object> params) {
        return "name：" + params.get("name") + "<br>age：" + params.get("age");
    }
}
```

### 4、参数是数组

1）请求地址

`http://localhost:8080/hello?name=hangge&name=google`

2）获取参数

```java
@RestController
public class HelloController {
    @GetMapping("/hello")
    public String hello(@RequestParam("name") String[] names) {
        String result = "";
        for(String name:names){
            result += name + "<br>";
        }
        return result;
    }
}
```

## 三、参数是对象

### 1、基本用法

1）如果get请求的参数太多，构造一个对象来接收参数比较方便

```jaav
@RestController
public class HelloController {
    @GetMapping("/hello")
    public String hello(User user) {
        return "name：" + user.getName() + "<br> age：" + user.getAge();
    }
}
```

SpringMVC底层其实还是将请求中的参数剥离出来，然后调用我们指定的这个对象（Java Bean）的setter方法来为同名的属性进行赋值，当我们用的时候，直接使用getter方法来用。

2）User类提供setter方法

```java
public class User {
    private String name;
    private Integer age;
 
    public String getName() {
        return name;
    }
 
    public void setName(String name) {
        this.name = name;
    }
 
    public Integer getAge() {
        return age;
    }
 
    public void setAge(Integer age) {
        this.age = age;
    }
}
```

3）测试样例

> <http://localhost:8080/hello?name=hangge\\&age=100>

4）如果传递的参数有前缀，且前缀与接收实体类的名称(user)相同，那么参数也是可以正常传递的：

> <http://localhost:8080/hello?user.name=hangge\\&user.age=100>

### 2、指定参数前缀

1）如果传递的参数有前缀，且前缀与接收实体类的名称不同相，那么参数无法正常传递：

> <http://localhost:8080/hello?u.name=hangge\\&u.age=100>

2）我们可以结合 @InitBinder 解决这个问题，通过参数预处理来指定使用的前缀为 u.

除了在 Controller 里单独定义预处理方法外，我们还可以通过 @ControllerAdvice 结合 @InitBinder 来定义全局的参数预处理方法，方便各个 Controller 使用。具体做法参考我之前的文章： &#x20;

*   [SpringBoot - @ControllerAdvice的使用详解3（请求参数预处理 @InitBinder）](https://www.hangge.com/blog/cache/detail_2483.html "SpringBoot - @ControllerAdvice的使用详解3（请求参数预处理 @InitBinder）")

```java
@RestController
public class HelloController {
    @GetMapping("/hello")
    public String hello(@ModelAttribute("u") User user) {
        return "name：" + user.getName() + "<br> age：" + user.getAge();
    }
 
    @InitBinder("u")
    private void initBinder(WebDataBinder binder) {
        binder.setFieldDefaultPrefix("u.");
    }
}
```

3）重启程序

### 3、构造多个对象接收参数

1）如果一个 get 请求的参数分属不同的对象，也可以使用多个对象来接收参数：

```java
@RestController
public class HelloController {
    @GetMapping("/hello")
    public String hello(User user, Phone phone) {
        return "name：" + user.getName() + "<br> age：" + user.getAge()
                + "<br> number：" + phone.getNumber();
    }
}
```

2）新增Phone实体类

```java
public class Phone {
    private String number;
 
    public String getNumber() {
        return number;
    }
 
    public void setNumber(String number) {
        this.number = number;
    }
}
```

3）测试样例

访问请求

> <http://localhost:8080/hello?name=hangge\\&age=100\\&number=1551708>
