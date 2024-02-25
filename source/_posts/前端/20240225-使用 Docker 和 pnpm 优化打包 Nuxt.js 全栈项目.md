我们将在此文章中探索如何使用 Docker 包装一个 Nuxt.js 的全栈项目，同时关注优化镜像大小，并使用了 pnpm 作为包管理器。

## 我的项目构成

- Nuxt3
- Prisma
- PNPM

首先，我们需要编写一个 `Dockerfile`，这是一个文本文档，包含了用于自动构建 Docker 镜像的所有命令。Nuxt.js 是一个基于 Vue.js 的服务器端渲染应用框架，非常适合于构建现代化的 web 应用。

为了保持构建后镜像的精简，我们将采取多阶段构建策略，并且选择了较小的基础镜像 `node:20-slim`。

## Dockerfile 总览

在多阶段构建中

- 第一阶段是'构建器'（builder），它负责安装依赖项和编译应用。
- 第二阶段是'生产'（production），它负责设置运行环境并运行编译后的应用。

以下是针对 Nuxt.js 应用的 Dockerfile 示例：

```Dockerfile
`# 使用较小的基础镜像
ARG NODE_VERSION=node:20-slim

# 第一阶段：构建应用
FROM $NODE_VERSION AS builder

WORKDIR /app

# 安装 pnpm
RUN npm install -g pnpm

# 仅拷贝 package 文件
COPY package.json pnpm-lock.yaml ./

# 使用 pnpm 安装依赖
RUN pnpm install --frozen-lockfile

# 拷贝应用的其余源代码
COPY . .

# 构建应用
RUN pnpm run build

# 清理不需要的文件以减小镜像大小
RUN pnpm prune --production

# 第二阶段：创建最终的生产镜像
FROM $NODE_VERSION AS production

WORKDIR /app

# 设置环境变量
ENV NUXT_HOST=0.0.0.0
ARG NUXT_APP_VERSION
ENV NUXT_APP_VERSION=${NUXT_APP_VERSION}
ENV DATABASE_URL=file:./db.sqlite
ENV NODE_ENV=production

# 从构建器阶段拷贝构建好的资源和 node_modules 到生产阶段
COPY --from=builder /app/.output ./.output
COPY --from=builder /app/node_modules ./node_modules

# 暴露端口 3000
EXPOSE 3000

# 启动应用
CMD [ "node", "./.output/server/index.mjs" ]` 
```

## 优化镜像大小

在这个 Dockerfile 中，我们使用了多阶段构建来优化最终镜像的大小。在构建阶段，我们首先安装了 pnpm 并仅拷贝了所需的 package 文件，然后进行安装依赖并构建源代码。完成后，我们执行了 `pnpm prune --production` 以移除开发依赖项，减小镜像大小。

在最终的生产镜像中，我们基于同样的精简基础镜像，并只从构建阶段拷贝必要的产物，比如已编译的 .output 目录和 node\_modules 文件夹，这使得生产镜像尽可能地轻量。

![93b559433eb57d0545d65675a3d5d915.png](https://i.dawnlab.me/93b559433eb57d0545d65675a3d5d915.png)

这个结果并不是最优结果，后续会继续优化。
