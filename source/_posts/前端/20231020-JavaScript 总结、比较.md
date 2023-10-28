
## 【总结比较】type 和 interface 的区别

20231026

在 TypeScript 中，`type`和`interface`是用来定义类型的关键字，它们有一些区别和特点。

### 相同点

- 都可以用来定义对象、函数、联合类型等。
- 都可以用来拓展（extends）其他类型。

### 不同点

- `type`可以声明基本类型别名、联合类型、交叉类型等复杂类型，而`interface`只能用来描述对象结构的类型 [[1]](https://zhuanlan.zhihu.com/p/558315566)[[2]](https://juejin.cn/post/6844903749501059085)。
- `type`可以使用`typeof`获取实例的类型进行赋值，而`interface`不支持 [[2]](https://juejin.cn/post/6844903749501059085)。
- `interface`支持声明合并，可以将多个同名的接口进行合并，而`type`不支持声明合并 [[1]](https://zhuanlan.zhihu.com/p/558315566)[[2]](https://juejin.cn/post/6844903749501059085)。

### 总结

- 一般情况下，如果能用`interface`实现，就优先使用`interface`，因为它扩展起来更方便，提示也更友好 [[2]](https://juejin.cn/post/6844903749501059085)。适用于定义对象结构类型和需要进行声明合并的场景。
- 如果需要定义复杂类型，或者需要使用`typeof`获取实例的类型进行赋值，就使用`type`[[2]](https://juejin.cn/post/6844903749501059085)。

### 其它

- 元组类型是 TypeScript 中的一种特殊数据类型，与普通数组不同的是，元组中的每个元素可以具有不同的类型。

---

参考：

1. [TypeScript 中 type 和 interface 有什么区别？ - 知乎](https://zhuanlan.zhihu.com/p/558315566)
2. [Typescript 中的 interface 和 type 到底有什么区别 - 掘金](https://juejin.cn/post/6844903749501059085)
3. [TS 篇-type 和 interface 的区别 - 掘金](https://juejin.cn/post/7100017218305392647)
4. [type 与 interface 的区别，你真的懂了吗？ - 掘金](https://juejin.cn/post/7072945053936648200)
5. [元组 · TypeScript 入门教程](https://ts.xcatliu.com/advanced/tuple.html)

## 【总结】JavaScript 变量作用域和闭包

20231020

1. Lexical Environment

- 每个运行的函数、代码块或脚本都有一个关联的 Lexical Environment 对象，它用来存储局部变量和函数声明。
- Lexical Environment 有两个组成部分：Environment Record（存储变量）和对外部 Lexical Environment 的引用。
- 函数在执行时会生成一个内部的 Lexical Environment 来存储局部变量和参数。

2. 变量

- 变量实际上就是 Lexical Environment 的属性。读取/修改变量就是读取/修改这个对象的属性。
- 函数声明会被立即初始化，但 let/const 变量要到声明后才能读取。
- 块级作用域 - 在 {} 内声明的变量只在块内可见。

3. 嵌套函数

- 嵌套函数可以访问外部函数的变量，这使其可以封装外部变量。
- 嵌套函数会通过 Environment 属性记住它被创建的位置，所以它总能访问该位置的外部变量。

4. 闭包

- 闭包是可以记住外部变量并访问这些变量的函数。
- JavaScript 中所有函数都是天然的闭包，它们通过 Environment 属性实现。

5. 垃圾回收

- 如果嵌套函数被引用，则外部 Lexical Environment 不会被回收。
- 但是如果外部变量没有被嵌套函数使用，引擎会对其进行优化，使其无法在嵌套函数中访问。

6. 其他

- 理解 Lexical Environment 和闭包的工作原理可以更好地编写 JavaScript 代码。
- 嵌套函数和闭包在组织代码和封装变量方面很有用。
- 变量优化可能会使开发调试变得困难。

---

- [Variable scope, closure](https://javascript.info/closure)

## 【总结】async 和 await 使用时的注意点

20231012

1. 使用`await`命令时，最好将其放在`try...catch`代码块中处理可能的`rejected`结果，或者使用`catch()`方法捕获错误。
2. 多个`await`命令后面的异步操作如果互不依赖，应该同时触发，可以使用`Promise.all()`方法或者使用多个变量并行赋值的方式。
3. `await`命令只能在`async`函数中使用，如果在普通函数中使用会报错。如果在普通函数中使用`await`，可能会导致异步操作并发执行而不是继发执行，正确的做法是使用`for`循环或者`reduce()`方法。
4. `async`函数可以保留运行堆栈，不会中断函数执行，可以在异步任务运行期间继续执行其他操作。这样可以保留错误堆栈的完整性。

---

- [async 函数](https://wangdoc.com/es6/async)
- [async function - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
- [await - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await)

## 【总结比较】遍历语法的比较

20231008

- 最原始的遍历方法是使用`for`循环，但这种写法相对繁琐。
- 数组提供了内置的`forEach`方法，可以简化遍历操作，但无法中途跳出循环。
- `for...in`循环可以遍历数组的键名，但存在一些缺点，如键名是字符串、会遍历手动添加的其他键和原型链上的键，以及遍历顺序不确定。
- `for...of`循环是一种新的遍历语法，相比其他方法具有以下优点：
  - 语法简洁，类似于`for...in`循环。
  - 不同于`forEach`方法，可以使用`break`、`continue`和`return`来控制循环。
  - 提供了统一的遍历数据结构的接口。

使用`for...of`循环可以更方便地遍历数组，同时具有灵活控制和统一操作接口的优势。

---

- [Iterator 和 for...of 循环](https://wangdoc.com/es6/iterator)
