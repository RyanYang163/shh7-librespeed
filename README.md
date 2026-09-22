# LibreSpeed

> TOS 7 application package for **LibreSpeed** — platform integration only.
> The application itself is provided by the upstream project, unmodified.

## Overview

Self-hosted HTML5 speed test server for measuring LAN and WAN throughput.

上游项目 / Upstream: <https://github.com/librespeed/speedtest>
上游许可证 / License: **LGPL-3.0**

## Features

- HTML5 browser-based speed test (no client software)
- Measures ping, jitter, download and upload
- Runs entirely client-side; telemetry disabled
- No database or user accounts

## Installation

1. Requirements: TOS 7.0+ and Docker Engine (install from the TOS App Center)
2. Install from the TOS App Center
3. Open the app and complete initial configuration

## Usage

1. Access URL: `http://${ip}:18807`
2. No login and no account: the speed test page is public by design
3. Key settings: see upstream documentation

## Permissions

| Permission | Rationale |
|---|---|
| Network: port 18807 | Web UI access |
| File system: `/Volume*/DockerAppData/shh7-librespeed/` | Application data persistence |
| User: container UID 1000 (`PUID`/`PGID` = 1000) | Isolated non-root execution of the web/PHP worker processes |

## Configuration

See `config.ini` for platform metadata; see `docker-compose.yml` for runtime configuration.

## Ports

| Port | Protocol | Purpose |
|---|---|---|
| 18807 | TCP | Web UI (LibreSpeed) |

## Support

- Documentation: https://github.com/librespeed/speedtest
- Issue tracker: https://github.com/librespeed/speedtest/issues
- Community: https://github.com/librespeed/speedtest

## Security & Compliance

- **License**: LGPL-3.0 — full text in [`LICENSE`](./LICENSE)
- **Attribution**: see [`NOTICE`](./NOTICE)
- **Privacy Policy**: public URL <https://github.com/RyanYang163/shh7-librespeed/blob/main/PRIVACY.md>
  (also linked from `config.ini`'s `help` field, so it is reachable from the App Center and
  findable inside the package); repo copy: [`PRIVACY.md`](./PRIVACY.md)
- **Vulnerability scan**: `trivy-report.txt` attached to each Release (fixable HIGH/CRITICAL must be 0)
- No `privileged` mode, no host network, no credential in the compose file

## Privacy Policy (C3 / C4 / C5)

**公网地址（可直接访问）**：<https://github.com/RyanYang163/shh7-librespeed/blob/main/PRIVACY.md>

- `config.ini` 的 `help` 字段就指向该地址 —— 平台应用详情页的「帮助」即可直达，因此**包内可查**。
- 已覆盖 C4 要求的全部要素：数据收集范围、**保存期限**、第三方共享与**数据存放地域**、
  安全措施、**用户权利**、**删除途径**、联系方式。

## 运行时写入路径清单（指引 12.9.6）

| 路径 | 由谁创建 | 内容 | 保留策略 |
|---|---|---|---|
| `/Volume*/DockerAppData/shh7-librespeed/data` | 应用（挂 `/config`） | 站点配置、nginx / php 日志 | 随数据保留，不自动过期 |
| `/Volume*/DockerAppData/shh7-librespeed/www` | 应用（挂 `/config/www`） | 可选的页面定制覆盖文件 | 同上 |
| 容器内 `/run` | 镜像自带 s6 / nginx | pid 与运行期状态 | 随容器生命周期 |

应用**不写入本清单以外的路径**。测速结果只在浏览器内计算，**不落盘**。

## 权限与最小化（对应 V1 豁免申请）

本应用**不写 `user:` 字段**：`linuxserver/librespeed` 基于 s6-overlay，init 需要 root 完成
`chown` / 降权（LinuxServer.io 官方明确不支持 `user:`），强行指定会让容器起不来。等价的最小权限措施：

- `PUID=1000` / `PGID=1000` —— **nginx 与 php-fpm 的 worker 进程实际以 uid 1000 运行**；
  root 只用于容器初始化与绑定 80 端口；
- `security_opt: no-new-privileges:true`；
- `cap_drop: [ALL]` + `cap_add` 白名单（**不授予** `NET_RAW` / `SETPCAP` / `SETFCAP`）；
- 无 `privileged`、无 `network_mode: host`，只发布一个 Web UI 端口 18807。

豁免申请材料见 `../../../开发应用计划/TOS社区应用V1豁免申请邮件-shh7-librespeed.md`。

## Changelog

### v1.0.007 (2026-09-22)
- Fixed per official review: privacy policy rewritten to cover every C4 element
  (retention period, user rights, security measures, third-party sharing, deletion channel)
  and made reachable via a public URL that is also declared in `config.ini`
- Least-privilege hardening: `cap_drop: [ALL]` plus an explicit `cap_add` allow-list
  (V1 root exemption applied for separately — the upstream image's s6 init cannot run without root)
- CI: Trivy now scans **every** image in the compose file, not just the first

### v1.0.1 (2026-09-20)
- Compliance update: added LICENSE / NOTICE / PRIVACY materials,
  declared upstream license inside the package, added container healthcheck

### v1.0.0
- Initial release

## License

**LGPL-3.0** — this packaging repository is distributed under the same license as the
upstream project. Full text: [`LICENSE`](./LICENSE).
