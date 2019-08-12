# 输出{output}

配置`output` 选项可以控制webpack如何向硬盘写入变异文件。 注意，即使可以存在多个 `入口` 起点， 但只指定一个输出配置。



## 用法{Usage}

在webpack中配置 `output`属性的最低要求是，将它的值设置为一个对象， 包括以下两点：

* `filename` 用于输出文件的文件名
* 目标输出目录 `path` 的绝对路径

webpack.config.js

```typescript
const config = {
    output:{
        filename: 'bundle.js',
        path: './home/project/public/path'
    }
}

module.exports = config;
```

此配置将一个单独的`bundle.js` 文件输出到`/home/project/public/path` 目录中

## 多个入口起点

如果配置创建了多个单独了“chunk”（列入，使用多个入口起点或使用像 CommonsChunkPlugin这样的插件），则应该使用占位符（substitutions）来确保每个文件具有唯一的名称。

```typescript
{
    entry:{
        app: './src/app.js',
        search: './src/search.js'
    },
    output:{
        filename: '[name].js',
        path: __dirname + '/dist'
    }
}

// 写入到硬盘：./dist/app.js, ./dist/search.js
```



## 高级进阶

一下是使用CDN和自愿hash的复杂示例：

config.js

```typescript
output: {
    path: '/home/project/cdn/path/[hash]',
    publicPath: 'http://cdn.example.com/path/[hash]/'
}
```

在变异时不知道最终输出文件的`publicPath`的情况下， `publicPath` 可以留空， 并且在入口起点文件运行时动态设置， 如果你在编译时不知道`publicPath`， 你可以先忽略他， 并且在入口起点设置。

`__webpack_public_path__`

```javascript
__webpack_public_path__ = myRuntimePublicPath

// 剩余的应用程序入口
```

