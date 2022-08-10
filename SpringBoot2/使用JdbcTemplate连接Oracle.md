

## 数据源与连接池

### 配置单个数据源

**第一步：添加依赖**

```apl
// 引入依赖
implementation group: 'org.springframework.data', name: 'spring-data-jdbc', version: '2.2.3'
implementation files('lib/ojdbc6.jar')
```

注意：ojdbc.jar 因为某些原因，需要去官网下载。

1. [Oracle Database 11g 第 2 版 JDBC 驱动程序下载 | Oracle 中国](https://www.oracle.com/cn/database/technologies/enterprise-edition/jdbc.html)
2. [ojdbc6.jar](https://www.oracle.com/webapps/redirect/signon?nexturl=https://download.oracle.com/otn/utilities_drivers/jdbc/11204/ojdbc6.jar)

**第二步：配置数据源**

```yml
#配置数据源
spring:
  datasource:
    url: jdbc:oracle:thin:@10.2.21.9:1521:orcl
    username: c##rsy
    password: 111111
    driver-class-name: oracle.jdbc.driver.OracleDriver
```





**第三步：注入数据源**

```java

import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.datasource.DriverManagerDataSource;

@Configuration
public class SpringJdbcConfig {

    @Value(value = "${spring.datasource.driver-class-name}")
    private String driverClassName;
    @Value(value = "${spring.datasource.url}")
    private String url;
    @Value(value = "${spring.datasource.username}")
    private String username;
    @Value(value = "${spring.datasource.password}")
    private String password;

    @Bean
    public DriverManagerDataSource dataSource() {
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName(driverClassName);
        dataSource.setUrl(url);
        dataSource.setUsername(username);
        dataSource.setPassword(password);

        return dataSource;
    }

    @Bean
    public JdbcTemplate jdbcTemplate(DriverManagerDataSource dataSource) {
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        jdbcTemplate.setDataSource(dataSource);

        return jdbcTemplate;
    }

}
```



### 配置多个数据源

[Spring Boot之JdbcTemplate多数据源配置与使用 - 割肉机 - 博客园](https://www.cnblogs.com/williamjie/p/9355899.html)



### 配置HikariCP连接池



## 使用JdbcTemplate

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyController {

    @Autowired
    JdbcTemplate jdbcTemplate;


    @RequestMapping("/user")
    public String index() {
        String sql = "insert into STU (id,name) values (7,'李四')";
        jdbcTemplate.execute(sql);
        System.out.println("执行完成");
        return "执行完成";
    }
}
```



## 搞定

![image-20210719174136214](https://gitee.com/zzursy/blog-image/raw/master/img/20210719174136.png)