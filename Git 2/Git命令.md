# Git命令

## 添加仓库

```git
git remote add origin 远程仓库地址
如果已经连接某个仓库，需要更改
git remote set-url origin 修改后的远程仓库地址

```

## 新建分支和切换分支

```git
git branch name
git checkout xxx
git pull
git checkout -b xxx
```

## 拉取远程分支

```git
git fetch         # 将远程主机的更新，全部取回本地
git branch -a     # 查看远程所有的分支
git branch        # 查看本地分支
git checkout -b   # 远程分支名 origin/远程分支名
git checkout      # 分支名（本地分支）切换分支
```

### 删除分支

```git
git push origin -d 分支名
git branch -d 分支名
```

假如我直接到github删除了某个分支，我在本地使用git branch -a查看远程分支，依然存在并且可以切换使用。我本地也想把远程分支记录删除怎么办？

```git
git branch -a
git remote show origin       # 查看远程分支和本地仓库分支的记录
git remote prune origin      # 来删除远程仓库已经删除过的分支
git branch -a                # 验证一下

```

## 两种合并方式的区别

一共有两种合并方式 Git Merge 和 Git ReBase

*   Git Merge：这种合并方式是将两个分支的历史合并到一起，现在的分支不会被更改，它会比对双方不同的文件缓存下来，生成一个commit，去push。

*   Git ReBase：这种合并方法通常被称为“衍合”。它是提交修改历史，比对双方的commit，然后找出不同的去缓存，然后去push，修改commit历史。
