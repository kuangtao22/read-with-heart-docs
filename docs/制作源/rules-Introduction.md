# 规则

## 协议边界

本文档覆盖当前 App 端已支持的规则语法。历史兼容说明会和正式协议在同一份文档里共存，但新规则应按正式协议写。

解析引擎目前支持 `xpath`、`jsonpath`、`css`。三种规则的具体写法分别见 [XPath规则](rule-xpath.md)、[JSONPath规则](rule-jsonpath.md)、[CSS规则](rule-css.md)。

## 书源顶层字段

下面列出的是书源 JSON 顶层字段，仅说明是否对规则作者可见或仅维护者可见。

| 字段 | 说明 | 可见性 |
| --- | --- | --- |
| `siteName` | 书源名称 | 规则作者可读 `@{siteName}` / `config.siteName` |
| `host` | 站点主地址 | 规则作者可读 `@{host}` / `config.host` |
| `icon` | 书源图标 URL | 维护者字段 |
| `version` | 书源版本 | 维护者字段 |
| `type` | 引擎类型 | 维护者字段 |
| `group` | 分类分组 | 维护者字段 |
| `author` / `authorUUID` / `authStatus` | 作者与编辑授权 | 维护者字段 |
| `contact` | 联系方式 | 维护者字段 |
| `openEditAuth` | 是否开放编辑授权 | 维护者字段 |
| `remarks` | 备注 | 维护者字段 |
| `password` / `passwordHint` | 编辑保护密码 | 维护者字段 |
| `updateTime` / `createTime` | 时间戳 | 维护者字段 |
| `publicJavascript` | 公共 JS 库 | 维护者字段 |
| `loginUrl` | 登录页 URL | 规则作者可见，运行时会经过规则求值 |
| `header` | 公共请求头 | 规则作者可见 |
| `openParams` | 开放参数 | 规则作者可见，详见 [openParams 开放参数](#openparams) |

## 参数说明

### 系统自带get参数

| 参数名称 | 说明 | 使用场景 | 用例 |
| --- | --- | --- | --- |
| keyword | 关键词 | 书籍搜索 | `@get{keyword}` |
| preResHeader1 | 前置请求标头 | 全局可用 | `@get{preResHeader1.xxx}` |
| preRepHeader1 | 前置响应标头 | 全局可用 | `@get{preRepHeader1.xxx}` |
| resHeader | 请求标头 | 全局可用 | `@get{resHeader.xxx}` |
| repHeader | 响应标头 | 全局可用 | `@get{repHeader.xxx}` |
| loginRequest | 登录请求头 | 全局可用 | `@get{loginRequest}` |
| loginResponse | 登录响应头 | 全局可用 | `@get{loginResponse}` |
| loginCookies | 登录cookies | 全局可用 | `@get{loginCookies}` |
| verifyHeader | 浏览器过盾请求头 | 正文请求 | `@get{verifyHeader}` |
| verifyCookies | 浏览器过盾cookies | 正文请求 | `@get{verifyCookies}` |
| bookUrl | 书籍地址 | 章节列表、正文使用 | `@get{bookUrl}` |
| chapterUrl | 章节地址 | 正文使用 | `@get{chapterUrl}` |
| host | 主地址 | 全局可用 | `@get{host}` |

**说明：**

- 前置响应标头使用数字标记，用于保存所有的前置请求（如 preResHeader1、preResHeader2...）
- `verifyHeader` 和 `verifyCookies`：正文请求如果有浏览器过盾，会自动加入过盾请求头和cookies，如果为空则会弹出过盾弹窗
- `loginRequest`、`loginResponse`、`loginCookies`：点击登录后会自动保存，可在书源中获取使用

### 系统内置JS函数

除了 App 对象提供的方法外，系统还内置了一些常用的 JavaScript 函数：

| 函数名称 | 说明 | 用例 |
| --- | --- | --- |
| `console.log()` | 打印调试信息 | `console.log('你好世界')` |
| `console.log([])` | 打印数组 | `console.log(['你好', '世界'])` |
| `unicode.decode()` | Unicode 解码 | `let string = "\\u006b"`<br/>`unicode.decode(string)` |

**说明：**

- `console.log()` 等同于 `App.log()`，可以互换使用
- `unicode.decode()` 等同于 `App.unicode.decode()`
- 更多 Native to Javascript 方法请参考[Native to Javascript](../native-to-js.md)文档

### 公共内置请求参数

| 参数名称 | 说明 | 用例 | 值类型示例 |
| --- | --- | --- | --- |
| params | 请求参数 | `config.params` |  |
| engine | 源引擎类型 | `config.engine` | xpath、jsonpath、css |
| method | 请求类型 | `config.method` | POST，GET |
| host | 站点地址 | `config.host` |  |
| header | 请求头 | `config.header` | 优先级别：正式请求头&gt;浏览器过盾请求头&gt;公共请求头 |
| mode | 请求模式 | `config.mode` | http、webview |
| requestEncode | 请求编码方式 | `config.requestEncode` | utf-8、gbk |
| responseEncode | 响应编码方式 | `config.responseEncode` | utf-8、gbk |
| cookies | cookies信息 | `config.cookies` |  |
| openParams | 开放参数当前生效值的扁平字典 | `config.openParams` | 详见 [openParams 开放参数](#openparams) |
| verifyCode | 验证码 | `config.verifyCode` |  |

### 编辑器可写 `config.*` 键总表

下表汇总当前代码编辑器中实际可补全的 `config.*` 键（具体可补全项以编辑器为准）：

| 键 | 适用场景 |
| --- | --- |
| `config.url` | 当前请求地址 |
| `config.method` | HTTP 方法 |
| `config.mode` | 请求模式（http / webview） |
| `config.engine` | 解析引擎（xpath / jsonpath / css） |
| `config.header` | 合并后的请求头 |
| `config.cookies` | 合并后的 Cookie |
| `config.params` | 显式请求参数 |
| `config.keyword` | 搜索关键词（搜索场景） |
| `config.bookUrl` | 书籍地址（章节 / 正文） |
| `config.bookName` | 书籍名称 |
| `config.bookAuthor` | 书籍作者 |
| `config.chapterUrl` | 章节地址（正文） |
| `config.chapterName` | 章节名称（正文） |
| `config.pageIndex` | 当前页码 |
| `config.infoUrl` | 书籍详情地址（详情 / 章节列表） |
| `config.siteIdent` | 站点唯一标识 |
| `config.host` | 站点主地址 |
| `config.jsLib` | 公共 JS 库 |
| `config.openParams` | 开放参数扁平字典 |
| `config.deviceId` | App 设备标识 |
| `config.verifyCode` | 验证码值 |
| `config.siteName` | 当前书源名称 |

### 搜索规则

| 参数名称 | 说明 | 用例 |
| --- | --- | --- |
| keyword | 搜索关键词 | `config.keyword` |
| url | 搜索地址 | `config.url` |

### 详情规则

### 章节列表规则

| 参数名称 | 说明 | 用例 |
| --- | --- | --- |
| bookUrl | 章节列表url | `config.bookUrl` |
| infoUrl | 书籍信息url | 如果书籍详情有规则，会先调用`infoUrl`请求，并获取`bookUrl` |
| bookName | 书籍名称 | `config.bookName` |
| bookAuthor | 作者 | `config.bookAuthor` |
| url | 章节列表请求地址 | `config.url` |
| pageIndex | 页码 | `config.pageIndex` |
| pageStart | 起始章节页数（老版本参数不建议使用） | 用于章节规则拼接，如第二章起，标记2 |

### 正文规则

| 参数名称 | 说明 | 用例 |
| --- | --- | --- |
| bookUrl | 章节列表Url | `config.bookUrl` |
| bookName | 书籍名称 | `config.bookName` |
| bookAuthor | 作者 | `config.bookAuthor` |
| chapterUrl | 章节详情Url | `config.chapterUrl` |
| infoUrl | 书籍详情Url | `config.infoUrl` |
| chapterName | 章节名称 | `config.chapterName` |
| url | 正文请求地址 | `config.url` |
| pageIndex | 页码 | `config.pageIndex` |
| pageStart | 起始章节页数（老版本参数不建议使用） | 用于正文规则拼接，如第二章起，标记2 |
| playUrl | 如果`playUrl`规则为空，自动获取正文url和正文header | `config.playUrl` |

### 发现规则

| 参数名称 | 说明 | 用例 |
| --- | --- | --- |
| url | 发现请求地址 | `config.url` |
| pageIndex | 章节页数序列 | `config.pageIndex` |

#### 发现请求里怎么读取筛选参数

发现场景下当前分类的筛选参数会同时以三种方式暴露给规则，规则作者可任选一种：

- `config.selectList`：JSON 数组，元素形如 `{"name": "<显示名>", "type": "<筛选类型标识>", "value": "<当前选中值>"}`。推荐主用法，新增筛选项不需要改规则。

  ```javascript
  <js>
  const params = config.selectList
    .map(it => `${it.type}=${encodeURIComponent(it.value)}`)
    .join('&');
  return `${config.url}?${params}`;
  </js>
  ```

- `config.filters`：扁平字典，键是 `type`，值是当前选中的 `value`。适合只取某个固定筛选项的当前值。

  ```javascript
  <js>
  return `${config.url}?category=${encodeURIComponent(config.filters.category ?? '')}`;
  </js>
  ```

- `config._<type>`：顶层字段，键是 `_<type>`，值是当前选中的 `value`。与旧书源命名习惯保持一致。

  ```javascript
  <js>
  return `${config.url}?id=${encodeURIComponent(config._category_id ?? '')}`;
  </js>
  ```

怎么选：

- 新规则：优先用 `config.selectList` 数组遍历，避免依赖具体的 `type` 命名。
- 维护历史规则：可以继续用 `config.filters` 或 `config._<type>`，两条路径目前都仍然有效。
- 不要因为“新”而去重写原本就写死了 `_<type>` 的旧规则。

边界：

- 这三种访问路径仅在发现场景下可用；搜索 / 章节 / 正文请求里没有这些字段。
- 筛选的“候选项列表”并不通过 `selectList` 暴露，它只暴露当前选中值；候选项请走书源 UI 配置。

### 表达式

APP内置了各种常用的表达式，可以进行替换获取和处理操作。

| 参数 | 名称 | 示例 | 说明 |
| --- | --- | --- | --- |
| `${}` | 获取参数 | `${key}` | 支持获取json的子集`${info.name}`；值为空时保留原文 |
| `@{}` | 获取参数 | `@{key}` | 支持获取json的子集`@{info.name}`；值为空时替换为空字符串 |
| `{{}}` | 运行规则 | `{{}}` | 可以运行当前引擎对应的 xpath、jsonpath、css 规则；`css` 段内不建议写 XPath |
| `@get{}` | 获取put信息 | `@get{name}` | 可以获取`@put{}`的数据 |
| `@put{}` | 保存信息 | `@put{name, '//*[@src]'}` | 目前只支持前置请求的put |
| `<js></js>` | 运行JS语言 | `<js>App.log('hello!');</js>` | 在标签内可以运行js的语言 |
| `@js:` | JS语言处理 | @js:<br/>let name = '张三';<br/>App.log(name); | 在规则中第一行添加`@js:`表示该规则使用js运行 |
| `@all` | 获取网页所有内容 | `@all` | 不使用任何规则，直接获取网页原文 |
| `##正则表达式#替换字符` | 正则替换 | `##a#b` | 替换一次 |
| `##正则表达式##替换字符` | 正则替换 | `##a##b` | 替换所有 |
| `##正则表达式` | 正则替换 | `##a` | 过滤所有 |
| `##正则表达式#$1` | 正则替换 | `##(*.?)#$1` | 取值，如果有子串，可按照$1$2$3...直接取值 |

## URL规则

### 图片URL

有些图片需要增加请求头才能正常加载，可以使用以下方式：

```javascript
// 在图片规则中使用 <js></js> 标签
data.image<js>
return `${data.image}, {
  "header": { 
    "Referer": "https://example.com",
    "User-Agent": "Mozilla/5.0"
  }
}`
</js>
```

### 过盾跨域名获取Cookie

当需要获取非当前主域名的内置浏览器cookie时，需要设置 `domains` 参数：

```json
{
  "domains": [
    "qidian.com",
    "example.com"
  ]
}
```

**说明：** 此配置用于浏览器过盾场景，允许跨域获取指定域名的Cookie信息。

## openParams 开放参数

`openParams` 用来给书源暴露可配置参数，例如线路、分类、账号开关或自定义 Token。它的定义写在书源顶层，规则运行时通过 `config.openParams` 读取当前生效值。

定义项只使用以下字段：

| 字段 | 说明 |
| --- | --- |
| `name` | 展示名称 |
| `key` | 参数键名，规则里通过这个键读取 |
| `value` | 当前值 |
| `defaultValue` | 默认值，用户未保存值时使用 |
| `type` | 参数类型，支持 `input` / `single` / `multiple` |
| `options` | 单选或多选候选项，元素通常包含 `name` 和 `value` |

运行时要记住一个简单模型：`config.openParams` 是 `[String: String]` 形式的扁平字典，只保存每个参数的当前生效值。

```javascript
// 规则 JS 中读取
const line = config.openParams.line ?? '';
```

```text
// 普通规则字符串中读取
@{openParams.line}
```

多选参数也会存成字符串，多个值使用英文逗号 `,` 分隔，例如 `男频,完结,免费`。如果需要数组，可以在 JS 中自行拆分：

```javascript
const tags = (config.openParams.tags ?? '').split(',').filter(Boolean);
```

本地 HTML 配置页在页面构建阶段还可以读取少量结构信息，用来渲染输入框、单选、多选等 UI：

| 写法 | 说明 |
| --- | --- |
| `@{openParams.line}` | 当前生效值 |
| `@{openParams.line.type}` | 参数类型，仅用于本地配置页结构渲染 |
| `@{openParams.line.options}` | 候选项数组，仅用于本地配置页结构渲染 |

注意：`config.openParams.line` 是字符串，不是对象。`type` / `options` 这种结构信息只在 `@html:` 本地页面预处理阶段开放，不会变成 JS 运行时里的 `config.openParams.line.type`。`effectiveValue`、`selectedValues`、`displayValue`、`separator` 等内部派生字段也不属于公开协议。

写回当前值有三种常用写法：

| 写法 | 适用场景 | 示例 |
| --- | --- | --- |
| `window.ParsingBook.setOpenParamValue(key, value)` | JS 函数内动态写回单个参数 | `window.ParsingBook.setOpenParamValue('line', line)` |
| `window.ParsingBook.setOpenParamValues(values)` | JS 函数内批量写回多个参数 | `window.ParsingBook.setOpenParamValues({ line: 'v1', source: '全部' })` |
| `@setOpenParams(key, valueExpression)` | 本地 HTML 事件属性中写回 | `<button onclick="@setOpenParams('line','v1')">保存</button>` |

`@setOpenParams(...)` 只在 HTML 事件属性里展开。脚本函数体里推荐直接调用 `window.ParsingBook.setOpenParamValue` 或 `setOpenParamValues`。

## loginUrl 本地 HTML 页规则速查

以下规则用于 `loginUrl` 的 `@html:` 本地登录/配置页。宿主在本地 HTML 加载前注入 `window.ParsingBook.setOpenParamValue` / `setOpenParamValues`，并只展开 HTML 事件属性中的 `@setOpenParams(...)`。

### 可用 config 参数

| 参数 | 写法 | 说明 | 注意事项 |
| --- | --- | --- | --- |
| `siteName` | `@{siteName}` | 当前书源名称 | 来自书源顶层 `siteName`。 |
| `siteIdent` | `@{siteIdent}` | 当前书源唯一标识 | 用于区分书源实例，不等同于设备 ID。 |
| `host` | `@{host}` | 当前书源主地址 | 本地 HTML 模式下也作为页面 `baseURL`。 |
| `deviceId` | `@{deviceId}` | App 设备标识 | 来自 App 内部稳定设备 ID，规则作者应避免上传到非必要第三方服务。 |
| `openParams.<key>` | `@{openParams.line}` | 开放参数当前生效值 | 用户保存值为空时使用 `defaultValue`。 |
| `openParams.<key>.type` | `@{openParams.line.type}` | 开放参数类型 | 仅用于本地配置页结构渲染，详见 [openParams 开放参数](#openparams)。 |
| `openParams.<key>.options` | `@{openParams.line.options}` | 开放参数候选项 | 返回候选项数组，元素包含 `name` 和 `value`。 |

### 本地 HTML 获取额外 Cookie 域名

`loginUrl` 使用 `@html:` 本地 HTML 时，也可以像普通登录 URL 一样在末尾追加 JSON 配置：

```
@html:<html>...</html>,{"domains":["qidian.com","example.com"]}
```

`domains` 用于额外收集指定域名范围内的内置浏览器 Cookie。建议填写裸域名或 host；URL 形式也会自动归一化为 host。

注意：配置 JSON 必须放在整个 `@html:` 内容的最后，并用英文逗号与 HTML 分隔。HTML 内容内部可以包含逗号，不会影响解析。只有尾随 JSON 包含 `domains` 或 `header` 字段时，才会被当作 URL 配置拆分。

### 配置页保存示例

`loginUrl` 本地 HTML 页可以按 [openParams 开放参数](#openparams) 中的方式读取和写回。常用写法如下：

```html
<input id="line" value="@{openParams.line}">
<button onclick='@setOpenParams("line", document.getElementById("line").value)'>保存</button>
```

在脚本函数里不要绕事件宏，直接调用 bridge：

```html
<script>
function saveLine(line) {
  window.ParsingBook.setOpenParamValue('line', line);
}
</script>
```
