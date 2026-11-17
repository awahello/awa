# awa · 一键脚本工具

面向 VPS 的交互式脚本工具箱：环境部署、LNMP 建站、Docker 管理、备份迁移、系统工具。

- 版本：**3.1.5**
- 支持：Debian 11/12 · Ubuntu 20.04/22.04/24.04 · CentOS 7/8 · AlmaLinux / RockyLinux
- 需要 root 权限

## 安装依赖

Debian / Ubuntu：

```bash
apt update -y && apt install -y curl
```

CentOS / AlmaLinux / Rocky：

```bash
yum update -y && yum install -y curl
```

## 运行

推荐用这种方式（不落盘，直接执行）：

```bash
bash <(curl -sS https://raw.githubusercontent.com/awahello/awa/main/sh/awash.sh)
```

或者下载到本地再跑：

```bash
curl -sS -O https://raw.githubusercontent.com/awahello/awa/main/sh/awash.sh
chmod +x awash.sh && ./awash.sh
```

> 国内网络访问 `raw.githubusercontent.com` 经常超时。脚本本身不挑 CDN，
> 把域名换成任意 GitHub 加速地址即可，或者本地 `wget` 后 `scp` 上去。

国内镜像版（下载源换成国内加速）：

```bash
bash <(curl -sS https://raw.githubusercontent.com/awahello/awa/main/sh/cn/awash.sh)
```

## 功能菜单

运行后会看到交互式菜单，主要分这几块：

| 分类 | 内容 |
|---|---|
| 系统信息 | CPU / 内存 / 磁盘 / 流量 / 系统版本检测 |
| 环境部署 | Docker、Nginx、MySQL、PHP、Redis、Node、Python |
| 建站 | LNMP 一键部署、站点管理、SSL 证书（含自动续期）、反向代理 |
| Docker | 容器管理、镜像管理、常用应用一键起 |
| 备份迁移 | 网站打包、数据库导出、S3 / 异地备份、整机迁移 |
| 系统工具 | 换源、BBR、防火墙、SSH 加固、时区、重装系统 |
| 应用部署 | Palworld、Discourse、Mailcaddy、MinIO、Bitcoin 节点等 |

## 备份与迁移速查

按时间戳打包站点：

```bash
cd /home/ && tar czvf web_$(date +"%Y%m%d%H%M%S").tar.gz web
```

把最新的包传到另一台机器：

```bash
cd /home/ && ls -t /home/*.tar.gz | head -1 | xargs -I {} scp {} root@目标IP:/home/
```

远端解压最新的包：

```bash
cd /home/ && ls -t /home/*.tar.gz | head -1 | xargs -I {} tar -xzf {}
```

只保留最近 3 个包：

```bash
cd /home/ && ls -t /home/*.tar.gz | tail -n +4 | xargs -I {} rm {}
```

自动备份到另一台 VPS，先装依赖：

```bash
apt update -y && apt install -y wget sudo sshpass
```

下载备份脚本并改配置（注意 `wget` 要用 `-O` 指定文件名）：

```bash
cd /home
wget -O beifen.sh https://raw.githubusercontent.com/awahello/awa/main/sh/beifen.sh
chmod +x beifen.sh
nano beifen.sh
```

## 注意

- 脚本会修改系统配置（防火墙、内核参数、软件源），**先在测试机跑一遍**
- `Tools/Linux_reinstall/` 下的重装脚本会**清空磁盘**，执行前确认没有重要数据
- 备份默认落在 `/home`，注意剩余空间
- 部分功能依赖外网（Docker Hub、GitHub、各软件源），国内可能需要换源

## 目录

```
awash.sh                主入口
cn/awash.sh             国内镜像版
ldnmp.sh                LNMP 一键部署
bt/                     面板相关
docker/                 Docker 环境与常用容器
nginx/ config/          配置模板
Tools/
  Linux_reinstall/      重装系统（全系发行版）
  Force_Modified_BBR/   强制开启 BBR
beifen.sh               备份
quanbubeifen.sh         整站备份
s3beifen.sh             S3 备份
auto_cert_renewal*.sh   证书自动续期
palworld.sh / discourse.sh / mailcaddy.sh   具体应用部署
```

## 致谢

整合了多位作者的脚本，来源见仓库根目录的 [NOTICE.md](../NOTICE.md)。
