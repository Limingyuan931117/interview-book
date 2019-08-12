# 入口起点【entry points】

正如我们在起步中提到的，在webpack配置中有多重方式定义`entry` 属性。 输了解释为什么他可能非常有用，还将张氏**如何去**配置 `entry` 属性



## 单个入口（简写）语法

用法： entry: string | Array<string>

webpack.config.js

```typescript
const config = {
    entry: './path/to/my/entry/file.js'
}
module.exports = config
```

entry 属性的单个入口语法， 是下面的 **<简写>**：

```typescript
const config = {
    entry: {
        main:'./path/to/my/entry/file.js'
    }
}
```

> 当向rentry传入一个数组是会发生什么？
>
> 向entry属性栓如【文件入境数组】将创建多个主入口。  在想要多个依赖文件一起注入， 并且将他们的依赖稻香到一个chunk时，  传入数组的方式就很有用



## 对象语法

用法： entry: {[entryChunkName: string]: string| Array<string>}

webpack.config.js

```typescript
const config = {
    entry: {
        app: './src/app.js',
        vendors: './src/vendors.js'
    }
};
```

对象语法会比较繁琐。 然而，这是应用程序中定义入口的最可扩展的方式。

> “可扩展的webpack配置”是指， 可重用并且可以与其他配置组合使用。这是一种流行的技术， 用于将关注点从环境， 构建目标， 运行时中分离， 然后使用专门的工具， 将他们合并 



## 常见场景

一下列出一些入口配置和他们的实际用例：

## 分离应用程序（app) 和第三方库{vendor} 入口

webpack.config.js

```typescript
const config  = {
    entry : {
        app: './src/app.js',
        vendors: './src/vendors.js'
    }
}
```

这是什么？

从表面上看，这告诉我们webpack从`app.js` 和`vendors.js` 开始创建依赖图(dependency graph)。这些依赖图是批次完全分离，互相独立的（每个bundle中都有一个webpack引导（bootstrap））。这种方式比较常见于，只有一个入口起点的单页应用程序中

为什么？

此设置允许使用`CommondsChunkPlugin` 从 应用程序bundle 中提取vendor引用到vendor bundle, 并把引用vendor的部分替换为 `_webpack_require_()` 调用。 如果应用程序bundle中没有vendor代码， 那么可以在webpack中实现并成为长期缓存的通用模式



## 多页面应用程序

webpack.config.js

```typescript
const config = {
    entry: {
        pageOne: './src/pageOne.js',
        pageTwo: './src/pageTwo.js',
        pageThree: './src/pageTree.js'
    }
};
```

这是什么？

我们告诉webpack需要3个肚子分离的依赖图

为什么？

在多页应用中， 服务器将为你获取一个新的HTML文档。 页面重新加载新文档， 并且自愿被冲洗下载，

#####  然而， 这给了我们特殊的机会去做很多事：

* 使用`CommonsChunkPlugin` 为每个页面间的应用程序共享代码创建 bundle。 鱿鱼入口起点增多， 多页应用能够复用入口起点之间的大量代码/模块， 从而可以极大地从这些技术中心收益

> 每个HTML文档只使用一个入口起点