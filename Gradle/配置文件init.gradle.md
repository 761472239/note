# 配置文件init.gradle

init.gradle文件在build开始之前执行，所以你可以在这个文件配置一些你想预先加载的操作 

例如配置build日志输出、配置你的机器信息，比如jdk安装目录，配置在build时必须个人信息，比如仓库或者数据库的认证信息，and so on. 

启用init.gradle文件的方法： 

1、在命令行指定文件，例如：`gradle --init-script yourdir/init.gradle -q taskName`你可以多次输入此命令来指定多个init文件 

2、把文件放到`USER_HOME/.gradle/` 目录下. 

3、把以结尾的文件放到`USER_HOME/.gradle/init.d/` 目录下. 

4、把以结尾的文件放到`GRADLE_HOME/init.d/ `目录下.

**注意**：`USER_HOME/.gradle/`就是gradle本地仓库地址 

如果存在上面的4种方式的2种以上，gradle会按上面的1-4序号依次执行这些文件

```json
allprojects {
    repositories {
        def ALIYUN_REPOSITORY_URL = 'https://maven.aliyun.com/repository/public/'
        def ALIYUN_JCENTER_URL = 'https://maven.aliyun.com/repository/jcenter/'
        def ALIYUN_GOOGLE_URL = 'https://maven.aliyun.com/repository/google/'
        def ALIYUN_GRADLE_PLUGIN_URL = 'https://maven.aliyun.com/repository/gradle-plugin/'
        all { ArtifactRepository repo ->
            if (repo instanceof MavenArtifactRepository) {
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
        maven {
            allowInsecureProtocol = true
            url ALIYUN_REPOSITORY_URL
        }
        maven {
            allowInsecureProtocol = true
            url ALIYUN_JCENTER_URL
        }
        maven {
            allowInsecureProtocol = true
            url ALIYUN_GOOGLE_URL
        }
        maven {
            allowInsecureProtocol = true
            url ALIYUN_GRADLE_PLUGIN_URL
        }
    }
}
```

第二种方法

```groovy
allprojects {
    repositories {
        maven{ url 'https://maven.aliyun.com/repository/central'}
        maven{ url 'https://maven.aliyun.com/repository/public' }
        maven{ url 'https://maven.aliyun.com/repository/google' }
        maven{ url 'https://maven.aliyun.com/repository/gradle-plugin' }
        maven{ url 'https://maven.aliyun.com/repository/spring' }
        maven{ url 'https://maven.aliyun.com/repository/spring-plugin' }
        maven{ url 'https://maven.aliyun.com/repository/grails-core' }
        maven{ url 'https://maven.aliyun.com/repository/apache-snapshots' }
        mavenLocal()
        mavenCentral()
    }
}
```
