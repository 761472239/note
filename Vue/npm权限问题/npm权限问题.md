# npm权限问题

## operation not permitted

在执行`npm install *** `的时候没有权限，必须使用管理员权限

## 修改npm全局和缓存路径

1.  创建两个文件夹

    ![image-20210710154337115](https://gitee.com/zzursy/blog-image/raw/master/img/20210710154337.png "image-20210710154337115")

2.  命令行中设置

3.  修改用户环境变量

4.  修改系统环境变量

5.  npm config list

    ![image-20210710154230787](https://gitee.com/zzursy/blog-image/raw/master/img/20210710154238.png "image-20210710154230787")

## install模块报错

![image-20210710154433578](https://gitee.com/zzursy/blog-image/raw/master/img/20210710154433.png "image-20210710154433578")

这里它说是我的权限不足，但是我之前npm install 模块的时候是没问题的，最后我查资料发现是缓存问题导致的。

删除这个文件 `c:\user\username\ .npmrc`

重新安装即可
