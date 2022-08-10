官方：

[The Missing Package Manager for macOS (or Linux) — Homebrew](https://brew.sh/)

[M1芯片Mac上Homebrew安装教程 - 知乎](https://zhuanlan.zhihu.com/p/341831809)

[**Mac安装Homebrew_rockvine的博客-CSDN博客_mac中安装homebrew**](https://blog.csdn.net/rockvine/article/details/121895416)

各个环境下Homebrew的安装路径：  Intel安装目录： /usr/local/Homebrew  m1 arm安装目录： /opt/homebrew  linux安装目录：/home/linuxbrew

如果上述命令执行卡在如下信息处

```text
==> Tapping homebrew/core
Cloning into ‘/opt/homebrew/Library/Taps/homebrew/homebrew-core’…
```

请Control+C中断脚本执行如下命令：

```text
cd "$(brew --repo)/Library/Taps/"
mkdir homebrew && cd homebrew
git clone git://mirrors.ustc.edu.cn/homebrew-core.git
```

安装cask 同样也有安装失败或者卡住的问题，解决方法也是一样：

```text
cd "$(brew --repo)/Library/Taps/"
cd homebrew
git clone https://mirrors.ustc.edu.cn/homebrew-cask.git
```

# 一、brew 安装脚本 （自动选择软件源）

`/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"`

# 二、brew 卸载脚本

`/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/HomebrewUninstall.sh)"`

# 三、常用命令

安装软件：brew install xxx

卸载软件：brew uninstall xxx

搜索软件：brew search xxx

更新软件：brew upgrade xxx

查看列表：brew list

更新brew：brew update

清理所有包的旧版本：brew cleanup

清理指定包的旧版本：brew cleanup $FORMULA

查看可清理的旧版本包，不执行实际操作：brew cleanup -n