# JS合并数组【5种】

## 目录

*   [For循环](#for循环)

*   [concat](#concat)

*   [apply](#apply)

*   [ES6语法](#es6语法)

*   [push(…arr)](#pusharr)

## For循环

```javascript
let arr = [1, 2]
let arr2 = [3, 4]

for (let i in arr2) {
    arr.push(arr2[i])
}

console.log(arr)
// [1, 2, 3, 4]

```

## concat

concat() 方法用于连接两个或多个数组。
该方法不会改变现有的数组，而仅仅会返回被连接数组的一个副本。

```javascript
let arr = [1, 2]
let arr2 = [3, 4]

arr = arr.concat(arr2)

console.log(arr)
// [1, 2, 3, 4]

```

## apply

**方法重用**

apply() 方法调用一个具有给定this值的函数，以及以一个数组（或类数组对象）的形式提供的参数。

```javascript
let arr = [1, 2]
let arr2 = [3, 4]

arr.push.apply(arr, arr2)

console.log(arr)
// [1, 2, 3, 4]

```

## ES6语法

```javascript
let arr = [1, 2]
let arr2 = [3, 4]

arr = [...arr, ...arr2]

console.log(arr)
// [1, 2, 3, 4]

```

## push(…arr)

```javascript
let arr = [1, 2]
let arr2 = [3, 4]

arr.push(...arr2)

console.log(arr)
// [1, 2, 3, 4]

```
