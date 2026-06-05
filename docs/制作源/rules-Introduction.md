# 规则

## 参数说明

### 系统自带get参数
| 参数名称          | 说明        | 使用场景           | 用例                    |
|:--------------|-----------|:---------------|:----------------------|
| keyword       | 关键词       | 书籍搜索           | `@get{keyword}`       |
| preResHeader1 | 前置请求标头    | 全局可用           | `@get{preResHeader1.xxx}` |
| preRepHeader1 | 前置响应标头    | 全局可用           | `@get{preRepHeader1.xxx}` |
| resHeader     | 请求标头      | 全局可用           | `@get{resHeader.xxx}` |
| repHeader     | 响应标头      | 全局可用           | `@get{repHeader.xxx}` |
| loginRequest  | 登录请求头     | 全局可用           | `@get{loginRequest}`  |
| loginResponse | 登录响应头     | 全局可用           | `@get{loginResponse}` |
| loginCookies  | 登录cookies | 全局可用           | `@get{loginCookies}`  |
| verifyHeader  | 浏览器过盾请求头  | 正文请求           | `@get{verifyHeader}`  |
| verifyCookies | 浏览器过盾cookies | 正文请求        | `@get{verifyCookies}` |
| bookUrl       | 书籍地址      | 章节列表、正文使用      | `@get{bookUrl}`       |
| chapterUrl    | 章节地址      | 正文使用           | `@get{chapterUrl}`    |
| host          | 主地址       | 全局可用           | `@get{host}`          |

**说明：**
- 前置响应标头使用数字标记，用于保存所有的前置请求（如 preResHeader1、preResHeader2...）
- `verifyHeader` 和 `verifyCookies`：正文请求如果有浏览器过盾，会自动加入过盾请求头和cookies，如果为空则会弹出过盾弹窗
- `loginRequest`、`loginResponse`、`loginCookies`：点击登录后会自动保存，可在书源中获取使用

### 系统内置JS函数

除了 App 对象提供的方法外，系统还内置了一些常用的 JavaScript 函数：

| 函数名称 | 说明 | 用例 |
|:---|:---|:---|
| `console.log()` | 打印调试信息 | `console.log('你好世界')` |
| `console.log([])` | 打印数组 | `console.log(['你好', '世界'])` |
| `unicode.decode()` | Unicode 解码 | `let string = "\\u006b"`<br/>`unicode.decode(string)` |

**说明：**
- `console.log()` 等同于 `App.log()`，可以互换使用
- `unicode.decode()` 等同于 `App.unicode.decode()`
- 更多 Native to Javascript 方法请参考[Native to Javascript](../native-to-js.md)文档

### 公共内置请求参数
| 参数名称           | 说明        | 用例                      | 值类型示例                      |
|:---------------|-----------|:------------------------|:---------------------------|
| params         | 请求参数      | `config.params`         |                            |
| engine         | 源引擎类型     | `config.engine`         | xpath、jsonpath             |
| method         | 请求类型      | `config.method`         | POST，GET                   |
| host           | 站点地址      | `config.host`           |                            |
| header         | 请求头       | `config.header`         | 优先级别：正式请求头>浏览器过盾请求头>公共请求头  |
| mode           | 请求模式      | `config.mode`           | http、webview               |
| requestEncode  | 请求编码方式    | `config.requestEncode`  | utf-8、gbk                  |
| responseEncode | 响应编码方式    | `config.responseEncode` | utf-8、gbk                  |
| cookies        | cookies信息 | `config.cookies`        |                            |
| openParams     | 开放参数，可自定义 | `config.openParams`     |                            |
| verifyCode     | 验证码       | `config.verifyCode`     |                            |

### 搜索规则

| 参数名称 | 说明 | 用例               |
|:---|:---|:-----------------|
| keyword | 搜索关键词 | `config.keyword` |
| url | 搜索地址 | `config.url`     |

### 详情规则

### 章节列表规则

| 参数名称   | 说明                             | 用例                                     |
|:---|:---|:---------------------------------------|
| bookUrl   | 章节列表url                       | `config.bookUrl`                       |
| infoUrl   | 书籍信息url                       | 如果书籍详情有规则，会先调用`infoUrl`请求，并获取`bookUrl` |
| bookName  | 书籍名称                         | `config.bookName`                      |
| bookAuthor| 作者                             | `config.bookAuthor`                    |
| url       | 章节列表请求地址                   | `config.url`                           |
| pageIndex | 页码                             | `config.pageIndex`                     |
| pageStart | 起始章节页数（老版本参数不建议使用） | 用于章节规则拼接，如第二章起，标记2                     |

### 正文规则

| 参数名称   | 说明               | 用例                   |
|:---|:---|:---------------------|
| bookUrl   | 章节列表Url         | `config.bookUrl`     |
| bookName  | 书籍名称           | `config.bookName`    |
| bookAuthor| 作者               | `config.bookAuthor`  |
| chapterUrl| 章节详情Url         | `config.chapterUrl`  |
| infoUrl   | 书籍详情Url         | `config.infoUrl`     |
| chapterName| 章节名称         | `config.chapterName` |
| url       | 正文请求地址        | `config.url`         |
| pageIndex | 页码               | `config.pageIndex`   |
| pageStart | 起始章节页数（老版本参数不建议使用） | 用于正文规则拼接，如第二章起，标记2   |
| playUrl   | 如果`playUrl`规则为空，自动获取正文url和正文header | `config.playUrl`     |

### 发现规则

| 参数名称 | 说明             | 用例                 |
|:---|:---|:-------------------|
| url     | 发现请求地址      | `config.url`       |
| pageIndex | 章节页数序列      | `config.pageIndex` |

### 表达式

APP内置了各种常用的表达式，可以进行替换获取和处理操作。

| 参数 | 名称 | 示例 | 说明 |
|:---|:---|:---|:---|
| `${}` | 获取参数 | `${key}` | 支持获取json的子集`${info.name}` |
| `@{}` | 获取参数 | `@{key}` | 等同于`${}` |
| `{{}}` | 运行规则 | `{{}}` |可以运行xpath、jsonpath的规则 |
| `@get{}` | 获取put信息 | `@get{name}` | 可以获取`@put{}`的数据 |
| `@put{}` | 保存信息 | `@put{name, '//*[@src]'}` | 目前只支持前置请求的put |
| `<js></js>` | 运行JS语言 | `<js>App.log('hello!');</js>` | 在标签内可以运行js的语言 |
| `@js:` | JS语言处理 | @js:<br/>let name = '张三';<br/>App.log(name); | 在规则中第一行添加`@js:`表示该规则使用js运行 |
| `@all` | 获取网页所有内容 | `@all` | 不使用任何规则，直接获取网页原文 |
| `##正则表达式#替换字符` | 正则替换 | `##a#b` | 替换一次 |
| `##正则表达式##替换字符` | 正则替换 | `##a##b` | 替换所有 |
| `##正则表达式` | 正则替换 | `##a` | 过滤所有 |
| `##正则表达式#$1` | 正则替换 | `##(*.?)#$1` | 取值，如果有子串，可按照$1\$2\$3...直接取值 |

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

### loginUrl 本地 HTML 页 openParams 写法速查

以下规则用于 `loginUrl` 的 `@html:` 本地登录/配置页。写回能力以 ParsingBook 主项目当前实现为准：宿主在本地 HTML 加载前注入 `window.ParsingBook.setOpenParamValue` / `setOpenParamValues`，并只展开 HTML 事件属性中的 `@setOpenParams(...)`。

| 写法 | 是否可用 | 适用场景 | 示例 | 注意事项 |
|:---|:---|:---|:---|:---|
| `@{openParams.key}` | 可用 | 加载前读取当前生效值 | `@{openParams.line}` | 由规则/HTML 预处理替换；页面运行后不会自动重新求值。 |
| `@{openParams.key.type}` / `@{openParams.key.options}` | 可用 | 加载前读取开放参数结构信息 | `@{openParams.line.options}` | 只暴露最小结构信息；不要依赖 `value`、`defaultValue`、`displayValue` 等内部字段。 |
| `onclick="@setOpenParams('key','value')"` | 可用 | HTML 事件属性中写回固定值 | `<button onclick="@setOpenParams('line','v1')">保存</button>` | 当前宏展开器只处理事件属性中的 `@setOpenParams(...)`。 |
| `onclick='@setOpenParams("key", document.getElementById("x").value)'` | 可用 | HTML 事件属性中执行 DOM 表达式后写回 | `<button onclick='@setOpenParams("line", document.getElementById("line").value)'>保存</button>` | Swift 不解析表达式；浏览器执行后 Native 接收最终值。 |
| `window.ParsingBook.setOpenParamValue(k, value)` | 可用，推荐 | JS 函数内使用运行时变量动态写回 | `window.ParsingBook.setOpenParamValue(k, value)` | 脚本函数里应优先直接调用 bridge，不要绕宏。 |
| `window.ParsingBook.setOpenParamValues({ a: '1', b: '2' })` | 可用 | JS 运行时批量写回多个参数 | `window.ParsingBook.setOpenParamValues({ line: 'v1', source: '全部' })` | 内部逐个调用 `setOpenParamValue`。 |
| 宿主注入别名后 `setOpenParams(k, v)` | 可用 | 给规则作者提供更短、更自然的 JS API | `setOpenParams(k, v)` | 需要宿主先注入 `window.setOpenParams = function(key, value) { window.ParsingBook.setOpenParamValue(key, value); }`。 |
| `` `@{setOpenParams('${k}', '${v}')}` `` | 不可用 | 不应使用 | `` `@{setOpenParams('${k}', '${v}')}` `` | JS 模板字符串运行在页面加载后，不会触发 Swift 宏重新扫描；`${k}` / `${v}` 不会动态映射。 |
| `<script>@setOpenParams('key','value')</script>` | 当前不支持/不推荐 | 不应作为写回方式 | `<script>@setOpenParams('line','v1')</script>` | 现有宏展开器只处理事件属性，不处理 script 中裸宏。 |
| 普通 HTML 文本节点里的 `@setOpenParams(...)` | 不可用/不推荐 | 不应使用 | `<div>@setOpenParams('line','v1')</div>` | 不是事件执行上下文，只会成为文本或被错误展示。 |
