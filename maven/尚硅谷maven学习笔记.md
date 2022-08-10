## 1、自动化构建工具：Maven

### 1.1 Maven简介

Maven 是 Apache 软件基金会组织维护的一款自动化**构建**工具，专注服务于 Java 平台的**项目构建**和**依赖管理**。Maven 这个单词的本意是：专家，内行。读音是['meɪv(ə)n]或['mevn]。

### 1.2 什么是构建

构建并不是创建，创建一个工程并不等于构建一个项目。要了解构建的含义我们应该由浅入深的从

以下三个层面来看：

1. 纯 Java 代码

大家都知道，我们 Java 是一门编译型语言，.java 扩展名的源文件需要编译成.class 扩展名的字节码文件才能够执行。所以编写任何 Java 代码想要执行的话就必须经过编译得到对应的.class 文件。 

2. Web 工程

当我们需要通过浏览器访问 Java 程序时就必须将包含 Java 程序的 Web 工程编译的结果“拿”到服务器上的指定目录下，并启动服务器才行。这个“拿”的过程我们叫部署。我们可以将未编译的 Web 工程比喻为一只生的鸡，编译好的 Web 工程是一只煮熟的鸡，编译部署的过程就是将鸡炖熟。

Web 工程和其编译结果的目录结构对比见下图：

![image-20210530155627284](https://gitee.com/zzursy/blog-image/raw/master/img/20210530155627.png)

③实际项目

在实际项目中整合第三方框架，Web 工程中除了 Java 程序和 JSP 页面、图片等静态资源之外，还包括第三方框架的 jar 包以及各种各样的配置文件。所有这些资源都必须按照正确的目录结构部署到服务器上，项目才可以运行。所以综上所述：构建就是以我们编写的 Java 代码、框架配置文件、国际化等其他资源文件、JSP 页面和图片等静态资源作为“原材料”，去“生产”出一个可以运行的项目的过程。

那么项目构建的全过程中都包含哪些环节呢？

### 1.3 构建过程的几个主要环节

①清理：删除以前的编译结果，为重新编译做好准备。
②编译：将 Java 源程序编译为字节码文件。
③测试：针对项目中的关键点进行测试，确保项目在迭代开发过程中关键点的正确性。
④报告：在每一次测试后以标准的格式记录和展示测试结果。
⑤打包：将一个包含诸多文件的工程封装为一个压缩文件用于安装或部署。Java 工程对应 jar 包，Web
工程对应 war 包。
⑥安装：在 Maven 环境下特指将打包的结果——jar 包或 war 包安装到本地仓库中。
⑦部署：将打包的结果部署到远程仓库或将 war 包部署到服务器上运行。

### 1.4 自动化构建

其实上述环节我们在 Eclipse 中都可以找到对应的操作，只是不太标准。那么既然 IDE 已经可以进
行构建了我们为什么还要使用 Maven 这样的构建工具呢？我们来看一个小故事：

![image-20210530202211014](https://gitee.com/zzursy/blog-image/raw/master/img/20210530202258.png)

### 1.5 Maven 核心概念

Maven 能够实现自动化构建是和它的内部原理分不开的，这里我们从 Maven 的九个核心概念入手，
看看 Maven 是如何实现自动化构建的
①POM
②约定的目录结构
③坐标
**④依赖管理**
⑤仓库管理
⑥生命周期
⑦插件和目标
⑧继承
⑨聚合



## 2、第一个Maven工程【目录】

### 2.1 约定的目录结构

约定的目录结构对于 Maven 实现自动化构建而言是必不可少的一环，就拿自动编译来说，Maven 必须能找到 Java 源文件，下一步才能编译，而编译之后也必须有一个准确的位置保持编译得到的字节码文件。我们在开发中如果需要让第三方工具或框架知道我们自己创建的资源在哪，那么基本上就是两种方式：

①通过配置的形式明确告诉它

②基于第三方工具或框架的约定

Maven 对工程目录结构的要求就属于后面的一种。

![image-20210530204210580](https://gitee.com/zzursy/blog-image/raw/master/img/20210530204210.png)



现在 JavaEE 开发领域普遍认同一个观点：**约定 > 配置 > 编码**。意思就是能用配置解决的问题就不编码，能基于约定的就不进行配置。而 Maven 正是因为指定了特定文件保存的目录才能够对我们的 Java 工程进行自动化构建。



### 2.2 常见的maven命令

mvn compile	编译
mvn clean	清理
mvn test	测试
mvn package	打包
注意：运行Maven命令时一定要进入pom.xml文件所在的目录！



## 3、POM

Project Object Model：项目对象模型。将 Java **工程**的相关信息封装为**对象**作为便于操作和管理的**模型**。

Maven 工程的核心配置。可以说学习 Maven 就是学习 pom.xml 文件中的配置。

## 4、坐标

1. 几何中的坐标
    在一个平面中使用 x、y 两个向量可以唯一的确定平面中的一个点。
    在空间中使用 x、y、z 三个向量可以唯一的确定空间中的一个点。

2. Maven的坐标使用如下三个向量在 Maven 的仓库中唯一的确定一个 Maven 工程。

    1. `g`roupid：公司或组织的域名倒序 + 当前项目名称
    2. `a`rtifactId：当前项目的**模块名称**
    3. `v`ersion：当前模块的**版本**

    ```xml
    <groupId>com.atguigu.maven</groupId>
    <artifactId>Hello</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    ```



3. 如何通过坐标到仓库中查找 jar 包？
    1. 将 gav 三个向量连起来
        `com.atguigu.maven+Hello+0.0.1-SNAPSHOT`
    2. 以连起来的字符串作为目录结构到仓库中查找
        `com/atguigu/maven/Hello/0.0.1-SNAPSHOT/Hello-0.0.1-SNAPSHOT.jar`

⭐我们自己的 Maven 工程必须执行安装操作才会进入仓库。安装的命令是：mvn install



## 5、依赖管理

### 5.1 依赖的范围

 compile、test、provided

1. 从项目结构角度理解 compile 和 test 的区别
    ![image-20210530213048217](https://gitee.com/zzursy/blog-image/raw/master/img/20210530213206.png)

2. 从开发和运行这两个不同阶段理解 compile 和 provided 的区别
    ![image-20210530213154890](https://gitee.com/zzursy/blog-image/raw/master/img/20210530213154.png)
    <img src="https://gitee.com/zzursy/blog-image/raw/master/img/20210530213729.png" alt="image-20210530213729205" style="zoom:80%;" />
    <img src="https://gitee.com/zzursy/blog-image/raw/master/img/20210530214158.png" alt="image-20210530214137420" style="zoom:80%;" />

    

3. 有效性总结

    |               | compile | test | provided |
    | :-----------: | ------- | ---- | -------- |
    |    主程序     | √       | ×    | √        |
    |   测试程序    | √       | √    | √        |
    | 参与打包→部署 | √       | ×    | ×        |

    

### 5.2 依赖的传递性

A 依赖 B，B 依赖 C，A 能否使用 C 呢？那要看 B 依赖 C 的范围是不是 compile，如果是则可用，否则不可用。

![image-20210531145149918](https://gitee.com/zzursy/blog-image/raw/master/img/20210531145150.png)

![image-20210531145241872](https://gitee.com/zzursy/blog-image/raw/master/img/20210531145241.png)

> 这里是项目直接依赖HelloFriend。
>
> HelloFriend依赖Hello
>
> Hello依赖spring-core
>
> spring-core依赖commos-logging

### 5.3 依赖的排除

> 排除之前：

![image-20210531151520973](https://gitee.com/zzursy/blog-image/raw/master/img/20210531151521.png)



```xml
<dependency>
    <groupId>com.atguigu.maven</groupId>
    <artifactId>HelloFriend</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <type>jar</type>
    <scope>compile</scope>
    <exclusions> 
        <exclusion> 
            <groupId>commons-logging</groupId> 
            <artifactId>commons-logging</artifactId> 
        </exclusion> 
    </exclusions> 
</dependency>
```

> 排除之后

![image-20210531152249258](https://gitee.com/zzursy/blog-image/raw/master/img/20210531152249.png)





### 5.4 依赖的原则

依赖的原则是为了解决Jar包冲突。

![image-20210531153915051](https://gitee.com/zzursy/blog-image/raw/master/img/20210531153915.png)



## 6、仓库管理

分类

[1]本地仓库：为当前本机电脑上的所有 Maven 工程服务。

[2]远程仓库

(1)私服：架设在当前局域网环境下，为当前局域网范围内的所有 Maven 工程服务。

![image-20210531155112126](https://gitee.com/zzursy/blog-image/raw/master/img/20210531155112.png)

(2)中央仓库：架设在 Internet 上，为全世界所有 Maven 工程服务。

(3)中央仓库的镜像：架设在各个大洲，为中央仓库分担流量。减轻中央仓库的压力，同时更快的响应用户请求。

仓库中的文件

[1]Maven 的插件

[2]我们自己开发的项目的模块

[3]第三方框架或工具的 jar 包

※不管是什么样的 jar 包，在仓库中都是按照坐标生成目录结构，所以可以通过统一的方式查询或依赖。



## 7、生命周期

Maven 生命周期定义了各个构建环节的执行顺序，有了这个清单，Maven 就可以自动化的执行构建命令了。

Maven 有三套相互独立的生命周期，分别是：

1. Clean Lifecycle 在进行真正的构建之前进行一些清理工作。
2. Default Lifecycle 构建的核心部分，编译，测试，打包，安装，部署等等。
3. Site Lifecycle 生成项目报告，站点，发布站点。

它们是相互独立的，你可以仅仅调用 clean 来清理工作目录，仅仅调用 site 来生成站点。当然你也可以直接运行 

`mvn clean install site`运行所有这三套生命周期。

每套生命周期都由一组阶段(Phase)组成，我们平时在命令行输入的命令总会对应于一个特定的阶段。比如，运行 mvn clean，这个 clean 是 Clean 生命周期的一个阶段。有 Clean 生命周期，也有 clean 阶段。

### 7.1 Clean 生命周期

Clean 生命周期一共包含了三个阶段：

1. pre-clean 执行一些需要在 clean 之前完成的工作
2. clean 移除所有上一次构建生成的文件
3. post-clean 执行一些需要在 clean 之后立刻完成的工作





### 7.2 Site 生命周期

①pre-site 执行一些需要在生成站点文档之前完成的工作

②site 生成项目的站点文档

③post-site 执行一些需要在生成站点文档之后完成的工作，并且为部署做准备

④site-deploy 将生成的站点文档部署到特定的服务器上

这里经常用到的是 site 阶段和 site-deploy 阶段，用以生成和发布 Maven 站点，这可是 Maven 相当强大的功能，Manager 比较喜欢，文档及统计数据自动生成，很好看。



### 7.3 Default 生命周期

Default 生命周期是 Maven 生命周期中最重要的一个，绝大部分工作都发生在这个生命周期中。这里，

只解释一些比较重要和常用的阶段：

> validate
>
> generate-sources
>
> process-sources
>
> generate-resources
>
> process-resources 复制并处理资源文件，至目标目录，准备打包。
>
> compile 编译项目的源代码。
>
> process-classes
>
> generate-test-sources
>
> process-test-sources
>
> generate-test-resources
>
> process-test-resources 复制并处理资源文件，至目标测试目录。
>
> test-compile 编译测试源代码。
>
> process-test-classes
>
> test 使用合适的单元测试框架运行测试。这些测试代码不会被打包或部署。
>
> prepare-package
>
> package 接受编译好的代码，打包成可发布的格式，如 JAR。
>
> pre-integration-test
>
> integration-test
>
> post-integration-test
>
> verify
>
> install 将包安装至本地仓库，以让其它项目依赖。
>
> deploy 将最终的包复制到远程的仓库，以让其它开发人员与项目共享或部署到服务器上运行。

### 7.4 生命周期与自动化构建

**运行任何一个阶段的时候，它前面的所有阶段都会被运行**，例如我们运行 mvn install 的时候，代码会被编译，测试，打包。这就是 Maven 为什么能够自动执行构建过程的各个环节的原因。此外，Maven 的插件机制是完全依赖 Maven 的生命周期的，因此理解生命周期至关重要。



## 8、Maven插件和目标

插件和目标

1. Maven 的核心仅仅定义了抽象的生命周期，具体的任务都是交由插件完成的。
2. 每个插件都能实现多个功能，每个功能就是一个插件目标。
3. Maven 的生命周期与插件目标相互绑定，以完成某个具体的构建任务。

例如：compile 就是插件 maven-compiler-plugin 的一个目标；pre-clean 是插件 maven-clean-plugin 的一个目标。





## 9、统一管理依赖的版本

对同一个框架的一组 jar 包最好使用相同的版本。为了方便升级框架，可以将 jar 包的版本信息统一提取出来。

1. 统一声明版本号

    ```xml
    <properties>
        <atguigu.spring.version>4.1.1.RELEASE</atguigu.spring.version>
    </properties>
    ```

2. 引用前面声明的版本号

    ```xml
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>${atguigu.spring.version}</version>
    </dependency>
    ```

3. 其他用法

    ```xml
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>
    ```

    



## 10、继承

为什么需要继承机制？
由于非 compile 范围的依赖信息是不能在“依赖链”中传递的，所以有需要的工程只能单独配置。例如：

hello项目

```xml
<dependency> 
    <groupId>junit</groupId> 
    <artifactId>junit</artifactId>
    <version>4.0</version>
    <scope>test</scope>
</dependency>
```

HelloFriend项目

```xml
<dependency> 
    <groupId>junit</groupId> 
    <artifactId>junit</artifactId>
    <version>4.0</version>
    <scope>test</scope>
</dependency>
```

MakeFriend项目

```xml
<dependency> 
    <groupId>junit</groupId> 
    <artifactId>junit</artifactId>
    <version>4.9</version>
    <scope>test</scope>
</dependency>
```

此时如果项目需要将各个模块的junit版本统一为**4.9**，那么到各个工程中手动修改无疑是非常不可取的。使用继承机制就可以将这样的依赖信息统一提取到父工程模块中进行统一管理。

### 10.1 操作步骤

1. 创建一个Maven工程作为父工程。注意：打包的方式pom
2. 在子工程中声明对父工程的引用。
3. 将子工程和父工程坐标中重复的内容删除。
4. 在父工程中统一junit的依赖。
5. 在子工程中删除junit依赖的版本号部分。



## 11、聚合

### 11.1 为什么要使用聚合？

将多个工程拆分为模块后，需要手动逐个安装到仓库后依赖才能够生效。修改源码后也需要逐个手动进行 clean 操作。而使用了聚合之后就可以批量进行 Maven 工程的安装、清理工作。

### 11.2 如何配置聚合？

```xml
<modules> 
    <module>../Hello</module>
    <module>../HelloFriend</module>
    <module>../MakeFriends</module>
</modules>
```





