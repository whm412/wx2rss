# wx2rss

把微信公众号文章整理为 RSS 2.0、JSON Feed 和 OPML，在自己的电脑、NAS 或服务器上运行。

> 这是 wx2rss 的公开介绍与用户反馈仓库。运行程序通过 Docker 镜像发布，本仓库不包含用户数据库、登录凭据、激活材料或服务端私密代码。

## 主要能力

- 微信扫码登录，自动获得公众号批量抓取能力；
- 订阅公众号并持续增量获取文章；
- 输出 RSS 2.0、JSON Feed 与 OPML；
- 多微信账号稳定分配、串行错峰与独立配额；
- 断网、休眠和恢复后的安全重排；
- 数据备份与还原，方便迁移设备；
- Telegram、Server酱、Webhook、Bark、钉钉机器人告警。

## 快速部署

中国大陆网络推荐阿里云镜像：

```bash
docker run --pull=always -d --name wx2rss -p 8000:8000 --restart unless-stopped -v "$PWD/data:/app/data" registry.cn-hangzhou.aliyuncs.com/whm412/wx2rss:latest
```

国际网络可使用 Docker Hub：

```bash
docker run --pull=always -d --name wx2rss -p 8000:8000 --restart unless-stopped -v "$PWD/data:/app/data" whm412/wx2rss:latest
```

启动后访问 `http://localhost:8000`。首次使用前请先完成微信与微信读书关联授权，再在 wx2rss 中扫码。

## 文档

- 产品介绍与部署说明：<https://www.wxsueq.cn/>
- Docker Hub：<https://hub.docker.com/r/whm412/wx2rss>
- 阿里云镜像：`registry.cn-hangzhou.aliyuncs.com/whm412/wx2rss`

## 反馈问题

请使用仓库的 Issue 模板提交问题或建议。提交日志前务必删除：激活码、邮箱授权码、微信 Token、Cookie、Webhook、数据库文件和完整机器码。

本项目调用的第三方服务可能调整接口或触发安全验证。项目会尽量减少请求、错峰执行和安全退避，但不能承诺永不触发第三方风控。
