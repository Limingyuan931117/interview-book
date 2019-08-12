# 插件{plugins}

插件是webpack的支柱功能。 webpack自身也是构建于， 你在webpack配置中用到的相同的插件系统之上！

插件目的在于解决loader无法实现的其他事

## 剖析

webpack插件是一个具有`apply` 属性的JavaScript 对象。 apply属性会被 webpack compiler调用， 并且compiler对象可在整个变异声明周期访问。

ConsoleLogOnBuildWebpackPlugin.js

```typescript
const pluginName = 'ConsoleLogOnBuildWebpackPlugin';

const ConsoleLogOnBuildWebpackPlugin {
    apply(compiler){
        compiler.hooks.run.tap((pluginName, compilation) => {
            console.log('webpack 构建过程开始')
        })
    }
}
```

compiler hook 的tap方法的第一个参数， 应该是驼峰是明明的插件名称， 建议为此使用一个常量， 一边他可以在所有hook中复用。

## 用法

鱿鱼插件可以携带参数/选项， 你必须在webpack配置中， 向`plugins` 属性传入`new` 示例。

根据你的webpack用法， 这里有多重方式使用插件。



## 配置

webpack.config.js

```typescript
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 通过 npm 安装
const webpack = require('webpack'); //访问内置插件
const path = require('path');

const config = {
    entry: './path/to/my/entry/file.js',
    output: {
        filename: 'build',
        path: path.resolve(__dirname, 'dist')
    },
    moduels:{
        reles:[
            {
                test:/\.(js|jsx)$/,
                use:'babel-loader'
            }
        ]
    },
    plugins:[
        new webpack.optimize.UglifyJsPlugin(),
        new HtmlWebpackPlugin({template: './src/index.html'})
    ]
}

module.exports = config;
```

## Node API

some-node-script.js

```typescript
const webpack = require('webpack');
const configuration = require('./webpack.config.js');

let compiler = webpack(configuration);
compiler.apply(new webpack.ProgressPlugin());

compiler.run((err, stats) => {//...})
```

> *你知道吗：以上看到的示例和* [webpack 自身运行时(runtime)](https://github.com/webpack/webpack/blob/e7087ffeda7fa37dfe2ca70b5593c6e899629a2c/bin/webpack.js#L290-L292) *极其类似。*[wepback 源码](https://github.com/webpack/webpack)*中隐藏有大量使用示例，你可以用在自己的配置和脚本中。*