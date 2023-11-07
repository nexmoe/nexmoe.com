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
