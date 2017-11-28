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

## 参考文档

[package.json](https://docs.npmjs.com/files/package.json)