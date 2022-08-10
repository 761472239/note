## 1、IDEA HTML中无法跳转到引入的JS/CSS文件



问题：

在基于 IDEA 工具开发 SpringBoot项目的过程中发现，在HTML中无法按照以往的方式通过 command + click 的方式跳转到引入的 JS 或 CSS 文件去，IDEA 会提示 “Cannot find declaration to go to” 。

解决方案：

将引入文件的上一级文件夹架配置成 Resource 即可。

例如：页面引入的JS路径为 `/js/admin/teacher_management.js`，那么就需要将“js” 的上一级文件夹`“static/js/...” ` 配置为“Resource”就可以了。 



![image-20210513155604914](https://gitee.com/zzursy/blog-image/raw/master/img/20210513155612.png)





## 2、idea的相关问题









