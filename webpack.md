---
title: webpack
---

# 概念
webpack是一个现代javascript应用程序的静态模块打包器,有一下。
* 入口(entry)
* 输出(output)
* loader
* 插件(plugins)
## 入口（entry）
入口起点 指示webpack 应该使用哪个模块，来作为构建其内部依赖图的开始，进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。

每个依赖项随即被处理，最后输出到称之为bundles 的文件中。

entry 属性用来配置一个入口或多个入口起点，默认值为"./src"

```
module.exports = {
    entry:"./path/to/my/entry/file.js"
}
```
## 出口（output）
output属性告诉webpack 在哪里输出它所创建的bundles，以及如何命名这些文件，默认值为"./dist",基本上，整个应用程序结构，都会被编译到你指定的输出路径的文件夹中。
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
## loader
loader 让webpack 能够去处理那些非javascript文件（webpack自身只理解javascript），loader 可以将所有类型的文件转换为webpack 能够处理的有效模块，然后利用webpack的打包能力，对它们进行处理。

本质上，webpack loader 将所有类型的文件，转换为应用程序的依赖图（和最终的bundle）可以直接引用的模块。
 
1、test 属性，用于标识出应该被对应的loader进行转换的某个或某些文件。
2、use 属性，表示进行转换时，应该使用哪个loader。
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

## 入口起点
### 单个入口语法 entry: string/Array<string>
```
const config = {
    entry: './path/to/my/enty'
}
```




