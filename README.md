### 起步
#### 模块
`webpack4`天然的支持 `import` 和 `export` ,但是如果要支持其他 ES6 的语法，需要 `babel` 支持
#### 使用一个配置文件
使用一个配置文件，比如这样 `npx webpack --config webpack.config.js`
如果 `webpack.config.js` 存在，则 `webpack` 命令将默认选择使用它。我们在这里使用 `--config` 选项只是向你表明，可以传递任何名称的配置文件。这对于需要拆分成多个文件的复杂配置是非常有用。[npx简介](https://www.jianshu.com/p/cee806439865)
### 管理资源
#### 加载CSS
为了从 `JavaScript` 模块中 `import` 一个 CSS 文件，你需要在 `module` 配置中 安装并添加 `style-loader` 和 `css-loader`
```
module: {
    rules: [{
      test: /\.css$/,
      use: [
        'style-loader',
        'css-loader'
      ]
    }]
  }
```
webpack 根据正则表达式，来确定应该查找哪些文件，并将其提供给指定的 loader。在这种情况下，以 .css 结尾的全部文件，都将被提供给 style-loader 和 css-loader。
这使你可以在依赖于此样式的文件中 `import './style.css'`。现在，当该模块运行时，含有 CSS 字符串的 `<style>` 标签，将被插入到 html 文件的 `<head>` 中。
#### 加载图片
假想，现在我们正在下载 CSS，但是我们的背景和图标这些图片，要如何处理呢？使用 `file-loader`，我们可以轻松地将这些内容混合到 CSS 中</br>
现在，当你 `import MyImage from './my-image.png'`，该图像将被处理并添加到 output 目录，并且 `MyImage` 变量将包含该图像在处理后的最终 url。当使用 `css-loader` 时，如上所示，你的 CSS 中的 `url('./my-image.png')` 会使用类似的过程去处理。loader 会识别这是一个本地文件，并将 `'./my-image.png'` 路径，替换为输出目录中图像的最终路径。`html-loader` 以相同的方式处理 `<img src="./my-image.png" />`。
### 管理输出
#### 预先准备
>webpack.config.js
```
  const path = require('path');

  module.exports = {
-   entry: './src/index.js',
+   entry: {
+     app: './src/index.js',
+     print: './src/print.js'
+   },
    output: {
-     filename: 'bundle.js',
+     filename: '[name].bundle.js',
      path: path.resolve(__dirname, 'dist')
    }
  };
```
#### 设定 HtmlWebpackPlugin
```
  const path = require('path');
+ const HtmlWebpackPlugin = require('html-webpack-plugin');

  module.exports = {
    entry: {
      app: './src/index.js',
      print: './src/print.js'
    },
+   plugins: [
+     new HtmlWebpackPlugin({
+       title: 'Output Management'
+     })
+   ],
    output: {
      filename: '[name].bundle.js',
      path: path.resolve(__dirname, 'dist')
    }
  };
```
在我们构建之前，你应该了解，虽然在 `dist/` 文件夹我们已经有 `index.html` 这个文件，然而 `HtmlWebpackPlugin` 还是会默认生成 `index.html` 文件。这就是说，它会用新生成的 `index.html` 文件，把我们的原来的替换。让我们看下在执行 `npm run build` 后会发生什么
#### 清理 /dist 文件夹
通常，在每次构建前清理 /dist 文件夹，是比较推荐的做法，因此只会生成用到的文件。让我们用 `clean-webpack-plugin` 完成这个需求。
> webpack.config.js
```
  const path = require('path');
  const HtmlWebpackPlugin = require('html-webpack-plugin');
+ const CleanWebpackPlugin = require('clean-webpack-plugin');

  module.exports = {
    entry: {
      app: './src/index.js',
      print: './src/print.js'
    },
    plugins: [
+     new CleanWebpackPlugin(['dist']),
      new HtmlWebpackPlugin({
        title: 'Output Management'
      })
    ],
    output: {
      filename: '[name].bundle.js',
      path: path.resolve(__dirname, 'dist')
    }
  };
  ```


