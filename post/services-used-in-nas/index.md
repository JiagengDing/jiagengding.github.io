
我在 NAS 底层安装了 PVE 系统用于虚拟化，主要使用的虚拟机是 OpenWRT 和 Debian。
OpenWRT 用于拨号和路由，分配了两个 CPUs 和 1G 内存，并且直通了一个网口用于 WAN 口链接光猫。
Debian 系统上负责运行我日常使用的大多数服务，包括提供 DNS，RSS 和影视服务等。


<!--more-->
{{< toc >}}

## 我的 NAS 概述

- CPU：4 x Intel(R) N100 
- RAM：8 GB
- Kernel：Linux 6.2.16-3-pve 
- Storage: 一条m.2固态 + 两个硬盘
- 两个前兆网口


## 使用的服务列表

| 类别 | 服务名称 | 功能描述 |
|------|----------|----------|
| 网络 | OpenWrt | 路由器操作系统 |
| | Proxmox VE | 虚拟化平台 |
| | AdGuard Home | 网络广告拦截和DNS管理 |
| | MosDns | 作为 ADG 上游提供 DNS 服务 |
| | DDNS-go | 动态DNS服务 |
| | Neko | 远程桌面服务 |
| RSS & 写作 | Miniflux | RSS阅读器 |
| | Wewe-rss | RSS订阅管理 |
| | Memos | 笔记工具 |
| | Dillinger | Markdown编辑器 |
| 文件 & 影音 | Aria2 | 下载工具 |
| | File Browser | 文件管理器 |
| | Plex | 媒体服务器 |
| | Jellyfin | 开源媒体系统 |
| | Alist | 文件列表工具 |
| 杂项 | Portainer | Docker容器管理 |
| | Watchtower | 自动更新Docker容器 |
| | Vault Warden | 密码管理器 |
| | Ntfy | 通知推送服务 |

表格中展示了我安装的大部分应用，所有应用都能在 Github 上找到。
其中 Homepage 是我最常打开的页面，其中包含了表中所有应用的链接，如下图：
![Homepage 页面展示](https://r2gallery.diing.uk/%E5%9B%BE%E5%BA%8A%2Fhomepage.png)

## 提供 DNS 服务
出于避免 DNS 污染以及避免 ISP 监视的目的，我在 Debian 虚拟机上最重要的服务就是获取最快 DNS 并为所有设备提供加密 DNS 服务。
同时，这项服务还为我提供了 DNS 缓存和分流的功能。

### Adgouard Home
我的终端设备**首先**会通过 DNS-over-TLS 以及 DNS-over-HTTPS 访问 Adgouard Home（后面缩写为ADG）来获取 DNS。
在 ADG 中设置上游 DNS 为 Mosdns 以及一个远程 DNS 服务（例如 VPS 上的 Mosdns）。由于距离差异 ADG 会首先获取到本地 DNS，只有在本地异常时才会使用远程应答。

### MosDNS
在 Mosdns 中要实现下述功能：
1. 缓存 DNS 记录并延长 TTL
2. 通过黑名单过滤广告
3. 通过测速工具修改 Cloudflare 等服务的响应 IP 为最佳速度
4. 通过阿里和腾讯的加密 DNS 同步查询**国内网址**
5. 通过 Cloudflare 和 DNS.SB 加密 DNS 同步查询**非国内地址**并设置 ECS
6. 将第五步中的 IP 验证以确保不会污染

### 其他
为了辅助上面功能，我们需要运行一个 DDNS 服务实时更新 NAS 的公网 IP，一个用于获取 TLS 证书的 acme.sh 服务。
同时，需要在 OpenWRT 中将 ADG 提供加密 DNS 服务的端口映射到公网。

## 阅读与写作

### RSS
RSS 是我日常获取信息的渠道，我在移动设备上使用 Read You 和 NetNewsWire 阅读，并在 Memos 上记录。

- 为了实现不同设备的同步，我在 docker 中搭建了 Miniflux 并通过 Fever 和 Google reader 集成到 APP。
- 对于公众号文章，我使用了 wewe-rss 进行获取（需要在 miniflux 中设置自动获取全文）。
- 对于没有设置 RSS 的网站（例如 B 站），可以通过 RSShub 获取。

### 写作
- Memos 提供了类 flomo 体验，但是可以由自己托管。我用来记录一些片段，并使用标签管理。

- Dillinger 是在 Typora 收费后我最喜欢的 markdown 开源书写工具。

## 文件管理
我的 NAS 系统中文件管理主要依赖于以下几个服务:

- Aria2: 这是一个强大的下载工具,支持多协议下载。我主要用它来处理大文件下载,以及一些需要长时间运行的任务。同时运行了一个 Aria2 可视化页面。
- File Browser: 这是一个轻量级的文件管理器,提供了直观的 Web 界面。
- Alist: 功能与 File Browser 有些重叠,但 Alist 的特点在于能够整合多个存储源。我用它来统一管理本地存储和一些云存储服务, 包括阿里云盘, cloudfare R2 和 AWS s3。

## 其他服务

- Portainer: 最著名的 Docker 容器 Web 管理界面。
- Watchtower: 这个服务自动检查和更新 Docker 容器。它让我的系统始终保持最新状态, 并在 Homepage 上实时显示更新信息。
- Vault Warden: 这是 Bitwarden 的 rust 实现,用作我密码管理器的服务端。自托管密码管理器让我对敏感信息有更多的控制,同时也避免了对第三方服务的依赖。
- Ntfy: 我用它来发送各种系统通知, 包括备份完成、存储空间警告、ssh登陆提醒等。它支持多种接收方式,包括移动应用和电子邮件,确保我能及时收到重要信息。


## 参考资料和推荐阅读
- [awesome-sysadmin](https://github.com/awesome-foss/awesome-sysadmin)
- [自托管精选](https://zituoguan.com/)
