# Idea更新、合并分支

## 目录

*   [把master代码合并到dev分支上](#把master代码合并到dev分支上)

*   [把dev分支代码合并到master上](#把dev分支代码合并到master上)

*   [更新开发分支的代码](#更新开发分支的代码)

# 把master代码合并到dev分支上

1.  切换本地当前分支为master，如果已经在master上就不用切换
    checkout master

2.  更新最新的代码，确保代码是最新的
    pull&#x20;

3.  然后再切换会dev分支上，在Local Branches中选择master分支，选择merge，这样就把本地的master merge到 本地仓库的dev上，然后再选择git push ，这样就把远程master merge到 dev分支上, 并会提示: Merged master to dev。

# 把dev分支代码合并到master上

和上面类似

# 更新开发分支的代码

首先切换到主分支，然后拉取代码，再切回自己的分支，将主分支代码合并到自己的分支。

git checkout master
git pull
git checkout dev
git merge master
