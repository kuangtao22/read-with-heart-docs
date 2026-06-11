# Web写源开放接口文档

> **通过 Web 接口快速创建和调试书源**
> 
> 本文档介绍如何使用 Web API 与用心读书 APP 进行交互，实现书源的创建、编辑、调试和管理。

## 文档边界

- 本文档描述的是基于 v1 接口形态的 Web 写源 API；它是历史参考，不应当成 ParsingBook 当前 V2 规则实现的最新管理入口。
- V2 规则管理仍以 ParsingBook App 前端书源配置为准；如需新增字段，应优先反馈到 `mcp-parsingbook-rules-nodejs` 仓库的 schema 文档，不要直接改这份接口文档当成新协议。
- 接口参数以 App 端实际接受为准；本档示例和 V2 字段之间如有差异，以 App 端字段为准。

---

## 📖 目录

1. [接口概述](#接口概述)
2. [书源管理接口](#书源管理接口)
3. [书源调试接口](#书源调试接口)
4. [辅助接口](#辅助接口)
5. [使用示例](#使用示例)
6. [响应格式说明](#响应格式说明)

---

## 🌐 接口概述

### 基本信息

**接口地址：** `http://localhost:8080/api/`

**请求方式：** `POST`

**数据格式：** `application/json`

**字符编码：** `UTF-8`

**认证方式：** 无需认证

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
- Put数据清除

---

## 📋 书源管理接口

### 1. 源列表

**接口：** `POST /api/site/list`

**功能：** 获取书源列表，支持搜索和分组筛选

**请求参数：**

| 参数名称 | 类型 | 必填 | 说明 |
|:---|:---|:---|:---|
| `keyword` | String | 否 | 搜索关键词（可搜索源名称/源网址） |
| `groupId` | Number | 否 | 源分组ID |

**请求示例：**

```http
POST /api/site/list
Content-Type: application/json

{
  "keyword": "示例",
  "groupId": 1
}
```

**响应示例：**

```json
{
  "code": 200,
  "message": "成功",
  "data": [
    {
      "id": 1,
      "name": "示例阅读站",
      "host": "https://novel.example.org",
      "index": 100,
      "groupId": 1,
      "status": 1,
      "finderStatus": 1,
      "remarks": "官方书源"
    }
  ]
}
```

---

### 2. 书源分组列表

**接口：** `POST /api/site/group`

**功能：** 获取所有书源分组

**请求参数：** 无

**请求示例：**

```http
POST /api/site/group
Content-Type: application/json

{}
```

**响应示例：**

```json
{
  "code": 200,
  "message": "成功",
  "data": [
    {
      "id": 1,
      "name": "官方源",
      "count": 10
    },
    {
      "id": 2,
      "name": "优质源",
      "count": 25
    }
  ]
}
```

---

### 3. 书源详情

**接口：** `POST /api/site/info`

**功能：** 获取单个书源的详细信息

**请求参数：**

| 参数名称 | 类型 | 必填 | 说明 |
|:---|:---|:---|:---|
| `id` | Number | 是 | 书源ID |
| `password` | String | 否 | 加密书源密码（如果书源加密必填） |

**请求示例：**

```http
POST /api/site/info
Content-Type: application/json

{
  "id": 1,
  "password": "123456"
}
```

**响应示例：**

```json
{
  "code": 200,
  "message": "成功",
  "data": {
    "id": 1,
    "name": "示例阅读站",
    "host": "https://novel.example.org",
    "siteJson": "{...书源JSON规则...}",
    "index": 100,
    "groupId": 1,
    "status": 1,
    "finderStatus": 1,
    "remarks": "官方书源"
  }
}
```

---

### 4. 书源保存

**接口：** `POST /api/site/save`

**功能：** 创建新书源或更新现有书源

**请求参数：**

| 参数名称 | 类型 | 必填 | 说明 |
|:---|:---|:---|:---|
| `siteJson` | String | 是 | 书源JSON规则串 |
| `id` | Number | 否 | 书源ID（编辑时必填） |
| `index` | Number | 否 | 排序（数字越大越靠前） |
| `groupId` | Number | 否 | 源分组ID |
| `status` | Number | 否 | 搜索状态（1启用，0禁用） |
| `finderStatus` | Number | 否 | 发现状态（1启用，0禁用） |
| `remarks` | String | 否 | 源笔记 |

**请求示例：**

```http
POST /api/site/save
Content-Type: application/json

{
  "siteJson": "{\"name\":\"示例书源\",\"host\":\"https://example.com\",\"search\":{...}}",
  "index": 100,
  "groupId": 1,
  "status": 1,
  "finderStatus": 1,
  "remarks": "测试书源"
}
```

**响应示例：**

```json
{
  "code": 200,
  "message": "保存成功",
  "data": {
    "id": 1
  }
}
```

---

### 5. 书源删除

**接口：** `POST /api/site/delete`

**功能：** 删除指定书源

**请求参数：**

| 参数名称 | 类型 | 必填 | 说明 |
|:---|:---|:---|:---|
| `id` | Number | 是 | 书源ID |

**请求示例：**

```http
POST /api/site/delete
Content-Type: application/json

{
  "id": 1
}
```

**响应示例：**

```json
{
  "code": 200,
  "message": "删除成功"
}
```

---

### 6. 单个源导出

**接口：** `POST /api/site/export`

**功能：** 导出单个书源的JSON配置

**请求参数：**

| 参数名称 | 类型 | 必填 | 说明 |
|:---|:---|:---|:---|
| `id` | Number | 是 | 书源ID |

**请求示例：**

```http
POST /api/site/export
Content-Type: application/json

{
  "id": 1
}
```

**响应示例：**

```json
{
  "code": 200,
  "message": "成功",
  "data": {
    "name": "示例阅读站",
    "host": "https://novel.example.org",
    "search": {...},
    "detail": {...},
    "catalog": {...},
    "content": {...}
  }
}
```

---

## 🔧 书源调试接口

### 7. 搜索调试

**接口：** `POST /api/site/search`

**功能：** 测试书源的搜索功能

**请求参数：**

| 参数名称 | 类型 | 必填 | 说明 |
|:---|:---|:---|:---|
| `keyword` | String | 是 | 书名搜索词 |
| `siteJson` | String | 是 | 书源JSON规则串 |

**请求示例：**

```http
POST /api/site/search
Content-Type: application/json

{
  "keyword": "斗破苍穹",
  "siteJson": "{\"name\":\"示例书源\",\"host\":\"https://example.com\",\"search\":{...}}"
}
```

**响应示例：**

```json
{
  "code": 200,
  "message": "成功",
  "data": {
    "books": [
      {
        "name": "斗破苍穹",
        "author": "天蚕土豆",
        "cover": "https://example.com/cover.jpg",
        "intro": "简介...",
        "bookUrl": "https://example.com/book/123"
      }
    ],
    "log": "搜索日志...",
    "time": 1234
  }
}
```

---

### 8. 章节详情调试

**接口：** `POST /api/site/bookinfo`

**功能：** 测试书源的书籍详情功能

**请求参数：**

| 参数名称 | 类型 | 必填 | 说明 |
|:---|:---|:---|:---|
| `bookJson` | String | 是 | 书籍JSON格式（包含bookUrl字段） |
| `siteJson` | String | 是 | 书源JSON规则串 |

**请求示例：**

```http
POST /api/site/bookinfo
Content-Type: application/json

{
  "bookJson": "{\"bookUrl\":\"https://example.com/book/123\",\"name\":\"斗破苍穹\"}",
  "siteJson": "{\"name\":\"示例书源\",\"detail\":{...}}"
}
```

**响应示例：**

```json
{
  "code": 200,
  "message": "成功",
  "data": {
    "name": "斗破苍穹",
    "author": "天蚕土豆",
    "cover": "https://example.com/cover.jpg",
    "intro": "三十年河东，三十年河西...",
    "category": "玄幻",
    "status": "完本",
    "lastChapter": "大结局",
    "log": "详情日志...",
    "time": 856
  }
}
```

---

### 9. 章节列表调试

**接口：** `POST /api/site/chapter`

**功能：** 测试书源的章节列表功能

**请求参数：**

| 参数名称 | 类型 | 必填 | 说明 |
|:---|:---|:---|:---|
| `bookJson` | String | 是 | 书籍JSON格式 |
| `siteJson` | String | 是 | 书源JSON规则串 |

**请求示例：**

```http
POST /api/site/chapter
Content-Type: application/json

{
  "bookJson": "{\"bookUrl\":\"https://example.com/book/123\",\"name\":\"斗破苍穹\"}",
  "siteJson": "{\"name\":\"示例书源\",\"catalog\":{...}}"
}
```

**响应示例：**

```json
{
  "code": 200,
  "message": "成功",
  "data": {
    "chapters": [
      {
        "name": "第一章 落魄少年",
        "url": "https://example.com/chapter/1"
      },
      {
        "name": "第二章 斗之力等级",
        "url": "https://example.com/chapter/2"
      }
    ],
    "log": "章节日志...",
    "time": 678
  }
}
```

---

### 10. 正文调试

**接口：** `POST /api/site/content`

**功能：** 测试书源的正文内容功能

**请求参数：**

| 参数名称 | 类型 | 必填 | 说明 |
|:---|:---|:---|:---|
| `catalogJson` | String | 是 | 章节JSON格式 |
| `siteJson` | String | 是 | 书源JSON规则串 |

**请求示例：**

```http
POST /api/site/content
Content-Type: application/json

{
  "catalogJson": "{\"url\":\"https://example.com/chapter/1\",\"name\":\"第一章\"}",
  "siteJson": "{\"name\":\"示例书源\",\"content\":{...}}"
}
```

**响应示例：**

```json
{
  "code": 200,
  "message": "成功",
  "data": {
    "content": "章节正文内容...",
    "log": "正文日志...",
    "time": 345
  }
}
```

---

### 11. 发现调试

**接口：** `POST /api/site/finder`

**功能：** 测试书源的发现功能

**请求参数：**

| 参数名称 | 类型 | 必填 | 说明 |
|:---|:---|:---|:---|
| `siteJson` | String | 是 | 书源JSON规则串 |
| `categoryUUID` | String | 是 | 分类UUID |
| `selectList` | String | 是 | 选择筛选项（格式：`{"cat": "0"}`） |
| `pageIndex` | Number | 是 | 页码 |

**请求示例：**

```http
POST /api/site/finder
Content-Type: application/json

{
  "siteJson": "{\"name\":\"示例书源\",\"finder\":{...}}",
  "categoryUUID": "finder_cat_001",
  "selectList": "{\"cat\": \"0\", \"status\": \"1\"}",
  "pageIndex": 1
}
```

**响应示例：**

```json
{
  "code": 200,
  "message": "成功",
  "data": {
    "books": [
      {
        "name": "书籍名称",
        "author": "作者",
        "cover": "封面URL",
        "bookUrl": "书籍URL"
      }
    ],
    "log": "发现日志...",
    "time": 567
  }
}
```

---

### 12. 取消调试请求

**接口：** `POST /api/site/cancelRequest`

**功能：** 取消正在进行的调试请求

**请求参数：**

| 参数名称 | 类型 | 必填 | 说明 |
|:---|:---|:---|:---|
| `type` | Number | 是 | 请求类型（0:搜索, 1:章节, 2:正文） |

**请求示例：**

```http
POST /api/site/cancelRequest
Content-Type: application/json

{
  "type": 0
}
```

**响应示例：**

```json
{
  "code": 200,
  "message": "已取消"
}
```

---

## 🛠️ 辅助接口

### 13. 调试清除

**接口：** `POST /api/debug/clear`

**功能：** 清除调试日志或Put数据

**请求参数：**

| 参数名称 | 类型 | 必填 | 说明 |
|:---|:---|:---|:---|
| `type` | Number | 是 | 清除类型（1:搜索log, 2:章节log, 3:正文log, 4:put数据, 5:发现log） |
| `siteIdent` | String | 否 | 源标识（type=4时必填） |

**请求示例：**

```http
POST /api/debug/clear
Content-Type: application/json

{
  "type": 1
}
```

**清空Put数据示例：**

```http
POST /api/debug/clear
Content-Type: application/json

{
  "type": 4,
  "siteIdent": "example_site"
}
```

**响应示例：**

```json
{
  "code": 200,
  "message": "清除成功"
}
```

---

## 💡 使用示例

### JavaScript (Fetch API)

```javascript
// 获取书源列表
async function getSiteList(keyword = '', groupId = null) {
  const response = await fetch('http://localhost:8080/api/site/list', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      keyword: keyword,
      groupId: groupId
    })
  });
  
  const data = await response.json();
  return data;
}

// 保存书源
async function saveSite(siteJson, options = {}) {
  const response = await fetch('http://localhost:8080/api/site/save', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      siteJson: siteJson,
      ...options
    })
  });
  
  const data = await response.json();
  return data;
}

// 搜索调试
async function debugSearch(keyword, siteJson) {
  const response = await fetch('http://localhost:8080/api/site/search', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      keyword: keyword,
      siteJson: siteJson
    })
  });
  
  const data = await response.json();
  return data;
}

// 正文调试
async function debugContent(catalogJson, siteJson) {
  const response = await fetch('http://localhost:8080/api/site/content', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      catalogJson: catalogJson,
      siteJson: siteJson
    })
  });
  
  const data = await response.json();
  return data;
}
```

### Python (requests)

```python
import requests
import json

BASE_URL = 'http://localhost:8080/api'

headers = {
    'Content-Type': 'application/json'
}

# 获取书源列表
def get_site_list(keyword='', group_id=None):
    response = requests.post(
        f'{BASE_URL}/site/list',
        headers=headers,
        json={
            'keyword': keyword,
            'groupId': group_id
        }
    )
    return response.json()

# 保存书源
def save_site(site_json, **options):
    data = {
        'siteJson': site_json,
        **options
    }
    response = requests.post(
        f'{BASE_URL}/site/save',
        headers=headers,
        json=data
    )
    return response.json()

# 搜索调试
def debug_search(keyword, site_json):
    response = requests.post(
        f'{BASE_URL}/site/search',
        headers=headers,
        json={
            'keyword': keyword,
            'siteJson': site_json
        }
    )
    return response.json()

# 章节列表调试
def debug_chapter(book_json, site_json):
    response = requests.post(
        f'{BASE_URL}/site/chapter',
        headers=headers,
        json={
            'bookJson': book_json,
            'siteJson': site_json
        }
    )
    return response.json()

# 正文调试
def debug_content(catalog_json, site_json):
    response = requests.post(
        f'{BASE_URL}/site/content',
        headers=headers,
        json={
            'catalogJson': catalog_json,
            'siteJson': site_json
        }
    )
    return response.json()

# 清除调试日志
def clear_debug_log(clear_type, site_ident=None):
    data = {'type': clear_type}
    if site_ident:
        data['siteIdent'] = site_ident
    
    response = requests.post(
        f'{BASE_URL}/debug/clear',
        headers=headers,
        json=data
    )
    return response.json()
```

### cURL

```bash
# 获取书源列表
curl -X POST "http://localhost:8080/api/site/list" \
  -H "Content-Type: application/json" \
  -d '{
    "keyword": "示例",
    "groupId": 1
  }'

# 保存书源
curl -X POST "http://localhost:8080/api/site/save" \
  -H "Content-Type: application/json" \
  -d '{
    "siteJson": "{\"name\":\"示例书源\",\"host\":\"https://example.com\"}",
    "index": 100,
    "status": 1
  }'

# 搜索调试
curl -X POST "http://localhost:8080/api/site/search" \
  -H "Content-Type: application/json" \
  -d '{
    "keyword": "斗破苍穹",
    "siteJson": "{\"name\":\"示例书源\",\"search\":{...}}"
  }'

# 正文调试
curl -X POST "http://localhost:8080/api/site/content" \
  -H "Content-Type: application/json" \
  -d '{
    "catalogJson": "{\"url\":\"https://example.com/chapter/1\"}",
    "siteJson": "{\"name\":\"示例书源\",\"content\":{...}}"
  }'

# 清除搜索日志
curl -X POST "http://localhost:8080/api/debug/clear" \
  -H "Content-Type: application/json" \
  -d '{
    "type": 1
  }'
```

---

## 📄 响应格式说明

### 统一响应格式

所有接口均返回以下格式：

```json
{
  "code": 200,
  "message": "成功",
  "data": {...}
}
```

### 响应字段说明

| 字段 | 类型 | 说明 |
|:---|:---|:---|
| `code` | Number | 状态码（200:成功, 其他:失败） |
| `message` | String | 响应消息 |
| `data` | Object/Array | 响应数据 |

### 常见状态码

| 状态码 | 说明 |
|:---|:---|
| 200 | 请求成功 |
| 400 | 请求参数错误 |
| 404 | 资源不存在 |
| 500 | 服务器内部错误 |

### 调试响应字段

调试接口（搜索、章节、正文、发现）的响应通常包含以下字段：

| 字段 | 类型 | 说明 |
|:---|:---|:---|
| `log` | String | 调试日志 |
| `time` | Number | 执行时间（毫秒） |
| `books` / `chapters` / `content` | Array/String | 解析结果 |

### 错误响应示例

```json
{
  "code": 400,
  "message": "参数错误：siteJson不能为空",
  "data": null
}
```

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
    - 可以通过防火墙限制访问IP

### CORS 配置

默认允许所有域名访问，可在设置中配置允许的域名列表：

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

- [规则说明](制作源/rules-Introduction.md) - 了解书源规则语法
- [Native to Javascript](native-to-js.md) - 查看可用的 JS 方法
- [正文富文本规则](content-richtext.md) - 正文格式说明

---

## 🎯 快速开始指南

### 1. 创建并保存书源

```javascript
const siteJson = JSON.stringify({
  name: "示例书源",
  host: "https://example.com",
  search: {
    url: "https://example.com/search?keyword=@{keyword}",
    list: "//div[@class='book-item']",
    name: "//h3/text()",
    author: "//span[@class='author']/text()",
    bookUrl: "//a/@href"
  },
  catalog: {
    url: "@{bookUrl}/catalog",
    list: "//div[@class='chapter']/a",
    name: "/text()",
    url: "/@href"
  },
  content: {
    url: "@{chapterUrl}",
    content: "//div[@id='content']/text()"
  }
});

// 保存书源
const result = await saveSite(siteJson, {
  index: 100,
  status: 1,
  finderStatus: 0,
  remarks: "测试书源"
});

console.log('书源ID:', result.data.id);
```

### 2. 调试书源

```javascript
// 1. 搜索调试
const searchResult = await debugSearch('斗破苍穹', siteJson);
console.log('搜索结果:', searchResult.data.books);
console.log('搜索日志:', searchResult.data.log);

// 2. 章节列表调试
const bookJson = JSON.stringify({
  bookUrl: searchResult.data.books[0].bookUrl,
  name: searchResult.data.books[0].name
});

const chapterResult = await debugChapter(bookJson, siteJson);
console.log('章节列表:', chapterResult.data.chapters);

// 3. 正文调试
const catalogJson = JSON.stringify({
  url: chapterResult.data.chapters[0].url,
  name: chapterResult.data.chapters[0].name
});

const contentResult = await debugContent(catalogJson, siteJson);
console.log('正文内容:', contentResult.data.content);
```

### 3. 管理书源

```javascript
// 获取书源列表
const list = await getSiteList('示例', 1);

// 获取书源详情
const detail = await fetch('http://localhost:8080/api/site/info', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ id: 1 })
}).then(res => res.json());

// 删除书源
await fetch('http://localhost:8080/api/site/delete', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ id: 1 })
});
```

---

📅 **更新日期**：2025-10-12  
📝 **版本**：v1.0  
🔗 **API 版本**：v1
