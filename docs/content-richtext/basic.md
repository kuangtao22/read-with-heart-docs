# 🎨 基础样式

> HTML 标签、CSS 样式表、完整示例与最佳实践的合集。自定义标签（`<img>` / `<comment>` / `<note>`）请见 [图片](image.md)、[段评](comment.md)、[批注](note.md) 专题页。

---

## 🚀 快速开始

### 支持的功能

✅ **HTML 标签**：`<b>`、`<i>`、`<u>`、`<span>`、`<h1>`-`<h6>`、`<p>`、`<br>` 等  
✅ **CSS 样式**：颜色、字体、大小、对齐、行高、间距等  
✅ **图片**：自动缩放、比例保持、占位符显示  
✅ **段评**：段落评论按钮、点击交互  
✅ **批注**：行间批注气泡，支持远程弹窗与本地批注  

### 正文格式

正文内容使用特定的**标记分隔符**格式：

```
---CATALOG---
章节标题
---CONTENT---
正文内容（支持 HTML、CSS、图片、段评等）
---CONTENT---
```

**完整示例：**

```
---CATALOG---
第一章 综合功能测试
---CONTENT---
<style>
.title {
  font-size: 1.3em;
  color: #333;
  font-weight: bold;
}
</style>

<h2 class="title">章节标题</h2>

<p>这是第一段纯文本。</p>

<p>这是使用 HTML 标签的<b>加粗文字</b>和<i>斜体文字</i>。</p>

<img src="https://example.com/image.jpg" width="400" height="300" />

<p>这是带段评的段落。<comment ident="para_001" count="5" /></p>
---CONTENT---
```

!!! note "格式说明"
    - `---CATALOG---` 标记后面跟章节标题
    - `---CONTENT---` 标记之间是正文内容
    - 正文内容可以包含多行，支持 HTML、CSS、图片等所有富文本功能
    - 系统会自动解析并转换为渲染所需的格式

---

## 📝 基础文本样式

### 1. HTML 标签

#### 文本格式

```html
<b>粗体文本</b>
<i>斜体文本</i>
<u>下划线文本</u>
<s>删除线文本</s>
```

**示例效果**：

- **粗体文本**
- *斜体文本*
- <u>下划线文本</u>
- ~~删除线文本~~

#### 标题

```html
<h1>一级标题</h1>
<h2>二级标题</h2>
<h3>三级标题</h3>
```

**样式说明**：

- `h1`：字号 1.5 倍，加粗
- `h2`：字号 1.3 倍，加粗
- `h3`：字号 1.1 倍，加粗

#### 段落与换行

```html
<p>这是一个段落</p>
<br/>  <!-- 换行 -->
```

#### 行内样式

```html
<span style="color:#ff0000;">红色文字</span>
<span style="background-color:#ffff00;">黄色背景</span>
<span style="font-size:1.2em;">放大文字</span>
```

---

## 🎨 CSS 样式表

### 1. 基础用法

#### 定义样式

```html
<style>
.red-text {
  color: #ff0000;
  font-size: 1.1em;
  font-weight: bold;
}
</style>
<p class="red-text">这段文字是红色的</p>
```

#### 颜色设置

支持多种颜色格式：

```css
/* 十六进制 */
color: #ff0000;        /* 红色 */
color: #333;           /* 深灰色（简写）*/

/* RGB */
color: rgb(255, 0, 0);

/* RGBA（带透明度）*/
color: rgba(255, 0, 0, 0.8);

/* 颜色名称 */
color: red;
color: blue;
```

#### 字体设置

```css
/* 字体大小 */
font-size: 1.2em;      /* 相对于默认字号 */
font-size: 18px;       /* 绝对大小（不推荐）*/

/* 字体粗细 */
font-weight: bold;     /* 粗体 */
font-weight: normal;   /* 正常 */

/* 字体样式 */
font-style: italic;    /* 斜体 */
font-style: normal;    /* 正常 */

/* 自定义字体 */
font-family: "SourceHanSB";   /* 使用本地字体 */
font-family: "LXGWZhenKaiGB-Regular";  /* 使用自定义导入字体 */
```

**内置字体**：

- `SourceHanSB` - 思源黑体 SemiBold
- `FZGWKTGBK` - 方正古文康体

**自定义字体使用**：

!!! note "字体导入说明"
    字体必须预先导入到 APP 的 `bookFont` 文件夹中才能使用。

**导入步骤：**

1. **进入字体管理**
   - 打开 APP
   - 进入设置 → 字体管理

2. **导入字体文件**
   - 点击"导入字体"按钮
   - 选择 `.ttf` 或 `.otf` 字体文件
   - 系统会自动将字体保存到 `bookFont` 文件夹

3. **使用导入的字体**
   - 字体导入后会显示字体名称
   - 在 CSS 中使用时，**字体名称必须与导入时显示的名称完全一致**

**示例：**

```css
/* 使用自定义字体 */
.custom-font {
  font-family: "LXGWZhenKaiGB-Regular";  /* 必须与导入的字体名称一致 */
  font-size: 1.2em;
  color: #2c3e50;
}
```

```html
<p class="custom-font">使用自定义字体的文字</p>
```

**注意事项：**

- ⚠️ 字体名称区分大小写，必须完全匹配
- ⚠️ 字体名称需要用引号包裹
- ⚠️ 如果字体未导入或名称不匹配，会使用系统默认字体
- ✅ 建议在字体管理中查看已导入字体的准确名称

#### 文本对齐

```css
text-align: left;      /* 左对齐（默认）*/
text-align: center;    /* 居中 */
text-align: right;     /* 右对齐 */
text-align: justify;   /* 两端对齐 */
```

#### 行高与间距

```css
/* 行高 */
line-height: 1.8;      /* 1.8 倍行高 */
line-height: 2.0;      /* 2.0 倍行高 */

/* 首行缩进 */
text-indent: 2em;      /* 缩进 2 字符 */
text-indent: 0em;      /* 不缩进 */

/* 段落间距 */
margin: 2em 0em;       /* 上下 2em */
margin: 1em 0em 2em 0em;  /* 上 1em，下 2em */
```

#### 背景色

```css
background-color: #faebd7;   /* 米色背景 */
background-color: #f0f0f0;   /* 浅灰背景 */
```

### 2. 常见样式类

#### 古风标题

```html
<style>
.ancient-title {
  font-family: "FZGWKTGBK";
  font-size: 1.5em;
  color: #8b4513;
  text-align: center;
  line-height: 2.2;
  margin: 5em 0em 3em 0em;
  text-indent: 0em;
}
</style>

<h2 class="ancient-title">第一回<br/>宴桃园豪杰三结义</h2>
```

**效果**：

- 使用古文康体字体
- 1.5 倍字号，棕色
- 居中对齐，上下留白大

#### 现代标题

```html
<style>
.modern-title {
  font-family: "SourceHanSB";
  font-size: 1.4em;
  color: #1976d2;
  font-weight: bold;
  text-align: left;
  line-height: 1.8;
  margin: 3em 0em 2em 0em;
  text-indent: 0em;
}
</style>

<h3 class="modern-title">第二回：桃园初遇</h3>
```

**效果**：

- 使用思源黑体，加粗
- 蓝色，左对齐
- 适中的留白

#### 诗词引用

```html
<style>
.poem {
  font-family: "SourceHanSB";
  font-size: 1.1em;
  color: #2f4f4f;
  text-align: center;
  line-height: 2.0;
  margin: 2em 0em;
  background-color: #faebd7;
  text-indent: 0em;
}
</style>

<p class="poem">
床前明月光<br/>
疑是地上霜<br/>
举头望明月<br/>
低头思故乡
</p>
```

**效果**：

- 居中显示
- 米色背景
- 行高 2.0，更有诗意

#### 对话样式

```html
<style>
.dialogue {
  margin: 1em 0em 1em 2em;
  font-size: 1.02em;
  line-height: 1.9;
  text-indent: 0em;
}

.speaker {
  color: #3498db;
  font-weight: bold;
}
</style>

<p class="dialogue">
  <span class="speaker">张飞：</span>
  "大丈夫不能为国出力，更待何时！"
</p>
```

**效果**：

- 说话人名字蓝色加粗
- 对话内容左缩进
- 不首行缩进

#### 旁白/动作

```html
<style>
.action {
  color: #95a5a6;
  font-size: 0.95em;
}
</style>

<p class="action">（刘备回头，只见一条壮汉...）</p>
```

**效果**：

- 灰色显示
- 字号略小
- 与正文区分

#### 引用文字

```html
<style>
.quote {
  font-size: 1.05em;
  color: #7f8c8d;
  text-align: center;
  line-height: 1.9;
  margin: 2em 0em;
}
</style>

<p class="quote">"滚滚长江东逝水，浪花淘尽英雄。"</p>
```

**效果**：

- 居中，灰色
- 字号略大
- 上下留白

---

## 📖 完整示例

### 示例 1：古典小说章节

```json
[
  "<style>",
  ".chapter-title {",
  "  font-family: \"SourceHanSB\";",
  "  font-size: 1.3em;",
  "  color: #2c3e50;",
  "  text-align: center;",
  "  line-height: 2.0;",
  "  margin: 4em 0em 2.5em 0em;",
  "  font-weight: bold;",
  "}",
  ".chapter-num {",
  "  font-family: \"FZGWKTGBK\";",
  "  color: #e74c3c;",
  "  font-size: 0.9em;",
  "}",
  ".quote {",
  "  font-size: 1.05em;",
  "  color: #7f8c8d;",
  "  text-align: center;",
  "  line-height: 1.9;",
  "  margin: 2em 0em;",
  "}",
  "</style>",
  
  "<h2 class=\"chapter-title\">",
  "  <span class=\"chapter-num\">第一回</span><br/>",
  "  宴桃园豪杰三结义<br/>",
  "  斩黄巾英雄首立功",
  "</h2>",
  
  "<p class=\"quote\">",
  "\"滚滚长江东逝水，浪花淘尽英雄。\"<comment ident=\"para_001\" count=\"23\" />",
  "</p>",
  
  "话说天下大势，分久必合，合久必分。<comment ident=\"para_002\" count=\"15\" />",
  
  "周末七国分争，并入于秦。及秦灭之后，楚、汉分争，又并入于汉。<comment ident=\"para_003\" count=\"8\" />"
]
```

**效果展示**：

1. 标题：居中，使用古文字体，红色回数
2. 引文：居中，灰色，带段评
3. 正文：默认样式，每段可独立段评

### 示例 2：现代网文

```json
[
  "<style>",
  ".modern-title {",
  "  font-size: 1.4em;",
  "  color: #1976d2;",
  "  font-weight: bold;",
  "  text-align: left;",
  "  margin: 3em 0em 2em 0em;",
  "}",
  ".dialogue {",
  "  margin: 1em 0em 1em 2em;",
  "  font-size: 1.02em;",
  "}",
  ".speaker {",
  "  color: #3498db;",
  "  font-weight: bold;",
  "}",
  ".action {",
  "  color: #95a5a6;",
  "  font-size: 0.95em;",
  "}",
  "</style>",
  
  "<h3 class=\"modern-title\">第二回：桃园初遇</h3>",
  
  "<p class=\"dialogue\">",
  "  <span class=\"speaker\">张飞：</span>",
  "  \"大丈夫不能为国出力，更待何时！\"",
  "  <comment ident=\"para_dlg_001\" count=\"45\" />",
  "</p>",
  
  "<p class=\"action\">",
  "（刘备回头，只见一条壮汉，豹头环眼...）",
  "<comment ident=\"para_act_001\" count=\"67\" />",
  "</p>",
  
  "<img src=\"https://cdn.example.com/character_zhangfei.jpg\" width=\"600\" height=\"800\" />",
  
  "<p class=\"dialogue\">",
  "  <span class=\"speaker\">刘备：</span>",
  "  \"壮士真英雄也！在下刘备，敢问壮士尊姓大名？\"",
  "  <comment ident=\"para_dlg_002\" count=\"88\" />",
  "</p>"
]
```

**效果展示**：

1. 标题：左对齐，蓝色，现代感
2. 对话：说话人蓝色加粗，左缩进
3. 旁白：灰色，字号略小
4. 图片：人物插图
5. 每段都有热门段评

### 示例 3：诗词赏析

```json
[
  "<style>",
  ".poem-title {",
  "  font-family: \"FZGWKTGBK\";",
  "  font-size: 1.5em;",
  "  color: #8b4513;",
  "  text-align: center;",
  "  margin: 5em 0em 3em 0em;",
  "}",
  ".poem-content {",
  "  font-family: \"SourceHanSB\";",
  "  font-size: 1.1em;",
  "  color: #2f4f4f;",
  "  text-align: center;",
  "  line-height: 2.0;",
  "  margin: 2em 0em;",
  "  background-color: #faebd7;",
  "}",
  ".annotation {",
  "  color: #7f8c8d;",
  "  font-size: 0.95em;",
  "  line-height: 1.8;",
  "}",
  "</style>",
  
  "<h2 class=\"poem-title\">静夜思<br/>【唐】李白</h2>",
  
  "<p class=\"poem-content\">",
  "床前明月光<br/>",
  "疑是地上霜<br/>",
  "举头望明月<br/>",
  "低头思故乡",
  "<comment ident=\"poem_001\" count=\"156\" />",
  "</p>",
  
  "<img src=\"https://cdn.example.com/moon.jpg\" width=\"800\" height=\"600\" />",
  
  "<p class=\"annotation\">",
  "【赏析】此诗写的是在寂静的月夜思念家乡的感受。诗人运用比喻、衬托等手法，表达客居思乡之情。",
  "<comment ident=\"anno_001\" count=\"234\" />",
  "</p>"
]
```

**效果展示**：

1. 标题：古文字体，棕色，居中
2. 诗文：米色背景，居中，行高 2.0
3. 插图：月夜意境图
4. 赏析：灰色，较小字号
5. 热门段评

---

## 💡 最佳实践

### 1. 样式设计原则

#### ✅ 推荐做法

**统一风格**

```css
/* 全书使用统一的样式类 */
.chapter-title { ... }
.dialogue { ... }
.action { ... }
```

**适当留白**

```css
/* 标题上下留白 */
margin: 3em 0em 2em 0em;

/* 段落间距 */
margin: 1em 0em;
```

**合理字号**

```css
/* 使用相对大小 */
font-size: 1.2em;  /* ✅ 相对于默认字号 */

/* 避免绝对大小 */
font-size: 18px;   /* ❌ 不随用户设置变化 */
```

**颜色对比**

```css
/* 确保文字可读 */
color: #333333;  /* ✅ 深灰色，易读 */
color: #999999;  /* ⚠️ 浅灰色，仅用于次要信息 */
color: #dddddd;  /* ❌ 太浅，难以阅读 */
```

**首行缩进**

```css
/* 正文段落首行缩进 */
text-indent: 2em;

/* 标题、对话不缩进 */
.chapter-title {
  text-indent: 0em;
}
```

#### ❌ 避免的做法

**过度装饰**

```css
/* ❌ 不要过度使用样式 */
.bad-example {
  font-size: 2em;
  color: #ff00ff;
  background-color: #00ffff;
  text-decoration: underline;
}
```

**样式冲突**

```html
<!-- ❌ 避免行内样式覆盖CSS -->
<p class="red-text" style="color: blue;">冲突的颜色</p>
```

**忽略性能**

```text
---CATALOG---
章节
---CONTENT---
<style>...(重复的CSS)</style>
第一段
<style>...(重复的CSS)</style>
第二段
---CONTENT---
```

**正确做法**：CSS 只在开头定义一次

### 2. 图片使用建议

#### 图片规格

- **封面**：600x900 (2:3)
- **插图**：800x600 (4:3)
- **横幅**：1000x562 (16:9)
- **头像**：200x200 (1:1)

#### 图片优化

**使用图床**

```html
<!-- ✅ 使用CDN -->
<img src="https://cdn.example.com/image.jpg" />

<!-- ❌ 不要使用本地路径 -->
<img src="/Users/xxx/image.jpg" />
```

**合理尺寸**

```html
<!-- ✅ 指定宽高 -->
<img src="url" width="600" height="800" />

<!-- ⚠️ 超大图会自动缩放 -->
<img src="url" width="2000" height="3000" />
```

**占位符**

- 加载中：灰色占位图
- 加载失败：灰色框 + 图标

### 3. 段评使用建议

#### 合理密度

```text
✅ 适中：每 3-5 段一个段评
⚠️ 过密：每段都有段评（干扰阅读）
⚠️ 过疏：整章只有 1-2 个段评
```

#### 热门段评

```html
<!-- 重点段落 -->
<p>精彩内容...<comment ident="hot_001" count="156" /></p>

<!-- 普通段落 -->
<p>普通内容...<comment ident="normal_001" count="12" /></p>
```

**视觉效果**：

- 评论数越多，越吸引读者注意
- 形成讨论热点

#### 段落标识

```html
<!-- ✅ 使用有意义的ID -->
<comment ident="chapter1_para_001" count="5" />
<comment ident="dlg_zhangfei_001" count="12" />

<!-- ❌ 无意义的ID -->
<comment ident="aaa" count="5" />
<comment ident="123" count="12" />
```

### 4. 性能优化

#### CSS 缓存

```json
[
  "<style>/* 全章CSS，只定义一次 */</style>",
  "第一段",
  "第二段",
  "..."
]
```

**说明**：

- CSS 会自动缓存
- 重复定义会浪费解析时间

#### HTML 缓存

- ✅ 自动缓存：相同的HTML内容只解析一次
- ✅ 颜色缓存：相同的颜色值只计算一次
- ✅ 字体缓存：字体样式自动复用

**无需手动处理**，系统已优化！

#### 长文本优化

- 适中长度的段落（推荐 200-500 字）
- 避免单段过长（超过 2000 字）
- 使用 `<br/>` 或 `<p>` 分段

---

## 🎯 快速上手模板

### 模板 1：简洁现代风格

```text
---CATALOG---
章节标题
---CONTENT---
<style>
.title {
  font-size: 1.3em;
  color: #333;
  font-weight: bold;
  text-align: center;
  margin: 3em 0em 2em 0em;
}
</style>

<h2 class="title">章节标题</h2>

<p>正文第一段...<comment ident="p1" count="5" /></p>

<p>正文第二段...<comment ident="p2" count="8" /></p>

<img src="https://example.com/img.jpg" width="600" height="400" />

<p>正文第三段...<comment ident="p3" count="12" /></p>
---CONTENT---
```

### 模板 2：古风典雅风格

```text
---CATALOG---
第一回 标题
---CONTENT---
<style>
.ancient {
  font-family: "FZGWKTGBK";
  font-size: 1.4em;
  color: #8b4513;
  text-align: center;
  margin: 5em 0em 3em 0em;
}
.poem {
  text-align: center;
  line-height: 2.0;
  background-color: #faebd7;
  margin: 2em 0em;
}
</style>

<h2 class="ancient">第一回<br/>标题</h2>

<p class="poem">
诗句第一行<br/>
诗句第二行<comment ident="poem1" count="23" />
</p>

<p>正文内容...<comment ident="p1" count="15" /></p>
---CONTENT---
```

### 模板 3：对话为主风格

```text
---CATALOG---
对话章节
---CONTENT---
<style>
.dialogue {
  margin: 1em 0em 1em 2em;
}
.speaker {
  color: #3498db;
  font-weight: bold;
}
.action {
  color: #95a5a6;
  font-size: 0.95em;
}
</style>

<p class="dialogue">
  <span class="speaker">角色A：</span>
  "对话内容..."<comment ident="dlg1" count="45" />
</p>

<p class="action">（旁白动作描述...）<comment ident="act1" count="12" /></p>

<p class="dialogue">
  <span class="speaker">角色B：</span>
  "回应内容..."<comment ident="dlg2" count="67" />
</p>
---CONTENT---
```

---

## ✨ 总结

恭喜你！现在你已经掌握了：

✅ **HTML 标签** - 粗体、斜体、标题等  
✅ **CSS 样式** - 颜色、字体、布局等  
✅ **[图片插入](image.md)** - 自动缩放、缓存优化  
✅ **[段评功能](comment.md)** - 互动评论、热门标记  
✅ **[批注气泡](note.md)** - 行间批注、远程弹窗  
✅ **性能优化** - 自动缓存、快速渲染  

**开始创作你的精美正文吧！** 🎨

---

!!! tip "提示"
    - 从简单的模板开始
    - 逐步添加样式和功能
    - 参考上面的完整示例和模板
    - 自定义标签的详细规则见各专题页

📅 **更新日期**：2025-10-12  
📝 **版本**：v1.0  
