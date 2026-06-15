# 🌐 Web 写源开放接口

> **通过 Web 接口快速创建和调试书源**
>
> 本文档介绍如何使用 Web API 与用心读书 APP 进行交互，实现书源的创建、编辑、调试和管理。

---

## 📖 文档结构

| 分类 | 内容 | 入口 |
|------|------|------|
| **书源管理** | 列表、分组、详情、保存、删除、导出（6 个接口） | [书源管理接口 →](site-management.md) |
| **书源调试** | 搜索、书籍详情、章节列表、正文、发现、取消请求（6 个接口） | [书源调试接口 →](debug.md) |
| **辅助接口** | 调试日志清除（1 个接口） | [辅助接口 →](misc.md) |
| **响应格式** | 统一响应结构、状态码、错误示例 | [响应格式说明 →](response.md) |
| **使用示例** | JavaScript / Python / cURL 调用样例 | [使用示例 →](examples.md) |
| **快速开始** | 三步上手：保存 → 调试 → 管理 | [快速开始指南 →](quickstart.md) |

---

## 📌 文档边界

- 本文档描述用心读书 App 当前提供的 Web 写源开放接口。
- 接口字段以 App 端实际返回为准；如本档示例与 App 实际行为不一致，以 App 端为准。
- 如需新增字段或反馈规则问题，可反馈到 `mcp-parsingbook-rules-nodejs` 仓库。

!!! warning "字段差异提示"
    本档部分响应字段示例可能滞后于 App 实现。例如 `/api/site/list` 实际返回字段为 `id/version/time/siteName/groupId/siteIdent/url/isPassword/status/finderStatus`，而示例中可能写作 `name/host/index/remarks`。**所有响应字段以 App 端实际返回为准**，本档示例仅用于结构参考。

---

## 🌐 接口概述

### 基本信息

**接口地址：** `http://localhost:8080/api/`

**请求方式：** `POST`（所有写源接口均为 POST）

**数据格式：** `application/json`

**字符编码：** `UTF-8`

**认证方式：** 无需认证

!!! note "请求参数类型"
    所有请求参数值在 App 内部统一按 **String** 处理（如 `id`、`groupId`、`type` 等都传字符串）。本档部分历史示例写作 Number 类型，请以字符串为准。

### 功能说明

Web 写源接口提供以下功能：

✅ **书源管理**

- 获取书源列表
- 获取书源详情
- 保存/更新书源
- 删除书源
- 导出书源
- 书源分组管理

✅ **书源调试**

- 搜索调试
- 章节详情调试
- 章节列表调试
- 正文调试
- 发现调试
- 取消调试请求

✅ **辅助功能**

- 调试日志清除

---

## 🔧 配置说明

### 启用 Web API

1. 打开 APP
2. 进入 设置 → 开发者选项
3. 开启"启用 Web API"
4. 设置监听端口（默认 8080）
5. 保存设置

### 安全建议

!!! warning "安全提示"
    - 仅在可信网络环境下使用
    - 建议仅在本地网络或内网中开放
    - 生产环境建议使用 HTTPS
    - 可以通过防火墙限制访问 IP

### CORS 配置

默认允许所有域名访问（`Access-Control-Allow-Origin: *`，允许 `GET,POST,PUT,DELETE,OPTIONS`）。如需收紧，可在设置中配置允许的域名列表：

```json
{
  "cors": {
    "allowOrigins": ["http://localhost:3000", "https://example.com"],
    "allowMethods": ["POST"],
    "allowHeaders": ["Content-Type"]
  }
}
```

---

## 📚 相关文档

- [规则说明](../制作源/rules-Introduction.md) - 了解书源规则语法
- [Native to Javascript](../native-to-js.md) - 查看可用的 JS 方法
- [正文富文本](../content-richtext/index.md) - 正文格式说明

---

📅 **更新日期**：2025-10-12
