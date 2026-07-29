# UsageMonitor - AI 用量监控工具

[![Platform](https://img.shields.io/badge/platform-Windows%2010%2F11-blue)]()
[![License: EULA](https://img.shields.io/badge/license-EULA-orange)](EULA.md)
[![SDK: Apache-2.0](https://img.shields.io/badge/SDK%20%26%20%E5%A3%B0%E6%98%8E%E5%8C%85-Apache--2.0-green)](LICENSE-APACHE)

一款轻量级 Windows 任务栏工具，把你在各家 AI 服务商（MiniMax、DeepSeek、Kimi、Qoder 等）的用量、额度与余额统一显示在任务栏和托盘悬浮窗中，随时一眼可查。

---

## 下载与安装

前往 Releases 页面下载最新版便携包：

**👉 [https://github.com/OWNER/UsageMonitor/releases](https://github.com/OWNER/UsageMonitor/releases)**

- **解压即用，无需安装**：下载 zip 包后解压到任意目录，双击 `UsageMonitor.exe`（或 Launcher）即可运行
- **无需管理员权限**，不写注册表，不装系统服务
- 卸载 = 直接删除整个目录

### 系统要求

| 项目 | 要求 |
|------|------|
| 操作系统 | Windows 10 / Windows 11 |
| 运行时 | 无需单独安装（便携包自带 .NET 运行时） |
| 权限 | 普通用户权限即可 |

### 下载校验

Release 附件包含 `SHA256SUMS` 文件，下载后建议校验完整性：

```powershell
Get-FileHash .\UsageMonitor-portable.zip -Algorithm SHA256
```

详见 [SECURITY.md](SECURITY.md) 第 6 节。

## 便携模式说明

UsageMonitor 采用**便携（Portable）设计**，所有数据跟着程序目录走：

- **`portable.mode` 标记文件**：程序目录下存在该文件时，启用便携模式——配置、凭据、历史数据全部存放在程序目录下的 `data/` 子目录，而不是 `%AppData%`
- **`data/` 目录整包迁移**：换电脑 / 换目录时，把整个程序目录（含 `data/`）拷贝走即可。注意：加密凭据与 Windows 账户绑定（DPAPI），跨机器迁移后需要重新登录一次
- **配置文件位置**：
  - 便携模式：`<程序目录>\data\config.json`
  - 非便携模式（无 `portable.mode` 文件）：`%AppData%\UsageMonitor\config.json`

## 功能特性

- **任务栏嵌入显示** — 用量圆环 / 迷你图表 / 文本直接嵌入 Windows 任务栏，不占屏幕空间
- **系统托盘 + 悬浮窗** — 托盘图标悬停即弹出用量概览，右键菜单快速刷新、打开设置
- **纯声明式插件** — 一个服务商 = 一份 JSON 声明包（零 DLL、零可执行代码），安装第三方声明包前可一键校验
- **定时自动刷新** — 刷新间隔可配置，自动拉取最新用量与余额
- **多服务商支持** — 随包提供 MiniMax / DeepSeek / Kimi / Qoder 等声明包，支持多账号
- **丰富图表** — 进度条、折线、曲线、热力图、堆叠柱、堆积进度条等卡片图表，以及任务栏迷你图
- **用量历史** — 本地持久化历史数据，支持按日 / 周 / 月聚合查看
- **双主题 UI** — 浅色 / 深色主题，支持外部主题包与图表样式包扩展
- **凭据本地加密** — 登录态与 API Key 经 Windows DPAPI 加密后仅存储在本地

## 界面截图

<!-- TODO: 截图 主窗口（用量卡片与图表） -->
<!-- TODO: 截图 任务栏嵌入效果 -->
<!-- TODO: 截图 托盘悬浮窗 -->
<!-- TODO: 截图 设置界面 -->

## 使用说明

1. 启动程序后，系统托盘会出现 UsageMonitor 图标
2. 右键托盘图标 → 「设置」，选择服务商并完成登录（🌐 获取登录态）或填写密钥
3. 启用「任务栏显示」，用量信息将嵌入任务栏；数据按设定间隔自动刷新
4. 悬停托盘图标可查看用量概览悬浮窗；主窗口提供完整卡片图表与历史统计

## 法律免责声明

- **非官方工具**：UsageMonitor 是独立开发的第三方工具，与 MiniMax、DeepSeek、Kimi（月之暗面）、Qoder 或任何其他 AI 服务商**均无隶属、授权或合作关系**
- **数据归属**：本工具仅帮助你**使用你自己的账户凭据，查询你自己账户内的用量数据**，不代理、不共享、不转售任何服务商的服务或数据
- **风险自担**：使用本工具查询用量属于对你自有账户的正常访问，但各服务商的服务条款可能随时变化，是否使用及由此产生的一切后果由用户自行承担
- **商标声明**：文中提及的第三方服务商名称、Logo 与商标归各自所有者所有

## 安全相关

- 凭据加密存储、网络行为边界、插件安全模型与漏洞披露方式，详见 **[SECURITY.md](SECURITY.md)**
- **关于杀毒软件误报**：UsageMonitor 是未混淆、未加壳的 .NET 程序，但由于程序未购买商业代码签名证书，部分杀毒软件可能对新发布的可执行文件产生启发式误报。若遇到误报：
  - 请通过 Release 附件的 `SHA256SUMS` 核对文件完整性，确认下载自官方 Releases 页面
  - 可将文件提交至 VirusTotal 交叉验证：<!-- TODO: VirusTotal 扫描链接 -->
  - 欢迎在 Issues 中反馈误报的杀软名称与版本
- 全部出站请求仅限各插件声明的服务商域名与 favicon 抓取，**没有遥测、没有数据上报**（可抓包验证）

## 许可证

本项目采用分层许可结构：

| 范围 | 许可证 | 说明 |
|------|--------|------|
| 主程序（UsageMonitor 可执行程序及其组件） | **专有免费软件 EULA** | 个人 / 非商业使用免费；**禁止再分发、禁止逆向工程**。全文见 [EULA.md](EULA.md)。 |
| SDK 契约 / 声明包 JSON / 插件模板 | **Apache License 2.0** | 社区可自由开发、分发第三方声明包与主题包，无商用限制。全文见 [LICENSE-APACHE](LICENSE-APACHE)。 |
| 云端 / 高级功能（规划中） | 专有闭源 | 多端同步、云备份、通知等高级能力，不属于本仓库范围。 |

> 第三方 AI 服务商的品牌 Logo、名称、商标归各自所有者所有。本工具**不随包分发**任何第三方 Logo——图标由程序运行时按服务商域名抓取 favicon 并缓存在本地，本项目不主张任何相关权利。

## 已知限制

- 仅支持 Windows 平台（Windows 10 / 11）
- 任务栏嵌入依赖 Windows Shell COM 接口，部分 Windows 11 版本可能存在兼容性问题
- 部分 AI 服务商未提供公开用量查询接口，相关声明包依赖网页端登录态

## 反馈与联系

- 功能建议 / 缺陷反馈：请提交 [GitHub Issues](https://github.com/OWNER/UsageMonitor/issues)
- 安全漏洞：请**勿**公开提交 Issue，按 [SECURITY.md](SECURITY.md) 第 5 节的私密渠道报告
