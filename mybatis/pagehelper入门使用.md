## 1、配置

```yml
mybatis:
  type-aliases-package: com.ren.bs_thesis.entity
  mapper-locations:
    - classpath:mapper/*Mapper.xml
  #  　　开启驼峰uName自动映射到u_name
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl #指定 MyBatis 所用日志的具体实现，未指定时将自动查找
    map-underscore-to-camel-case: true #开启自动驼峰命名规则（camel case）映射
    lazy-loading-enabled: true #开启延时加载开关
    aggressive-lazy-loading: false #将积极加载改为消极加载（即按需加载）,默认值就是false
    lazy-load-trigger-methods: "" #阻挡不相干的操作触发，实现懒加载
    cache-enabled: true #打开全局缓存开关（二级环境），默认值就是true

#logging:
#  level:
#    com.ren.bs_thesis.mapper: trace # 改成你的mapper文件所在包路径


#  helper-dialect：配置使用哪种数据库语言，不配置的话pageHelper也会自动检测。
#  reasonable：在启用合理化时，如果 pageNum<1，则会查询第一页；如果 pageNum>pages，则会查询最后一页。
#  support-methods-arguments：支持通过Mapper接口参数来传递分页参数，默认值false，分页插件会从查询方法的参数值中，自动根据上面 params 配置的字段中取值，查找到合适的值时就会自动分页。

#MyBatis使用pageHelper分页
pagehelper:
  helper-dialect: mysql
  reasonable: true
  support-methods-arguments: true


```

## 2、maven配置

```xml
<!--pagehelper-->
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-starter</artifactId>
    <version>1.2.10</version>
</dependency>
```





## 3、发起请求

`http://localhost:8080/index#/admin/department_management?pageNum=1&pageSize=5 `

```java
@RequestMapping("department_management")
@ResponseBody
public PageInfo<Department> queryAllDepartment(Integer pageNum, Integer pageSize) {

    PageInfo<Department> pageInfo = null;
    try {
        List<Department> list;
        pageInfo = null;

        Page<Department> page = PageHelper.startPage(pageNum, pageSize);
        list = pageHelperSerService.queryAllDepartment();
        pageInfo = new PageInfo<>(list);

        //        List<Department> list = pageHelperSerService.queryAllDepartment();
        //        Page page = (Page) list;
        //        System.out.println("每页展示条数：" + page.getPageSize());
        //        System.out.println("总条数：" + page.getTotal());
        //        System.out.println("当前页：" + page.getPageNum());
        //        System.out.println("总页数：" + page.getPages());
        //        PageInfo<Department> pageInfo = new PageInfo<>(list);

    } catch (Exception e) {
        e.printStackTrace();
    }
    return pageInfo;
}
```





![image-20210509134609908](https://gitee.com/zzursy/blog-image/raw/master/img/20210509140106.png)



## 4、其他博客

1. [MyBatis分页插件PageHelper的使用_JustryDeng-CSDN博客](https://blog.csdn.net/justry_deng/article/details/82933941?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-4.vipsorttest&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-4.vipsorttest)



