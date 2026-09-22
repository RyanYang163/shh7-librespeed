# Privacy Policy — LibreSpeed

> Review items covered / 适用审核项：C3 (policy provided) · C4 (completeness) · C5 (accessibility) ·
> C6 (data collection consistency) · C7 (user rights) · C8 (third-party disclosure)
>
> Public URL / 公网地址：<https://github.com/RyanYang163/shh7-librespeed/blob/main/PRIVACY.md>

**Effective date / 生效日期**：2026-09-22
**Applies to / 适用版本**：1.0.007
**Publisher / 开发者**：shh
**Package / 包名**：`shh7-librespeed` (Docker)

---

## 1. Data Collected / 收集哪些数据

LibreSpeed is a self-hosted HTML5 speed test server. It has **no accounts, no database and no
user profiles**.

本应用是自托管的 HTML5 测速服务，**没有账号体系、没有数据库、不建立用户档案**。

- The measurement itself runs **entirely in the visitor's browser**. Ping, jitter, download and
  upload figures are computed client-side and displayed; they are never written to disk or sent
  to the developer.
  测速**完全在访问者的浏览器内完成**，结果只在页面展示，不落盘、不回传开发者。
- The bundled web server writes ordinary **access logs** (source IP, timestamp, requested URL,
  user agent) to the container's own log directory. These stay on the device.
  内置 Web 服务器会写常规**访问日志**（来源 IP、时间、请求路径、User-Agent），只落在本机容器
  自己的日志目录里。
- **Telemetry is disabled.** The upstream telemetry/result-sharing endpoint is not enabled in this
  package, so no result is ever posted anywhere.
  **telemetry 已关闭**：本包未启用上游的结果上报端点，不会有任何测速结果被外发。
- Optional ISP identification is **off by default** (see section 3).

**The developer collects nothing**: no analytics, no telemetry, no crash reporting, no account
with the developer is required or possible.
**开发者不收集任何数据**：无埋点、无遥测、无崩溃上报。

## 2. Retention Period / 数据保存期限

- **No measurement data is retained at all** — nothing is stored per test.
  **不保存任何测速数据**，每次测速都不留记录。
- Access logs are the only runtime record. They are confined to the container log directory
  (`/Volume*/DockerAppData/shh7-librespeed/`) and are rotated by the container runtime (Docker
  `json-file` driver) with no developer-side retention window, because the developer operates no
  server.
  访问日志是唯一的运行期记录，只存在于本机日志目录，由 Docker 自身轮转；**开发者没有服务端**，
  不存在任何服务端保留期。
- Everything is removed when the app is uninstalled with "delete data" selected.
  卸载时勾选「同时删除数据」即全部清除。

## 3. Third-Party Sharing and Data Location / 第三方共享与数据存放地域

- **Default: no third party is involved and no data leaves the device.**
  **默认不涉及任何第三方，数据不出设备。**
- The optional ISP lookup feature is **off by default**. If an administrator turns it on, the
  server side consults a locally bundled ipinfo database (or the configured endpoint) to label an
  IP with its carrier. It is not enabled in this package.
  可选的 ISP 识别功能**默认关闭**；若管理员开启，服务端会查询本地随附的 ipinfo 数据库（或所
  配置的端点）来标注运营商。本包中该功能未启用。
- No request is ever made to the developer, and no result is aggregated, shared or published.
  不会向开发者发起任何请求，测速结果不做聚合、共享或发布。

| Feature / 功能 | Destination / 去处 | Default / 默认 |
|---|---|---|
| Speed measurement / 测速 | browser ⇄ this server only | enabled / 启用 |
| Telemetry (result sharing) / 结果上报 | none configured | **disabled / 关闭** |
| ISP identification / ISP 识别 | local ipinfo database | **disabled / 关闭** |

- **Data location / 数据存放地域**：the device the user installed the app on. The developer
  stores no copy anywhere, in any region.

## 4. Security Measures / 数据安全措施

This is a Docker application; the statements below describe what is actually configured in
`docker-compose.yml` and are verifiable there.

- Web server and PHP processes run as a **non-root UID (`PUID=1000` / `PGID=1000`)**; the
  container's init only drops privileges to that UID.
- `security_opt: no-new-privileges:true` — no process can gain privileges at runtime.
- **Capability allow-list**: `cap_drop: [ALL]` followed by an explicit `cap_add` list, instead of
  Docker's unrestricted default set. `NET_RAW`, `SETPCAP` and `SETFCAP` are **not** granted.
- No `privileged` mode and no `network_mode: host`; only the single Web UI port (`18807`) is
  published.
- No credentials or secrets are baked into the compose file; the speed test service requires none.
- The service is stateless: it holds no user data to leak, and nothing is relayed through a
  developer-operated server.

## 5. User Rights / 用户权利

There is no per-user account and therefore no per-user dataset. The administrator of the device
can, at any time and without contacting the developer:

- **Access / 查阅**：read the access logs under `/Volume*/DockerAppData/shh7-librespeed/`.
- **Erase / 清除**：delete those logs (see section 6) — after which nothing about past visits
  remains on the device.
- **Opt out / 拒绝采集**：no measurement data is collected in the first place; the optional ISP
  lookup and telemetry features stay off unless the administrator deliberately enables them.

## 6. Deletion Channel / 数据删除途径

1. **Logs / 日志**：delete the log files under `/Volume*/DockerAppData/shh7-librespeed/`, or let
   the container runtime rotate them out.
2. **Uninstall with data / 卸载时删除**：uninstall from the TOS App Center and select
   "delete data" — this removes `/Volume*/DockerAppData/shh7-librespeed/` entirely.
3. **Manual / 手动**：delete the `/Volume*/DockerAppData/shh7-librespeed/` directory on the device.

Uninstalling **without** selecting "delete data" keeps the configuration directory. There is no
developer-side copy to delete.

## 7. Contact / 联系方式

- Packaging repository / 本封装仓库：<https://github.com/RyanYang163/shh7-librespeed/issues>
- Upstream project / 上游项目：<https://github.com/librespeed/speedtest/issues>

Questions about this policy can be raised in either tracker.
