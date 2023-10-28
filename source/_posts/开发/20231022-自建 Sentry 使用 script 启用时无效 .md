最近在服务器上面自建了 Sentry。

用 script 方法加入到网站后，始终没有效果，然后在控制台中发现了下面的报错。

> The Sentry loader you are trying to use isn't working anymore, check your configuration.

于是去 Github 上找了下，看着应该是国内网络的问题。

在 sentry/sentry.conf.py 内添加下面内容可以解决。

```python
JS_SDK_LOADER_DEFAULT_SDK_URL = "https://browser.sentry-cdn.com/%s/bundle.tracing.replay.debug.min.js"
```

然后记得重启 Sentry 的 Docker 服务

```shell
sudo docker compose restart
sudo docker compose up -d
```

---

- [Sentry JS lazily-loadable loader doesn&#39;t work · Issue #22715 · getsentry/sentry](https://github.com/getsentry/sentry/issues/22715)
