# 插件清单格式说明

> **注意**：宿主消费逻辑（自动更新检测）归 req-126 实现，本文档仅定义格式。

## plugins-manifest.json

| 字段 | 类型 | 说明 |
|------|------|------|
| schemaVersion | string | 格式版本，当前 "1.0" |
| generatedAt | string (ISO 8601) | 生成时间 |
| providers | array | 插件列表 |

### providers 数组元素

| 字段 | 类型 | 说明 |
|------|------|------|
| id | string | 插件标识符（如 "minimax"） |
| name | string | 显示名称 |
| version | string | 插件版本号 |
| minSdkVersion | string | 最低 Core SDK 版本 |
| downloadUrl | string | 下载地址 |
| sha256 | string | 包 SHA256 校验值 |
| size | number | 包大小（字节） |
| releaseNotes | string | 版本说明 |

### 示例

```json
{
  "schemaVersion": "1.0",
  "generatedAt": "2026-07-29T00:00:00Z",
  "providers": [
    {
      "id": "minimax",
      "name": "MiniMax",
      "version": "1.2.0",
      "minSdkVersion": "0.49.0",
      "downloadUrl": "https://github.com/masclown/UsageMonitor/releases/download/plugins/minimax-1.2.0.zip",
      "sha256": "<包文件的 SHA256 校验值>",
      "size": 24576,
      "releaseNotes": "分页回补支持；修复热力图档位。"
    }
  ]
}
```

## 与 update-manifest.json 的区别

- `update-manifest.json`：Core 主程序更新清单（由 Launcher 消费）
- `plugins-manifest.json`：插件更新清单（由宿主插件管理器消费，归 req-126）
