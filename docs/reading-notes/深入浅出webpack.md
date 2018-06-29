### II.深入浅出webpack

原书在线地址：[《深入浅出webpack》](http://webpack.wuhaolin.cn/)

1.7 核心概念：
> Webpack 有以下几个核心概念：
> 
> * Entry：入口，Webpack 执行构建的第一步将从 Entry 开始，可抽象成输入。
> * Module：模块，在 Webpack 里一切皆模块，一个模块对应着一个文件。Webpack 会从配置的 Entry 开始递归找出所有依赖的模块。
> * Chunk：代码块，一个 Chunk 由多个模块组合而成，用于代码合并与分割。
> * Loader：模块转换器，用于把模块原内容按照需求转换成新内容。
> * Plugin：扩展插件，在 Webpack 构建流程中的特定时机注入扩展逻辑来改变构建结果或做你想要的事情。
> * Output：输出结果，在 Webpack 经过一系列处理并得出最终想要的代码后输出结果。
