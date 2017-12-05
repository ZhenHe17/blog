# 学习vue-webpack模板：二、搭建开发环境

## 新建vue项目

在搭建开发环境之前，我们需要新建一个vue项目，这里将使用官方模板中的默认结构，目前项目目录如下：
```
+ |- /src
+   |- /assets
+     |- logo.png
+   |- /components
+     |- HelloWorld.vue
+   |- App.vue
+   |- main.js
+ |- index.html
  |- package.json
```
* index.html --作为webpack插件：[HtmlWebpackPlugin](https://doc.webpack-china.org/plugins/html-webpack-plugin/)的html模板文件
* main.js --作为webpack打包的**入口起点（entry）**

## 开始搭建开发环境

根据[package.json](https://github.com/ZhenHe17/blog/blob/master/example/vue-webpack-boilerplate/chapter1/package-init-by-vue-cli.json)中的
```
  "scripts": {
    "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
```
可以看出：由[webpack-dev-server](https://webpack.github.io/docs/webpack-dev-server.html)负责运行开发环境，且[build/webpack.dev.conf.js](https://github.com/ZhenHe17/blog/blob/master/example/vue-webpack-boilerplate/chapter2/build/webpack.dev.conf.js)是它的配置文件。我们将从配置文件入手，开始搭建开发环境。