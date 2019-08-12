# loader

loader 用于对模块的源代码进行转换。 loader可以使你在 `import` 或 加载 模块时预处理文件。因此， loader类似于其他构建工具中“任务” ， 并提供了处理前端构建步骤的强大方法。 loader可以将文件从不同的语言（如TypeScript）转换为 Javascript， 或将内联图像转换为data URL。 loader甚至孕育你直接在JavaScript模块中`import` CSS 文件！



## 示例：

例如， 你可以使用loader告诉webpack加载CSS文件， 或者将TypeScript转为Javascri。 为此，首先安装相对应的loader：

```typescript
npm install --save-dev css-loader
npm install --save-dev ts-loader
```

然后指示webpack对每个 `.css` 使用 `css-loader` ， 以及对所有 `.ts` 文件用`ts-loader`；

webpack.config.js

```typescript
module.exports = {
    module: {
        rules: [
            {test: /\.css$/, use: 'css-loader'},
            {test: /\.ts$/, use: 'ts-loader'}
        ]
    }
}
```

## 使用loader

在你的应用程序中， 有3种使用loader的方式：

* 配置（推荐）： 在webpack.config.js文件中指定loader。
* 内联： 在每个`import` 语句中显示指定loader
* CLI：在shell命令中指定他们

## 配置[Configuration] 推荐

`module.rules` 允许你在webpack配置中**指定多个loader**。指示展示loader的一种简明方式， 并且有助于使代码变得简洁。 同事让你对各个loader有个全局概览：

```typescript
module:{
    rules: [
        {
            test:/\.css$/,
            use:[
                {loader:'style-loader'},
                {
                    loader:'css-loader',
                    options:{
                        modules:true
                    }
                }
            ]
        }
    ]
}
```

## 内联

可以在`import` 语句或任何等效于`import` 的方式中指定loader。

使用`!` 将资源中的loader分开。 分开的每个部分都相对于当前目录解析。

```typescript
import Styles from 'style-loader!css-loader?modules./styles.css';
```

通过前置所有规则及使用`!`， 可以对应覆盖到配置中的任意loader。

选项可以传递查询参数， 例如`?key=value&&foo=bar`， 

或者一个JSON对象，例如`{’key': 'value', 'foo':'bar'}`

> 尽可能使用moduels.rules， 因为这样可以减少源码中代码量， 并且可以在出错时， 更快的调试和定位loader中的问题

## CLI

你可以通过CLI使用loader：

```typescript
webpack --module-bind jade-loader --module-bind 'css=style-loader!css-loader'
```

这会对`.jade` 文件使用 `jade-loader`， 对`.css` 文件使用 `style-loader` 和`css-loader`；



## loader特性

* loader支持链式传递。 能够对资源使用流水线（pipeline）。 一组链式的loader将照相反的顺序执行。 loader链中的第一个loader返回值给下一个loader。 在最后一个loader， 返回webpack所预期的Javascript。
* loader可以是同步的， 也可以是异步的。
* loader运行在Node.js中， 并且能够执行任何可能的操作。
* loader接受查询参数。用于对loader传递配置。
* loader也能够使用options 对象进行配置。
* 除了使用`package.json` 常见`main`属性， 还可以将普通的npm模块导出为loader， 做法事在`package.json`里定义一个`loader`字段
* 插件(plugin)可以为loader带来更多特性。
* loader能够产生额外的任意文件

loader通过（loader）预处理函数， 为JavaScript生态系统提供了更多能力，。 用户现在可以更加灵活地引入细粒度逻辑， 例如压缩， 打包， 语言翻译和其他更多



## 解析loader

loader遵循标准的模块解析， 多数情况下，loader将从模块路径（通常将模块路径认为是`npm install, node_modules`） 解析。

loader 模块需要导出为一个函数， 并且使用Node.js兼容的JavaScript编写， 通常使用npm进行管理， 但是也可以将自定义loader作为应用程序中的文件。按照约定， loader通常被命名为 `xxx-loader` 