# 万字前端效率大提速系列 🚀 ：五、前端应具备的NodeJS知识及常用工具

NodeJS是什么？官网的介绍是：一个基于 Chrome V8 引擎的 JavaScript 运行时（runtime），简单来说就是**执行 JS 代码**。浏览器可以执行 JS、NodeJS 可以执行 JS，主流引擎都是 V8 引擎，乍一看好像没啥区别，实际上区别非常大。这里说一下我的理解供大家参考：

本系列的第二篇有说到，前端应用有静态构建和服务端渲染两种部署方式，无论哪种，最终浏览器都会得到我们处理后的HTML、CSS、JS等资源。最终 JS 代码会在 HTML 被解析的时候执行。注意，也就是**用户的浏览器**执行代码，来自五湖四海的用户使用浏览器，下载了我们Web应用的前端代码并本地执行，此时前端代码的运行环境是浏览器，功能是受到很大的限制的。

而 NodeJS 是运行在服务器或者开发者本地的，此时 NodeJS 的权限是比较大的（根据你分配的权限决定，最高可以获取服务器管理员级别的权限），这意味着 NodeJS 能做的事情比浏览器多得多，浏览器功能是 NodeJS 功能的子集，因为你可以在 NodeJS 中运行无界面浏览器。

由于运行环境不同，同一份 JS 代码未必都能在这两个环境中运行，取决于你有没有使用当前环境才提供的全局变量。浏览器中比较典型的全局变量有：`window`、`document`。NodeJS 中比较典型的全局变量有：`process`、`__dirname`，其他 NodeJS 提供的方法可以以 CommonJS 模块系统引入。

## 应用场景

基于 NodeJS 强大的功能，我们看看有哪些应用场景，用上了哪些内置模块，了解这些场景下热门的技术栈：

### 构建服务端应用

得益于 NodeJS 的事件循环机制，使它在 IO 密集型的场景中表现很好，但使用 NodeJS 构建服务端最大的优点仍是使用一种语言编写前后端代码。语言学习和切换是有成本的，统一开发语言提升的效率和舒适性是巨大的。在性能和社区生态上，相对 Java、GoLang 等其他语言没有明显的优势。可以说 NodeJS 适用场景为全栈开发、IO密集型场景和 服务端渲染的前端应用。

热门技术栈：开发体验类似 Java 的 [Nest.js](https://docs.nestjs.cn/)、阿里团队主打企业级框架的[Egg.js](https://www.eggjs.org/zh-CN)、主打性能的[Fastify](https://www.fastify.io/)、老牌框架生态成熟的[Express](https://www.expressjs.com.cn/)

主要涉及内置模块：http、https、url

### 构建浏览器之外的客户端应用

用 NodeJS 编写客户端应用的优势在于，可以以编写 Web 应用的方式开发界面，同时又有调用操作系统的能力。

热门技术栈：打造跨平台桌面APP的[electron](https://github.com/electron/electron)、专注于物联网应用开发，多平台兼容的硬件框架的[johnny-five](https://github.com/rwaldron/johnny-five)

主要涉及内置模块：os、fs

### 打造前端开发工具链

JS 作为一门脚本语言，用来打造前端的开发工具链也是再合适不过啦。我们将代码上线之前都可以用工具做批处理，例如：压缩混淆代码、处理兼容问题、注入环境变量、转换文件内容、校验代码风格等等

热门技术栈：社区生态独占鳌头的打包工具[webpack](https://webpack.js.org/)、以自动化及增强工作流著称的[gulp](https://www.gulpjs.com/)、更专注于打包JS的[rollup](https://rollupjs.org/)、代码风格校验工具[eslint](https://github.com/eslint/eslint)、模板处理工具[ejs](https://github.com/mde/ejs)、单元测试工具[jest](https://github.com/facebook/jest)等等

主要涉及内置模块：fs、stream、readline

### 实用工具 serve

推荐大家一个非常实用的静态文件服务启动工具：serve，本地启动 html 调试环境非常方便，假设我们的目录是这样的：

```
- src
  - detail
    - index.html
    - index.js
    - index.css
  - home
    - index.html
    - index.js
    - index.css
```

我们可以这样使用 serve：

```sh
# 安装 serve
npm i serve -g
# 切换到 src 目录
cd src
# 启动本地服务
serve
```

此时命令行就会输出已运行的本地静态服务，打开 `http://localhost:3000` 或者 `http://localhost:3000/home/index` 即可看到目录或页面。注意：因为 serve 会暴露启动路径下的所有目录，不应该在线上环境使用。

### WebAssembly

当运行的代码遇到性能瓶颈时，可以考虑用高性能语言（C++、Rust）编译成 .wasm，在 NodeJS 中引入执行。[官方教程：使用 WebAssembly](http://nodejs.cn/learn/nodejs-with-webassembly)

## Webpack

Webpack 作为 `create-react-app` 和 `vue-cli` 的内置打包工具，前端同学已经绕不开他了。他的社区生态非常成熟完善，通过各式各样的 loader 和 plugin ，能够处理开发者编写的代码，最终输出能够部署的静态资源或者启动本地开发环境。实战中最明显的就是处理 .vue 文件、.less 文件、.ts文件。如果你不想你的代码经过处理，可能只能低效率编写规范兼容的 HTML/CSS/JS 文件了。这里我们介绍 [Webpack5](https://webpack.docschina.org/)的一些知识和实战技巧：

### 概念

[官网](https://webpack.docschina.org/)介绍了七个概念：entry、output、loader、plugin、mode、browser compatibility、environment，我们先记住两个:entry 和 output 即 入口和输出。Webpack 的工作很纯粹，如官网的首图展示，你给我一堆东西，我只管弄成 HTML/CSS/JS/JPG 以让浏览器执行。打个比方就是你吃饭吃面吃蛋糕，最后都消化成糖分蛋白质。而怎么处理，最主要的是依靠各类 loader 和 plugin。

### loader

loader 的作用是预处理文件，输入一个文件的内容，输出一个或多个文件。loader是简单、能链式调用、模块化、无状态的，一组 loader 按顺序将处理结果往下传递，最后一个 loader 返回符合 webpack 要求的 JS 文件。

常用的 loader 有：

- babel-loader，负责将 ES2015+ 代码转换为 ES5，同时支持 jsx 语法
- ts-loader 将 TypeScript 2.0+ 转换为 JS 代码
- vue-loader 将 .vue 代码转换为 JS 代码
- less-loader 将 .less 代码转换为 CSS 代码，常和 css-loader、style-loader 配合使用

### plugin

Webpack 有非常强大的插件装载机制，可以做任何 NodeJS 能做的事情。Webpack 会在根据配置，在合适的编译生命周期调用插件并传入 webpack compiler 作为参数。也就是说，插件可以在打包过程中的任意时机执行，且可以访问到过程中 Webpack 暴露的所有信息。Webpack 编译过程有[二十多个生命周期](https://webpack.docschina.org/api/compiler-hooks/)钩子函数。我们看看常用的 plugin 来了解能做些什么：

- [html-webpack-plugin](https://github.com/jantimon/html-webpack-plugin) - 生成 HTML 文件并注入打包结果中
- [copy-webpack-plugin](https://github.com/webpack-contrib/copy-webpack-plugin) - 复制静态资源到指定目录
- [clean-webpack-plugin](https://github.com/johnagan/clean-webpack-plugin) - 负责清理指定目录
- [terser-webpack-plugin](https://github.com/webpack-contrib/terser-webpack-plugin) - 压缩 JS 文件
- [webpack-bundle-analyzer](https://github.com/webpack-contrib/webpack-bundle-analyzer) - 分析打包后的文件大小
- [webpack-dev-server](https://github.com/webpack/webpack-dev-server) - 启动本地开发环境服务
- [webpack-hot-middleware](https://github.com/webpack-contrib/webpack-hot-middleware) - 开发环境文件改动热更新

### 实践技巧

- 使用 ProvidePlugin 注入全局变量，可以是 jquery， lodash 或者 process.env.NODE_ENV 等等
- 如何提升打包速度：对 loader 处理的文件做筛选，例如 babel-loader 配置 exclude 选项；配置 cacheDirectory 将处理结果缓存到文件系统中，避免重复处理；
- 对于 项目A 需要引用 项目B 的某个模块时，可以使用[模块联邦](https://webpack.docschina.org/concepts/module-federation/#root)特性跨项目引用，轻教程：[了解 webpack5 模块联邦](https://juejin.cn/post/6844904134563512334)。
- 对于 Vue、React 这类官方初始化的单页应用，项目中如果有几个互不相干的模块，可以加入多个入口（entry），将不同的模块输出成不同的 html 文件，形成多个单页应用 模式。能够很好的减小打包体积，加快各模块的加载速度。

## 依赖管理：npm、cnpm、yarn、pnpm

当我们要使用社区生态里的工具库时，我们会使用依赖管理工具，例如官方的npm。在 package.json 中记录依赖库和版本号，然后使用 `npm install` 命令安装。此时会生成 package.lock.json 文件，记录依赖库及他们依赖的其他库的信息，确保每个人安装依赖的结果能够相同。

### 依赖安装常见问题处理：

- 依赖库会存放在安装路径的 node_modules 下，如果安装有问题想重新安装，可以删除这个目录；
- 如果有的同学安装成功，有的同学安装不成功，实在难以解决的话，可以把安装成功的 node_modules 目录拷贝给不成功的同学用；
- 长期维护的项目，package.json 中应指定固定版本号，不应使用 `^16.8.0` 或 `~16.8.0`，而是写定 `16.8.0`，避免不必要的更新，能够确认的更新则可以直接修改固定版本号。
- node-sass等难安装的依赖库如果安装失败了，可以尝试用 nvm 切换 node 版本、或者切换国内镜像源、或者使用科学上网重试

### 镜像源

官方使用的依赖管理工具是npm，日常使用推荐切换到国内镜像源，加快依赖安装速度，使用下面的命令切换淘宝镜像源地址：

```shell
npm config set registry https://registry.npm.taobao.org
```

cnpm 适合国内使用，yarn 和 pnpm 在安装速度上有优化，不论使用哪一种，最终都是希望能安装上相应的依赖。一个团队内应该统一依赖安装工具。
