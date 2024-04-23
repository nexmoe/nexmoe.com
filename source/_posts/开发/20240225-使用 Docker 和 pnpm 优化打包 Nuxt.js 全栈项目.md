本文将指导你如何为一个结合了 Prisma 和 Nuxt.js 的全栈项目创建优化后的 Docker 镜像，并使用 pnpm 作为包管理器。

我的项目最终镜像大小从 1.12GB 缩减到了 160.21MB。

## 我的项目构成

Nuxt.js 是一个基于 Vue.js 的服务器端渲染应用框架，非常适合于构建现代化的 Web 应用。

我的项目直接采用 Nuxt 构建全栈项目。

- Nuxt3
- Prisma
- PNPM

## 开始构建

首先，我们将使用 `node:20-alpine` 这个更轻量级的基础镜像来减小最终镜像的大小。Alpine Linux 因其安全、简单且体积小而广受欢迎。

多阶段构建是减少 Docker 镜像大小的有效策略之一。我们将使用三个阶段来构建我们的镜像。

### 第一阶段：构建依赖项

```docker
ARG NODE_VERSION=node:20-alpine

FROM $NODE_VERSION AS dependency-base

WORKDIR /app

RUN npm install -g pnpm

COPY package.json pnpm-lock.yaml ./

RUN pnpm install --frozen-lockfile`
```

这一阶段负责安装我们项目的依赖项。我们使用了 pnpm 来代替 npm，pnpm 在缓存和磁盘使用上更为高效。

大部分项目也用 pnpm 而不是 npm 作为包管理工具了。

### 第二阶段：构建应用程序

```docker
FROM dependency-base AS production-base

COPY . .

RUN pnpm run build 
```

在这一阶段，我们复制了项目代码并执行构建命令。这里的构建指的是 Nuxt.js 的构建过程，它会生成静态文件和服务器端渲染所需的资源。

### 第三阶段：生成生产镜像

```docker
FROM $NODE_VERSION AS production

COPY --from=production-base /app/.output /app/.output

ENV NUXT_HOST=0.0.0.0 \
    NUXT_APP_VERSION=latest \
    DATABASE_URL=file:./db.sqlite \
    NODE_ENV=production

WORKDIR /app

EXPOSE 3000

CMD ["node", "/app/.output/server/index.mjs"]
```

最后，我们创建了适用于生产环境的镜像。这个镜像仅包含用于运行应用程序的必要文件，减少了不必要的层，使得镜像尽可能地保持精简。

我们还定义了一些环境变量，比如 `NUXT_HOST` 和 `DATABASE_URL`，这些是 Nuxt.js 应用和 Prisma 所需要的。其中，`DATABASE_URL` 被设置为使用项目根目录下的 SQLite 文件作为数据库。

最终通过暴露端口 `3000` 并指定启动命令来运行 Nuxt.js 应用程序。

## 不同构建方式的镜像大小比较

分别为：

- 3 步构建
- 2 步构建
- 直接构建

![a3c345aaa51a4b8b802c25bc9d3591c0.png](https://i.dawnlab.me/a3c345aaa51a4b8b802c25bc9d3591c0.png)

## Dockerfile 总览

```docker
# Use a smaller base image
ARG NODE_VERSION=node:20-alpine

# Stage 1: Build dependencies
FROM $NODE_VERSION AS dependency-base

# Create app directory
WORKDIR /app

# Install pnpm
RUN npm install -g pnpm

# Copy the package files
COPY package.json pnpm-lock.yaml ./

# Install dependencies using pnpm
RUN pnpm install --frozen-lockfile

# Stage 2: Build the application
FROM dependency-base AS production-base

# Copy the source code
COPY . .

# Build the application
RUN pnpm run build

# Stage 3: Production image
FROM $NODE_VERSION AS production

# Copy built assets from previous stage
COPY --from=production-base /app/.output /app/.output

# Define environment variables
ENV NUXT_HOST=0.0.0.0 \
    NUXT_APP_VERSION=latest \
    DATABASE_URL=file:./db.sqlite \
    NODE_ENV=production

# Set the working directory
WORKDIR /app

EXPOSE 3000

# Start the app
CMD ["node", "/app/.output/server/index.mjs"]
```
