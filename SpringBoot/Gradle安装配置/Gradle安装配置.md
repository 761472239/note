# Gradle安装配置

## 安装配置Gradle

以前在安卓AS中使用过Gradle 但是并没有关心，现在从配置开始 在Windows系统上安装并配置Gradle参考。

1.  [Gradle的安装与配置 - 苍凉温暖 - 博客园](https://www.cnblogs.com/NyanKoSenSei/p/11458953.html "Gradle的安装与配置 - 苍凉温暖 - 博客园")

2.  [gradle本地、远程仓库配置\_晚晴小筑-CSDN博客\_gradle配置远程仓库地址](https://n3verl4nd.blog.csdn.net/article/details/75040806?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-4.control\&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-4.control "gradle本地、远程仓库配置_晚晴小筑-CSDN博客_gradle配置远程仓库地址")

配置本地环境，更换阿里镜像.............

### 配置环境变量

1.  创建变量 `GEADLE_HOME`，值 `G:\gradle-7.1.1`

2.  然后添加到path中：`%GEADLE_HOME%\bin`

### 本地仓库配置

配置环境变量`GRADLE_USER_HOME`，并指向你的一个本地目录，用来保存Gradle下载的依赖包。

![image-20210707112849355](https://gitee.com/zzursy/blog-image/raw/master/img/20210707114935.png "image-20210707112849355")

### 远程仓库配置

一般Gradle、maven从中央仓库mavenCentral（） [http://repo1.maven.org/maven2/下载依赖包，但是在国内下载速度巨慢，我们只能使用国内的镜像。](http://repo1.maven.org/maven2/下载依赖包，但是在国内下载速度巨慢，我们只能使用国内的镜像。 "http://repo1.maven.org/maven2/下载依赖包，但是在国内下载速度巨慢，我们只能使用国内的镜像。")
所以每个Gradle构建的项目中，我们可以在build.gradle做如下配置

```纯文本
repositories {
    maven {
        url 'http://maven.aliyun.com/nexus/content/groups/public/'
    }
    mavenCentral()
}
```

或者

```纯文本
buildscript {
    repositories {
        maven { url 'https://maven.aliyun.com/repository/google/' }
        maven { url 'https://maven.aliyun.com/repository/jcenter/'}
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.2.3'

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }        
}

allprojects {
    repositories {
        maven { url 'https://maven.aliyun.com/repository/google/' }
        maven { url 'https://maven.aliyun.com/repository/jcenter/'}
    }
}

```

### 全局仓库配置

每个项目都如此配置难免麻烦些，我们可以配置一个全局配置文件。

`.gradle\init.gradle`

```纯文本
allprojects{
    repositories {
        def ALIYUN_REPOSITORY_URL = 'https://maven.aliyun.com/repository/public/'
        def ALIYUN_JCENTER_URL = 'https://maven.aliyun.com/repository/jcenter/'
        def ALIYUN_GOOGLE_URL = 'https://maven.aliyun.com/repository/google/'
        def ALIYUN_GRADLE_PLUGIN_URL = 'https://maven.aliyun.com/repository/gradle-plugin/'
        all { ArtifactRepository repo ->
            if(repo instanceof MavenArtifactRepository){
                def url = repo.url.toString()
                if (url.startsWith('https://repo1.maven.org/maven2/')) {
                    project.logger.lifecycle "Repository ${repo.url} replaced by $ALIYUN_REPOSITORY_URL."
                    remove repo
                }
                if (url.startsWith('https://jcenter.bintray.com/')) {
                    project.logger.lifecycle "Repository ${repo.url} replaced by $ALIYUN_JCENTER_URL."
                    remove repo
                }
                if (url.startsWith('https://dl.google.com/dl/android/maven2/')) {
                    project.logger.lifecycle "Repository ${repo.url} replaced by $ALIYUN_GOOGLE_URL."
                    remove repo
                }
                if (url.startsWith('https://plugins.gradle.org/m2/')) {
                    project.logger.lifecycle "Repository ${repo.url} replaced by $ALIYUN_GRADLE_PLUGIN_URL."
                    remove repo
                }
            }
        }
        maven { url ALIYUN_REPOSITORY_URL }
        maven { url ALIYUN_JCENTER_URL }
        maven { url ALIYUN_GOOGLE_URL }
        maven { url ALIYUN_GRADLE_PLUGIN_URL }
    }
}

```

### init.gradle简介

init.gradle文件在build开始之前执行，所以你可以在这个文件配置一些你想预先加载的操作
例如配置build日志输出、配置你的机器信息，比如jdk安装目录，配置在build时必须个人信息，比如仓库或者数据库的认证信息，and so on.

**启用init.gradle文件的方法：**

1.  在命令行指定文件，例如：gradle –init-script yourdir/init.gradle -q taskName.你可以多次输入此命令来指定多个init文件

2.  把init.gradle文件放到USER\_HOME/.gradle/ 目录下.

3.  把以.gradle结尾的文件放到USER\_HOME/.gradle/init.d/ 目录下.

4.  把以.gradle结尾的文件放到GRADLE\_HOME/init.d/ 目录下.

如果存在上面的4种方式的2种以上，gradle会按上面的1-4序号依次执行这些文件，如果给定目录下存在多个init脚本，会按拼音a-z顺序执行这些脚本
类似于build.gradle脚本，init脚本有时groovy语言脚本。每个init脚本都存在一个对应的gradle实例，你在这个文件中调用的所有方法和属性，都会
委托给这个gradle实例，每个init脚本都实现了Script接口

下面的例子是在build执行之前给所有的项目制定maven本地库，这个例子同时在 build.gradle文件指定了maven的仓库中心，注意它们之间异同

build.gradle

```纯文本
repositories {
    mavenCentral()
}

task showRepos << {
    println "All repos:"
    println repositories.collect { it.name }
}
```

init.gradle

```纯文本
allprojects {
    repositories {
        mavenLocal()
    }
}
```

在命令行输入命令:gradle –init-script init.gradle -q showRepos

```纯文本
> gradle --init-script init.gradle -q showRepos
All repos:
[MavenLocal, MavenRepo]
```
