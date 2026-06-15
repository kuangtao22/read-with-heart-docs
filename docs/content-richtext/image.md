# 🖼️ 图片

`<img>` 标签用于在正文中插入图片，支持网络图、本地路径、Base64、SVG 等多种格式，并可选 `ident` 属性让图片可点击跳转 WebView。

---

## 1. 基本语法

```html
<img src="图片URL" width="宽度" height="高度" />
```

!!! warning "注意"
    图片标签必须是**自闭合**的（以 `/>` 结尾）

## 2. 属性规则

| 属性 | 是否必填 | 说明 |
|------|----------|------|
| `src` | 是 | 图片地址。为空时不会创建图片占位。 |
| `width` | 推荐 | 图片显示宽度，支持纯数字、`px`、小数和百分比。 |
| `height` | 推荐 | 图片显示高度，支持纯数字、`px`、小数和百分比。 |
| `ident` | 否 | 点击图片时打开的 WebView 地址。为空时图片只展示，不拦截翻页点击。 |
| `alt` | 否 | 图片语义说明，当前阅读绘制链路不使用。 |
| `style` | 否 | 支持 `width` / `height`，如 `style="width:300px;height:40px"`。 |

!!! tip "分页稳定性"
    建议同时填写 `width` 和 `height`。明确同时填写时，App 会把它们作为最终显示尺寸，只在超过阅读区域时等比缩小，不会自动放大。

## 3. 示例

### 指定尺寸

```html
<img src="https://example.com/cover.jpg" width="400" height="600" />
```

**说明**：

- 图片会按指定尺寸显示
- 如果超出屏幕宽度，会自动缩放

### 不推荐仅指定宽度

```html
<img src="https://example.com/banner.jpg" width="800" />
```

**说明**：

- 网络图片或网络 SVG 首次分页时，App 通常无法同步获取真实高度
- 只写 `width` 或 `height` 时会进入现有尺寸兜底逻辑，可能不是服务端期望比例
- 为避免加载后高度变化影响分页，正文规则应尽量同时输出 `width` 和 `height`

### 网络图片

```html
<img src="https://cdn.example.com/image.jpg" width="600" height="400" />
```

**说明**：

- 自动使用 Kingfisher 缓存
- 加载时显示占位符
- 失败时显示灰色占位图

### SVG 图片

```html
<img
  src="https://example.com/comment/card?bookId=1&chapterId=2&svg=1&img=1"
  width="300"
  height="40" />
```

**说明**：

- SVG 继续使用 `<img>` 标签，不支持正文内联 `<svg>...</svg>`
- 支持 `.svg` 地址、`data:image/svg+xml`、URL query 中包含 `svg=1` 的接口图片
- 网络响应 MIME 为 `image/svg+xml` 或数据内容像 SVG XML 时，也会按 SVG 渲染
- SVG 会渲染为静态图片，不支持动画和脚本
- 网络 SVG 必须显式写 `width` 和 `height`，避免影响分页

### 可点击图片

```html
<img
  ident="https://example.com/chapterComments?bookId=1&chapterId=2"
  src="https://example.com/chapterEndComments?bookId=1&chapterId=2&page=1&svg=1&img=1"
  alt="本章讨论"
  width="300"
  height="40" />
```

**说明**：

- `ident` 非空时，点击图片会打开对应 WebView
- `ident` 应是可解析的完整 URL
- 点击带 `ident` 的图片后，不再继续执行翻页点击

## 4. 携带自定义 HTTP Header

`src` 和 `ident` 都支持在 URL 后面追加 `,{"header":{...}}` 格式注入自定义 HTTP Header：

```html
<img
  ident="https://api.example.com/comments,{'header':{'cookie':'sid=abc','x-test':'tag'}}"
  src="https://api.example.com/card.png,{'header':{'x-trace':'123'}}"
  width="300"
  height="40" />
```

**格式说明**：

- `URL` 后面跟 `,` + JSON
- JSON 顶层只识别 `header: {String:String}` 字段
- 支持**单引号**写 JSON（避免和 HTML 双引号冲突），如 `{'header':{...}}`
- 解析后 URL 原样保留，header 注入到对应请求中
- URL 自身含 `,` 时仍能正确解析
- 老格式（不含 `,` 后跟 JSON）保持原行为

**注入目标**：

- `src` 的 header 注入到**图片下载请求**
- `ident` 的 header 注入到**点击图片后打开的 WebView 请求**
- 两者**独立**，互不影响

!!! warning "Cookie 头说明"
    iOS 系统中 `Cookie` 头受 `HTTPCookieStorage` 接管，可能被忽略或合并。建议先用自定义头（如 `x-test`）验证链路。该格式约束与 [`<comment>`](comment.md#url_1) / [`<note>`](note.md) 完全一致。

---

## 5. 图片位置

### 独立段落

```json
[
  "第一段文字",
  "<img src=\"url\" width=\"400\" height=\"300\" />",
  "第二段文字"
]
```

**效果**：图片单独成行

### 段内插入

```html
<p>
  这是一段文字
  <img src="url" width="200" height="150" />
  继续文字
</p>
```

**效果**：图片嵌入在段落中

## 6. 图片尺寸建议

| 场景 | 推荐宽度 | 说明 |
|------|---------|------|
| **封面图** | 600-800px | 竖版，保持 2:3 比例 |
| **插图** | 400-600px | 横竖均可 |
| **小图标** | 100-200px | 装饰性图片 |
| **全屏横图** | 800-1000px | 横版，保持 16:9 |

!!! tip "性能提示"
    - 图片会自动缓存，无需担心重复加载
    - 超大图片会自动按屏幕宽度缩放
    - 网络 SVG 和带 `ident` 的入口图属于正文交互规则，必须提供稳定宽高
    - 建议使用 CDN 或图床服务
