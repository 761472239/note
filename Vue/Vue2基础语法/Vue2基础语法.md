# Vue2基础语法

## 目录

*   [开始使用](#开始使用)

*   [v-if和v-show](#v-if和v-show)

*   [v-for](#v-for)

    *   [排序循环输出](#排序循环输出)

    *   [排序对象](#排序对象)

*   [v-text & v-html](#v-text--v-html)

*   [v-on：绑定事件监听器](#v-on绑定事件监听器)

*   [v-model指令](#v-model指令)

    *   [绑定其他组件](#绑定其他组件)

*   [v-bind 指令](#v-bind-指令)

*   [其他内部指令(v-pre & v-cloak & v-once)](#其他内部指令v-pre--v-cloak--v-once)

*   [Vue.directive 自定义指令](#vuedirective-自定义指令)

*   [Vue.extend构造器的延伸](#vueextend构造器的延伸)

*   [Vue.set全局操作](#vueset全局操作)

*   [Template 制作模版](#template-制作模版)

    *   [1、直接写在template选项后边](#1直接写在template选项后边)

    *   [2、写在template标签里面](#2写在template标签里面)

    *   [3、写在script标签里面](#3写在script标签里面)

*   [Component 初识组件](#component-初识组件)

    *   [全局注册组件](#全局注册组件)

    *   [局部注册组件](#局部注册组件)

    *   [组件和指令的区别](#组件和指令的区别)

*   [组件props属性设置](#组件props属性设置)

    *   [在构造器里向组件中传值](#在构造器里向组件中传值)

## 开始使用

```javascript
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
```

```html
<!DOCTYPE html>
<html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>

    <body>
        <!-- 开发环境版本，包含了有帮助的命令行警告 -->
        <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
        
        <div id="app">
            {{ message }}
        </div>

        <script>
            var app = new Vue({
                el: "#app",
                data: {
                    message: "hello",
                }
            })
        </script>
    </body>
</html>

```

## v-if和v-show

```html

<div id="app">
    {{ message }}
    <br />
    <span v-if="isTrue">{{ msg }}</span>
</div>

<script>
    var app = new Vue({
        el: "#app",
        data: {
            message: "hello",
            msg: "hello world",
            isTrue: true,
            items: [1, 2, 3, 4, 5]
        }
    })
    app.fullName = 'John Doe'
    console.log(app.fullName)

```

## v-for

```html
<div id="app">
    {{ message }}
    <br />
    <span v-if="isTrue">{{ msg }}</span>
    <span>for 循环</span>
    <ul v-for="item in items">
        <li>{{item}}</li>
    </ul>
</div>

<script>
    var app = new Vue({
        el: "#app",
        data: {
            message: "hello",
            msg: "hello world",
            isTrue: true,
            items: [1, 2, 3, 4, 5]
        }
    })
    app.fullName = 'John Doe'
    console.log(app.fullName)
</script>
```

![image-20210713222654017](https://gitee.com/zzursy/blog-image/raw/master/img/20210713222654.png "image-20210713222654017")

### 排序循环输出

```html

<div id="app">
    <span>for 循环</span>
    <ul v-for="item in sortItems">
        <li>{{item}}</li>
    </ul>
</div>

<script>
    function sortNumber(a, b) {
        return a - b;
    }
    var app = new Vue({
        el: "#app",
        data: {
            items: [82, 65, 33, 46, 57]
        },
        computed: {
            sortItems: function () {
                return this.items.sort(sortNumber);
            }
        }
    });

</script>
```

![image-20210713223757456](https://gitee.com/zzursy/blog-image/raw/master/img/20210713223757.png "image-20210713223757456")

### 排序对象

```html
<ul>
    <li v-for="student in sortStudent">
        {{student.name}} - {{student.age}}
    </li>
</ul>
<script>
    var app = new Vue({
        el: "#app",
        data: {
            students: [
                { name: 'jspang', age: 32 },
                { name: 'Panda', age: 30 },
                { name: 'PanPaN', age: 21 },
                { name: 'King', age: 45 }
            ]
        },
        computed: {
            sortStudent: function () {
                return sortByKey(this.students, 'age');
            }
        }
    });
    //数组对象方法排序:
    function sortByKey(array, key) {
        return array.sort(function (a, b) {
            var x = a[key];
            var y = b[key];
            return ((x < y) ? -1 : ((x > y) ? 1 : 0));
        });
    }
</script>
```

![image-20210713224345813](https://gitee.com/zzursy/blog-image/raw/master/img/20210713224345.png "image-20210713224345813")

## v-text & v-html

```html
<div id="app">

    <span>{{ message }}</span>=<span v-text="message"></span><br />
    <span v-html="msgHtml"></span>
</div>

<script>

    var app = new Vue({
        el: "#app",
        data: {
            message: 'hello Vue!',
            msgHtml: '<h2>hello Vue!</h2>'
        }
    })
</script>
```

## v-on：绑定事件监听器

v-on 就是监听事件，可以用v-on指令监听DOM事件来触发一些javascript代码。

编写一个加分减分的程序

    \<div id="app">
      分数:{{point}}\<br />
      \<button v-on:click="add">加分
      \<button @click="subtract">减分
    

```html
<script>
    var app = new Vue({
        el: "#app",
        data: {
            point: 0,
        },
        methods: {
            add: function () {
                return ++this.point;
            },
            subtract: function () {
                return --this.point;
            },
        },
    });
</script>
```

## v-model指令

v-model指令，绑定数据源在特定的表单上

![image-20210714091315109](https://gitee.com/zzursy/blog-image/raw/master/img/20210714091315.png "image-20210714091315109")

```html
<div id="app">
    <p>原始文本信息：{{message}}</p>
    <h3>文本框</h3>
    <input type="text" v-model="message" />
</div>

<script>
    var app = new Vue({
        el: "#app",
        data: {
            message: "hello Vue!",
        },
    });
</script>
```

*   .lazy：取代 input 监听 change 事件
    `<input type="text" v-model.lazy="message">`

*   .number：输入字符串转为数字
    `<input type="text" v-model.number="message">`

*   .trim：输入去掉首尾空格

![image-20210714092305628](https://gitee.com/zzursy/blog-image/raw/master/img/20210714092340.png "image-20210714092305628")

### 绑定其他组件

```html

<h3>文本域</h3>
<textarea  < cols="30" rows="10" v-model="message"></textarea>

<h3>多选按钮绑定一个值</h3>
<input type="checkbox" id="isTrue" v-model="isTrue">
<label for='isTrue'>{{isTrue}}</label>

<h3>多选绑定一个数组</h3>
<input type="checkbox" id="JSPang" value="JSPang" v-model="web_Names" />
<label for="JSPang">JSPang</label><br />
<input type="checkbox" id="Panda" value="Panda" v-model="web_Names" />
<label for="JSPang">Panda</label><br />
<input type="checkbox" id="PanPan" value="PanPan" v-model="web_Names" />
<label for="JSPang">PanPan</label>
<p>{{web_Names}}</p>

<h3>单选按钮绑定</h3>
<input type="radio" id="one" value="男" v-model="sex">
<label for="one">男</label>
<input type="radio" id="two" value="女" v-model="sex">
<label for="one">女</label>
<p>{{sex}}</p>
```

![image-20210714093144863](https://gitee.com/zzursy/blog-image/raw/master/img/20210714093144.png "image-20210714093144863")

![image-20210714093734928](https://gitee.com/zzursy/blog-image/raw/master/img/20210714093734.png "image-20210714093734928")

## v-bind 指令

```html
<!-- 完整语法 -->
<a v-bind:href="url"></a>
<!-- 缩写 -->
<a :href="url"></a>
```

```html
<div id="app">
    <div>默认情况</div>
    <div v-bind:class="className">1、绑定classA</div>
    <div v-bind:class="className">1、绑定classA（简单写法 :class ）</div>
    <div :class="{classA:false}">2、绑定class中的判断(isOk=false)</div>
    <div :class="{classA:isOk}">2、绑定class中的判断(isOk=true)</div>
    <div :class="[classA,classB]">3、绑定class中的数组</div>
    <div :class="isOk?classA:classB">4、绑定class中的三元表达式判断</div>
    <div :style="{color:red,fontSize:font}">5、绑定style</div>
    <div :style="styleObject">6、用对象绑定style样式</div>
</div>

<script>
    var app = new Vue({
        el: "#app",
        data: {
            className: "classA",
            isOk: true,
            classA: "classA",
            classB: "classB",
            styleObject: {
                fontSize: "24px",
                color: "green",
            },
        },
    });
</script>
```

![image-20210714100503666](https://gitee.com/zzursy/blog-image/raw/master/img/20210714100503.png "image-20210714100503666")

## 其他内部指令(v-pre & v-cloak & v-once)

> [v-pre指令](https://jspang.com/detailed?id=21#toc328 "v-pre指令")

在模板中跳过vue的编译，直接输出原始值。就是在标签中加入v-pre就不会输出vue中的data值了。

```纯文本
<div v-pre>{{message}}</div>
```

这时并不会输出我们的message值，而是直接在网页中显示{{message}}

> [v-cloak指令](https://jspang.com/detailed?id=21#toc329 "v-cloak指令")

在vue渲染完指定的整个DOM后才进行显示。它必须和CSS样式一起使用，

```html
[v-cloak] {
  display: none;
}
<div v-cloak>
    {{ message }}
</div>
```

> [v-once指令](https://jspang.com/detailed?id=21#toc330 "v-once指令")

在第一次DOM时进行渲染，渲染完成后视为静态内容，跳出以后的渲染过程。

```html
<div v-once>第一次绑定的值：{{message}}</div>
<div><input type="text" v-model="message"></div>
```

## Vue.directive 自定义指令

```html

<div id="app">
    <div v-jspang="color" id="demo">{{num}}</div>
    <div>
        <button @click="add">Add</button>
        <button onclick="unbind()">unbind</button>
    </div>
</div>

<script>
    // 解绑
    function unbind() {
        app.$destroy();
    }

    Vue.directive("jspang", {
        bind: function (el, binding, vnode) {
            //被绑定
            console.log("1 - bind");
            el.style = "color:" + binding.value;
        },
        inserted: function () {
            //绑定到节点
            console.log("2 - inserted");
        },
        update: function () {
            //组件更新
            console.log("3 - update");
        },
        componentUpdated: function () {
            //组件更新完成
            console.log("4 - componentUpdated");
        },
        unbind: function () {
            //解绑
            console.log("5 - bind");
        },
    });

    var app = new Vue({
        el: "#app",
        data: {
            num: 10,
            color: "green",
        },
        methods: {
            add: function () {
                this.num++;
            },
        },
    });
</script>
```

![image-20210714110950383](https://gitee.com/zzursy/blog-image/raw/master/img/20210714110950.png "image-20210714110950383")

## Vue.extend构造器的延伸

Vue.extend 返回的是一个“扩展实例构造器”,也就是预设了部分选项的Vue实例构造器。经常服务于Vue.component用来生成组件，可以简单理解为当在模板中遇到该组件名称作为标签的自定义元素时，会自动调用“扩展实例构造器”来生产组件实例，并挂载到自定义元素上。

```html
<--使用组件的两种方式-->
<div id="author"></div>
<author></author>

<script type="text/javascript">
    var authorExtend = Vue.extend({
        template:"<p><a :href='authorUrl'>{{authorName}}</a></p>",
        data:function(){
            return{
                authorName:'JSPang',
                authorUrl:'http://www.jspang.com'
            }
        }
    });
    new authorExtend().$mount('author');
</script>
```

**挂载到普通标签上**
还可以通过HTML标签上的id或者class来生成扩展实例构造器，Vue.extend里的代码是一样的，只是在挂载的时候，我们用类似jquery的选择器的方法，来进行挂载就可以了。

```javascript
new authorExtend().$mount('#author');
```

## Vue.set全局操作

Vue生命周期

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Document</title>
    </head>

    <body>
        <!-- 开发环境版本，包含了有帮助的命令行警告 -->
        <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>

        <div id="app">
            <h1>构造器的生命周期</h1>
            <span>分数:{{point}}</span><br />
            <button v-on:click="add">绑定加分</button>
            <button v-on:click="unbind">解绑</button>
        </div>

        <script>
            var app = new Vue({
                el: "#app",
                data: {
                    point: 0,
                },
                methods: {
                    add: function () {
                        ++this.point;
                    },
                    unbind: function () {
                        app.$destroy();
                    },
                },
                beforeCreate: function () {
                    console.log("1-beforeCreate 初始化之后");
                },
                created: function () {
                    console.log("2-created 创建完成");
                },
                beforeMount: function () {
                    console.log("3-beforeMount 挂载之前");
                },
                mounted: function () {
                    console.log("4-mounted 被创建");
                },
                beforeUpdate: function () {
                    console.log("5-beforeUpdate 数据更新前");
                },
                updated: function () {
                    console.log("6-updated 被更新后");
                },
                activated: function () {
                    console.log("7-activated");
                },
                deactivated: function () {
                    console.log("8-deactivated");
                },
                beforeDestroy: function () {
                    console.log("9-beforeDestroy 销毁之前");
                },
                destroyed: function () {
                    console.log("10-destroyed 销毁之后");
                },
            });
        </script>
    </body>
</html>
```

![image-20210714114646948](https://gitee.com/zzursy/blog-image/raw/master/img/20210714114647.png "image-20210714114646948")

## Template 制作模版

### 1、直接写在template选项后边

```html

    <div id="app">
      <p>{{message}}</p>
      <br />
    </div>

    <script>
      var app = new Vue({
        el: "#app",
        data: {
          message: "hello world!",
        },
        template: `<h1>hello world!</h1>`,
      });
    </script>
```

### 2、写在template标签里面

```html
   <template id="demo2">
             <h2 style="color:red">我是template标签模板</h2>
    </template>

    <script type="text/javascript">
        var app=new Vue({
            el:'#app',
            data:{
                message:'hello Vue!'
            },
            template:'#demo2'
        })
    </script>
```

### 3、写在script标签里面

这种写法还可以让模板文件从外部引入

```html
<script type="x-template" id="demo3">
        <h2 style="color:red">我是script标签模板</h2>
</script>

<script type="text/javascript">
    var app=new Vue({
        el:'#app',
        data:{
            message:'hello Vue!'
        },
        template:'#demo3'
    })
</script>
```

![image-20210714120706561](https://gitee.com/zzursy/blog-image/raw/master/img/20210714120706.png "image-20210714120706561")

`    <p>{{message}}</p>`，这个用法只显示template中的代码

## Component 初识组件

> 定义没有属性的组件

### 全局注册组件

```html
    <div id="app">
      <jspang></jspang>
    </div>
    <script>
      // 注册全局组件
      Vue.component("jspang", {
        template: `<div style="color:blue;">全局注册组件</div>`,
      });
      var app = new Vue({
        el: "#app",
        data: {},
      });
    </script>
```

全局化就是在构造器的外部用Vue.component来注册

### 局部注册组件

局部注册组件局部注册组件和全局注册组件是向对应的，局部注册的组件只能在组件注册的作用域里进行使用，其他作用域使用无效。

```html
    <div id="app">
      <panda></panda>
    </div>
    <script>
      var app = new Vue({
        el: "#app",
        data: {},
        components: {
          panda: {
            template: `<div style="color:red;">局部注册组件</div>`,
          },
        },
      });
    </script>
```

![image-20210714142038280](https://gitee.com/zzursy/blog-image/raw/master/img/20210714142038.png "image-20210714142038280")

### 组件和指令的区别

组件注册的是一个标签，而指令注册的是已有标签里的一个属性。在实际开发中我们还是用组件比较多，指令用的比较少。

> 定义有属性的组件

## 组件props属性设置

```html
<div id="app">
    <panda here="中国"></panda>
</div>

<script>
    var app = new Vue({
        el: "#app",
        data: {},
        components: {
            panda: {
                template: `<div style="color:blue;">熊猫在{{here}}</div>`,
                props: ["here"],
            },
        },
    });
</script>
```

![image-20210714142758947](https://gitee.com/zzursy/blog-image/raw/master/img/20210714142759.png "image-20210714142758947")

注意：在Vue中不支持`form-here`这种命名规则的属性

使用驼峰命名规则。

### 在构造器里向组件中传值

```html
<div id="app">
    <panda v-bind:here="message"></panda>
</div>

<script>
    var app = new Vue({
        el: "#app",
        data: { message: `中国，吃果子` },
        components: {
            panda: {
                template: `<div style="color:blue;">熊猫在{{here}}</div>`,
                props: ["here"],
            },
        },
    });
</script>
```
