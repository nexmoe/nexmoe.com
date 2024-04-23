---
cover: https://i.dawnlab.me/44d311a9a11c6418fd98b3d5e5d69bc9.png
coverHeight: 412
coverWidth: 843
---

在多人局域网游戏如星露谷物语、饥荒、我的世界等日益流行的今天，拥有一个稳定且低延迟的游戏联机环境对于提升玩家体验至关重要。本文将介绍如何使用 Docker 快速部署一个私有的 ZeroTier Planet 服务器，实现联机游戏低延迟，高稳定性。通过虚拟局域网技术，让不同地点的玩家能够如同在同一个局域网中一样进行游戏。

## ZeroTier 简介

ZeroTier 是一种 P2P VPN 解决方案，它允许用户在互联网上创建一个虚拟的局域网，使得不同地理位置的设备能够像在同一个局域网内一样直接通信。也就是说，通过 ZeroTier，你可以和你的朋友在各自的家里，组成同一个局域网。然后你们就可以一起玩各种局域网联机游戏，如星露谷物语、饥荒、我的世界等。

ZeroTier 的核心组件包括 PLANET、MOON 和 LEAF，分别代表根服务器、卫星服务器和网络客户端。PLANET 是 ZeroTier 的根服务器，负责维护网络的全局状态和路由信息。

官方的 ZeroTier 服务器位于海外，对于国内用户来说，连接可能会不稳定。自建 PLANET 服务器可以有效解决这一问题，提升网络连接的稳定性和速度。

## 开始安装

### 准备条件

- 一台具有公网 IP 的服务器，需要开放 3443/tcp、9994/tcp 和 9994/udp 端口。
- 安装 Docker
- Debian10+，Ubuntu20+ 等内核大于 5.0 的系统

### 使用 Docker 安装

新建 docker-compose.yml 文件，插入下面内容，
记得修改下面中的 `IPV4IP ADDRESS` 为你的公网 IP 地址。

```docker
version: '3'

services:
  myztplanet:
    image: xubiaolin/zerotier-planet:latest
    container_name: ztplanet
    ports:
      - 9994:9994
      - 9994:9994/udp
      - 3443:3443
      - 3000:3000
    environment:
      - IP_ADDR4=[IPV4IP ADDRESS]
      - ZT_PORT=9994
      - API_PORT=3443
      - FILE_SERVER_PORT=3000
    volumes:
      - ./data/zerotier/dist:/app/dist
      - ./data/zerotier/ztncui:/app/ztncui
      - ./data/zerotier/one:/var/lib/zerotier-one
      - ./data/zerotier/config:/app/config
    restart: unless-stopped
```

在 docker-compose.yml 文件中目录下，使用如下命令启动

```bash
docker-compose up -d
```

### 后台配置

1. 访问 <http://ip:3443> 进入 controller 页面，使用默认账号为：admin，默认密码为：password

2. 进入后创建一个网络 (Add network)，可以得到一个网络 ID

3. 最后需要分配网络 IP: 选中 Easy setup->Generate network address

![1d6b3d12c3a688385c45f6b940f42fae.png](https://i.dawnlab.me/1d6b3d12c3a688385c45f6b940f42fae.png)

![b19f85de2600c57daefe7b9b344b27dc.png](https://i.dawnlab.me/b19f85de2600c57daefe7b9b344b27dc.png)

## Windows 客户端配置

1. 首先去 ZeroTier 官网下载一个 ZeroTier 客户端

2. 将服务器中 ./data/zerotier/dist 目录下 planet 文件覆盖粘贴到自己电脑的 `C:\ProgramData\ZeroTier\One` 中！[16b4f746c777faf281bfa6bf04d60365.png](https://i.dawnlab.me/16b4f746c777faf281bfa6bf04d60365.png)

3. Win+S 搜索 “服务”，打开 “服务”，找到 ZeroTier One，并且重启服务！[14bf0b688b1cbb9fb1525b66d68de414.png](https://i.dawnlab.me/14bf0b688b1cbb9fb1525b66d68de414.png)

4. 加入网络，在后台找到 ID，和平常使用 ZeroTier 加入网络的操作一样。

5. 授权访问：在 ZeroTier 控制器的管理后台，找到新加的客户端并授权其访问。![ba3580778567b5048dafedd2140ef540.png](https://i.dawnlab.me/ba3580778567b5048dafedd2140ef540.png)

6. 连通验证，马赛克处为你的公网 IP![bd952a5f1c0a915cd2b14128ec018783.png](https://i.dawnlab.me/bd952a5f1c0a915cd2b14128ec018783.png)

## 其他客户端

对于安卓客户端，目前只有非官方的客户端可用
<https://github.com/kaaass/ZerotierFix>

而 MacOS 用户需要替换 `/Library/Application Support/ZeroTier/One/` 目录下的 planet 文件，并重启 ZeroTier-One：`cat /Library/Application\ Support/ZeroTier/One/zerotier-one.pid | sudo xargs kill`，后续则与 Windows 配置相似。

## 游戏联机

你的朋友也需要完成客户端配置，才能与你组成局域网。你可以手把手的教他如何操作，也可以让他参考下面的文章进行操作。

[3 分钟加入朋友的私有 ZeroTier Plant 网络中](https://nexmoe.com/1KY107B.html)

当网络组成后，你可以在后台找到朋友在局域网中的 IP，

然后使用 `ping <IP>` 命令测试网络延迟。

## 猜你也想读

- [Caddy, Docker 简单的自建 Tailscale DERP](https://nexmoe.com/1A9J5KM.html)
- [用 VS Code 管理服务器，我有独特的服务器管理方式](https://nexmoe.com/1M3R9E6.html)