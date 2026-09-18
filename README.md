# LibreSpeed

| 项 | 值 |
|---|---|
| 应用 ID | `shh7-librespeed` |
| 形态 | Deb 应用（单包模式） · WebUI 内嵌（iframe） |
| 版本 | 1.0.0 |
| 上游项目 | https://github.com/librespeed/speedtest |
| 上游许可证 | LGPL-3.0 |
| 宿主端口 | 18807 |

## 简介

自建网络测速服务端：浏览器打开即可测 NAS 与各设备之间的内网带宽。

## 打包

```bash
./build.sh                # 默认 x86_64
./build.sh aarch64        # ARM（Deb 应用）
```

产物在 `build/output/`，同级生成 `<包名>.sha256`。

## 提交前必办事项

- 上游提供 PHP / Node / 纯静态多种后端，本封装采用**纯静态**模式，因此不依赖 PHP（PHP 在 TOS 7 未预装）。
- 构建产物放入 webui/ 后由 build.sh 打包为 webui.bz2。
- ⚠️ 上游 Dockerfile 默认 TELEMETRY=false，封装保持该默认值。
- [ ] 真机安装、启动、停止、卸载残留四项实测
- [ ] 首屏加载 ≤ 5 秒（指引 H10）
- [ ] x86_64 与 aarch64 分别构建并测试（指引 H7）
- [ ] 提交前跑一遍指引 13.9 上架前自查清单

## 隐私政策

见 [PRIVACY.md](./PRIVACY.md)（对应审核项 C3–C8）。

## 许可证与出处

本仓库**仅包含 TOS 平台集成所需的配置文件与打包脚本**，应用本体的源码与二进制来自上游项目：https://github.com/librespeed/speedtest

上游许可证：**%s**。本封装保留上游许可证声明，未修改上游代码（Deb 形态下按上游许可证要求随包提供 LICENSE）。

应用名称与图标为上游项目的标识；本仓库图标为自行绘制的简易图形，不含上游商标元素（对应审核项 H19）。
