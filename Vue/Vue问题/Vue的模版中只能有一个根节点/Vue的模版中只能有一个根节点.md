# Vue的模版中只能有一个根节点

vue的模版中只能有一个根节点，所以在\<template>中插入第二个元素就会报错

![](image/image_dsu9bDjuLf.png)

解决方案：

将\<template>中的元素用一个大的\<div>包起来，这样就可以在其中添加多个元素了，可以参考以下示例：
