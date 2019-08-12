# 模式{mode}

提供`mode` 配置选项， 告知webpack 使用相应模式的内置优化；

string



## 用法

值在配置中提供`mode`

 选项：

```typescript
module.exports = {
    mode: 'production' | mode: 'development'
}
```

或者从CLI参数中传递：

```typescript
webpack --mode = production
```

支持一下字符串值：

| 选项        | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| development | 会将`process.env.NODE_ENV` 的值设为`development`。 启用`NamedChunksPlugin` 和 `NamedModulesPlugin`。 |
| production  | 会将`process.env.NODE_ENV` 的值设为`production`。 启用`FlagDependencyUsagePlugin`, `FlagIncludedChunksPlugin`, `ModuleConcatenationPlugin`, `NoEmitOnErrorsPlugin`, `OccurrenceOrderPlugin`, `SideEffectsFlagPlugin` 和 `UglifyJsPlugin` |

> 注意， 只设置Node_ENV ， 则不会自动设置mode

### mode: development

```typescript
// webpack.development.config.js
module.exports = {
+ mode: 'development'
- plugins: [
-   new webpack.NamedModulesPlugin(),
-   new webpack.DefinePlugin({ "process.env.NODE_ENV": JSON.stringify("development") }),
- ]
}
```

### mode: production

```typescript
// webpack.production.config.js
module.exports = {
+  mode: 'production',
-  plugins: [
-    new UglifyJsPlugin(/* ... */),
-    new webpack.DefinePlugin({ "process.env.NODE_ENV": JSON.stringify("production") }),
-    new webpack.optimize.ModuleConcatenationPlugin(),
-    new webpack.NoEmitOnErrorsPlugin()
-  ]
}
```

