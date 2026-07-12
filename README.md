# IPTV-EPG

独立的 **EPG 节目单 / 台标 / 频道名** 生成项目（从 IPTV-Manage 拆出，单独成库）。

本仓库用 GitHub Actions 自动运行三个工作流，生成的产物提交回本仓库自行托管：

| 工作流 | 文件 | 作用 | 运行时间（北京时间） |
|---|---|---|---|
| EPG 聚合 | `.github/workflows/epg.yml` | 下载合并多个 EPG 源，生成 `e.xml` / `e.xml.gz` | 每天 00:30 / 06:30 / 12:30 / 18:30 |
| 台标同步 | `.github/workflows/logo.yml` | 从多个公开仓库同步台标 PNG 到 `logo/` | 每周五 08:00 |
| 频道名提取 | `.github/workflows/name.yml` | 提取频道显示名，生成 `display_names.txt` | 仅手动触发 |

## 一、启用方式（让 Actions 真正跑起来）

> 工作流文件放在本地不会自动运行，必须推送到 GitHub 仓库后才会按计划执行。

1. 在本文件夹初始化 git 并推送到你的 GitHub 新仓库（默认分支，例如 `main`）。
2. 在仓库的 **Settings → Actions → General** 里，确认 Workflow 权限为「Read and write permissions」（默认通常已够用）。
3. 之后工作流会按上表时间自动运行；也可在仓库 **Actions** 页面点对应工作流 → `Run workflow` 手动触发。
4. 运行结果（`e.xml`、`e.xml.gz`、`logo/`、`display_names.txt`）会由机器人账号自动提交回仓库。

## 二、自定义

- **改 EPG 源 / 台标仓库 / 频道名 URL**：直接编辑对应 workflow 文件里的数组（`EPG_SOURCES` / `REPOS` / `URLS`）。
- **改运行时间**：编辑 workflow 里的 `cron` 表达式（注意是 **UTC 时间**，北京时间 = UTC+8）。
- `epg.yml` 生成的 `e.xml` 头部 `generator-info-url` 可按需改为本仓库地址。

## 三、说明

- 本仓库**只**负责 EPG / 台标 / 频道名三类数据的生成与托管；IPTV 直播源聚合（`main.py`）仍在原 IPTV-Manage 仓库，互不影响。
- 运行依赖：GitHub Actions 的 Ubuntu 环境会经由 `apt` 安装 `opencc`、`gzip`，无需额外配置。
- 许可证：MIT。
