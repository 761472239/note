# TortoiseGit安装配置

## 目录

*   [1 TortoiseGit简介](#1-tortoisegit简介)

*   [2 下载安装](#2-下载安装)

*   [3 TortoiseGit配置](#3-tortoisegit配置)

*   [4 Git文件上角标符号说明](#4-git文件上角标符号说明)

*   [5 将代码提交到服务器](#5-将代码提交到服务器)

## 1 TortoiseGit简介

参考地址 [https://www.cnblogs.com/xiuxingzhe/p/9312929.html](https://www.cnblogs.com/xiuxingzhe/p/9312929.html "https://www.cnblogs.com/xiuxingzhe/p/9312929.html")

**tortoiseGit**是一个开放的git版本控制系统的源客户端。

*   git是命令行模式

*   tortoiseGit界面化操作模式，不用记git相关命令就可以直接操作

## 2 下载安装

安装顺序：先安装程序包，然后安装语言包。

安装说明：因为TortoiseGit 只是一个程序壳，必须依赖一个 Git Core，**所以安装前请确定已完成git安装和配置。**

可参考：Git安装：[https://www.cnblogs.com/xiuxingzhe/p/9300905.html](https://www.cnblogs.com/xiuxingzhe/p/9300905.html "https://www.cnblogs.com/xiuxingzhe/p/9300905.html")

　　　　Git生成秘钥及GitLab配置：[ http://www.cnblogs.com/xiuxingzhe/p/9303278.html](http://www.cnblogs.com/xiuxingzhe/p/9303278.html " http://www.cnblogs.com/xiuxingzhe/p/9303278.html")

## 3 TortoiseGit配置

## 4 Git文件上角标符号说明

文件上的图标，可以反映出当前文件或者文件夹的状态：

　　1、正常的：\*\*绿色的对号 \*\*

![img](https://gitee.com/isrsy/blog-image/raw/master/img/20210706205653.png "img")

　　2、被修改过的：\*\*红色感叹号 \*\*

![img](https://gitee.com/isrsy/blog-image/raw/master/img/20210706205652.png "img")

　　3、新添加的：**蓝色的加号**

![img](https://gitee.com/isrsy/blog-image/raw/master/img/20210706205650.png "img")

　　4、未受控的（无版本控制的）：**蓝色的问号**

![img](https://gitee.com/isrsy/blog-image/raw/master/img/20210706205648.png "img")

　　5、忽略不受控的：**灰色的减号**

![img](https://gitee.com/isrsy/blog-image/raw/master/img/20210706205646.png "img")

　　6、删除的：\*\*红色的x号 \*\*

![img](https://gitee.com/isrsy/blog-image/raw/master/img/20210706205644.png "img")

　　7、有冲突的：**黄色的感叹号**&#x20;

![img](https://gitee.com/isrsy/blog-image/raw/master/img/20210706205642.png "img")

若小乌龟图标看不见自行解决。[https://www.cnblogs.com/dmcl/p/12330832.html](https://www.cnblogs.com/dmcl/p/12330832.html "https://www.cnblogs.com/dmcl/p/12330832.html")

## 5 将代码提交到服务器

参考：[https://www.cnblogs.com/anayigeren/p/10177027.html](https://www.cnblogs.com/anayigeren/p/10177027.html "https://www.cnblogs.com/anayigeren/p/10177027.html")

Git的使用类似TFS、SVN等源代码或者文件管理器，惯例的流程：

![img](https://gitee.com/isrsy/blog-image/raw/master/img/20210706205639.png "img")

\*\*第一步：\*\* 改动，修改本地项目中的某些文件，如修改 README.md 内容，还可以增加一些文件， 如Hello.txt。

**第二步：** 提交本地，在本地项目的空白处点击鼠标右键，选择 【Git提交(C) -> "master"...】

![img](https://gitee.com/isrsy/blog-image/raw/master/img/20210706205633.png "img")

在弹出提交（Commit）对话框中完成提交说明信息，和选择需要提交的文件，可根据需要新建分支，然后点击 【提交】 按钮，将修改提交到**本地仓库：**

![img](https://gitee.com/isrsy/blog-image/raw/master/img/20210706205631.png "img")

弹出提交进度窗口，提交成功后还需要“推送”将本地仓库的修改推送到**远程仓库**。

![img](https://gitee.com/isrsy/blog-image/raw/master/img/20210706205626.png "img")

**第三步：** 同步拉取，在实际工作中，如果多人协作或者多个客户端进行修改，那么我们还要拉取别人推送到在线仓库的内容，所以在推送之前需要先执行同步拉取(Pull ...)操作。

　　在本地仓库文件夹上【右击鼠标】→【Git同步】：

![img](https://gitee.com/isrsy/blog-image/raw/master/img/20210706205622.png "img")

打开Git同步窗口（包括常规操作及日志，同右击菜单快捷操作一样），点击【拉取(P)】，将远程分支拉取到本地：

![img](https://gitee.com/isrsy/blog-image/raw/master/img/20210706205617.png "img")

如果服务器上的文件没有被修改过，就会直接提示已经更新到最新，那你就可以直接进行下一步“推送(H)”操作了：

![img](https://gitee.com/isrsy/blog-image/raw/master/img/20210706205615.png "img")

反之，如果服务器上的文件被修改过了（本地文件修改前不是最新版本），就会提示冲突。先要解决冲突，然后再提交结果：

![img](https://gitee.com/isrsy/blog-image/raw/master/img/20210706205612.png "img")

**需要注意的是，和使用TFS、SVN的习惯一样，你在修改本地内容之前，最好先 拉取（pull）一下，减少冲突的可能。**

**第四步：推送远程**，将提交到本地仓库的修改推送到远程仓库，可以直接在提交成功后的提示窗口上点击【推送(H)...】，或者在Git同步窗口点击【推送(H)...】，鼠标右击的菜单上也有相应的快捷操作：

选择 【TortoiseGit(T)】→【推送(H)...】

![img](https://gitee.com/isrsy/blog-image/raw/master/img/20210706205606.png "img")

一般保持默认,点击 “确定” 按钮

　　然后弹出推送进度界面，可能要求你输入用户名，点击【确定】，然后要求输入密码，密码输入正确后，显示推送成功界面：

![img](https://gitee.com/isrsy/blog-image/raw/master/img/20210706205602.png "img")

　如果你按照上一小节**Tortoisegit 配置**的设置操作，则输入密码以后会记住密码。密码会明文保存在C:\Users\用户名.git-credentials 这个文件中，请小心保存。
