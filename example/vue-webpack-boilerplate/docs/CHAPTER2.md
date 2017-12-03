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