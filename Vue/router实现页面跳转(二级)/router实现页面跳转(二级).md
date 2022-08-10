# router实现页面跳转(二级)

## 创建文件

在`src\components`文件下创建几个文件

**First.vue**

```html
<template>
    <div>
        <router-link to="/a"> 转向A</router-link>,
        <router-link to="/b"> 转向B</router-link>
    </div>
</template>
<script>
</script>
<style>
</style>
```

**A.vue**

```html
<template>
    <div>
        我是A , <router-link to="/">返回</router-link>
    </div>
</template>
```

**B.vue**

```html
<template>
    <div>
        我是B,<router-link to="/">返回</router-link>
    </div>
</template>
```

## 修改路由文件

修改路径下的文件，`src\router\index.js`

```js
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import First from '@/components/First'
import A from '@/components/A'
import B from '@/components/B'

Vue.use(Router)

export default new Router({
    routes: [
        {
            path: '/',
            name: 'First',
            component: First
        },
        {
            path: '/a',
            component: A
        }
        , {
            path: '/b',
            component: B
        }
    ]
})

```

## 运行结果

![image-20210711132642572](https://gitee.com/zzursy/blog-image/raw/master/img/20210711132651.png "image-20210711132642572")

![image-20210711132750721](https://gitee.com/zzursy/blog-image/raw/master/img/20210711132750.png "image-20210711132750721")
