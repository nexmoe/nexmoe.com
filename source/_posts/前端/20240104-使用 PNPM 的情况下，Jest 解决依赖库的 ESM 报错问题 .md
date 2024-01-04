## 环境

- NX
- PNPM
- lodash-es
- Jest

## 从 karma 转移到 Jest 遇到了如下报错

主要原因是 "node_modules" 文件夹中 ESM（ECMAScript Modules） 库不被 Jest 支持。

鉴于 Jest ESM 支持还在几乎不可用的试验阶段，而目前我主要是在公司项目上迁移到 Jest。所以本文主要采用 `transformIgnorePatterns` 和 `moduleNameMapper` 两种配置来解决这个问题。

![11c629a593c4c8484b6cb8ca44d6aa5f.png](https://i.dawnlab.me/11c629a593c4c8484b6cb8ca44d6aa5f.png)

```shell
Test suite failed to run

Jest encountered an unexpected token

Jest failed to parse a file. This happens e.g. when your code or its dependencies use non-standard JavaScript syntax, or when Jest is not configured to support such syntax.

Out of the box Jest supports Babel, which will be used to transform your files into valid JS based on your Babel configuration.

By default "node_modules" folder is ignored by transformers.

Here's what you can do:
    • If you are trying to use ECMAScript Modules, see https://jestjs.io/docs/ecmascript-modules for how to enable it.
    • If you are trying to use TypeScript, see https://jestjs.io/docs/getting-started#using-typescript
    • To have some of your "node_modules" files transformed, you can specify a custom "transformIgnorePatterns" in your config.
    • If you need a custom transformation specify a "transform" option in your config.
    • If you simply want to mock your non-JS modules (e.g. binary assets) you can stub them out with the "moduleNameMapper" config option.

You'll find more details and examples of these config options in the docs:
https://jestjs.io/docs/configuration
For information about custom transformations, see:
https://jestjs.io/docs/code-transformation
```

以下配置主要以 lodash-es 作为参考。

## transformIgnorePatterns

官方文档的解释是：正则表达式模式字符串的数组，在转换之前与所有源文件路径匹配。如果文件路径与任何模式匹配，则不会对其进行转换。
即 `transformIgnorePatterns` 用于指定在进行代码转换时应该忽略的文件或文件夹。

而在 NX 默认的 Jest 配置中，配置为 `node_modules/(?!.*\\.mjs$)`。
这个正则表达式的含义是，匹配以 `node_modules/` 开头的文件夹路径，但排除那些以 `.mjs` 为扩展名的文件夹路径。`?!` 是一个否定预查，表示不匹配这样的文件夹路径。

```javascript
transformIgnorePatterns: ['node_modules/(?!.*\\.mjs$)'],
```

以上配置意思就是将会把以 `.mjs` 为扩展名的文件从 ESM 转换为 CommonJS，以支持 Jest。

## 添加转换 lodash-es

顺便支持一下 PNPM

```javascript
const esModules = ['.*\\.mjs$', 'lodash-es'].join('|');

export default {
    ...
    transformIgnorePatterns: [`node_modules/(?!.pnpm|${esModules})`],
    ...
}
```

转换后 failed 数量从 15 减少到 11，但是这么做会有一个转换的过程会有额外的支出，需要 51s。不过第一次转换完后貌似就会缓存然后就不用转换了。

![ef4e6aeef369b021b707664f9c03549a.png](https://i.dawnlab.me/ef4e6aeef369b021b707664f9c03549a.png)

## 支出更少的方法 moduleNameMapper

这种方法需要库本身有对应的 CommonJS，就不需要转换了。可以跑到 12s

```javascript
export default {
    ...
    moduleNameMapper: {
        '^lodash-es$': 'lodash',
    },
  ...
}
```

![e87d8ad99b64c8f836a8c1777ec217bf.png](https://i.dawnlab.me/e87d8ad99b64c8f836a8c1777ec217bf.png)

## 最终配置参考如下

```javascript
/* eslint-disable */
const esModules = ['.*\\.mjs$'].join('|');

export default {
  displayName: 'pc',
  preset: '../../jest.preset.js',
  setupFilesAfterEnv: ['<rootDir>/src/test-setup.ts'],
  coverageDirectory: '../../coverage/apps/pc',
  moduleNameMapper: {
    '^lodash-es$': 'lodash',
  },
  transform: {
    '^.+\\.(ts|mjs|js|html)$': [
      'jest-preset-angular',
      {
        tsconfig: '<rootDir>/tsconfig.spec.json',
        stringifyContentPathRegex: '\\.(html|svg)$',
      },
    ],
  },
  transformIgnorePatterns: [`node_modules/(?!.pnpm|${esModules})`],
  snapshotSerializers: [
    'jest-preset-angular/build/serializers/no-ng-attributes',
    'jest-preset-angular/build/serializers/ng-snapshot',
    'jest-preset-angular/build/serializers/html-comment',
  ],
};
```

## 参考

1. [Jest setup &quot;SyntaxError: Unexpected token export&quot;](https://stackoverflow.com/questions/42260218/jest-setup-syntaxerror-unexpected-token-export)
2. [Configuring Jest · Jest](https://jestjs.io/docs/configuration#transformignorepatterns-arraystring)
3. [ECMAScript Modules · Jest](https://jestjs.io/docs/ecmascript-modules)
4. [Configuring Jest · Jest](https://jestjs.io/docs/configuration#modulenamemapper-objectstring-string--arraystring)
