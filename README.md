# awa · VPS 一键脚本工具箱

> 面向 Debian / Ubuntu / CentOS 的 VPS 运维脚本集合：环境部署、LNMP 建站、Docker 管理、备份迁移、系统工具。
>
> 当前版本 **3.1.5** ｜ 最近整理 2026-11-17

## 一条命令跑起来

```bash
bash <(curl -sS https://raw.githubusercontent.com/awahello/awa/main/sh/awash.sh)
```

想先下载再执行：

```bash
curl -sS -O https://raw.githubusercontent.com/awahello/awa/main/sh/awash.sh
chmod +x awash.sh && ./awash.sh
```

> 国内访问 `raw.githubusercontent.com` 不稳时，把域名换成 `ghproxy` 类加速地址，或者直接
> `wget` 到本地再传上去。脚本本身不依赖特定 CDN。

**支持系统**：Debian 11/12 · Ubuntu 20.04/22.04/24.04 · CentOS 7/8 · AlmaLinux / RockyLinux

## 里面有什么

```
sh/
  awash.sh                主入口：交互式菜单，环境部署 / 建站 / Docker / 系统工具
  cn/awash.sh             国内镜像版（下载源换成国内加速）
  ldnmp.sh                LNMP 一键部署（Nginx + MySQL + PHP + Redis）
  bt/                     面板相关脚本
  docker/                 Docker 环境与常用容器
  nginx/  config/         配置模板
  Tools/
    Linux_reinstall/      重装系统（Debian/Ubuntu/Alpine/Arch/RedHat/Windows 全系）
    Force_Modified_BBR/   强制开启 BBR
  beifen.sh  quanbubeifen.sh  s3beifen.sh    备份 / 迁移
  auto_cert_renewal*.sh   证书自动续期
  palworld.sh  discourse.sh  mailcaddy.sh    具体应用部署
  ...                     其余为单项工具脚本
```

## 2026-11-17 这次改了什么

### 修了一个会让新手直接卡住的错

`sh/README.md` 里第一条安装命令指向 `https://akkdiy.cn/awash.sh` —— 这个地址现在返回的是
**158 字节的 HTML 页面**（不是脚本），`chmod +x` 之后执行会直接报错。已改成 raw 地址。

顺带修了同一份文档里的 `wget beifen.sh https://...` —— `wget` 没有这种写法，
会先尝试下载一个叫 `beifen.sh` 的主机名然后报错，正确写法是 `wget -O beifen.sh <url>`。

### 修了 8 个脚本的执行入口

| 问题 | 影响 | 处理 |
|---|---|---|
| `luntanbeifen.sh` / `luntanbeifen1.sh` 首行是空行，`#!/bin/bash` 在第 2 行 | shebang 不生效 | 删掉首行空行 |
| `debianInit.sh`、`auto_cert_renewal-1.sh`、`auto_cert_renewal-yuanshi.sh`、`pal_log.sh`、`update_log.sh` 没有 shebang | `./xxx.sh` 直接执行会走错解释器 | 补 `#!/bin/bash` |
| `auto_cert_renewal-不要80端口docker配置.sh` 内容是 docker-compose 片段，不是脚本 | 名字骗人，`bash` 执行会报错 | 改名成 `.txt` |

### 修了一批 404 下载地址（这些是静默失败，最难查）

| 位置 | 问题 | 影响 |
|---|---|---|
| `sh/awash.sh`、`sh/cn/awash.sh`、`sh/awashwanzhengbeifen.sh` | `raw.githubusercontent.com/…/**blob**/main/…` | raw 域名下 `blob` 路径返回 **404**，20 处下载全失败 |
| `sh/cn/awash.sh` | 下载 Docker 安装脚本写成 `sh/dockeinstall` | 文件不存在（真实路径 `sh/docker/install`），国内版装 Docker 必失败 |
| `sh/cn/awash.sh` | `gh_proxy="https://www.akkdiy.cn/"` | 那个站不做 GitHub 反代，拼出来是 `https://www.akkdiy.cn/https://raw.githubusercontent.com/…`，**连接直接超时**，国内版所有下载都受影响 |
| `sh/cn/awash.sh` | 反代配置地址写成 `awahello/awahello/awa/blob/main/…` | 仓库名重复 + 多余 `blob`，双重错误 |
| `sh/palworld.sh` | 引导命令指向已失效的 `akkdiy.cn/awash.sh` | 用户照抄会拿到 158 字节的 HTML |

最坑的是 `blob` 这一类：`wget` 会把 **14 字节的 `404: Not Found`** 写进 `nginx.conf`，
不报错，站点直接起不来。

### 修了信息面板的两处未闭合引号

`sh/awash.sh` 第 13348–13349 行：

```bash
echo -e "${gl_kjlan}官网:  ${gl_bai}https://www.akkdiy.cn/             
echo -e "${gl_kjlan}博客:  ${gl_bai}https://www.akkdiy.cn/        
```

两行都少了收尾的 `"`，bash 会把后面的行一起吞进这个字符串，
导致「awa周边」那段输出错位（分隔线消失、文字粘连）。已补全。

### 加了 `.gitattributes`（防未来踩坑）

仓库里原来没有这条。在 Windows 上编辑一次再提交，**所有 `.sh` 会变成 CRLF 行尾**，
到 Linux 上执行就报：

```
bad interpreter: /usr/bin/env bash^M
```

这种错误极难看出原因。现在钉死了 `*.sh text eol=lf`，`.bat` 保持 CRLF。

## 使用须知

- 脚本大多需要 **root** 权限，且会改系统配置（防火墙、内核参数、软件源），**在测试机上先跑一遍**
- 重装系统类脚本会**清空磁盘**，用之前确认没有重要数据
- 备份脚本默认落在 `/home`，注意磁盘空间
- 部分功能依赖外网（Docker Hub、GitHub、各软件源），国内环境可能需要换源

## 致谢

本仓库整合了多位作者的脚本，来源与改动归属见 [NOTICE.md](./NOTICE.md)。
重装系统部分基于 [bin456789/reinstall](https://github.com/bin456789/reinstall) 与
[leitbogioro/Tools](https://github.com/leitbogioro/Tools)。
