# let 关键字

*   let 变量不能重复声明，使用var可以

*   块儿级作用域 （全局，函数，eval ），只在代码块中有效，var是在全局作用域中添加属性，任何地方都可以调用

    *   块级作用域有{}、if、else、where、for

*   let不存在变量提升（不允许在变量声明之前使用该变量），var可以，但是输出结果为undefined

*   不影响作用域链

```javascript
<script>
      //声明变量
      let a;
      let b, c, d;
      let e = 100;
      let f = 521, g = 'iloveyou', h = [];

      //1. 变量不能重复声明
      // let star = '罗志祥';
      // let star = '小猪';

      //2. 块儿级作用域  全局, 函数, eval
      // if else while for 
      // {
      //     let girl = '周扬青';
      // }
      // console.log(girl);

      //3. 不存在变量提升
      // console.log(song);
      // let song = '恋爱达人';

      //4. 不影响作用域链
      {
          let school = '尚硅谷';
          // fu函数里没有school变量，会向上一级找
          function fn() {
              console.log(school);
          }
          // 首先执行fn函数
          fn();
      }

</script>
```
