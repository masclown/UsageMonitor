# UsageMonitor - AI Usage Monitor

**[简体中文](README.md) | English**

[![Platform](https://img.shields.io/badge/platform-Windows%2010%2F11-blue)]()
[![License: EULA](https://img.shields.io/badge/license-EULA-orange)](EULA.md)
[![SDK: Apache-2.0](https://img.shields.io/badge/SDK%20%26%20Declarations-Apache--2.0-green)](LICENSE-APACHE)

A lightweight Windows taskbar tool that consolidates usage, quota, and balance from your AI providers (MiniMax, DeepSeek, Kimi, Qoder, etc.) directly into the taskbar and tray popup, glanceable at any time.

---

## Download & Install

Get the latest portable package from the Releases page:

**👉 [https://github.com/masclown/UsageMonitor/releases](https://github.com/masclown/UsageMonitor/releases)**

- **Portable, no installation required**: download the zip, extract it anywhere, and double-click `UsageMonitor.exe` (or the Launcher) to run
- **No administrator privileges**, no registry writes, no system services
- **Uninstall = delete the folder**

### System Requirements

| Item | Requirement |
|------|-------------|
| OS | Windows 10 / Windows 11 |
| Runtime | None required (portable package bundles the .NET runtime) |
| Privilege | Standard user is sufficient |

### Integrity Verification

Each Release includes a `SHA256SUMS` file. Verify the downloaded archive:

```powershell
Get-FileHash .\UsageMonitor-portable.zip -Algorithm SHA256
```

See [SECURITY.md](SECURITY.md) section 6 for details.

## Portable Mode

UsageMonitor is portable by design — all data lives with the program folder:

- **`portable.mode` marker file**: when present in the program folder, portable mode is enabled — configuration, credentials, and history are stored under the `data/` subfolder instead of `%AppData%`
- **Whole-folder migration**: move the entire program folder (including `data/`) to another PC / directory. Note: encrypted credentials are bound to your Windows account (DPAPI) — re-login once after cross-machine migration
- **Configuration locations**:
  - Portable mode: `<program>\data\config.json`
  - Non-portable (no `portable.mode`): `%AppData%\UsageMonitor\config.json`

## License & Activation

UsageMonitor uses a "Free + Pro" licensing model:

- **Free**: ready to use after install, with basic features (up to 2 providers, 1 account per provider, 30-day history, etc.). See [FEATURES.md](FEATURES.md)
- **Pro**: unlock everything with an activation code (unlimited providers/accounts, all mini-chart types, unlimited history, custom charts, etc.)
- **Activation**: Settings → License page, enter the code (format `UM-XXXX-XXXX-XXXX`), one-time online redemption; the local credential works offline afterwards (with periodic online validation of license status)
- **Switching PC / Reinstall**: keep the program folder (including `data/`) before reinstalling the OS to carry the license; when switching computers, use "Deactivate" on the License page first and send the obtained token to the developer for a renewal code

## Features

### Taskbar Embedding

![Taskbar embedding](docs/images/taskbar-embed.jpg)

- Usage rings / mini charts / text embedded directly into the Windows taskbar, **zero extra screen space**
- Configurable mini chart types, display count, and scroll switching

[Taskbar scroll switching demo](http://xhslink.cn/o/880RSEgQ3lf)

### System Tray + Popup

![Tray popup](docs/images/tray-tooltip.png)

- Hover the tray icon for an instant usage overview
- Right-click menu for quick refresh, settings, and display mode switching

### Card-based Main Window

![Main window: usage cards and charts](docs/images/main-window.png)

- One card per account, including **progress bars, numbers, balance snapshots, and charts**
- Supports charts (line/curve/heatmap/stacked bar, etc.) and custom Tooltip fields
- Supports custom card ordering and granularity slicing (hour/day/week/month)

[Custom ordering demo for cards, charts, and data](http://xhslink.cn/o/8PqgJZ512qJ)

### Multi-provider + Multi-account

![Account management](docs/images/accounts.png)

- Manage multiple accounts from **multiple providers** in one app
- Each account has independent login state, refresh policy, and history

### Scheduled Auto-refresh

- 5-hour countdown auto-refresh
- Custom refresh intervals
- Manual refresh anytime with one click

### Usage History

- **Local persistence** with daily/weekly/monthly aggregation
- History window supports slicing, sorting, TopN, cumulative sum and other aggregation operators

### Rich Chart Types

![Number overview](docs/images/chart-overview.png)

![Line chart](docs/images/chart-line.png)

![Curve chart](docs/images/chart-curve.png)

![Heatmap](docs/images/chart-heatmap.png)

![Stacked bar chart](docs/images/chart-stacked-bar.png)

![Stacked progress bar](docs/images/chart-stacked-progress.png)

- Progress bars, line charts, curve charts, heatmaps, stacked bars, stacked progress
- Taskbar mini charts support multiple styles (ring, text, mini line)

### Dual Themes

![Dark theme](docs/images/dark-theme.png)

- Built-in light / dark dual themes
- Supports free combination of third-party theme packs, chart style packs, and mini-chart style packs

### Declarative Plugins

- One provider = one **JSON declaration package** (zero DLL, zero executable code)
- Third-party packages can be validated before install, zero trust risk
- Supports browser QR login / Cookie login / API Key login

### Security & Compliance

- Login states and API Keys are encrypted with **Windows DPAPI** and stored locally
- Outbound requests limited to provider domains declared by plugins (whitelist + SSRF protection)
- Update packages use **Ed25519 signature + SHA256 verification** against tampering
- All data isolated in program folder (portable mode) or `%AppData%`, **no data upload**

## Design Philosophy

UsageMonitor is designed around five core principles:

### Simple

- **Portable, zero-install**: extract and run; no installation, no admin rights, no registry writes; uninstall = delete the folder
- **Simple folder layout**: `core/` (program), `packs/` (plugin & style declaration packages), `data/` (your data), `tools/` (utilities) — each with a single responsibility
- **Whole-folder migration**: all data follows the program folder; copy the entire folder to another PC

![Portable folder structure](docs/images/portable-structure.png)

### Secure

- **Account-based encryption**: sensitive data (cookies / API keys) is encrypted with Windows DPAPI (CurrentUser scope) before hitting disk, with a self-describing prefix and HMAC-SHA256 integrity signature — only your Windows account can decrypt it
- **No plaintext leakage**: sensitive values are never written to logs or plaintext config; credentials are isolated per account (SID)
- **File access control**: supports NTFS ACL tightening — Cookie and config directories can be restricted to the current Windows user only
- **Verifiable network boundary**: outbound traffic is limited to provider domains declared by plugins (whitelist + SSRF protection), no telemetry, no data upload; update manifests are Ed25519-signed with SHA256 verification against tampering

![Security settings](docs/images/security.png)

### Free

- **Pure declarative plugins**: one provider = one JSON declaration package (zero DLL, zero executable code); fetching (`fetch`), card charts (`card`), and taskbar mini charts (`taskbar`) are all defined declaratively
- **Customizable appearance**: theme packs, chart style packs, and mini-chart style packs can be freely combined
- **Plug & play**: third-party declaration packages can be validated, installed, and removed without polluting the host

![Declarative plugin JSON example](docs/images/plugin-json.png)

### Glanceable

- **Taskbar at a glance**: usage rings / mini charts / text embedded directly into the Windows taskbar, no extra screen space
- **Tray popup**: hover the tray icon for an instant usage overview; right-click for quick refresh / settings
- **Clear card charts**: progress bars, line charts, curve charts, heatmaps, and more aggregate usage trends at a glance

### AI-Assisted

- **AI-assisted plugin development**: a bundled set of AI agent skills (data-source research, plugin development, card charts, mini charts, themes, SDK audit) — for any AI provider: research its usage page → let AI generate the declaration package → validate & install. No hand-written code required for the whole adaptation

![AI skills](docs/images/ai-skills.png)

## Screenshots

![Settings window](docs/images/settings.png)

## Getting Started

1. After launch, a UsageMonitor icon appears in the system tray
2. Right-click the tray icon → "Settings", choose a provider and sign in (🌐 fetch login state) or enter an API key
3. Enable "Taskbar display" to embed usage info into the taskbar; data refreshes automatically at the configured interval
4. Hover the tray icon for the usage overview popup; the main window provides full card charts and history statistics
5. Supports custom card ordering, chart type customization, data group and Tooltip field customization

## Legal Disclaimer

- **Unofficial tool**: UsageMonitor is an independently developed third-party tool. It has no affiliation, endorsement, or partnership with MiniMax, DeepSeek, Kimi (Moonshot AI), Qoder, or any other AI provider
- **Data ownership**: this tool only helps you use your own account credentials to query your own usage data. It does not proxy, share, or resell any provider's service or data
- **Use at your own risk**: querying usage with this tool is normal access to your own account, but provider terms of service may change at any time; all consequences of use are your own responsibility
- **Trademarks**: third-party provider names, logos, and trademarks mentioned herein belong to their respective owners

## Security

- Credential encryption, network behavior boundaries, plugin security model, and vulnerability disclosure are documented in **[SECURITY.md](SECURITY.md)**
- **About antivirus false positives**: UsageMonitor is an unobfuscated, unpacked .NET program. Since no commercial code-signing certificate is purchased, some antivirus engines may heuristically flag newly released executables. If you encounter a false positive:
  - Verify file integrity against the `SHA256SUMS` file in the Release attachments; only download from the official Releases page
  - Submit the file to VirusTotal for cross-validation: https://www.virustotal.com/gui/file/ (query by the hash in SHA256SUMS)
  - Report the antivirus vendor and version in Issues
- All outbound requests are exhaustively enumerated in [SECURITY.md section 2](SECURITY.md#2-network-behavior-boundary-verifiable-outbound-whitelist); update checks and anonymous statistics have settings toggles, and collected fields are bound by the whitelist in [`docs/telemetry-schema.json`](docs/telemetry-schema.json) as the single contract (verifiable by packet capture)

## License

This project uses a layered license structure:

| Scope | License | Description |
|-------|---------|-------------|
| Main program (executable and components) | **Proprietary freeware EULA** | Free for personal / non-commercial use; redistribution and reverse engineering prohibited. See [EULA.md](EULA.md). |
| SDK contracts / declaration JSON / plugin templates | **Apache License 2.0** | The community is free to develop and distribute third-party declaration packages and theme packs without commercial restrictions. See [LICENSE-APACHE](LICENSE-APACHE). |
| Cloud / advanced features (planned) | Proprietary closed source | Multi-device sync, cloud backup, notifications, etc. — outside the scope of this repository. |

> Third-party AI provider brand logos, names, and trademarks belong to their respective owners. This tool does not bundle any third-party logos — icons are fetched at runtime by provider domain (favicon) and cached locally; this project claims no rights to them.

## Known Limitations

- Windows only (Windows 10 / 11)
- Taskbar embedding relies on Windows Shell COM interfaces; some Windows 11 builds may have compatibility issues
- Some AI providers do not offer public usage APIs; their declaration packages depend on web login state

## Feedback & Contact

- Feature requests / bug reports: open a [GitHub Issue](https://github.com/masclown/UsageMonitor/issues)
- Security vulnerabilities: do **not** file a public issue; report privately per [SECURITY.md](SECURITY.md) section 5
- Community discussion: join our Feishu group

  ![Feishu group](docs/images/feishu-group.png)

- Video demos & tutorials: [Xiaohongshu collection](https://www.xiaohongshu.com/collection/item/6a659ebb15b6000000000001?xhsshare=&appuid=63889cd9000000001f01adf9&apptime=1786286834&share_id=e6674a2755b94083a0db59e96c185513&share_channel=copy_link)
