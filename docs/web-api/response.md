# 📄 响应格式说明

## 统一响应格式

所有接口均返回以下结构：

```json
{
  "code": "200",
  "message": "成功",
  "data": {}
}
```

## 响应字段说明

| 字段 | 类型 | 说明 |
|:---|:---|:---|
| `code` | String | 状态码（`"200"`: 成功，`"600"`: 失败） |
| `message` | String | 响应消息 |
| `data` | Object / Array / null | 响应数据 |

!!! warning "code 字段是字符串"
    App 实际返回的 `code` 是**字符串**类型（`"200"` / `"600"`），不是数字。本档部分历史示例写作数字 `200`，已统一修正，请以字符串为准。

## 常见状态码

| 状态码 | 含义 |
|:---|:---|
| `"200"` | 请求成功 |
| `"600"` | 请求失败（参数错误、内部错误等） |

## 调试响应字段

调试接口（搜索、书籍详情、章节列表、正文、发现）的 `data` 字段在 App 实际实现中通常包含以下结构（来自 `ResponsesModel`）：

| 字段 | 类型 | 说明 |
|:---|:---|:---|
| `resultData` | Object | 解析后的结构化结果 |
| `list` | Array | 列表型结果（书籍列表、章节列表等） |
| `jsLog` | String | JS 引擎日志 |
| `gets` | Object | HTTP 请求记录 |

!!! warning "字段差异提示"
    本档历史示例中的 `books/chapters/content/log/time` 等字段，**已被 App 实际返回的 `resultData/list/jsLog/gets` 取代**。建议调用方按 `ResponsesModel` 结构解析调试响应。

## 错误响应示例

```json
{
  "code": "600",
  "message": "参数错误：siteJson 不能为空",
  "data": null
}
```

---

📅 **更新日期**：2025-10-12
