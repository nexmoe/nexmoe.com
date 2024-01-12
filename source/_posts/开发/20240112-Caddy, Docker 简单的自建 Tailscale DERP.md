作为一个拥有全端加密且能进行端到端连接的服务。Tailscale 现在的免费账户已经支持连接100台设备，这对于个人用户来说绰绰有余。我的内网设备几乎都在使用Tailscale连接。在前段时间发布的 <https://zhuanlan.zhihu.com/p/659427990> 中表明我很喜欢用 Remote SSH，我经常借住 Tailscale 组成的内网使用 Remote SSH 进行远程开发。

然而，Tailscale 在中国大陆的网络环境中存在一个问题，就是经常出现高延迟或者连接不上的情况。好在官方允许用户自建DERP服务，以充当中继，解决这个问题。再也不用担心写代码写一半就突然断了，而且优秀的网络体验也能提高 VS Code 的 Port Forward 的体验，方便远程预览开发。

由于我本身就有一台低配置的云服务器，过去曾经使用 Caddy 作为反向代理服务器来运行我的 Alist 项目。所以这次也考虑在同一台服务器上使用 Caddy 作为反向代理来部署 DERP 项目。

采用 Caddy 主要是他相比较于 Nginx 使用很简单的配置就能满足大部分需求，还有体验良好的自动 SSL 管理。能省很多事。

废话不多说，直接开始配置了。

## 配置 Docker

```yml
// docker-compose.yml
version: '3'
services:
  derper:
    image: fredliang/derper
    restart: always
    ports:
      - 3478:3478/udp
      - 23333:443
    environment:
      - DERP_DOMAIN=derp.example.com
```

然后启动

```sh
sudo docker compose up
```

## 配置 Caddy

```yml
// Caddyfile
derp.example.com {
    reverse_proxy localhost:23333
}
```

重载 Caddy 的配置

```sh
sudo docker compose exec -w /etc/caddy caddy caddy reload
```

别忘了解析你的域名到你的 Caddy 服务器上。

## 配置 Tailscale

在 Access Controls 中配置

直达链接：<https://login.tailscale.com/admin/acls/file>

```json5
{
  // ... 其他 ACL 配置
  "derpMap": {
    "OmitDefaultRegions": true, // 是否只连接自建 derper 节点
    "Regions": {
      "900": {
        "RegionID": 900,
        "RegionCode": "myderp",
        "Nodes": [
          {
            "Name": "1",
            "RegionID": 900,
            "HostName": "derp.example.com", // 域名
            "STUNPort": 3478,
            "DERPPort": 443,
          }
        ]
      }
    }
  }
}
```

结束。

## 参考

1. <https://github.com/fredliang44/derper-docker>
2. <https://tailscale.com/kb/1118/custom-derp-servers>
3. <https://tailscale.com/blog/how-nat-traversal-works>
4. <https://icloudnative.io/posts/custom-derp-servers>
5. <https://kiprey.github.io/2023/11/tailscale-derp>
