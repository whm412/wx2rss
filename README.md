# wx2rss

把微信公众号文章整理为 RSS 2.0、JSON Feed 和 OPML，在自己的电脑、NAS 或服务器上运行。

> 这是 wx2rss 的公开介绍与用户反馈仓库。运行程序通过 Docker 镜像发布，本仓库不包含用户数据库、登录凭据、激活材料或服务端私密代码。

## 主要能力

- 微信扫码登录，自动获得公众号批量抓取能力；
- 订阅公众号并持续增量获取文章；
- 输出 RSS 2.0、JSON Feed 与 OPML；
- 多微信账号稳定分配、串行错峰与独立配额；
- 断网、休眠和恢复后的安全重排；
- 任一账号触发风控后实例级冷却，禁止立即切换其它账号连续试探；
- 数据备份与还原，方便迁移设备；
- Telegram、Server酱、Webhook、Bark、钉钉机器人告警。

## 快速部署

中国大陆网络推荐阿里云镜像：

```bash
docker run --pull=always -d --name wx2rss -p 127.0.0.1:8000:8000 --restart unless-stopped -v "$PWD/data:/app/data" registry.cn-hangzhou.aliyuncs.com/whm412/wx2rss:latest
```

国际网络可使用 Docker Hub：

```bash
docker run --pull=always -d --name wx2rss -p 127.0.0.1:8000:8000 --restart unless-stopped -v "$PWD/data:/app/data" whm412/wx2rss:latest
```

启动后访问 `http://localhost:8000`。首次使用前请先完成微信与微信读书关联授权，再在 wx2rss 中扫码。

## 从旧版本升级

升级前先备份原来的 `data` 目录。只要新版继续挂载同一个 `data` 目录，微信账号、订阅、文章、配置、机器码和授权都会保留。

以下示例假设原容器名为 `wx2rss`，数据目录为当前目录下的 `data`：

```bash
cd /你的/wx2rss目录
cp -a data "data-backup-$(date +%Y%m%d-%H%M%S)"
docker pull registry.cn-hangzhou.aliyuncs.com/whm412/wx2rss:latest
docker stop wx2rss
docker rename wx2rss wx2rss-backup
docker run -d --name wx2rss -p 127.0.0.1:8000:8000 --restart unless-stopped \
  -v "$PWD/data:/app/data" \
  registry.cn-hangzhou.aliyuncs.com/whm412/wx2rss:latest
```

打开 `http://localhost:8000`，确认账号、订阅、授权和版本正常后，再决定是否删除旧容器。原部署如果使用了不同的容器名、端口、数据路径、代理或其他环境变量，请沿用原配置。国际网络可把镜像地址替换为 `whm412/wx2rss:latest`。

更详细的部署和升级说明：<https://www.wxsueq.cn/#faq-upgrade>

## 文档

- 产品介绍与部署说明：<https://www.wxsueq.cn/>
- 免费公众号订阅：<https://www.wxsueq.cn/#free-feeds>
- Docker Hub：<https://hub.docker.com/r/whm412/wx2rss>
- 阿里云镜像：`registry.cn-hangzhou.aliyuncs.com/whm412/wx2rss`

## 微信小程序

扫描下方二维码，可在微信中查看 wx2rss 产品介绍、自托管部署方法、版本说明和常见问题，
也可以联系人工客服。免费公众号订阅列表统一在网站维护。

<p align="center">
  <img src="wx2rss-miniprogram.jpg" width="220" alt="wx2rss 微信小程序二维码">
</p>

## 反馈问题

请使用仓库的 Issue 模板提交问题或建议。提交日志前务必删除：激活码、邮箱授权码、微信 Token、Cookie、Webhook、数据库文件和完整机器码。

本项目调用的第三方服务可能调整接口或触发安全验证。项目会尽量减少请求、错峰执行和安全退避，但不能承诺永不触发第三方风控。

重新扫码只会更新登录凭据，不代表第三方服务端的风控已经解除。扫码及恢复探测后会进入养护期，请不要连续点击抓取或轮换账号测试。

## 推荐免费公众号

欢迎通过“推荐免费公众号”Issue 表单提交候选公众号。请提供公众号名称、至少一篇可公开访问的文章链接、推荐理由以及大致更新频率；不要提交 Cookie、Token、登录截图或其他账号凭据。

推荐并不代表自动收录。维护者会人工核对公众号身份、数字 ID、内容合规性、更新情况和抓取稳定性，通过后再逐步加入免费订阅列表。已注销、冻结、长期停更、主要转载或存在明显版权与合规风险的公众号可能不予收录。

[提交公众号推荐](https://github.com/whm412/wx2rss/issues/new?template=recommend-public-account.yml)
