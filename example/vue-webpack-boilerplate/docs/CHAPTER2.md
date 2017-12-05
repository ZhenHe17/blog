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
> 开发环境(development)和生产环境(production)的构建目标差异很大。在开发环境中，我们需要具有强大的、具有实时重新加载(live reloading)或热模块替换(hot module replacement)能力的 source map 和 localhost server。而在生产环境中，我们的目标则转向于关注更小的 bundle，更轻量的 source map，以及更优化的资源，以改善加载时间。由于要遵循逻辑分离，我们通常建议为每个环境编写彼此独立的 webpack 配置。虽然，以上我们将生产环境和开发环境做了略微区分，但是，请注意，我们还是会遵循不重复原则(Don't repeat yourself - DRY)，保留一个“通用”配置。为了将这些配置合并在一起，我们将使用一个名为 webpack-merge 的工具。通过“通用”配置，我们不必在环境特定(environment-specific)的配置中重复代码。
```
+ |- /build
+   |- webpack.base.conf.js
+   |- webpack.dev.conf.js
  |- /src
    |- /assets
      |- logo.png
    |- /components
      |- HelloWorld.vue
    |- App.vue
    |- main.js
  |- index.html
  |- package.json
```
