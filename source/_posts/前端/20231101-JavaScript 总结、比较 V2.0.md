## 为什么 Bun 这么快

### JavaScriptCore 引擎

Bun 使用 JavaScriptCore 引擎，这是 Safari 浏览器使用的引擎，而不是基于 Chromium 的浏览器和 Node.js 使用的 V8 引擎。JavaScriptCore 引擎经过了针对更快启动时间的优化，这有助于 Bun 的速度。

### 性能分析和优化

大量的性能优化。Bun 的初衷就是要快。[[2]](https://dev.to/builderio/a-first-look-at-bun-is-it-really-3x-faster-than-nodejs-and-deno-45od)。

### Zig 语言

Bun 利用 Zig 语言进行低级内存控制和消除隐藏控制流。Zig 的设计原则注重性能，通过利用 Zig，Bun 可以实现更好的内存管理和控制，从而提高速度 [[2]](https://dev.to/builderio/a-first-look-at-bun-is-it-really-3x-faster-than-nodejs-and-deno-45od)。

### 了解更多

1. [Bun 1.0 | Bun Blog](https://bun.sh/blog/bun-v1.0)
2. [A first look at Bun: is it really 3x faster than Node.js and Deno? - DEV Community](https://dev.to/builderio/a-first-look-at-bun-is-it-really-3x-faster-than-nodejs-and-deno-45od)
