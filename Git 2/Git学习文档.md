# Git学习文档

## 目录

*   [安装配置](#安装配置)

*   [常见命令](#常见命令)

*   [创建SSH Key](#创建ssh-key)

*   [重要操作](#重要操作)

*   [时空穿梭机](#时空穿梭机)

    *   [版本回退](#版本回退)

    *   [工作区和暂存区](#工作区和暂存区)

    *   [管理修改](#管理修改)

    *   [撤销修改](#撤销修改)

    *   [删除文件](#删除文件)

*   [远程仓库](#远程仓库)

    *   [添加仓库](#添加仓库)

    *   [从远程库克隆](#从远程库克隆)

*   [分支管理](#分支管理)

    *   [创建合并分支](#创建合并分支)

        *   [switch（重要）](#switch重要)

        *   [小结](#小结)

    *   [解决冲突](#解决冲突)

        *   [小结](#小结-1)

    *   [分支策略](#分支策略)

    *   [Bug分支（重点）](#bug分支重点)

    *   [Feature分支](#feature分支)

    *   [多人协作](#多人协作)

        *   [抓取分支](#抓取分支)

        *   [小结](#小结-2)

    *   [Rebase](#rebase)

*   [标签管理](#标签管理)

    *   [创建标签](#创建标签)

        *   [创建标签](#创建标签-1)

        *   [小结](#小结-3)

    *   [操作标签](#操作标签)

## 安装配置

安装省略.....

**最后一步的配置**

```powershell
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```

因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。

注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

## 常见命令

1.  创建版本库

    ```powershell
    $ git init
    Initialized empty Git repository in /Users/michael/learngit/.git/
    ```

2.  把文件添加到版本库

    ```powershell
    $ git add file1.txt
    $ git add file2.txt file3.txt
    ```

3.  提交到仓库，可以多次add，提交一次

    ```powershell
    $ git commit -m "add 3 files."
    ```

4.  添加文件到Git仓库，分两步：

    1.  使用命令`git add <file>`，注意，可反复多次使用，添加多个文件；

    2.  使用命令`git commit -m <message>`，完成。

5.  `git status`告诉你有文件被修改过

6.  用`git diff`可以查看修改内容

7.  `git log`命令显示从最近到最远的提交日志

    ```powershell
    $ git log --pretty=oneline
    1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master) append GPL
    e475afc93c209a690c39c13a46716e8fa000c366 add distributed
    eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0 wrote a readme file
    简化版本
    ```

8.  把当前版本`append GPL`回退到上一个版本`add distributed`，就可以使用`git reset`命令：
    `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭
    使用命令`git reset --hard commit_id`。

    ```powershell
    $ git reset --hard HEAD^
    HEAD is now at e475afc add distributed
    ```

9.  再回到版本`append GPL`，1094a.....就是上面的`append GPL`的`commit id`是1094adb...

    ```powershell
    $ git reset --hard 1094a
    HEAD is now at 83b0afe append GPL
    ```

10. `git reflog`查看命令历史
    现在，你回退到了某个版本，关掉了电脑，第二天早上就后悔了，想恢复到新版本怎么办？找不到新版本的`commit id`怎么办？
    在Git中，总是有后悔药可以吃的。当你用`$ git reset --hard HEAD^`回退到`add distributed`版本时，再想恢复到`append GPL`，就必须找到`append GPL`的commit id。Git提供了一个命令`git reflog`用来记录你的每一次命令：

    ```powershell
    $ git reflog
    e475afc HEAD@{1}: reset: moving to HEAD^
    1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
    e475afc HEAD@{3}: commit: add distributed
    eaadf4e HEAD@{4}: commit (initial): wrote a readme file
    ```

11. `git checkout -- file`可以丢弃工作区的修改。
    `git checkout -- file`命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到`git checkout`命令。

12. `git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区

13. 直接删除文件，工作区和版本库就不一致了，`git status`命令会立刻告诉你哪些文件被删除了，
    现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`：
    先手动删除文件，然后使用`git rm <file>`和`git add <file>`效果是一样的。
    另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：
    `git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

    ```powershell
    $ git rm test.txt
    rm 'test.txt'

    $ git commit -m "remove test.txt"
    [master d46f35e] remove test.txt
     1 file changed, 1 deletion(-)
     delete mode 100644 test.txt
    ```

    ```powershell
    $ git checkout -- test.txt
    ```

## 创建SSH Key

1.  在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：
    `id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

    ```powershell
    ssh-keygen -t rsa -C "761472239@qq.com"
    ```

## 重要操作

*   添加远程仓库

```powershell
$ git remote add origin git@github.com:michaelliao/learngit.git
```

请千万注意，把上面的`michaelliao`替换成你自己的GitHub账户名，否则，你在本地关联的就是我的远程库，关联没有问题，但是你以后推送是推不上去的，因为你的SSH Key公钥不在我的账户列表中。

**添加后，远程库的名字就是****，这是Git默认的叫法，也可以改成别的，但是**​**这个名字一看就知道是远程库。**

*   把本地库的所有内容推送到远程库上

```powershell
$ git push -u origin master
```

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。之后就可以直接使用`git push origin master`。

*   当你第一次使用Git的`clone`或者`push`命令连接GitHub时，会得到一个警告：
    这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入`yes`回车即可。
    Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了：
    这个警告只会出现一次，后面的操作就不会有任何警告了。

    ### SSH警告

    ```powershell
    The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
    RSA key fingerprint is xx.xx.xx.xx.xx.
    Are you sure you want to continue connecting (yes/no)?
    ```

    ```纯文本
    Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
    ```

*   **删除远程库**
    如果添加的时候地址写错了，或者就是想删除远程库，可以用`git remote rm <name>`命令。使用前，建议先用`git remote -v`查看远程库信息：
    然后，根据名字删除，比如删除`origin`：
    **此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除。**

    ```powershell
    $ git remote -v
    origin  git@github.com:michaelliao/learn-git.git (fetch)
    origin  git@github.com:michaelliao/learn-git.git (push)
    ```

    ```powershell
    $ git remote rm origin
    ```

*   **克隆仓库**
    Git支持多种协议，包括`https`，但`ssh`协议速度最快。

    ```powershell
    git clone git@github.com:michaelliao/gitskills.git
    ```

## 时空穿梭机

### 版本回退

*   `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。

*   穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。

*   要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

### 工作区和暂存区

暂存区是Git非常重要的概念，弄明白了暂存区，就弄明白了Git的很多操作到底干了什么。

没弄明白暂存区是怎么回事的童鞋，请向上滚动页面，再看一次。

### 管理修改

每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中。

### 撤销修改

又到了小结时间。

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考[版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192 "版本回退")一节，不过前提是没有推送到远程库。

### 删除文件

命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失**最近一次提交后你修改的内容**。

## 远程仓库

### 添加仓库

要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；

关联一个远程库时必须给远程库指定一个名字，`origin`是默认习惯命名；

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！

### 从远程库克隆

要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。

Git支持多种协议，包括`https`，但`ssh`协议速度最快。

## 分支管理

### 创建合并分支

首先，我们创建`dev`分支，然后切换到`dev`分支：

```powershell
$ git checkout -b dev
Switched to a new branch 'dev'
```

`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：

```powershell
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```

**然后，用**​**命令查看当前分支：**

```powershell
$ git branch
* dev
  master
```

`git branch`命令会列出所有分支，当前分支前面会标一个`*`号。

然后，我们就可以在`dev`分支上正常提交，比如对`readme.txt`做个修改，加上一行代码。

然后提交：

```powershell
$ git add readme.txt 
$ git commit -m "branch test"
[dev b17d20e] branch test
 1 file changed, 1 insertion(+)
```

现在，`dev`分支的工作完成，我们就可以切换回`master`分支：

```powershell
$ git checkout master
Switched to branch 'master'
```

切换回`master`分支后，再查看一个`readme.txt`文件，刚才添加的内容不见了！因为那个提交是在`dev`分支上，而`master`分支此刻的提交点并没有变：

![git-br-on-master](https://gitee.com/zzursy/blog-image/raw/master/img/20210707084636.png "git-br-on-master")

**现在，我们把**​**分支的工作成果合并到****分支上：** ​

```powershell
$ git merge dev
Updating d46f35e..b17d20e
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

`git merge`命令用于合并指定分支到当前分支。合并后，再查看`readme.txt`的内容，就可以看到，和`dev`分支的最新提交是完全一样的。

注意到上面的`Fast-forward`信息，Git告诉我们，这次合并是“快进模式”，也就是直接把`master`指向`dev`的当前提交，所以合并速度非常快。

当然，也不是每次合并都能`Fast-forward`，我们后面会讲其他方式的合并。

合并完成后，就可以放心地删除`dev`分支了：

```纯文本
$ git branch -d dev
Deleted branch dev (was b17d20e).
```

删除后，查看`branch`，就只剩下`master`分支了：

```纯文本
$ git branch
* master
```

#### switch（重要）

我们注意到切换分支使用`git checkout <branch>`，而前面讲过的撤销修改则是`git checkout -- <file>`，同一个命令，有两种作用，确实有点令人迷惑。

实际上，切换分支这个动作，用`switch`更科学。因此，最新版本的Git提供了新的`git switch`命令来切换分支：

创建并切换到新的`dev`分支，可以使用：

```纯文本
$ git switch -c dev
```

直接切换到已有的`master`分支，可以使用：

```纯文本
$ git switch master
```

使用新的`git switch`命令，比`git checkout`要更容易理解。

#### 小结

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`或者`git switch <name>`

创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

### 解决冲突

创建分支feature1

```powershell
$ git switch -c feature1
修改文件readme.txt
`Creating a new branch is quick AND simple.`
$ git add readme.txt
$ git commit -m "AND simple"
```

切换到master分支

```powershell
$ git switch master
Git还会自动提示我们当前master分支比远程的master分支要超前1个提交。
修改文件readme.txt
`Creating a new branch is quick & simple.`
$ git add readme.txt 
$ git commit -m "& simple"
```

现在，`master`分支和`feature1`分支各自都分别有新的提交，变成了这样：

![git-br-feature1](https://gitee.com/zzursy/blog-image/raw/master/img/20210707084902.png "git-br-feature1")

这种情况下，Git无法“快速合并”，只能试图把各自的修改合并起来，但是这种合并可能会引发冲突，试试看：

```powershell
$ git merge feature1
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
自动合并 readme.txt

CONFLICT（内容）：在 readme.txt 中合并冲突

自动合并失败； 修复冲突，然后提交结果。
```

Git告诉我们，`readme.txt`文件存在冲突，必须手动解决冲突后再提交。`git status`也可以告诉我们冲突的文件。

我们可以直接查看readme.txt的内容：

```powershell
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
<<<<<<< HEAD
`Creating a new branch is quick & simple.`
=======
`Creating a new branch is quick AND simple.`
>>>>>>> feature1
```

再次修改文件

```powershell
修改文件readme.md
`Creating a new branch is quick and simple.`
$ git add readme.txt 
$ git commit -m "conflict fixed"

[master cf810e4] conflict fixed
冲突修复
```

现在，`master`分支和`feature1`分支变成了下图所示：

![git-br-conflict-merged](https://gitee.com/zzursy/blog-image/raw/master/img/20210707091115.png "git-br-conflict-merged")

用带参数的`git log`也可以看到分支的合并情况：

```powershell
$ git log --graph --pretty=oneline --abbrev-commit
*   cf810e4 (HEAD -> master) conflict fixed
|\  
| * 14096d0 (feature1) AND simple
* | 5dc6824 & simple
|/  
* b17d20e branch test
* d46f35e (origin/master) remove test.txt
* b84166e add test.txt
* 519219b git tracks changes
* e43a48b understand how stage works
* 1094adb append GPL
* e475afc add distributed
* eaadf4e wrote a readme file
```

最后删除分支`git branch -d feature1`。

#### 小结

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

用`git log --graph`命令可以看到分支合并图。

### 分支策略

*   **快速模式**
    Git合并分支时，默认使用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

*   **普通模式**
    如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

```powershell
git merge --no-ff -m "merge with no-ff" dev

git log --graph --pretty=oneline --abbrev-commit
```

请注意`--no-ff`参数，表示禁用`Fast forward`

再用`git log`命令查看分支历史

可以看到，不使用`Fast forward`模式，merge后就像这样：

![git-no-ff-mode](https://gitee.com/zzursy/blog-image/raw/master/img/20210707092026.png "git-no-ff-mode")

### Bug分支（重点）

[https://www.liaoxuefeng.com/wiki/896043488029600/900388704535136](https://www.liaoxuefeng.com/wiki/896043488029600/900388704535136 "https://www.liaoxuefeng.com/wiki/896043488029600/900388704535136")

### Feature分支

添加新功能的分支

[https://www.liaoxuefeng.com/wiki/896043488029600/900394246995648](https://www.liaoxuefeng.com/wiki/896043488029600/900394246995648 "https://www.liaoxuefeng.com/wiki/896043488029600/900394246995648")

### 多人协作

多人协作需要抓取和推送，首先需要查看origin地址，如果没有权限，就看不到push的地址。

```powershell
$ git remote       简单
$ git remote -v    详细
```

但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

*   `master`分支是主分支，因此要时刻与远程同步；

*   `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；

*   bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；

*   feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

#### 抓取分支

多人协作时，大家都会往`master`和`dev`分支上推送各自的修改。

1.  克隆matser分支

    ```powershell
    $ git clone git@github.com:michaelliao/learngit.git
    ```

2.  现在，你的小伙伴要在`dev`分支上开发，就必须创建远程`origin`的`dev`分支到本地，于是他用这个命令创建本地`dev`分支：

    ```powershell
    $ git checkout -b dev origin/dev

    ```

.....

[https://www.liaoxuefeng.com/wiki/896043488029600/900375748016320](https://www.liaoxuefeng.com/wiki/896043488029600/900375748016320 "https://www.liaoxuefeng.com/wiki/896043488029600/900375748016320")

#### 小结

*   查看远程库信息，使用`git remote -v`；

*   本地新建的分支如果不推送到远程，对其他人就是不可见的；

*   从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；

*   在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；

*   建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；

*   从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

### Rebase

曲线变直线

## 标签管理

### 创建标签

*   在需要打标签的分支下
    `git tag` 还可以查询所有的的标签

    ```powershell
    git tag v1.0
    ```

*   打过去提交的历史标签
    找到历史提交的commit id，然后打上就可以了：
    然后打印

    ```powershell
    $ git log --pretty=oneline --abbrev-commit

    12a631b (HEAD -> master, tag: v1.0, origin/master) merged bug fix 101
    4c805e2 fix bug 101
    e1e9c68 merge with no-ff
    `f52c633 add merge`
    cf810e4 conflict fixed
    5dc6824 & simple
    14096d0 AND simple
    b17d20e branch test
    d46f35e remove test.txt
    b84166e add test.txt
    519219b git tracks changes
    e43a48b understand how stage works
    1094adb append GPL
    e475afc add distributed
    eaadf4e wrote a readme file
    ```

    ```powershell
    git tag v0.9 f52c633
    ```

*   标签不是按时间顺序列出，而是按字母排序的。可以用`git show <tagname>`查看标签信息：
    还可以创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字：

    ```powershell
    $ git tag -a v0.1 -m "version 0.1 released" 1094adb
    ```

    > **标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。**

#### 创建标签

阅读: 38550684

***

在Git中打标签非常简单，首先，切换到需要打标签的分支上：

```纯文本
$ git branch
* dev
  master
$ git checkout master
Switched to branch 'master'
```

然后，敲命令`git tag <name>`就可以打一个新标签：

```纯文本
$ git tag v1.0
```

可以用命令`git tag`查看所有标签：

```纯文本
$ git tag
v1.0
```

默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？

方法是找到历史提交的commit id，然后打上就可以了：

```纯文本
$ git log --pretty=oneline --abbrev-commit
12a631b (HEAD -> master, tag: v1.0, origin/master) merged bug fix 101
4c805e2 fix bug 101
e1e9c68 merge with no-ff
f52c633 add merge
cf810e4 conflict fixed
5dc6824 & simple
14096d0 AND simple
b17d20e branch test
d46f35e remove test.txt
b84166e add test.txt
519219b git tracks changes
e43a48b understand how stage works
1094adb append GPL
e475afc add distributed
eaadf4e wrote a readme file
```

比方说要对`add merge`这次提交打标签，它对应的commit id是`f52c633`，敲入命令：

```纯文本
$ git tag v0.9 f52c633
```

再用命令`git tag`查看标签：

```纯文本
$ git tag
v0.9
v1.0
```

注意，标签不是按时间顺序列出，而是按字母排序的。可以用`git show <tagname>`查看标签信息：

```纯文本
$ git show v0.9
commit f52c63349bc3c1593499807e5c8e972b82c8f286 (tag: v0.9)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:56:54 2018 +0800

    add merge

diff --git a/readme.txt b/readme.txt
...
```

可以看到，`v0.9`确实打在`add merge`这次提交上。

还可以创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字：

```纯文本
$ git tag -a v0.1 -m "version 0.1 released" 1094adb
```

用命令`git show <tagname>`可以看到说明文字：

```纯文本
$ git show v0.1
tag v0.1
Tagger: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 22:48:43 2018 +0800

version 0.1 released

commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (tag: v0.1)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

diff --git a/readme.txt b/readme.txt
...
```

注意：标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。

#### 小结

*   命令`git tag <tagname>`用于新建一个标签，默认为`HEAD`，也可以指定一个commit id；

*   命令`git tag -a <tagname> -m "blablabla..."`可以指定标签信息；

*   命令`git tag`可以查看所有标签。

### 操作标签

*   命令`git push origin <tagname>`可以推送一个本地标签；

*   命令`git push origin --tags`可以推送全部未推送过的本地标签；

*   命令`git tag -d <tagname>`可以删除一个本地标签；

*   命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。
