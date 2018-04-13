### I.你不知道的javascript

#### 作用域和闭包：
1.4：
> 作用域是一套规则，用于确定在何处以及如何查找变量(标识符)。如果查找的目的是对变量进行赋值，那么就会使用 LHS 查询;如果目的是获取变量的值，就会使用 RHS 查询。

4.4：
> 我们习惯将var a = 2;看作一个声明，而实际上JavaScript引擎并不这么认为。它将var a和 a = 2 当作两个单独的声明，第一个是编译阶段的任务，而第二个则是执行阶段的任务。

5.6：
> 当函数可以记住并访问所在的词法作用域，即使函数是在当前词法作用域之外执行，这时就产生了闭包。

#### this和对象原型
1.4：
> this 实际上是在函数被调用时发生的绑定，它指向什么完全取决于函数在哪里被调用。

2.6：
> 如果要判断一个运行中函数的 this 绑定，就需要找到这个函数的直接调用位置。找到之后就可以顺序应用下面这四条规则来判断 this 的绑定对象。
> 1. 由new调用?绑定到新创建的对象。
> 2. 由call或者apply(或者bind)调用?绑定到指定的对象。
> 3. 由上下文对象调用?绑定到那个上下文对象。
> 4. 默认:在严格模式下绑定到undefined，否则绑定到全局对象。