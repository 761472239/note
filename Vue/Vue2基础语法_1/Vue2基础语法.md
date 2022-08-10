# Vue2基础语法

## ---------------第一季 内部指令--------------

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

## ---------------第二季 全局API--------------

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

## 父子组件关系

在实际开发中我们经常会遇到在一个自定义组件中要使用其他自定义组件，这就需要一个父子组件关系。

局部组件是在构造器内部编写的，如果组件代码量很大，会影响构造器的可读性，造成拖拉和错误。

1.  我们把组件编写的代码放到构造器外部或者说单独文件。

2.  我们需要先声明一个对象,对象里就是组件的内容。

3.  声明好对象后在构造器里引用就可以了。

```html
    <div id="app">
      <jspang></jspang>
    </div>

    <script>
      var city = {
        template: `<div>Sichuan of China!</div>`,
      };
      var jspang = {
        template: `<div>
                    <p> Panda from China!</p>
                    <city></city>
                    </div>`,
        components: {
          city: city,
        },
      };

      var app = new Vue({
        el: "#app",
        data: {},
        components: {
          jspang: jspang,
        },
      });
    </script>
```

![image-20210714153101904](https://gitee.com/zzursy/blog-image/raw/master/img/20210714153101.png "image-20210714153101904")

## 动态绑定组件

```html

<div id="app">
    <component v-bind:is="who"></component>
    <button @click="changeComponent">changeComponent</button>
</div>

<script>
    var componentA = {
        template: `<div style="color:red;">I'm componentA</div>`,
    };
    var componentB = {
        template: `<div style="color:green;">I'm componentB</div>`,
    };
    var componentC = {
        template: `<div style="color:yellow;">I'm componentC</div>`,
    };

    //   function changeComponent(){

    //   }
    var app = new Vue({
        el: "#app",
        data: { who: componentA },
        components: {
            componentA: componentA,
            componentB: componentB,
            componentC: componentC,
        },
        methods: {
            changeComponent: function () {
                if (this.who == "componentA") {
                    this.who = "componentB";
                } else if (this.who == "componentB") {
                    this.who = "componentC";
                } else {
                    this.who = "componentA";
                }
            },
        },
    });
</script>
```

![image-20210714154840785](https://gitee.com/zzursy/blog-image/raw/master/img/20210714154840.png "image-20210714154840785")

## ------------第三季 构造器里的选项-----------

## propsData option 全局扩展的数据传递

propsData 不是和属性有关，他用在全局扩展时进行传递数据。先回顾一下全局扩展的知识，作一个的扩展标签出来。实际我们并不推荐用全局扩展的方式作自定义标签，我们学了组件，完全可以使用组件来做，这里只是为了演示propsData的用法。

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <script type="text/javascript" src="../assets/js/vue.js"></script>
        <title>PropsData Option Demo</title>
    </head>
    <body>
        <h1>PropsData Option Demo</h1>
        <hr>
        <header></header>

        <script type="text/javascript">
            var  header_a = Vue.extend({
                template:`<p>{{message}}</p>`,
                data:function(){
                    return {
                        message:'Hello,I am Header'
                    }
                }
            }); 
            new header_a().$mount('header');
        </script>
    </body>
</html>
```

扩展标签已经做好了，这时我们要在挂载时传递一个数字过去，我们就用到了propsData。 我们用propsData三步解决传值：

1.  在全局扩展里加入props进行接收。`propsData:{a:1} `

2.  传递时用propsData进行传递。`props:['a']`

3.  用插值的形式写入模板。`{{ a }}`
    完整代码：

```javascript
var  header_a = Vue.extend({
    template:`<p>{{message}}-{{a}}</p>`,
    data:function(){
        return {
            message:'Hello,I am Header'
        }
    },
    props:['a']
}); 
new header_a(
    {
        propsData:{
            a:1
        }
    }
).$mount('header');
```

propsData在实际开发中我们使用的并不多，我们在后边会学到Vuex的应用，他的作用就是在单页应用中保持状态和数据的。

## computed Option 计算选项

computed 的作用主要是对原数据进行改造输出。多用于输出之前。

包括格式的编辑，大小写转换，顺序重排，添加符号……。

**computed 属性是非常有用，在输出数据前可以轻松的改变数据。**

*   格式化输出结果

```html
<div id="app">{{newPrice}}</div>

<script>
    var app = new Vue({
        el: "#app",
        data: {
            price: 100,
        },
        computed: {
            newPrice: function () {
                return  ("￥" + this.price + "元");
            },
        },
    });
</script>
```

*   改变数组

## Methods Option  方法选项

*   methods中的参数的传递
    ` <button @click="add(2,$event)">add+2</button><br />`

*   methods中的`$event`参数，event包含了大部分鼠标事件的属性

*   native 给组件绑定构造器里的原生事件
    在实际开发中经常需要把某个按钮封装成组件，然后反复使用，如何让组件调用构造器里的方法，而不是组件里的方法。就需要用到我们的`.native`修饰器了。

*   作用域外部调用构造器里的方法-(不推荐)

```html
<div id="app">
    分数：{{point}}
    <br />
    <button @click="add(2,$event)">add+2</button><br />
    <but @click.native="add(3)"></but>
</div>
<button onclick="app.add(4)">构造器外部ADD+4</button>

<script>
    var but = {
        template: `<button>组件ADD+3</button>`,
    };
    var app = new Vue({
        el: "#app",
        data: {
            point: 10,
        },
        methods: {
            add: function (num, event) {
                this.point += num;
                console.log(event);
            },
        },
        // 挂载
        components: {
            but: but,
        },
    });
</script>
```

![image-20210714164243424](https://gitee.com/zzursy/blog-image/raw/master/img/20210714164243.png "image-20210714164243424")

![image-20210714164643329](https://gitee.com/zzursy/blog-image/raw/master/img/20210714164643.png "image-20210714164643329")

## Watch 选项 监控数据

**需求：**

温度大于26度时，我们建议穿T恤短袖，温度小于26度大于0度时，我们建议穿夹克长裙，温度小于0度时我们建议穿棉衣羽绒服。

**用实例属性写watch监控**

```html
    <div id="app">
      <p>今日温度：{{temperature}}</p>
      <p>穿衣建议：{{this.suggestion}}</p>
      <p>
        <button @click="add">添加温度</button>
        <button @click="reduce">减少温度</button>
      </p>
    </div>

    <script>
      var suggestion = ["T恤短袖", "夹克长裙", "棉衣羽绒服"];

      var app = new Vue({
        el: "#app",
        data: { temperature: 14, suggestion: "T恤短袖" },
        methods: {
          add: function () {
            this.temperature += 5;
          },
          reduce: function () {
            this.temperature -= 5;
          },
        },
        // watch: {
        //   temperature: function (newVal, oldVal) {
        //     if (newVal >= 26) {
        //       this.suggestion = suggestion[0];
        //     } else if (newVal < 26 && newVal >= 0) {
        //       this.suggestion = suggestion[1];
        //     } else {
        //       this.suggestion = suggestion[2];
        //     }
        //   },
        // },
      });
      app.$watch("temperature", function (newVal, oldVal) {
        if (newVal >= 26) {
          this.suggestion = suggestion[0];
        } else if (newVal < 26 && newVal >= 0) {
          this.suggestion = suggestion[1];
        } else {
          this.suggestion = suggestion[2];
        }
      });
    </script>
```

## Mixins 混入选项操作

1.  在你已经写好了构造器后，需要增加方法或者临时的活动时使用的方法，这时用混入会减少源代码的污染。

2.  很多地方都会用到的公用方法，用混入的方法可以减少代码量，实现代码重用。

**需求：**

我们现在有个数字点击递增的程序，假设已经完成了，这时我们希望每次数据变化时都能够在控制台打印出提示：“数据发生变化”

![image-20210714173820933](https://gitee.com/zzursy/blog-image/raw/master/img/20210714173821.png "image-20210714173820933")

```html
<div id="app">
    <p>num:{{ num }}</p>
    <P><button @click="add">增加数量</button></P>
</div>

<script type="text/javascript">
    //额外临时加入时，用于显示日志
    var addLog={
        updated:function(){
            console.log("数据放生变化,变化成"+this.num+".");
        }
    }
    var app=new Vue({
        el:'#app',
        data:{
            num:1
        },
        methods:{
            add:function(){
                this.num++;
            }
        },
        mixins:[addLog]//混入
    })
</script>
```

### 混入的执行顺序

从执行的先后顺序来说，都是混入的先执行，然后构造器里的再执行，需要注意的是，这并不是方法的覆盖，而是被执行了两边。

在上边的代码的构造器里我们也加入了updated的钩子函数：

```javascript
updated:function(){
      console.log("构造器里的updated方法。")
},
```

这时控制台输出的顺序是：

> mixins 数据放生变化，变化成 2
> 构造器里的updated方法。
> **PS：当混入方法和构造器的方法重名时，混入的方法无法展现，也就是不起作用。**

### 全局API混入方式

我们也可以定义全局的混入，这样在需要这段代码的地方直接引入js，就可以拥有这个功能了。我们来看一下全局混入的方法：

```javascript
Vue.mixin({
    updated:function(){
        console.log('我是全局被混入的');
    }
})
```

PS：全局混入的执行顺序要前于混入和构造器里的方法。

## Extends Option 扩展选项

```html
<div id="app">
    {{message}}
    <p><button @click="add">add</button></p>
</div>
<script type="text/javascript">
    var extendsObj = {
        created: function () {
            console.log("我是被扩展出来的");
        },
        methods: {
            add: function () {
                console.log("我是被扩展出来的方法！");
            },
        },
    };

    var app = new Vue({
        el: "#app",
        data: {
            message: "hello Vue!",
        },
        methods: {
            add: function () {
                console.log("我是原生方法");
            },
        },
        extends: extendsObj,
    });
</script>
```

![image-20210714175525933](https://gitee.com/zzursy/blog-image/raw/master/img/20210714175526.png "image-20210714175525933")

有点像继承，被扩展出来的方法如果和构造器中的方法名一样，会先执行构造器中的方法。

## delimiters 选项

`delimiters: ["${", "}"]`,`{{message}}`这样的取值就不能用了，需要使用​`${message} `才能取值

## ------------第四季 实例和内置组件-----------

## 实例属性

*   Vue和Jquery一起使用

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <script type="text/javascript" src="../assets/js/vue.js"></script>
        <script type="text/javascript" src="../assets/js/jquery-3.1.1.min.js"></script>
        <title>Early Examples Demo</title>
    </head>
    <body>
        <h1>Early Examples Demo</h1>
        <hr>
        <div id="app">
            {{message}}
        </div>

        <script type="text/javascript">
            var app=new Vue({
                el:'#app',
                data:{
                    message:'hello Vue!'
                },
                //在Vue中使用jQuery
                mounted:function(){
                    $('#app').html('我是jQuery!');
                }
                ,
                methods:{
                    add:function(){
                        console.log("调用了Add方法");
                    }
                }
            });
            app.add();
        </script>
    </body>
</html>
```

**实例调用自定义方法**

`app.add();`

PS：我们有可能把`app.add()`的括号忘记或省略，这时候我们得到的就是方法的字符串，但是并没有执行，所以必须要加上括号。

**这是Vue的属性**

![image-20210714214550074](https://gitee.com/zzursy/blog-image/raw/master/img/20210714214550.png "image-20210714214550074")

## 实例方法

### \$mount

\$mount方法是用来挂载我们的扩展的，我们先来复习一下扩展的写法。

这里我们作了jspang的扩展，然后用\$mount的方法把jspang挂载到DOM上，我们也生成了一个Vue的实例，直接看代码。

```html
<div id="app">{{message}}</div>

<script>
    var jspang = Vue.extend({
        template: `<p>{{message}}</p>`,
        data: function () {
            return {
                message: "Hello ,I am JSPang",
            };
        },
    });
    var vm = new jspang().$mount("#app");
</script>
```

### \$destroy() 卸载方法

```javascript
function destroy(){
   vm.$destroy();
}
```

### \$forceUpdate() 更新方法

```javascript
vm.$forceUpdate()
```

### \$nextTick() 数据修改方法

当Vue构造器里的data值被修改完成后会调用这个方法，也相当于一个钩子函数吧，和构造器里的updated生命周期很像。

```javascript
function tick(){
    vm.message="update message info ";
    vm.$nextTick(function(){
        console.log('message更新完后我被调用了');
    })
}
```

## 实例事件

### \$on

在构造器外部添加事件

### \$emit

如果按钮在作用域外部，可以利用\$emit来执行。

\$on接收两个参数，第一个参数是调用时的事件名称，第二个参数是一个匿名方法。

### \$once

执行一次的事件

### \$off

关闭事件

```html
<div id="app">
    <p>{{num}}</p>
    <button @click="add">add</button>
</div>
<p><button onclick="reduce()">reduce</button></p>
<p><button onclick="reduceOnce()">reduceOnce</button></p>
<p><button onclick="off()">off</button></p>

<script type="text/javascript">
    var app=new Vue({
        el:'#app',
        data:{num:1},
        methods:{
            add:function(){
                this.num++;
            }
        }
    })
    //实例事件
    app.$on('reduce',function(){
        console.log('执行了reduce()');
        this.num--;
    });
    //只使用一次的实例方法
    app.$once('reduceOnce',function(){
        console.log('只执行一次的方法');
        this.num--;
    });
    //关闭事件
    function off(){
        app.$off('reduce');
    }
    //外部调用内部事件
    function reduce(){
        app.$emit('reduce');
    }
    function reduceOnce(){
        app.$emit('reduceOnce');
    }
</script>
```

## slot内置组件

看官网
