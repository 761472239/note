# set

```javascript
<script>
    //声明一个 set
    let s = new Set();
    let s2 = new Set(['大事儿', '小事儿', '好事儿', '坏事儿', '小事儿']);

    //元素个数
    console.log(s2.size);
    //添加新的元素
    s2.add('喜事儿');
    //删除元素
    s2.delete('坏事儿');
    //检测
    console.log(s2.has('糟心事'));
    //清空
    s2.clear();
    console.log(s2);

    for (let v of s2) {
        console.log(v);
    }

</script>
```

## set的使用

1.  数组去重

    ```javascript
      let arr = [1,2,3,4,5,4,3,2,1];
      let result = [...new Set(arr)];
      console.log(result); 
    ```

2.  取交集

    ```javascript
    let arr2 = [4, 5, 6, 5, 6];
      let result = [...new Set(arr)].filter(item => {
          let s2 = new Set(arr2); // 4 5 6
          if (s2.has(item)) {
              return true;
          } else {
              return false;
          }
      });
    // let result = [...new Set(arr)].filter(item => new Set(arr2).has(item));
    console.log(result); 
    ```

3.  并集

    ```javascript
    let union = [...new Set([...arr, ...arr2])];
    console.log(union);
    ```

4.  差集

    ```javascript
    let diff = [...new Set(arr)].filter(item => !(new Set(arr2).has(item)));
    console.log(diff);
    ```
