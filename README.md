### 起步
#### 模块
`webpack4`天然的支持 `import` 和 `export` ,但是如果要支持其他 ES6 的语法，需要 `babel` 支持
#### 使用一个配置文件
使用一个配置文件，比如这样 `npx webpack --config webpack.config.js`
如果 `webpack.config.js` 存在，则 `webpack` 命令将默认选择使用它。我们在这里使用 `--config` 选项只是向你表明，可以传递任何名称的配置文件。这对于需要拆分成多个文件的复杂配置是非常有用。[npx简介](https://www.jianshu.com/p/cee806439865)