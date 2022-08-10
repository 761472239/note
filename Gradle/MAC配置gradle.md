# MAC配置gradle

## 目录

* [注意事项](#注意事项)

* [Idea与gradle](#idea与gradle)

## 注意事项

上述情况适合使用系统自带的shell ， 若本地安装了zsh , 则上述操作只能适用于当前窗口，需要进行下面进一步的操作。

当mac机器上安装了zsh后 .bash\_profile 文件中的环境变量就无法起到作用。

**接下来的解决方案：**

1） cd \~

2） open .zshrc

3） 在.zshrc文件末尾增加.bash\_profile的引用：source \~/.bash\_profile

4）更新文件 source ～/.bash\_profile

## Idea与gradle

当mac本地配置完成gradle的环境变量的时候，用idea创建新的项目，会自动在\${user\_home}/.gradle目录下载gradle的源码文件（版本和.bash\_profile中配置的版本一样）

/Users/rsy/.gradle/wrapper/dists
