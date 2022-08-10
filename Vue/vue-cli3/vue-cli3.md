# vue-cli3

如果不用vue-cli模板，怎么搭建环境？

可以利用npm

*   安装依赖:`	npm install`

*   初始化:`npm install -f`

*   安装组件，安装之后并查看

## 手动创建项目

1.  `npm init` 创建package.json文件

2.  `npm i vue-router -D` 安装依赖

注意 如果使用 -g 就是安装到nodejs 目录下的那个node\_modules里面

如果不使用就是安装到本地的node\_modules目录里面

最好还是使用模板安装

## 安装cli3

1.  先卸载cli2：`npm uninstall vue-cli -g`

2.  在安装  `npm install -g @vue/cli`

## vue-cli3创建项目

一定要使用这个命令

`vue create vue-test3`

如果继续使用cli2中的命令，init 。。。就会创建vue2的项目

![image-20210711193123648](https://gitee.com/zzursy/blog-image/raw/master/img/20210711193123.png "image-20210711193123648")

![http://img3.mukewang.com/60db26e20001ca0a04100148.jpg](https://gitee.com/zzursy/blog-image/raw/master/img/20210711200640.jpeg "http://img3.mukewang.com/60db26e20001ca0a04100148.jpg")

![http://img.mukewang.com/60db275d000124c109380474.jpg](https://gitee.com/zzursy/blog-image/raw/master/img/20210711200649.jpeg "http://img.mukewang.com/60db275d000124c109380474.jpg")

use history mode for router(Y/N)Y

In package.json

后面都默认
