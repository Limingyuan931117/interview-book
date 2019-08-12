# 概念

本质量，webpack是一个现代Javascript应用程序的静态模块打包器， 当webpack处理应用程序时，他会递归的构建一个依赖关系图， 其中包含应用程序需要的每个模块， 然后将所有这些模块打包成一个或多个bundle（捆绑）

从webpack4 开始， 可以不用引入一个配置文件， 然而，webpack仍然还是高度可配置的。在开始前你需要先理解4个核心概念：

* 入口（entry）
* 输出（output）
* loader
* 插件（plugins）



## 入口（entry）

入口起点（entry point）指示webpack应该使用哪个模块， 来作为构建其内部依赖图的开始。进入入口起点后，webpack会找出有哪些模块和库时入口起点（直接和间接）依赖的。

每个依赖项随即被处理，最后输出到位置为bundles的文件中， 我们将在下一章节详细讨论这个过程。

可以通过在webpack配置中配置entry属性，来指定一个入口起点（或多个入口起点）。默认值为./src。

接下来我们看一个entry配置的最简单例子：

webpack.config.js

```js
module.exports = {
    entry: './path/index.js'
}
```

## 出口（output）

output 属性告诉webpack在哪里输出它所创建的bundles， 以及如何命名这些文件，默认值为`./dis`  。基本上，整个应用程序结构， 都会被变异到你指定的输出路径的文件夹中。可以通过在配置中指定一个output字段，来配置这些处理过程：

webpack.config.js

```js
const path = require('path');

module.exports = {
    entry:'./path/path.js';
    output{
    	path: path.resolve(_dirname,'dist'),
    	filename:'my-first-webpack.bundle.js'
	}
}
```

## loader

loader让wbpack 能够去处理那些非javascript文件（webpack自身只理解javascript）。loader可以将所有类型的文件转换成webpack能够处理的有效模块， 然后就可以利用webpack的打包能力， 对他们进行处理， 

本质上， webpack loader将所有类型的文件， 转换为应用程序的依赖图（和最终的bundle）可以直接引用的模块。

> loader能够 import 导入任何类型的模块（列入。css文件） ， 这是webpack特有的功能， 其他打包程序或任务执行器的可能并不支持，我们认为这种语言扩展是有很必要的 因为这可以是开发人员创建出更准确的依赖关系图。

在更高层面，在webpack的配置中loader有2个目标：

1. test 属性， 用于标识出应该被对应的loader进行转换的某个或某些文件。
2. use  属性， 标识进行转换时， 应该使用哪个loader

webpack.config.js

```typescript
const path = require('path');

const config = {
    output: {
        filename:'my-first-webpack.bundle.js'
    },
    module: {
        reles: [
            {test:/\.txt$/, use: 'raw-loader'}
        ]
    }
}
```

上面配置中，对一个单独的module对象定义了rules属性， 里面包含2个必须属性： test和use， 这告诉webpack编译器如下信息：

> ‘hello， webpack编译器， 当你碰到 【在require（）/ import 语句中被解释为'.txt' 的路径】 时， 在你对他打包之前先使用 raw-loader转换一下

最重要的是要记得，在webpack配置中定义loader时， 要定义在module.reles中， 而不是rules。 然而，在定义错误是， webpack会给出严重的警告。 如果没有按照正确方式去做， webpack会给**出警告**



## 插件【plugins】

loader被用于转换某些类型的模块， 而插件则可以用于执行范围更广的任务。插件的范围包括， 从打包优化和压缩， 一直到重新定义环境中的变量， 插件接口功能及其强大， 可以用来处理各种各样的任务。

想要使用一个插件， 你只需要require（）他， 然后把它添加到plugins数组中。 多数插件可以通过选项（options）自定义。 你也可以在一个配置文件中因为不同目的而多次使用同一个插件， 这是需要通过使用new操作符来创建它的一个实例。

webpack.config.js

```typescript
const HtmlWebpackPlugins = require('html-webpack-plugin') // 通过npm安装
const webpack = require('webpack') // 用于访问内置插件

const config = {
    module:{
        reles:[
            {test:/\.txt$/, use:'raw-loader'}
        ]
    },
    pulgins:[
        new HtmlWebpackPlugin({
            template:'./src/index.html'
        })
    ]
}

module.export = config
```

webpack 提供许多开箱可用的插件

在webpack配置中使用插件是简单直接的， 然而也有很多值得我们进一步探讨的用例

## 模式

通过选择 development 或 production 之中的一个， 来设置mode参数， 可以弃用响应模式下的webpack内置的优化

```typescript
module.exports = {
    mode: 'production'
}
```



