# slice、splice、split

*   slice 切片
    `str.slice(start,end)`
    返回截取的元素（字符串或者是数组）（必选，可选）

*   splice 拼接
    `arr.splice(start,num,arg1,arg2...)`（必选，必选，可选...）
    向/从数组中添加/删除项目，然后返回被删除的项目。

    ```javascript
    let arr = [1,2,3,4,5,6,7,8,9]
    // 从第四个开始。依次向后删除两个。
    arr.splice(3,2)
    // 删除两个，同时添加三个元素
    // 在 4 5 的位置上添加123
    arr.splice(3,2,1,2,3)
    // num 为0 不删除元素，同时在下标为3的元素的位置插入三个元素
    // 原数组改变为：[1, 2, 3, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    arr.splice(3,0,1,2,3)

    ```

*   split 分割
    `stringObject.split(`**`,`**`)` （必选，可选）
    split() 方法用于把一个字符串分割成字符串数组。
