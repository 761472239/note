# Vue-router

## vue-router入门

**安装vue-router**

`npm install vue-router --save-dev`

**解读**​**文件**

```js
import Vue from 'vue'   //引入Vue
import Router from 'vue-router'  //引入vue-router
import Hello from '@/components/Hello'  //引入根目录下的Hello.vue组件

Vue.use(Router)  //Vue全局使用Router

export default new Router({
    routes: [              //配置路由，这里是个数组
        {                    //每一个链接都是一个对象
            path: '/',         //链接路径
            name: 'Hello',     //路由名称，
            component: Hello   //对应的组件模板
        }
    ]
})
```

**使用步骤**

1.  在`src/components`目录下，新建`Hi.vue`文件

2.  编写内容

3.  引入Hi组件

    ```js
     import Hi from '@/components/Hi'
    ```

4.  增加路由配置

    ```json
    {
        path:'/hi',
        name:'Hi',
        component:Hi
    }
    ```

**router-link制作导航**

router-link 类似a 标签

```html
<router-link to="/">[显示字段]</router-link>
```

**编写App.vue**

```html
<p>导航 ：
    <router-link to="/">首页</router-link>
    <router-link to="/hi">Hi页面</router-link>
</p>
```

![image-20210715092845797](https://gitee.com/zzursy/blog-image/raw/master/img/20210715092845.png "image-20210715092845797")

## 配置子路由

```js
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import Hi from '@/components/Hi'
import Hi1 from '@/components/Hi1'
import Hi2 from '@/components/Hi2'

Vue.use(Router)

export default new Router({
    routes: [
        {
            path: '/',
            name: 'HelloWorld',
            component: HelloWorld
        }, {
            path: '/hi',
            component: Hi,
            children: [
                {
                    path: '/hi1',
                    component: Hi1
                }, {
                    path: '/hi2',
                    component: Hi2
                }
            ]
        }
    ]
})

```

## vue-router传递参数

**第一种（不推荐）**

1.  在路由文件`src/router/index.js`里配置name属性

2.  模板里`(src/App.vue)`用{`{{$route.name}}`的形式接收

**第二种（推荐）**

通过`<router-link>` 标签中的to传参。

传参的语法：

```html
<router-link :to="{name:xxx,params:{key:value}}">valueString</router-link>
```

这里的to前边是带冒号的，后边跟的是一个对象形式的键值对.

*   `name`：就是我们在路由配置文件中起的name值。

*   `params`：就是我们要传的参数，它也是对象，在对象里可以传递多个值

接下来修改App.vue里的`<router-link>`标签。

```html
<router-link v-bind:to="{name:'Hi1',params:{key:'传递值Hi1'}}">Hi1页面</router-link>
<router-link :to="{name:'Hi2',params:{key:'传递值Hi2'}}">Hi2页面</router-link>
```

在路由配置文件中添加name，最后在模板里(src/components/App.vue)用`$route.params.username`进行接收。

![image-20210715100154656](https://gitee.com/zzursy/blog-image/raw/master/img/20210715100154.png "image-20210715100154656")

## 单页面多路由区域操作

**实际需求：**

*   在一个页面里我们有2个以上`<router-view>`区域，我们通过配置路由的js文件，来操作这些区域的内容。

例如我们在src/App.vue里加上两个`<router-view>`标签。我们用`vue-cli`建立了新的项目，并打开了src目录下的App.vue文件，在`<router-view>`下面新写了两行`<router-view>`标签,并加入了些CSS样式。

**App.vue**

```html
<template>
  <div id="app">
    <img src="./assets/logo.png" />
    <router-view></router-view>
    <router-view
      name="left"
      style="float: left; width: 50%; background-color: #ccc; height: 300px"
    ></router-view>

    <router-view
      name="right"
      style="float: right; width: 50%; background-color: #c0c; height: 300px"
    ></router-view>
    <router-link to="/">首页</router-link> |
    <router-link to="/hi">Hi页面</router-link> 
  </div>
</template>

```

**router/index.js**

```js
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import Hi from '@/components/Hi'
import Hi1 from '@/components/Hi1'
import Hi2 from '@/components/Hi2'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      components: {
        default: HelloWorld,
        left: Hi1,
        right: Hi2
      }
    }, {
      path: '/hi',
      name: "Hi",
      components: {
        default: HelloWorld,
        left: Hi2,
        right: Hi1
      }
    }
  ]
})
```

![image-20210715101736651](https://gitee.com/zzursy/blog-image/raw/master/img/20210715101736.png "image-20210715101736651")

## vue-router 利用url传递参数

1.  \*\*【:冒号】的形式传递参数 \*\* 我们可以在理由配置文件里以  **【:冒号】** 的形式传递参数，这就是对参数的绑定。 在配置文件里以冒号的形式设置参数。我们在`/src/router/index.js`文件里配置路由。

```json
{
    path:'/params/:newsId/:newsTitle',
     component:Params
}
```

我们需要传递参数是新闻ID`（newsId）`和新闻标题`（newsTitle）`

1.  在`src/components`目录下建立我们`params.vue`组件，也可以说是页面。我们在页面里输出了url传递的的新闻ID和新闻标题。

```html
<template>
    <div>
        <h2>{{ msg }}</h2>
        <p>新闻ID：{{ $route.params.newsId}}</p>
        <p>新闻标题：{{ $route.params.newsTitle}}</p>
    </div>
</template>

<script>
export default {
  name: 'params',
  data () {
    return {
      msg: 'params page'
    }
  }
}
</script>
```

1.  在App.vue文件里加入我们的`<router-view>`标签。这时候我们可以直接利用url传值了。

```html
<router-link to="/params/198/jspang website is very good">params</router-link> |
```

**正则表达式在URL传值中的应用**

加入正则需要在路由配置文件里（`/src/router/index.js`）以圆括号的形式加入

```json
{
    path:'/params/:newsId(\\d+)/:newsTitle',
    component:Params
}
```

## vue-router重定向

**1. redirect基本重定向**

在路由配置文件中，把原来的component换成redirect参数就可以了

```js
export default new Router({
    routes: [
        {
            path: '/',
            component: Hello
        },{
            path:'/params/:newsId(\\d+)/:newsTitle',
            component:Params
        },{
            path:'/goback',
            redirect:'/'
        }

    ]
})
```

这里我们设置了goback路由，但是它并没有配置任何component（组件），而是直接redirect到`path:"/"`路径下，这就是一个简单的重新定向。

**2. 重定向时传递参数**

```json
{
    path:'/params/:newsId(\\d+)/:newsTitle',
    component:Params
},{
    path:'/goParams/:newsId(\\d+)/:newsTitle',
    redirect:'/params/:newsId(\\d+)/:newsTitle'
}
```

## alias别名的使用

使用alias别名的形式，我们也可以实现类似重定向的效果。

1.  创建别名

```json
{
    path: '/hi1',
    component: Hi1,
    alias:'/jspang'
}
```

1.  使用

```html
<router-link < to="/jspang">jspang</router-link>
```

❌ 别名请不要用在path为`"/"`中，如下代码的别名是不起作用的

```json
{
  path: '/',
  component: Hello,
  alias:'/home'
}
```

## 路由的过渡动画

[第8节：路由的过渡动画](https://jspang.com/detailed?id=25#toc28 "第8节：路由的过渡动画")

## mode和404

\*\*mode的两个值 \*\*

1.  `histroy`:当你使用 `history` 模式时，URL 就像正常的 url，例如 `http://jsapng.com/lms/`，也好看！

2.  `hash`:默认’hash’值，但是hash看起来就像无意义的字符排列，不太好看也不符合我们一般的网址浏览习惯。

\*\*404页面的设置： \*\*

用户会经常输错页面，当用户输错页面时，我们希望给他一个友好的提示，为此美工都会设计一个漂亮的页面，这个页面就是我们常说的404页面。vue-router也为我们提供了这样的机制.

1.  设置我们的路由配置文件（`/src/router/index.js`）：

```json
{
    path:'*',
    component:Error
}
```

这里的`path:’*’`就是找不到页面时的配置，`componen`t是我们新建的一个`Error.vue`的文件。

1.  新建404页面：

在`/src/components/`文件夹下新建一个`Error.vue`的文件。简单输入一些有关错误页面的内容。

```html
<template>
    <div>
        <h2>{{ msg }}</h2>
    </div>
</template>
<script>
    export default {
        data () {
            return {
                msg: 'Error:404'
            }
        }
    }
</script>
```

1.  我们在用`<router-link>`瞎写一个标签的路径。

```纯文本
<router-link to="/bbbbbb">我是瞎写的</router-link> |
```

预览一下我们现在的结果，就已经实现404页面的效果。

## 路由中的钩子

\*\*路由配置文件中的钩子函数 \*\*

我们可以直接在路由配置文件`（/src/router/index.js）`中写钩子函数。但是在路由文件中我们只能写一个beforeEnter,就是在进入此路由配置时。先来看一段具体的代码：

```json
{
    path:'/params/:newsId(\\d+)/:newsTitle',
    component:Params,
    beforeEnter:(to,from,next)=>{
        console.log('我进入了params模板');
        console.log(to);
        console.log(from);
        next();
    },
```

我们在params路由里配置了bdforeEnter得钩子函数，函数我们采用了ES6的箭头函数，需要传递三个参数。我们并在箭头函数中打印了to和from函数。具体打印内容可以在控制台查看object。

三个参数：

1.  `to`:路由将要跳转的路径信息，信息是包含在对像里边的。

2.  `from`:路径跳转前的路径信息，也是一个对象的形式。

3.  `next`:路由的控制参数，常用的有next(true)和next(false)。

![image-20210715114005233](https://gitee.com/zzursy/blog-image/raw/master/img/20210715114005.png "image-20210715114005233")

\*\*写在模板中的钩子函数 \*\* 在配置文件中的钩子函数，只有一个钩子-beforeEnter，如果我们写在模板中就可以有两个钩子函数可以使用：

*   beforeRouteEnter：在路由进入前的钩子函数。

*   beforeRouteLeave：在路由离开前的钩子函数。

```js
export default {
    name: 'params',
    data () {
        return {
            msg: 'params page'
        }
    },
    beforeRouteEnter:(to,from,next)=>{
        console.log("准备进入路由模板");
        next();
    },
    beforeRouteLeave: (to, from, next) => {
        console.log("准备离开路由模板");
        next();
    }
}
</script>
```

这是我们写在params.vue模板里的路由钩子函数。它可以监控到路由的进入和路由的离开，也可以轻易的读出to和from的值。

## 编程式导航

前10节课的导航都是用`<router-link>`标签或者直接操作地址栏的形式完成的，那如果在业务逻辑代码中需要跳转页面我们如何操作？这就是我们要说的编程式导航，顾名思义，就是在业务逻辑代码中实现导航。

***

这两个编程式导航的意思是后退和前进，功能跟我们浏览器上的后退和前进按钮一样，这在业务逻辑中经常用到。比如条件不满足时，我们需要后退。

`router.go(-1)`代表着后退，我们可以让我们的导航进行后退，并且我们的地址栏也是有所变化的。

1.  我们先在`app.vue`文件里加入一个按钮，按钮并绑定一个`goback()`方法。

```纯文本
<button @click="goback">后退</button>
```

1.  在我们的script模块中写入goback()方法，并使用this.\$router.go(-1),进行后退操作。

```html
<script>
    export default {
        name: 'app',
        methods:{
            goback(){
                this.$router.go(-1);
            }
        }
    }
</script>
```

打开浏览器进行预览，这时我们的后退按钮就可以向以前的网页一样后退了。

`router.go(1)`:代表着前进，用法和后退一样

***

```纯文本
this.$router.push(‘/xxx ‘)
```

这个编程式导航的作用就是跳转，比如我们判断用户名和密码正确时，需要跳转到用户中心页面或者首页，都用到这个编程的方法来操作路由。

我们设置一个按钮，点击按钮后回到站点首页。

1.  先编写一个按钮，在按钮上绑定`goHome( )`方法。

```纯文本
<button @click="goHome">回到首页</button>
```

1.  在`<script`>模块里加入`goHome`方法，并用`this.$router.push(‘/’)`导航到首页

```js
export default {
    name: 'app',
    methods:{
        goback(){
            this.$router.go(-1);
        },
        goHome(){
            this.$router.push('/');
        }
    }
}
```
