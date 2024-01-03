## Promise 与 RxJS Observables 的区别

### Promise

- Promise 是 JavaScript 中内置的，不需要任何额外的库。
- Promise 表示可能现在或将来可用的单个值。
- Promise 是急切的，也就是说一旦 Promise 被解析，`.then()`回调会立即执行。
- Promise 只能发出单个值。
- Promise 非常适合处理产生单个结果的简单异步操作。

### RxJS Observables

- Observables 是 RxJS 库的一部分，需要额外安装依赖。
- Observable 表示可以随时间发出的值流。
- Observable 是惰性的，也就是说在订阅之前不会执行任何操作。
- Observable 可以发出多个值，包括零个或多个值。
- 可以使用各种 RxJS 操作符对 Observable 进行转换和组合，以创建新的定制流。
- Observable 非常适合处理复杂的异步操作，例如实时数据流或事件驱动编程。

### 参考

1. [JavaScript Theory: Promise vs Observable - Medium](https://medium.com/javascript-everyday/javascript-theory-promise-vs-observable-d3087bc1239a)
2. [angular - What is the difference between Promises and Observables? - Stack Overflow](https://stackoverflow.com/questions/37364973/what-is-the-difference-between-promises-and-observables)
3. [JavaScript Promises vs. RxJS Observables](https://auth0.com/blog/javascript-promises-vs-rxjs-observables/)

## 模版语法的简单实现

```javascript
const name = 'Nexmoe';
const message = 'My name is {{name}} and I\'m {{getAge(20)}} years old.';

function getAge(age) {
  return age;
}

const replacedMessage = message.replace(/\{\{(.*?)\}\}/g, (match, variableOrFunction) => {
  const trimmedValue = variableOrFunction.trim();

  if (trimmedValue.includes('(')) {  // 如果占位符包含括号，则表示为带参数的函数替换
    const [functionName, ...args] = trimmedValue.split(/\(|\)/).filter(Boolean);
    const func = eval(functionName);
    return func(...args);
  } else {  // 否则为变量替换
    return eval(trimmedValue);
  }
});

onsole.log(replacedMessage);
```

先检查占位符中是否包含括号，如果包含括号，则表示是一个带参数的函数调用。使用`split`方法和正则表达式来解析函数名和参数，并将其存储在`functionName`和`args`变量中。然后，使用`eval`函数将函数名转换为实际的函数对象，并使用扩展运算符 (`...`) 将参数作为参数列表传递给函数。函数执行后，将返回值作为替换后的字符串返回。

如果占位符不包含括号，则表示是一个变量。直接使用`eval`函数将变量名转换为实际的变量值，并返回其值作为替换后的字符串。

⚠️ 注意：使用`eval`函数执行代码具有一定的安全风险，因为它可以执行任意的 JavaScript 代码。有相当多的建议建议不使用`eval`。准备过段时间研究研究不用`eval`的方法。

## MVVM 是什么

MVVM 代表 Model-View-ViewModel，在 MVVM 中，Model 表示应用程序的数据和业务逻辑，View 表示用户界面，ViewModel 充当 Model 和 View 之间的中介。

### 模型（Model）

- 模型代表应用程序中的数据和业务逻辑。
- 它可以是从服务器获取的数据、本地存储的数据或通过其他方式获取的数据。
- 模型通常实现了一些方法来操作、存储和管理数据。
- 对应的是组件中的 data、props 属性。

### 视图（View）

- 视图是用户界面的可见部分。
- 它负责展示数据给用户，并接收用户的交互操作。
- 在 Vue.js 中，视图通常由 Vue 组件表示，可以包含 HTML 模板和样式。

### 视图模型（ViewModel）

- 视图模型是连接模型和视图的中间层。
- 视图模型通常包含了与视图相关的数据、计算属性和方法，以及与模型交互的逻辑。
- 通过双向绑定（data-binding）将视图和模型连接起来。当模型中的数据发生变化时，视图会自动更新。通过 DOM 事件监听，当用户在视图中输入数据或进行其他交互操作时，视图模型会自动更新模型中的数据。

### 优势

- 分离关注点：将数据逻辑与视图逻辑分离，使代码更易于维护和测试。
- 提高开发效率：通过双向数据绑定和声明式编程风格，减少了手动操作 DOM 的代码量。
- 可重用性：通过组件化的方式，视图和视图模型可以在不同的应用程序中进行复用。
- 响应式更新：当模型中的数据发生变化时，视图自动更新，提供了更好的用户体验。

### 参考

1. [为什么尤雨溪尤大说 VUE 没有完全遵循 MVVM？ - 知乎](https://www.zhihu.com/question/327050991)
2. [Vue 的 MVVM 思想（包含三个常见面试题） - 掘金](https://juejin.cn/post/6879300070962003982)
3. [MVC，MVP 和 MVVM 的图示 - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2015/02/mvcmvp_mvvm.html)
4. [Getting Started - vue.js](https://012.vuejs.org/guide/)
5. [Comparing Vue.js to new JavaScript frameworks - LogRocket Blog](https://blog.logrocket.com/comparing-vue-js-to-new-javascript-frameworks/)

## MVC 是什么

MVC 这个概念已经存在很久了，用了这么多年，今天了解一下概念做个总结。

MVC（Model-View-Controller）设计模式将应用程序中的对象分为三个角色：模型（Model）、视图（View）和控制器（Controller）。该模式不仅定义了对象在应用程序中的角色，还定义了对象之间的通信方式。每种类型的对象都通过抽象边界与其他类型的对象分离，并在这些边界上与其他类型的对象进行通信。应用程序中某种 MVC 类型的对象的集合有时被称为层，例如模型层。

![848723f97c7a1b862e10abe0445da348.png](https://i.dawnlab.me/848723f97c7a1b862e10abe0445da348.png)

### 模型（Model）

- 封装应用程序特定的数据，并定义操作和处理数据的逻辑。
- 可以表示应用程序中的实体，如游戏中的角色或地址簿中的联系人。
- 可以与其他模型对象建立关联，形成对象图。
- 应该存储应用程序的持久状态数据。
- 不应与呈现数据和用户界面相关的视图对象直接连接。

### 视图（View）

- 用户可见的对象，负责显示数据和响应用户操作。
- 知道如何绘制自身，并可以与用户进行交互。
- 通常通过控制器对象从模型对象中获取数据进行展示和编辑。
- 在 MVC 应用程序中与模型对象解耦，提供一致性和重用性。

### 控制器（Controller）

- 充当视图对象和模型对象之间的中介。
- 负责处理用户操作，并将其传递给模型层进行数据处理和更新。
- 可以执行应用程序的设置和协调任务，管理其他对象的生命周期。
- 在模型对象发生变化时，将新的模型数据传递给视图对象进行显示。

### 优势

- 提供良好的应用程序设计，使对象更具可重用性和接口定义明确性。
- 支持应用程序的可扩展性，易于添加新功能和模块。
- 分离关注点，使代码更易于维护和测试。
- 应用程序的模型层、视图层和控制层之间保持了清晰的分离，实现了代码的结构化和职责的明确划分，从而提高了应用程序的可维护性和可扩展性。

### 参考

1. <https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/MVC.html>

## 为什么 Bun 这么快

### JavaScriptCore 引擎

Bun 使用 JavaScriptCore 引擎，这是 Safari 浏览器使用的引擎，而不是基于 Chromium 的浏览器和 Node.js 使用的 V8 引擎。JavaScriptCore 引擎经过了针对更快启动时间的优化，这有助于 Bun 的速度。

### 性能分析和优化

大量的性能优化。Bun 的初衷就是要快。[[2]](https://dev.to/builderio/a-first-look-at-bun-is-it-really-3x-faster-than-nodejs-and-deno-45od)。

### Zig 语言

Bun 利用 Zig 语言进行低级内存控制和消除隐藏控制流。Zig 的设计原则注重性能，通过利用 Zig，Bun 可以实现更好的内存管理和控制，从而提高速度 [[2]](https://dev.to/builderio/a-first-look-at-bun-is-it-really-3x-faster-than-nodejs-and-deno-45od)。

### 参考

1. [Bun 1.0 | Bun Blog](https://bun.sh/blog/bun-v1.0)
2. [A first look at Bun: is it really 3x faster than Node.js and Deno? - DEV Community](https://dev.to/builderio/a-first-look-at-bun-is-it-really-3x-faster-than-nodejs-and-deno-45od)
