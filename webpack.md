---
title: webpack
---
# 概念

webpack是一个现代javascript应用程序的静态模块打包器,有以下几个概念。
* 入口(entry)
* 输出(output)
* loader
* 插件(plugins)

## 入口（entry）

入口起点 指示webpack 应该使用哪个模块，来作为构建其内部依赖图的开始，进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。

每个依赖项随即被处理，最后输出到称之为bundles 的文件中。

entry 属性用来配置一个入口或多个入口起点，默认值为"./src"

### 1、单个入口语法 ‘entry: string/Array<string>’

```
const config = {
    entry: './path/to/my/entry/file.js'
}
module.exports = config;
```

### 2、对象语法 ‘entry: {[entryChunkName: string]: string|Array<string>}’

```
const config = {
  entry: {
    app: './src/app.js',
    vendors: './src/vendors.js'
  }
};
```
对象语法会比较繁琐。然而，这是应用程序中定义入口的最可扩展的方式。

“可扩展的 webpack 配置”是指，可重用并且可以与其他配置组合使用。这是一种流行的技术，用于将关注点(concern)从环境(environment)、构建目标(build target)、运行时(runtime)中分离。然后使用专门的工具（如 webpack-merge）将它们合并。

### 3、常见场景

#### 分离 应用程序(app) 和 第三方库(vendor) 入口

```
const config = {
    entry: {
        app: './src/app.js',
        vendors: './src/vendors.js'
    }
}
```
vendors是利用CommonsChunkPlugin插件抽出公用模块打包出来的文件。大小跟你依赖的第三方库有关

#### 多页面应用程序
```
const config = {
    entry: {
        pageOne: './src/pageOne/index.js',
        pageTwo: './src/pageTwo/index.js',
        pageThree: './src/pageThree/index.js'
    }
}
```
这是什么？我们告诉 webpack 需要 3 个独立分离的依赖图（如上面的示例）。

为什么？在多页应用中，（译注：每当页面跳转时）服务器将为你获取一个新的 HTML 文档。页面重新加载新文档，并且资源被重新下载。然而，这给了我们特殊的机会去做很多事：

使用 CommonsChunkPlugin 为每个页面间的应用程序共享代码创建 bundle。由于入口起点增多，多页应用能够复用入口起点之间的大量代码/模块，从而可以极大地从这些技术中受益。

## 出口（output）

output属性告诉webpack 在哪里输出它所创建的bundles，以及如何命名这些文件，默认值为"./dist",基本上，整个应用程序结构，都会被编译到你指定的输出路径的文件夹中。

* filename 用于输出文件的文件名。
* 目标输出目录 path 的绝对路径。

```
const path = require('path');
module.exports = {
    entry:"./path/to/my/entry/file.js",
    output:{
        path: path.resolve(__dirname,'dist),
        filename: 'my-first-webpack.bundle.js'
    }
}
```
多个入口起点，使用占位符来确保每个文件具有唯一的名称
```
{
  entry: {
    app: './src/app.js',
    search: './src/search.js'
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist'
  }
}
// 写入到硬盘：./dist/app.js, ./dist/search.js
```
高级进阶 (没看懂)

```
output: {
  path: "/home/proj/cdn/assets/[hash]",
  publicPath: "http://cdn.example.com/assets/[hash]/"
}
```

在编译时不知道最终输出文件的 publicPath 的情况下，publicPath 可以留空，并且在入口起点文件运行时动态设置。如果你在编译时不知道 publicPath，你可以先忽略它，并且在入口起点设置 __webpack_public_path__。

```
__webpack_public_path__ = myRuntimePublicPath
```

## loader

loader 让webpack 能够去处理那些非javascript文件（webpack自身只理解javascript），loader 可以将所有类型的文件转换为webpack 能够处理的有效模块，然后利用webpack的打包能力，对它们进行处理。

本质上，webpack loader 将所有类型的文件，转换为应用程序的依赖图（和最终的bundle）可以直接引用的模块。
 
* test 属性，用于标识出应该被对应的loader进行转换的某个或某些文件。

* use 属性，表示进行转换时，应该使用哪个loader。

```
const path = require('path');
const config = {
    output:{
        filename: 'my-first-webpack.bundle.js'
    },
    module:{
        rules:[
            {test: /\.txt$/,use:'raw-loader'}
        ]
    }
}
module.exports = config;
```
以上配置中，对一个单独的module 定义了rules规则，当遇到require()/import 语句中被解析为.txt 的路径时，先用raw-loader转换一下。

module.rules 允许你在 webpack 配置中指定多个 loader。 这是展示 loader 的一种简明方式，并且有助于使代码变得简洁。同时让你对各个 loader 有个全局概览：

```
 module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: 'style-loader' },
          {
            loader: 'css-loader',
            options: {
              modules: true
            }
          }
        ]
      }
    ]
  }
```

## 插件plugins
loader 被用于转换某些类型的模块，而插件可以用于执行范围更广的任务。

插件的范围包括，从打包优化压缩，一直到重新定义环境中的变量。

插件需要先require 然后添加到plugins数组中。多数插件可以通过选型option 来定义。也可以在一个配置文件中因为不同目的而多次使用一个插件，通过new 操作符来创建一个它的实例。
```
const HtmlWebpackPlugin = require('htmi-webpack-plugin');  //通过npm安装
const webpack = require('webpack');  //用于访问内置插件

const config = {
    module: {
        rules: [
            {test: /\.txt$/,use: 'raw-loader'}
        ]
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: './src/index.html'
        })
    ]
};
module.exports = config;
```
## 模式 
通过选择development 或 production中的一个，来设置mode参数，可以启用相应模式下的webpack内置的优化。
```
module.exports = {
  mode: 'production'
};
```









