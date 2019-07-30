# 打造舒适的 IDE —— VSCode

VSCode 是由微软研发的开源编辑器，由于其高效的 JavaScript 智能提示功能和插件机制广受好评。我们采用 vscode 作为前端项目的 IDE。建议安装的 VSCode 插件：

* [EditConfig for Vs Code](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig)
* [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
* [react code snippets](https://marketplace.visualstudio.com/items?itemName=xabikos.ReactSnippets)
* [Typescript React code snippets](https://marketplace.visualstudio.com/items?itemName=infeng.vscode-react-typescript)
* [JavaScript 注释 - document this](https://marketplace.visualstudio.com/items?itemName=joelday.docthis)
* [多项目快速切换 - project manager](https://marketplace.visualstudio.com/items?itemName=alefragnani.project-manager)
* [显示文件大小 - file-size](https://marketplace.visualstudio.com/items?itemName=zh9528.file-size)
* [Git 辅助神器 - GitLens — Git supercharged](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
* [Jest](https://marketplace.visualstudio.com/items?itemName=Orta.vscode-jest)
* [markdown 语法检查 - markdownlint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint)
* [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
* [stylelint](https://marketplace.visualstudio.com/items?itemName=shinnn.stylelint)
* [tslint](https://marketplace.visualstudio.com/items?itemName=eg2.tslint)
* [vscode-icons](https://marketplace.visualstudio.com/items?itemName=robertohuertasm.vscode-icons)
* [vscode-styled-components](https://marketplace.visualstudio.com/items?itemName=jpoissonnier.vscode-styled-components)
* [TODO Highlight](https://marketplace.visualstudio.com/items?itemName=wayou.vscode-todo-highlight)
* [Debugger for Chrome](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome)
* [一款不错的主题 - Dracular Official](https://marketplace.visualstudio.com/items?itemName=dracula-theme.theme-dracula)
* [change-case](https://marketplace.visualstudio.com/items?itemName=wmaurer.change-case)
* [Regex Previewer](https://marketplace.visualstudio.com/items?itemName=chrmarti.regex)
* [Document this - 编写 JavaScript API 文档的辅助工具](https://marketplace.visualstudio.com/items?itemName=joelday.docthis)
* [import cost](https://marketplace.visualstudio.com/items?itemName=wix.vscode-import-cost)
* [auto close tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag)
* [auto rename tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-rename-tag)

建议的 VSCode 配置：（[如何设置？看这里](https://code.visualstudio.com/docs/getstarted/settings)）

```json
{
  "editor.formatOnSave": true,
  "editor.fontFamily":
    "Source Code Pro, Menlo, Monaco, 'Courier New', monospace",
  "editor.rulers": [80, 120],
  "search.exclude": {
    "**/node_modules": true,
    "**/bower_components": true,
    "**/build": true,
    "**/lib": true,
    "**/dist": true,
    "**/coverage": true
  },
  "emmet.triggerExpansionOnTab": true,
  "emmet.showSuggestionsAsSnippets": true,
  "editor.snippetSuggestions": "top",
  "emmet.includeLanguages": {
    "javascript": "javascriptreact",
    "typescript": "typescriptreact"
  },
  "emmet.variables": {
    "lang": "zh-cmn-Hans",
    "charset": "UTF-8"
  },
  "terminal.external.osxExec": "iTerm.app",
  "workbench.colorTheme": "Dracula Soft",
  "workbench.iconTheme": "vscode-icons",
  "git.autofetch": true,
  "editor.wordWrap": "on"
}
```

* 配置 vscode 保存时自动格式化
* 使用[Source Code Pro](https://www.fontsquirrel.com/fonts/source-code-pro)字体
* 一行代码超过一屏时自动换行显示
* 分别在 80 和 120 字符处设置编辑器尺子
* 配置 vscode 检索文件的排除范围
* 在按 tab 键时启用 emmet

在项目的根目录下配置合适的各种检查规则：

* editorconfig
* eslint
* tslint
* prettier

查看[vscode 的中文配置](https://github.com/Microsoft/vscode/blob/master/i18n/chs/src/vs/workbench/electron-browser/main.contribution.i18n.json)。

使用命令行打开 vscode：`vscode .`。

## 常用快捷键

| 动作         | mac              | windows          | 是否需要设置 |
| ------------ | ---------------- | ---------------- | ------------ |
| 删除一行文本 | shift + cmd + k  |                  |              |
| 转换为小写   | ctrl + u         | ctrl + u         | 是           |
| 转换为大写   | ctrl + shift + u | ctrl + shift + u | 是           |

### 如何配置快捷键

1. 打开vscode快捷键设置界面：

    ![打开vscode快捷键设置界面](images/open_key_settings.jpg)

2. 在打开的快捷键设置界面中输入关键词找到需要设置快捷键的命令，然后点击左边的加号，设置快捷键

    ![设置快捷键](images/key_settings.jpg)
