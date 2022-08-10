参考博客：[Swagger 3.0配置整合使用教程_南伯基尼-CSDN博客](https://changxin.blog.csdn.net/article/details/109137799?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-3.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-3.control)

## 1、swagger介绍

Swagger 是一套基于 OpenAPI 规范（OpenAPI Specification，OAS）构建的开源工具，后来成为了 Open API 标准的主要定义者。
对于 Rest API 来说很重要的一部分内容就是文档，Swagger 为我们提供了一套通过代码和注解自动生成文档的方法，这一点对于保证API 文档的及时性将有很大的帮助。

swagger2于17年停止维护，现在最新的版本为17年发布的 Swagger3（Open Api3）。



## 2、springfox介绍

SpringFox是 spring 社区维护的一个项目（非官方）
由于Spring的流行，Marty Pitt编写了一个基于Spring的组件swagger-springmvc，用于将swagger集成到springmvc中来，而springfox则是从这个组件发展而来。



## 3、springfox-swagger 2

SpringBoot项目整合swagger2需要用到两个依赖：`springfox-swagger2 `和`springfox-swagger-ui `，用于自动生成swagger文档。

- springfox-swagger2：这个组件的功能用于帮助我们自动生成描述API的json文件
- springfox-swagger-ui：就是将描述API的json文件解析出来，用一种更友好的方式呈现出来。

## 4、SpringFox 3

**此版本的亮点：**

Spring5，Webflux支持（仅支持请求映射，尚不支持功能端点）。
Spring Integration支持。
SpringBoot支持springfox Boot starter依赖性（零配置、自动配置支持）。
支持OpenApi 3.0.3。
零依赖。几乎只需要spring-plugin，swagger-core ，现有的swagger2注释将继续工作并丰富openapi3.0规范。



**与2.x版本的差异：**

- 应用主类添加注解`@EnableOpenApi `(swagger2是@EnableSwagger2)
- swagger配置类`SwaggerProperties.class `，与swagger2.xx 版本有差异，具体看下文
- 自定义一个配置类 `SwaggerConfiguration.class`，看下文
- 访问地`</u> `址：http://localhost:8080/swagger-ui/index.html (swagger2.xx版本访问的地址为http://localhost:8080/swagger-ui.html)



## 5、Springboot整合过程

