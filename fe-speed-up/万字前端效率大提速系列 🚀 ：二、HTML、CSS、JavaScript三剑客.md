# 万字前端效率大提速系列 🚀 ：二、HTML、CSS、JavaScript三剑客

前端的同学肯定最熟悉这哥仨了，但是随着React和Vue的普及，很多本是理应了解的知识也逐渐变得陌生，本篇也以“知道什么能让我效率提升”为出发点做一个汇总。

首先大家需要知道[MDN](https://developer.mozilla.org/zh-CN/)，里面的三剑客文档比较完整也比较严谨，遇到没见过的API优先看MDN。部分没有翻译或者难以理解的内容，可以辅以[W3C](https://www.w3.org/)和[菜鸟教程](https://www.runoob.com/)来学习。

## HTML

浏览器之所以能展示我们的页面，是因为它能解析HTML文件。对于前端开发者来说，只要使自己的HTML文件及HTML包含的其他资源能够被下载，就能够使页面展示出来。通常我们会使用Apache和Nginx作为Web服务器，形式也主要分为静态构建和SSR（Server Side Rendering）两种。

### 静态构建和SSR

这里简述下两者的形态，以下属于静态构建：

1.目前主流的Vue、React项目多是通过```npm run build```打包出dist目录文件，再将文件放置到服务器上完成部署

2.其他任何以提供 ***下载文件*** 的形式，都属于静态构建，可以通过gulp、webpack等工具预处理，也可以不使用工具直接部署项目代码文件。

而SSR则是以服务器端提供接口，接口即时返回HTML的形式提供服务。早年有JSP (基于JAVA)、ThinkPHP (基于PHP)，现当下有 Express (基于NodeJS)、NextJS (基于React、NodeJS)、NuxtJS (基于Vue、NodeJS)等SSR框架，其中部分框架直接支持静态构建和SSR两种模式。

静态构建和SSR的优缺点是一道常见面试题，对于官网、静态展示页等场景，静态构建无疑是首选的，在部署了CDN之后，其访问速度和服务器开销都是极佳的。当然因为业务形态比较简单，即使使用SSR也不会有很大的差距。

我们经常说SSR利于SEO，也只是针对于当下的Vue、React的静态构建项目，因为这两个框架都使用虚拟DOM渲染真实DOM的方法，在JS没有执行前，HTML通常只挂有一个id=root的根节点，因爬虫抓取之后不会执行JS，无法获取更多的页面信息，因此对SEO不友好。而SSR可以选择在返回前渲染好HTML片段，达成SEO目的。如果你的SSR同样采用了异步获取数据、动态生成DOM的方式，那么效果也是不佳的。同理如果你直接提供了完整内容的静态文件，那么也是SEO友好的。

基于Vue、React框架的静态构建项目，因为是异步获取数据、动态生成DOM、单页应用等特性，导致了依赖包较大，首屏加载速度较慢。但因其单页应用的特性，在页面切换时的体验较好。而SSR正好相反，浏览器直接展示准备好的HTML页面，达到首屏加载速度的极致，但也使服务器的开销变大，部署维护更加繁琐。

我认为两者适用的场景不同，他们的关系也不是非此即彼，公司主营业务甚至可以通过首屏SSR+静态构建的方式，即首屏服务端渲染，而后加载构建资源，再将控制权交由浏览器实现单页体验的实践。除了要折腾，其他方面可以说是兼顾了两者的优点。

### 封装复用HTML片段

许多同学只封装方法或者逻辑完整的组件，其实多封装小段HTML也是很有好处的，可以避免很多复制粘贴带来的维护难题。例如：

``` html
<!-- 场景一：随处可用的图标 -->
<span class="iconfont-@#$xxx"></span>

<!-- 场景二：某个活动页面中有很多小标题 -->
<h2 class="xxpage-sub-title">小标题一</h2>
<h2 class="xxpage-sub-title">小标题二</h2>
<h2 class="xxpage-sub-title">小标题三</h2>

<!-- 场景三：固定展示的公司名称、邮箱等 -->
<div class="company-name">
  <span class="company-name-text">公司名称</span>
  <span class="company-email">公司邮箱</span>
</div>
```

场景一和场景三中，推荐封装为全局片段方便使用；在场景二中，只需要当前页面使用。下面看看在各类场景中，如何快速封装HTML片段。

原生HTML：

``` js
var renderCompanyInfo = function(dom) {
  const companyHTML = `
  <div class="company-name">
    <span class="company-name-text">公司名称</span>
    <span class="company-email">公司邮箱</span>
  </div>
  `;
  if (dom) {
    dom.innerHTML = companyHTML;
  }
};
// 使用时： 
renderCompanyInfo(document.querySelector('.company-info'));
```

JSX中：

``` jsx
  const companyHTML = (
  <div class="company-name">
    <span class="company-name-text">公司名称</span>
    <span class="company-email">公司邮箱</span>
  </div>
);
// 使用时： 
<div>
    {companyHTML}
</div>
```

在没有引入JSX的Vue项目里，只能以组件的形式封装HTML片段，使用不太方便，此时不建议封装小段HTML。

### 使用emmet快速生成HTML代码

举例看看[emmet](https://docs.emmet.io/)是怎么快速生成代码的，

在编辑器中输入：

```
    ul#nav>li.item$*4>a{Item $}
```

按emmet生成键输出：

```html
    <ul id="nav">
        <li class="item1">
            <a>Item 1</a>
        </li>
        <li class="item2">
            <a>Item 2</a>
        </li>
        <li class="item3">
            <a>Item 3</a>
        </li>
        <li class="item4">
            <a>Item 4</a>
        </li>
    </ul>
```

非常方便快速，想在VSCode中使用emmet，可以在设置中搜索emmet并设置，我是写代码时按tab触发的。其他的编辑器通常也有对应的设置或者插件。

## CSS

目前编写CSS大多都加入了预处理器和后处理器，热门预处理器有SASS、LESS、STYLUS，主要是加入更强大的语法支持，将相应的.scss、.less、.styl文件转换成.css文件，个人不推荐STYLUS，过于自由，在团队中，自由不见得是好事儿；后处理器有CSS clean、PostCSS等，功能有压缩文件、添加浏览器适配后缀等，输入和输出都是CSS文件。

### 善用变量

在各预处理器和原生CSS中都支持CSS变量，在antd中应用如下：

``` less
@primary-color: #1890ff; // 全局主色
@link-color: #1890ff; // 链接色
@success-color: #52c41a; // 成功色
@warning-color: #faad14; // 警告色
@error-color: #f5222d; // 错误色
@font-size-base: 14px; // 主字号
```

良好的变量命名可以大大的提升代码可读性、可维护性。而且妥善配置变量也可以做到秒换主题等效果。试想在项目中看到 ```@success-color``` 比看到 ```#52c41a``` 总顺畅多了吧，颜色维护、替换也非常方便。同时在项目初期规划CSS变量，开发成本是比较低的，可以说是有利无害。

### 善用样式继承和初始值

在应用样式继承之前，需要注意：不同的浏览器，甚至同个浏览器不同的版本，其默认样式是有差异的。项目中先要统一默认样式，最简单的统一边距的代码有：


```css
* {
    margin: 0;
    padding: 0;
}
```

标准些的做法是引入样式重置库例如[minireset.css](https://www.npmjs.com/package/minireset.css)，将边距、字体、表格、图片等统一设置，将浏览器差异抹平。

有了统一的默认样式之后，就可以安全的应用样式继承了。绝大部分情况下，HTML和CSS代码都是越少越利于维护的，多使用样式继承，能够有效的减少CSS代码量。

CSS属性分为继承属性和非继承属性两类，继承属性没有指定值时，取父元素的同属性计算值，常见的继承属性有：

```css
visibility、cursor、z-index
white-space、line-height、color、font、 font-family、font-size 等文字属性
text-indent、text-align、list-style、border-collapse 等块状元素、列表、表格属性
```

非继承属性没有指定值时，则取属性的初始值，可以使用 ```inherit```关键字显式的指定样式继承。常见的非继承属性有：

```css
background、height、position、border、float等
```

样式继承最常用于文字属性，如果我们设置了如下属性：

```css
body {
    color: #333;
    font-size: 14px;
}
```

很多同学出于不自信，或者直接从sketch拷贝了样式，导致项目里大量的出现了 ```color: #333; font-size: 14px;``` 等样式。初始样式同理，例如div标签的```display: block;```也是多余的。希望大家在写下样式之前问问自己，这条样式真的有必要吗，能不能通过继承样式或初始样式得到一样的效果。如果害怕不同浏览器的样式差异，也可以手动设置```div { display: block } ```来抹平。

### 布局技巧

布局前先注意，编写HTML和CSS应尽量**一次性写完再看浏览器**，不要频繁的切换刷新页面，很影响效率。如果有困难可以先按模块拆解，做到写完一个模块再看页面；布局应该**由外到内**，以页面 -> 大模块 -> 小模块 -> 某元素的顺序进行布局，外面没写好，里面很可能白写导致返工；多使用**流式布局，少写死宽高**，注意窗口大小适配，图片不需要拉伸的时候只写死宽或者高，一些间距使用换行或者padding、margin撑开。

以下列出一些技巧快速对页面进行布局：

- 栅格化布局，将页面24等分（或自定义等分），快速完成多列布局
  - [antd栅格](https://ant.design/components/grid-cn/)
  - [bootstrap-grid](https://getbootstrap.com/docs/4.1/layout/grid/)
- flex 布局
  - 使用 ` flex: <num>` 将子元素按比例等分
  - 使用 ` justify-content(align-items): center` 将子元素水平或垂直居中
  - 使用 ` justify-content(align-items): space-between` 将子元素两侧布局
  - 使用 ` justify-content(align-items): space-around` 将子元素等间距布局
- grid 布局仍存在一定的浏览器兼容性问题，目前不推荐使用[grid兼容情况](https://caniuse.com/css-grid)

### 模块化CSS、全局样式、原子类

为了避免全局污染，项目中多会引入模块化样式，其原理是在你写的class中加上页面独特的哈希值，例如`.mySpecialPage__<hash>`以达到不同页面样式相互独立的效果。其实如果有良好的实践，使用类嵌套：

```less
.my-special-page {
    .other-class{

    }
    .top{

    }
}
```

为每个页面加上页面的命名空间，不需要模块化样式也能达到一样的样式隔离效果。CSS模块化节省了一层嵌套的编写、同时避免和其他引入库的样式冲突，仅此而已。

有些同学会为使用模块化样式还是全局样式争辩，我的想法是：**小孩子才做选择，我全部都要**。在活动页，特殊页面中使用模块化，同时多提取公共样式如边距、文字属性、通用组件等，在项目中以以下方式混用，既可以做到样式独立，也不影响公共样式的复用。

```jsx
<div className={`${S.specialPage} public-padding-class`}>
</div>
```

最近开源社区还流行一种原子类编写样式的实践，代表库有 [tailwindcss](https://tailwindcss.com/)。原子类的实践就是引入库以后，只需要写class，不需要再编写CSS，通过大量的复用显著减少CSS体积，对响应式的支持也很友好。缺点是需要记忆大量好记或者不好记的类名，实践观念也需要转换，样式复用上以组件封装和官方的 @apply 指令为主。

### CSS问题通用思路

- 所谓CSS层叠样式表，最终生效的样式是由1.是否选中和2.优先级决定的。样式生不生效，先排查是否选中，再排查优先级。观察最终样式通过浏览器的devtool看computedStyle
- 布局由外到内，自顶向下。外层的结构（主要是宽和窗口适应）没有做好就不写里层
- 部分样式需要先决条件，例如 `flex: 1`需要 `display: flex`才生效，`vertical-align: middle`需要`display: inline、inline-block、table-cell`才生效，遇到样式不生效的时候要注意先决条件。
- 部分样式会影响子元素的行为，这个只能通过经验积累，例如子元素`position:fixed` 会被父元素`transform`影响，`display: flex`默认将`block`的纵向改为横向布局
- 通过 padding、margin 来控制高度而不是写死高度，能很好的解决换行导致样式错乱的问题。图片通过`height: auto`来解决图片拉伸的问题
- 快速页面布局，记住**栅格化和flex一把梭**
- 无他，唯手熟尔。文档查的多，代码写得多，就见怪不怪了

## JavaScript

如果说HTML、CSS相当于骨架和皮肉，JavaScript可以说相当于大脑和神经了。其运用也是前端开发人员最主要的考核点。

### 编写代码之前

编写代码是细节实现的过程，在编写代码之前，要对需求有一个整体的认识。在需求评审环节和团队达成一致，评审后花时间捋清每一个步骤，对关键的难点进行技术调研。以我的习惯，会先对需求进行分层建模分析，下面以常见的电商购物下单为例：

- 领域数据层
  - 领域数据层指业务模型映射到代码数据结构的一层，储存着当前业务的信息和状态。
  - 用户的登录态、商品信息、优惠券信息、支付信息、生成的订单信息等都属于领域数据层。
- 事件交互层
  - 事件交互层负责事件触发后，更新领域数据层的数据
  - 进入页面更新商品信息、点击购买生成支付订单、支付完成更新订单信息等都属于事件交互层。
- 界面展示层
  - 负责将领域数据层的数据渲染到界面上
  - 页面展示商品信息、支付信息、订单信息等都属于界面展示层。
  - 主流框架Vue、React都有能够更新数据到视图层的手段，如Vue的双向绑定，React的setState，各类store更新props等。
  - **注意**，在界面展示层既要保留领域数据层的数据格式，又要有数据对应的展示状态，不要为了展示而修改了领域数据层的数据结构。

对业务有了清晰的认识之后，就可以愉快的开始写代码啦。

### 编写高质量的 JavaScript 代码

什么是高质量的代码，我认为在一个长期维护的项目中，可读性应该是评价代码的第一要素；服务器性能和客户端性能则依场景而定，下面举一些编写中可能忽视的例子

- 在非极端场景中选择可读性高的写法
  - 1.`const doubleNum = num * 2` 和 2.`const doubleNum = num << 1;`，从性能上说是2更好，但是非极端情况，大家还是写1吧，keep it simple, stupid! 
- 编写高内聚、低耦合的模块化代码
  - 封装和方法和模块尽量纯净无外部状态依赖，能够固定输入、固定输出。尽量不影响传入的引用变量。
- 多使用内置函数以提高可读性
  - 多使用Array、Math等的内置方法，例如用`Math.pow(a, 2)`代替`a * a`，用`Array.prototype.map、reduce`代替一些场景下的for循环。

### TypeScript

当前社区趋势，TypeScript增长非常迅猛，我认为其最主要的好处是：用类型在编写者和调用者之间搭上了一座桥梁，通过方法名+返回类型、参数名+参数类型使编写的代码具有自描述性。

开发人员本身就应该对变量类型胸有成竹，TypeScript能够引导我们从开始编码时就确定类型，以测试驱动开发的角度思考，有机会大家应该多接触、多使用，对提升编码质量和编码速度有很大帮助。至于是否应该再项目中实践，再根据场景做判断。

当前社区有这样一种实践：前后端统一使用TypeScript开发，ORM使用TypeORM，数据类型一处定义，两端通用，能够极大的减少低级错误，提升开发效率。例如前端TypeScript + React，后端TypeScript + Nest.js。

### 学习其他库来自我提升

站在巨人的肩膀上，我们可以快速的自我提升。例如学习jQuery，我们能更清楚的认识很多DOM操作是如何实现的；学习lodash，如何写出便捷好用无副作用的方法；学习Vue，能够知道一个开发爱用的框架是如何设计的、如何处理模板语言的、数据如何便捷映射到视图等等。附上一些好的学习资源：

- [You-Dont-Need-jQuery](https://github.com/nefe/You-Dont-Need-jQuery)
- [You-Dont-Know-JS](https://github.com/getify/You-Dont-Know-JS)
- [You-Dont-Need-Lodash-Underscore](https://github.com/you-dont-need/You-Dont-Need-Lodash-Underscore)

### JavaScript问题通用思路

- 要搞清楚JavaScript运行的环境，代码是否经过打包工具的兼容处理。不同环境的内置方法和执行顺序会有所不同，常见环境有Chrome、NodeJS、微信公众号和小程序、IOS、Android的WebView等。
- 由于JavaScript是弱类型语言，编写时尽量固定变量类型或使用TypeScript。注意变量的初始类型和变化后的类型是否一致、是否都有处理。注意函数执行后的返回值及其类型。例如：

```js
const arr = [1,2,3]
const deleted = arr.splice(1, 1)
此时 arr 为 [1,3]，deleted 为 [2]，不要写出 arr = arr.splice(1, 1)导致非预期的值。
```

- 闭包是非常常见的面试题，我喜欢从作用域和栈内存的角度解释闭包。强调函数作用域和块级作用域，且因为执行代码时，变量的值或者引用地址是储存在栈内存中。由于栈内存先进后出的特性，在执行里层代码时，总是能访问到先进入栈底的外层变量。
- 熟练使用开发者工具或移动端vConsole工具调试代码，可以逐行执行来排查问题，坚信是自己代码错了。
