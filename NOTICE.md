# 第三方来源与改动归属

本仓库是**自用脚本集合**，整合了多位作者的作品。这里逐项列清来源。

> **关于署名**：脚本内部的作者署名保留未改。把别人的开源作品改成自己的名字，
> 既不合规，社区也一眼认得出来。本仓库主张的是**整合、修复、文档**这部分工作。

## 主要来源

| 部分 | 来源 | 本仓库改动 |
|---|---|---|
| `sh/Tools/Linux_reinstall/` | [bin456789/reinstall](https://github.com/bin456789/reinstall)、[leitbogioro/Tools](https://github.com/leitbogioro/Tools) | 未改代码；补了 `debianInit.sh` 的 shebang |
| `sh/bt/` | 宝塔面板相关脚本 | 未改 |
| `sh/awash.sh` 主框架 | 在社区通用 VPS 工具箱结构上二次开发 | 修 shebang / 文档 / 入口命令 |
| `sh/Tools/Force_Modified_BBR/` | 社区通用 BBR 脚本 | 未改 |
| `sh/PandoraNext/` | PandoraNext 配置模板 | 未改 |

## 本次（2026-11-17）的改动归属

以下改动由本仓库维护者完成：

1. 根 `README.md` 重写（原来只有一个 `# awa`，5 字节）
2. `sh/README.md` 重写：清掉 79 行纯 `-` 占位、修坏掉的安装命令、补功能与目录说明
3. 修 `wget beifen.sh <url>` 的错误写法 → `wget -O beifen.sh <url>`
4. 补 5 个脚本的 shebang，删 2 个脚本首行的空行（原 shebang 因此失效）
5. `auto_cert_renewal-不要80端口docker配置.sh` 改名 `.txt`（内容是 compose 片段，不是脚本）
6. 修 20 处 `raw.githubusercontent.com/…/blob/main/…` 的 404 路径
7. 修 `sh/dockeinstall` → `sh/docker/install`（文件不存在）
8. 修 `gh_proxy` 指向不存在的 GitHub 反代（拼接后连接超时）→ 置空并加注释
9. 修反代配置下载地址（仓库名重复 `awahello/awahello/awa` + 多余 `blob`）
10. 修 `sh/awash.sh` 两处未闭合的引号（导致信息面板输出错位）
11. 修 `sh/palworld.sh` 里指向已失效的 `akkdiy.cn/awash.sh` 的引导命令
12. 新增 `.gitattributes`，钉死 `*.sh text eol=lf`
13. 新增本文件

## 许可

- 各部分脚本的著作权归原作者
- 本仓库自己的文档与上述修复可自由取用
- 二次分发请前往原仓库确认其许可条款
