### II.深入浅出webpack

原书在线地址：[《深入浅出webpack》](http://webpack.wuhaolin.cn/)

1-7 ：
> Webpack 有以下几个核心概念：
> 
> * Entry：入口，Webpack 执行构建的第一步将从 Entry 开始，可抽象成输入。
> * Module：模块，在 Webpack 里一切皆模块，一个模块对应着一个文件。Webpack 会从配置的 Entry 开始递归找出所有依赖的模块。
> * Chunk：代码块，一个 Chunk 由多个模块组合而成，用于代码合并与分割。
> * Loader：模块转换器，用于把模块原内容按照需求转换成新内容。
> * Plugin：扩展插件，在 Webpack 构建流程中的特定时机注入扩展逻辑来改变构建结果或做你想要的事情。
> * Output：输出结果，在 Webpack 经过一系列处理并得出最终想要的代码后输出结果。

2-10 ：
> 通常你可用如下经验去判断如何配置 Webpack：
> 
> * 想让源文件加入到构建流程中去被 Webpack 控制，配置 entry。
> * 想自定义输出文件的位置和名称，配置 output。
> * 想自定义寻找依赖模块时的策略，配置 resolve。
> * 想自定义解析和转换文件的策略，配置 module，通常是配置 module.rules 里的 Loader。
> * 其它的大部分需求可能要通过 Plugin 去实现，配置 plugin。
