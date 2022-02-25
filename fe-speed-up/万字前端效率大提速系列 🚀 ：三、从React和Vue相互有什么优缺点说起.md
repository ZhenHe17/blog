# 万字前端效率大提速系列 🚀 ：三、从React和Vue相互有什么优缺点说起

虽然React的官网介绍是UI库，Vue的官网介绍是渐进式框架。但是围绕这两者产生的社区及生态俨然成为了当下前端的两大体系。下面直接用一个超常见的辩题来讨论他俩：

## 超常见辩题：React和Vue相互有什么优缺点？

仁者见仁智者见智，其中“优缺点”仅是个人看法，这里抛出我自己的观点供大家参考总结。

1. 首先这两者的功能和使用场景都是相同的，社区生态也积累了多年非常的完善，不存在说有的场景只能用其中一个而另一个做不到。所以两者的比较主要是开发体验 (developer experience) 上的比较。

2. 从React是UI库，而Vue是框架上可以看出，React的封装程度较低，主要负责组件的渲染和生命周期执行；为了编写的方便引入了JSX语法，在编写时HTML标签只是有着特殊数据结构的JS对象（即HTML标签经JSX转换为虚拟DOM）；引入的概念和API少，完全是使用JS自身的语法来写代码。而Vue在生命周期上和React相似，但在HTML上使用了模板语言，因而引入了指令、过滤器、插槽的概念。在这一层面，我认为React的设计哲学更胜一筹，没有重复造轮子，完全用JS来控制DOM，但因为JS本身的强大，使得代码变得五花八门，在代码风格上需要更多的约束来统一。而Vue因为统一使用 v-if、v-for等指令，虽然引入了更多的API，但因为二次封装的简单好用统一，更符合KISS（keep it simple, stupid）原则。

3. 从数据流上看，React是单向数据流，通过显式的setState更新组件的状态刷新视图；Vue则是通过观察者模式将视图和数据进行双向绑定；虽然Vue的方式代码量更少更便捷，但由于观察者模式本身的缺点，多个观察者之间互不清楚彼此的存在，同时触发可能造成意外情况。并且作为开发人员，对数据流有清晰的认识和控制也非常重要，长期使用双向绑定会产生依赖。

4. 单独说些React的缺点，因为封装程度低，开发人员需要花更多时间在项目的基建上，不同的公司、项目的基建差异会非常大，代码风格差异也非常大。归根结底是本身的灵活性带来的弊端。反之这些则是Vue的优点，项目的基建和代码风格更统一，开发人员的项目切换成本和学习成本都小。

5. Vue我最想说的缺点就是内置指令带来的数据类型混乱，例如v-on既可以传入表达式又可以传入函数：

```html
  <button @click="state.count++">count is: {{ state.count }}</button>
  <button @click="onAdd">count is: {{ state.count }}</button>
  <button @click="onAdd(5)">count is: {{ state.count }}</button>
  <button @click="() => {onAdd(10)}">count is: {{ state.count }}</button>
```

对于熟练的同学，这是个便捷的语法糖，但对于代码执行时的数据格式尚不能完全掌握的同学，这简直是噩梦般的影响，能分清上面4行代码传给@click的分别是什么数据类型吗？长期懵懵懂懂的这样写，对理解代码执行中的数据类型、理解JS的函数式调用都是十分有害的。

列出了这些“优缺点”，做一个简单的总结，从开发体验上，我是喜欢React的开发模式的，不用记忆很多的API，灵活且便捷。但从团队的角度，抛开公司已有的基建考虑，Vue显然更适合团队使用，有内置API、代码风格高度统一；学习成本较低的优点。

## React常用代码技巧

回顾一下JSX的功能，HTML会被转换成特殊结构的JS对象。就是说我们编写时是把HTML标签作为对象数据类型来声明、操作、传递的。例如：

```jsx
    visible && <div>
       visible 为true时显示、false时隐藏
       应确保visible的数据类型为布尔值，可以用!!visible进行转换
    </div>
```

```jsx
    visible ? <div>
       visible 为true时显示
    </div> : <div>
       visible 为false时显示
    </div>
```

```jsx
    const starIcon = <i className="fa fa-star" />
    ...
    render() {
        return <div>
            {starIcon} 可以多处复用的HTML片段
        </div>
    }
```

```jsx
    const renderHeader = title => (
        // 函数返回了html片段
        <h1>{title}</h1>
    )
    const renderList = items => {
        // 函数返回了数组
        return items.map(item => (
            <li>{item.listContent}</li>
        ))
    }
    const renderParentContent = () => {
        // 函数返回了带父组件状态的html片段
        const { parentState } = this.state
        return <div>{parentState}</div>
    }
    ...
    render() {
        return <div>
            {renderHeader('Hello World')}
            {renderList([{listContent: 'Hello'}, {listContent: 'World'}])}

            <!-- 把函数传递给子组件，使子组件渲染带有父组件状态的html片段，子组件调用this.props.renderContent() -->
            <MyComponents renderContent={renderParentContent} />
        </div>
    }
```

### 关于Class类组件和Function函数式组件

由于Hooks的引入，使函数式组件能够携带组件状态，在功能上和类组件一样强大了。实践上也出现了两种语法混用、或者只使用其中一种的情况，社区里也有两者好坏的争论。相比之下，函数式组件写起来也更为简洁。但在比较复杂的场景，函数式组件的思考负担很重，例如useEffect的第二个参数依赖数组会很长，开发人员需要思考这些依赖改变了都会触发useEffect；同时执行异步函数时，也会产生闭包问题，导致拿到旧的组件状态。而类组件的生命周期触发是固定时机次数的、且属性挂载在this上，获取到的都是最新的状态，不会产生闭包问题。所以我认为简单的组件、页面是可以使用函数式组件的，开发体验好、编写效率高；不熟练的同学面对复杂的场景还是使用类组件更稳妥。

## Vue常用代码技巧

Vue的代码技巧主要围绕内置API在不同场景下的使用。官网的示例和最佳实践也非常完善，这里提一些容易忽略的点：

1. 使用计算属性或全局属性

我们将接口获取的原始数据作为数据源，在`<template />`里多使用计算属性或全局属性将数据源转换为展示数据，不要改变数据源中的数据。

```jsx
res.data = {
    startTimestamp: 1568888888,
}

app.config.globalProperties.$filters = {
    formatDate: function (value) {
        return moment(value).format('YYYY-MM-DD')
    }
}

<div>{{startDate}}</div>
<div>{{ $filters.formatDate(startTimestamp) }}</div>

VueComponent = {
    computed: {
        startDate: {
            get() {
                return moment(res.data.startTimestamp).format('YYYY-MM-DD')
            }
        }
    }
}

```

2. 使用社区中好用的指令、封装符合自己业务的自定义指令

用好拿来主义，社区里有很多好用的指令，比如：

- [vue-infinite-scroll](https://github.com/ElemeFE/vue-infinite-scroll) 给DOM绑定无限滚动加载事件，通常用于无限滚动列表
- [v-hotkey](https://github.com/Dafrok/v-hotkey) 给DOM绑定键盘按键事件（如回车键、Esc等）
- [vue-dragging](https://github.com/hilongjw/vue-dragging) 给DOM绑定可拖动事件
- [v-copy](https://github.com/egoist/v-copy) 点击复制内容

多逛逛社区，找到能用上的资源对开发效率的提升是巨大的，可能一个需求搬个库过来就解决了。

## 新起一个项目应该考虑哪些问题？

工作中有时会需要新起一个项目做，最终希望新的项目结构应该是上手快、开发效率高、性能好、能够长期维护的，要做到这些得先考虑下面几个问题：

1. 考虑团队的配置

当前团队擅长哪些技术栈？大家希望使用哪些技术栈？如果是练手的小项目可以选择感兴趣的技术做；如果是公司准备长期迭代的重点项目，则优先考虑小伙伴是否擅长，生态是否完善、学习成本高不高、公司内对应的基建怎么样。

2. 分析应用的场景

Web应用的场景有几种，PC端、移动端H5、APP内嵌页面、小程序。如需要SEO可以考虑服务端渲染；需要页面过渡自然则应该使用单页应用；无论哪端的应用都应该尽量减少体积，提升加载速度；内嵌WebView要考虑和native端的通信交互，在必要场景下通过native组件同层渲染来提高用户体验等

3. 新项目应具有的功能

除纯静态页面外，应用中最基础的就是和服务器端的交互，封装好对应场景下的通用请求函数是必要的。在请求函数里用拦截器对不同的公共错误码做处理，例如401跳登录、403报无权限等。

一个成熟的前端项目通常还应该引入基础组件库（如antd、echart）；有符合自己业务二次封装的自定义组件（指令、插槽）；有通用的、业务用的函数库（如moment、axios）；要考虑到不同屏幕宽度下的展示；中后台系统还要考虑页面头部脚部菜单等。

### 项目实战

开发用于移动端app内的h5页面，主要是一些活动页和功能页

#### 分析项目特点

由于是用于移动端app的页面，大致有以下几个需要注意的点：
1.体积尽可能轻量
2.不需要注意老浏览器的兼容问题
3.需要注意不同屏幕尺寸、不同机型的显示差异
4.需要注意安卓端和IOS端的差异

#### 技术栈选择

因为之前有react的开发经验、且react的社区成熟又丰富，所以考虑使用react相关的技术栈。又因为体积尽可能轻而选择了preact。preact打包后仅3kb，技术栈风格注重轻量简洁，在使用preact-compat后能达到近乎和react一样的开发体验、使用绝大多数社区中成熟的react轮子。所以preact技术栈很适合这个项目。

css相关：

* css预处理器选择了less
* 栅格化布局选择了bootstrap-grid，由于12等分的布局有所局限，所以修改了官方仓库的参数，打包了一个24等分且没有gutter的版本，约27kb
* css reset使用了minireset.css，约0.5kb
* 使用rem作为单位，根据屏幕的宽和设备dpr调整显示比例（[rem.js](https://github.com/ZhenHe17/blog/blob/master/example/lib/rem.js)）。

其他辅助库：

* 在ajax请求上使用了axios
* 辅助函数库使用了lodash，因为使用时是按需加载，所以对打包体积影响很小

在这套技术栈下的开发体验良好，最终生产环境的js包大小约280kb，加载完资源（load）到页面展示（finish）约0.3秒，在网络畅通的环境下能够在1秒内展示出页面，效果较为满意。

## 组件封装技巧

在平时的开发工作中，我们可以通过编写自研组件来提高开发效率，并且为团队提供技术沉淀。组件作为面向开发人员的产品，核心是关注开发体验，编写的时候注意标明参数类型和使用示例，先让自己愿意用，再做到大家都愿意用。组件都会写，但是写组件 -> 写出团队爱用的组件 -> 将组件体系化 每一步都需要大量的积累和实践。

### 不同的组件抽象类型

#### 基础组件

基础组件举例：公共组件库如 vant、 antd 等，项目中自研的搜索框、按钮等组件都属于基础组件。

基础组件的特点通常有：

- 具有跨项目通用性，不同项目相同场景下，组件可以通用，比如大家的后台管理系统都可以用antd
- 固定输入、固定输出，通过 props 可以完全控制组件的行为、状态

#### 业务组件

针对某个业务场景特地编写的组件，如后台中二次封装的表格组件，约定了和本公司后端接口的分页方式等

业务组件的特点通常有：

- 具有定制化的业务逻辑，仅能在特定业务项目中使用
- 约定大于配置，在使用上有需要注意的约定部分和默认行为

#### 高阶组件

> 具体而言，高阶组件是参数为组件，返回值为新组件的函数。

由于 vue 组件是由 .vue 文件编写的，所以高阶组件在 vue 中的应用不广泛。更多是通过 ``` slot-scopes ``` 和 二次封装组件的方式进行逻辑抽象

### 从需求评审中分析可以抽象的组件

常见场景：带有底部按钮的弹窗

可抽象的基础组件（或者使用公共组件库）：弹窗、按钮
可抽象的业务组件：底部固定按钮 - 通过对按钮组件进行二次封装来契合场景，统一用户体验

### 需求开发中，如何平衡逻辑抽象与组件复杂性之间的矛盾

场景示例：表单

#### 写法1：

``` jsx

<templete>
    <Input v-model="userName">
    <Input v-model="password">
    <Radio v-model="isRemember">
    <DatePicker v-model="currentDate">
</templete>
```

#### 写法2：

``` jsx
const formArr = [
    {
        type: 'input',
        key: 'userName',
    },
    {
        type: 'input',
        key: 'password',
    },
    {
        type: 'radio',
        key: 'isRemember',
    },
    {
        type: 'date',
        key: 'currentDate',
    },
]

<templete>
    <div v-for="item in formArr">
        <div v-if="item.type === 'input'">
            <Input v-model="item.key">
        </div>
        <div v-if="item.type === 'radio'">
            <Radio v-model="item.key">
        </div>
        <div v-if="item.type === 'date'">
            <DatePicker v-model="item.key">
        </div>
    </div>
</templete>
```

这样的逻辑抽象值得吗？节省了多个相同 type 的表单元素

#### 写法3：

``` jsx
const formArr = [
    {
        type: 'input',
        key: 'userName',
    },
    {
        type: 'input',
        key: 'password',
    },
    {
        type: 'radio',
        key: 'isRemember',
    },
    {
        type: 'date',
        key: 'currentDate',
    },
]

<templete>
    <GenerateForm config="formArr" />
</templete>
```

抽象出了 GenerateForm 后，在开发体验上有提升吗

在保证代码的可读性、不会过于耦合的情况下，适当的逻辑抽象才能够提高开发效率。有时简单的复制粘贴也不失是一种高可读、低耦合的代码编写方式

### 最佳实践

自研组件在团队中的最佳实践有：

- 编写基于 TypeScript 的私有基础组件库
- 对基础组件做完整的单元测试覆盖
- 根据需求，分析合适的业务组件场景
- 通过二次封装基础组件的方式，编写合适的业务组件

## 小结

这俩是前端日常工作中接触最多的了，我们可以站在巨人的肩膀上，一方面多学习框架的原理、一方面多学习优秀的组件，结合工作上的实践，不断做到实践上的自我提升。

---小尾巴--- 我叫葛圣明，擅长前端，有想法有问题欢迎私聊讨论 ^_^

