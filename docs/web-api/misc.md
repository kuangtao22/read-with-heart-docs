# 🛠️ 辅助接口

## 调试清除

**接口：** `POST /api/debug/clear`

**功能：** 清除指定类型的调试日志

**请求参数：**

| 参数名称 | 类型 | 必填 | 说明 |
|:---|:---|:---|:---|
| `type` | String | 是 | 清除类型（`"1"`: 搜索 log，`"2"`: 章节 log，`"3"`: 正文 log，`"5"`: 发现 log） |
| `siteIdent` | String | 否 | 源标识 |

**请求示例：**

```http
POST /api/debug/clear
Content-Type: application/json

{
  "type": "1"
}
```

**响应示例：**

```json
{
  "code": "200",
  "message": "清除成功",
  "data": true
}
```

---

📅 **更新日期**：2025-10-12
