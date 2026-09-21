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
2. Default credentials: see upstream documentation
3. Key settings: see upstream documentation

## Permissions

| Permission | Rationale |
|---|---|
| Network: port 18807 | Web UI access |
| File system: `/Volume*/DockerAppData/shh7-librespeed/` | Application data persistence |
| User: shh7librespeed | Isolated non-root service execution |

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
- **Privacy Policy**: see [`PRIVACY.md`](./PRIVACY.md)
- **Vulnerability scan**: `trivy-report.txt` attached to each Release (HIGH/CRITICAL must be 0)
- Runs as a non-root dedicated user; no privileged mode, no host network

## Changelog

### v1.0.1 (2026-09-20)
- Compliance update: added LICENSE / NOTICE / PRIVACY materials,
  declared upstream license inside the package, added container healthcheck

### v1.0.0
- Initial release

## License

**LGPL-3.0** — this packaging repository is distributed under the same license as the
upstream project. Full text: [`LICENSE`](./LICENSE).
