# vue-cli2

## npm和cnpm的区别

1.  npm是nodejs的包管理器，用于node插件管理

2.  cnpm：因为npm安装插件是从国外服务器下载的，淘宝。。。使用了国内镜像代替了国外服务器

## -g 参数

*   全局安装

*   使用`npm  root -g` 查看全局安装的文件夹位置

## 安装vue-cli2

`npm install -g vue-cli`

`cnpm install -g vue-cli`

vue-cli的版本查看

```纯文本
vue -V
```

vue-cli的3.0+以后使用的不是vue-cli了，如果用以上的安装命令安装的并不是最新版的3.0+的，而如果安装3.0的话就需要使用新的

```纯文本
npm install @vue/cli -g
```

如果原来已经安装了vue-cli的话需要先卸载原来的安装

```纯文本
npm uninstall vue-cli -g
```

## 根据版本安装

`npm i -g vue-cli@2.9.6`

## 手动指定从哪个镜像服务器获取资源

1.  `npm install -gd express --registry=http://registry.npm.taobao.org`
    (使用频率不高,只有在安装脚手架的时候才会使用)

2.  为了避免每次安装都需要`--registry`参数,可以使用如下命令进行永久设置
    `npm config set registry http://registry.npm.taobao.org`

## -S参数

\-S

\--save

安装包信息将加入到dependencies（生产阶段的依赖）

## -D参数

\-D

相当于`--save-dev`

安装包信息将加入到devDependencies（开发阶段的依赖）

i 是 install 的缩写（注意：前面没有“-”）

`npm root -g`  查看全局安装的位置

`npm init -f` 配置文件的初始化

## 创建vue-cli项目

`vue init webpack ***`

输入命令后，会询问我们几个简单的选项，我们根据自己的需要进行填写就可以了。

*   Project name :项目名称 ，如果不需要更改直接回车就可以了。注意：这里不能使用大写，所以我把名称改成了vueclitest

*   Project description:项目描述，默认为A Vue.js project,直接回车，不用编写。

*   Author：作者，如果你有配置git的作者，他会读取。

*   Install vue-router? 是否安装vue的路由插件，我们这里需要安装，所以选择Y

*   Use ESLint to lint your code? 是否用ESLint来限制你的代码错误和风格。我们这里不需要输入n，如果你是大型团队开发，最好是进行配置。

*   setup unit tests with Karma + Mocha? 是否需要安装单元测试工具Karma+Mocha，我们这里不需要，所以输入n。

*   Setup e2e tests with Nightwatch?是否安装e2e来进行用户行为模拟测试，我们这里不需要，所以输入n。

## vue-cli项目结构

```纯文本
.
|-- build                            // 项目构建(webpack)相关代码
|   |-- build.js                     // 生产环境构建代码
|   |-- check-version.js             // 检查node、npm等版本
|   |-- dev-client.js                // 热重载相关
|   |-- dev-server.js                // 构建本地服务器
|   |-- utils.js                     // 构建工具相关
|   |-- webpack.base.conf.js         // webpack基础配置
|   |-- webpack.dev.conf.js          // webpack开发环境配置
|   |-- webpack.prod.conf.js         // webpack生产环境配置
|-- config                           // 项目开发环境配置
|   |-- dev.env.js                   // 开发环境变量
|   |-- index.js                     // 项目一些配置变量
|   |-- prod.env.js                  // 生产环境变量
|   |-- test.env.js                  // 测试环境变量
|-- src                              // 源码目录
|   |-- components                     // vue公共组件
|   |-- store                          // vuex的状态管理
|   |-- App.vue                        // 页面入口文件
|   |-- main.js                        // 程序入口文件，加载各种公共组件
|-- static                           // 静态文件，比如一些图片，json数据等
|   |-- data                           // 群聊分析得到的数据用于数据可视化
|-- .babelrc                         // ES6语法编译配置
|-- .editorconfig                    // 定义代码格式
|-- .gitignore                       // git上传需要忽略的文件格式
|-- README.md                        // 项目说明
|-- favicon.ico 
|-- index.html                       // 入口页面
|-- package.json                     // 项目基本信息
.
```

## 重要文件

### package.json

文件是项目根目录下的一个文件，定义该项目开发所需要的各种模块以及一些项目配置信息（如项目名称、版本、描述、作者等）。

package.json 里的scripts字段，这个字段定义了你可以用npm运行的命令。在开发环境下，在命令行工具中运行npm run dev 就相当于执行 node build/dev-server.js .也就是开启了一个node写的开发行建议服务器。由此可以看出script字段是用来指定npm相关命令的缩写。

```js
"scripts": {
    "dev": "node build/dev-server.js",
    "build": "node build/build.js"
  },
```

dependencies字段和devDependencies字段

`dependencies`字段指项目运行时所依赖的模块；

`devDependencies`字段指定了项目开发时所依赖的模块；

在命令行中运行npm install命令，会自动安装dependencies和devDempendencies字段中的模块。package.json还有很多相关配置，如果你想全面了解，可以专门去百度学习一下。

### webpack配置相关

我们在上面说了运行npm run dev 就相当于执行了node build/dev-server.js,说明这个文件相当重要，先来熟悉一下它。 我贴出代码并给出重要的解释。

dev-server.js

```js
// 检查 Node 和 npm 版本
require('./check-versions')()

// 获取 config/index.js 的默认配置
var config = require('../config')

// 如果 Node 的环境无法判断当前是 dev / product 环境
// 使用 config.dev.env.NODE_ENV 作为当前的环境

if (!process.env.NODE_ENV) process.env.NODE_ENV = JSON.parse(config.dev.env.NODE_ENV)

// 使用 NodeJS 自带的文件路径工具
var path = require('path')

// 使用 express
var express = require('express')

// 使用 webpack
var webpack = require('webpack')

// 一个可以强制打开浏览器并跳转到指定 url 的插件
var opn = require('opn')

// 使用 proxyTable
var proxyMiddleware = require('http-proxy-middleware')

// 使用 dev 环境的 webpack 配置
var webpackConfig = require('./webpack.dev.conf')

// default port where dev server listens for incoming traffic

// 如果没有指定运行端口，使用 config.dev.port 作为运行端口
var port = process.env.PORT || config.dev.port

// Define HTTP proxies to your custom API backend
// https://github.com/chimurai/http-proxy-middleware

// 使用 config.dev.proxyTable 的配置作为 proxyTable 的代理配置
var proxyTable = config.dev.proxyTable

// 使用 express 启动一个服务
var app = express()

// 启动 webpack 进行编译
var compiler = webpack(webpackConfig)

// 启动 webpack-dev-middleware，将 编译后的文件暂存到内存中
var devMiddleware = require('webpack-dev-middleware')(compiler, {
    publicPath: webpackConfig.output.publicPath,
    stats: {
        colors: true,
        chunks: false
    }
})

// 启动 webpack-hot-middleware，也就是我们常说的 Hot-reload
var hotMiddleware = require('webpack-hot-middleware')(compiler)
// force page reload when html-webpack-plugin template changes
compiler.plugin('compilation', function (compilation) {
    compilation.plugin('html-webpack-plugin-after-emit', function (data, cb) {
        hotMiddleware.publish({ action: 'reload' })
        cb()
    })
})

// proxy api requests
// 将 proxyTable 中的请求配置挂在到启动的 express 服务上
Object.keys(proxyTable).forEach(function (context) {
    var options = proxyTable[context]
    if (typeof options === 'string') {
        options = { target: options }
    }
    app.use(proxyMiddleware(context, options))
})

// handle fallback for HTML5 history API
// 使用 connect-history-api-fallback 匹配资源，如果不匹配就可以重定向到指定地址
app.use(require('connect-history-api-fallback')())

// serve webpack bundle output
// 将暂存到内存中的 webpack 编译后的文件挂在到 express 服务上
app.use(devMiddleware)

// enable hot-reload and state-preserving
// compilation error display
// 将 Hot-reload 挂在到 express 服务上
app.use(hotMiddleware)

// serve pure static assets
// 拼接 static 文件夹的静态资源路径
var staticPath = path.posix.join(config.dev.assetsPublicPath, config.dev.assetsSubDirectory)
// 为静态资源提供响应服务
app.use(staticPath, express.static('./static'))

// 让我们这个 express 服务监听 port 的请求，并且将此服务作为 dev-server.js 的接口暴露
module.exports = app.listen(port, function (err) {
    if (err) {
        console.log(err)
        return
    }
    var uri = 'http://localhost:' + port
    console.log('Listening at ' + uri + '\n')

    // when env is testing, don't need open it
    // 如果不是测试环境，自动打开浏览器并跳到我们的开发地址
    if (process.env.NODE_ENV !== 'testing') {
        opn(uri)
    }
})
```

### webpack.base.confg.js

webpack的基础配置文件

```js
...
...
module.export = {
    // 编译入口文件
    entry: {},
    // 编译输出路径
    output: {},
    // 一些解决方案配置
    resolve: {},
    resolveLoader: {},
    module: {
        // 各种不同类型文件加载器配置
        loaders: {
        ...
        ...
        // js文件用babel转码
        {
            test: /\.js$/,
            loader: 'babel',
            include: projectRoot,
            // 哪些文件不需要转码
            exclude: /node_modules/
        },
        ...
        ...
        }
    },
    // vue文件一些相关配置
    vue: {}
}
```

[.babelrc](https://jspang.com/detailed?id=26#toc46 ".babelrc")

Babel解释器的配置文件，存放在根目录下。Babel是一个转码器，项目里需要用它将ES6代码转为ES5代码。如果你想了解更多，可以查看babel的知识。

```纯文本
{
  //设定转码规则
  "presets": [
    ["env", { "modules": false }],
    "stage-2"
  ],
  //转码用的插件
  "plugins": ["transform-runtime"],
  "comments": false,
  //对BABEL_ENV或者NODE_ENV指定的不同的环境变量，进行不同的编译操作
  "env": {
    "test": {
      "presets": ["env", "stage-2"],
      "plugins": [ "istanbul" ]
    }
  }
}
```

[.editorconfig](https://jspang.com/detailed?id=26#toc47 ".editorconfig")

该文件定义项目的编码规范，编译器的行为会与.editorconfig文件中定义的一致，并且其优先级比编译器自身的设置要高，这在多人合作开发项目时十分有用而且必要。

```纯文本
root = true

[*]    // 对所有文件应用下面的规则
charset = utf-8                    // 编码规则用utf-8
indent_style = space               // 缩进用空格
indent_size = 2                    // 缩进数量为2个空格
end_of_line = lf                   // 换行符格式
insert_final_newline = true        // 是否在文件的最后插入一个空行
trim_trailing_whitespace = true    // 是否删除行尾的空格
```

## 下载依赖

安装到生产环境

`npm i eslint --save`

安装到开发环境

`npm i eslint --save-dev `

## 解读模板

### npm run build 命令

如何把写好的Vue网页放到服务器上，那我就在这里讲解一下，主要的命令就是要用到

`npm run build`命令。我们在命令行中输入`npm run build`命令后，`vue-cli`会自动进行项目发布打包。你在`package.json`文件的scripts字段中可以看出，你执行的`npm run build`命令就相对执行的 `node build/build.js `。

`package.json`的scripts 字段：

```json
"scripts": {
    "dev": "node build/dev-server.js",
    "build": "node build/build.js"
},
```

在执行完`npm run build`命令后，在你的项目根目录生成了dist文件夹，这个文件夹里边就是我们要传到服务器上的文件。

**dist文件夹下目录包括：**

*   `index.html`主页文件：因为我们开发的是单页web应用，所以说一般只有一个html文件。

*   static 静态资源文件夹：里边js、CSS和一些图片。
