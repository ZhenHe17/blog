# 学习vue-webpack模板：一、从package.json开始

package.json中包含了项目的一些基本信息、依赖包等内容。通过package.json，我们可以清楚的了解项目及其中使用的技术栈。下面将逐行分析[模板中的package.json](https://github.com/ZhenHe17/blog/blob/master/example/vue-webpack-boilerplate/chapter1/package-init-by-vue-cli.json)，并和[npm init --y生成的package.json](https://github.com/ZhenHe17/blog/blob/master/example/vue-webpack-boilerplate/chapter1/package-init-by-npm5.5.1.json)做对比。想了解package.json中每个字段的详细内容，可以参考官方文档[package.json](https://docs.npmjs.com/files/package.json)。

## 逐行分析[模板中的package.json](https://github.com/ZhenHe17/blog/blob/master/example/vue-webpack-boilerplate/chapter1/package-init-by-vue-cli.json)

### 第 1 ~ 6 行

这里介绍了项目的一些基本信息：
```
{
  "name": "vue-webpack",                  //项目名称
  "version": "1.0.0",                     //版本
  "description": "A Vue.js project",      //项目说明
  "author": "geshengming17@gmail.com",    //作者
  "private": true,                        //为true时，npm不会发布这个包
```

### 第 7 ~ 12 行

这里介绍了在项目目录下，可执行的npm脚本及对应的具体内容。
```
  "scripts": {
    "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
    "start": "npm run dev",

    "build": "node build/build.js"
  },
```

例：执行 ``` $ npm run dev ``` 等同于 ``` $ webpack-dev-server --inline --progress --config build/webpack.dev.conf.js ```

#### 脚本分析：

- 执行脚本 ``` $ npm run dev ``` 将会启动[webpack-dev-server](https://webpack.github.io/docs/webpack-dev-server.html)并传入三个配置``` --inline 、--progress 、--config build/webpack.dev.conf.js ```
  - [webpack-dev-server](https://webpack.github.io/docs/webpack-dev-server.html)：基于node.js、express的服务器，用于 **运行开发环境**
  - ``` --inline ``` 在包(bundle)中插入处理实时重载的脚本，并且webpack的构建消息将会出现在浏览器控制台
  - ``` --progress ``` 将运行进度实时输出在控制台（终端）中
  - ``` --config build/webpack.dev.conf.js ``` 指定[webpack-dev-server](https://webpack.github.io/docs/webpack-dev-server.html)的配置文件：build/webpack.dev.conf.js

- 执行脚本 ``` $ npm start ``` 将会执行 ``` $ npm run dev ```。结果同上

- 执行脚本 ``` $ npm run build ``` 将会在node环境中执行 build/build.js
  - build/build.js 负责将项目打包成供生产环境使用的静态资源

由此可见：[webpack-dev-server](https://webpack.github.io/docs/webpack-dev-server.html)负责开发环境，而build/build.js负责生产环境。关于[webpack-dev-server](https://webpack.github.io/docs/webpack-dev-server.html)及它的配置文件build/webpack.dev.conf.js、build/build.js将会在以后的章节中详细学习。

### 第 13 ~ 15 行

这里指定了项目的依赖包和版本，本模板没有选择安装[vue-router](https://router.vuejs.org/zh-cn/)，所以只有[vue](https://cn.vuejs.org/index.html)：
```
  "dependencies": {
    "vue": "^2.5.2"
  },
```

### 第 16 ~ 49 行

这里指定了项目开发时的依赖包和版本，这些依赖包主要用于项目的构建及开发环境的配置等，且**不会被部署到生产环境**。
<!--因源码较长，不再粘贴。但这些依赖包用于，所以将逐个进行初步的介绍和认识。

* [autoprefixer](https://github.com/postcss/autoprefixer)
* [babel-core](https://github.com/postcss/autoprefixer)
* [babel-loader](https://github.com/postcss/autoprefixer)
* [babel-plugin-transform-runtime](https://github.com/postcss/autoprefixer)
* [babel-preset-env](https://github.com/postcss/autoprefixer)
* [babel-preset-stage-2](https://github.com/postcss/autoprefixer)
* [babel-register](https://github.com/postcss/autoprefixer)
* [chalk](https://github.com/postcss/autoprefixer)
* [copy-webpack-plugin](https://github.com/postcss/autoprefixer)
* [css-loader](https://github.com/postcss/autoprefixer)
* [eventsource-polyfill](https://github.com/postcss/autoprefixer)
* [extract-text-webpack-plugin](https://github.com/postcss/autoprefixer)
* [file-loader](https://github.com/postcss/autoprefixer)
* [friendly-errors-webpack-plugin](https://github.com/postcss/autoprefixer)
* [html-webpack-plugin](https://github.com/postcss/autoprefixer)
* [webpack-bundle-analyzer](https://github.com/postcss/autoprefixer)
* [node-notifier](https://github.com/postcss/autoprefixer)
* [postcss-import](https://github.com/postcss/autoprefixer)
* [postcss-loader](https://github.com/postcss/autoprefixer)
* [semver](https://github.com/postcss/autoprefixer)
* [shelljs](https://github.com/postcss/autoprefixer)
* [optimize-css-assets-webpack-plugin](https://github.com/postcss/autoprefixer)
* [ora](https://github.com/postcss/autoprefixer)
* [rimraf](https://github.com/postcss/autoprefixer)
* [url-loader](https://github.com/postcss/autoprefixer)
* [vue-loader](https://github.com/postcss/autoprefixer)
* [vue-style-loader](https://github.com/postcss/autoprefixer)
* [vue-template-compiler](https://github.com/postcss/autoprefixer)
* [portfinder](https://github.com/postcss/autoprefixer)
* [webpack](https://github.com/postcss/autoprefixer)
* [webpack-dev-server](https://github.com/postcss/autoprefixer)
* [webpack-merge](https://github.com/postcss/autoprefixer) -->

### 第 50 ~ 53 行

这里指定项目所需的环境及版本：
```
  "engines": {
    "node": ">= 4.0.0",
    "npm": ">= 3.0.0"
  },
```
- node.js 版本大于等于 4.0.0
- npm 版本大于等于 3.0.0

### 第 54 ~ 58 行

这里说明了本项目对浏览器的兼容情况。官方文档[browserslist](https://www.npmjs.com/package/browserslist)

所有依赖于Browserslist的工具都会在这儿找到对应的配置：
```
  "browserslist": [
    "> 1%",
    "last 2 versions",
    "not ie <= 8"
  ]
```
- ``` "> 1%", ``` 兼容符合其他条件且全球市场份额大于1%的浏览器
- ``` "last 2 versions", ``` 兼容符合其他条件且是最近两个版本内的浏览器
- ``` "not ie <= 8" ``` 不兼容ie8及以下

## 小结

通过逐行分析package.json，我们发现package.json就像顺藤摸瓜里的藤，帮助我们从源头探索整个工程。

下一章：[]()

## 参考文档

[package.json | npm](https://docs.npmjs.com/files/package.json)