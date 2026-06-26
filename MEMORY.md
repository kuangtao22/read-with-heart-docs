# MEMORY.md - Read With Heart Docs

## 项目信息
- 项目名：read-with-heart-docs
- 日期：2026-06-26
- 技术栈：MkDocs、Material for MkDocs、Markdown
- 项目类型：Read With Heart / ParsingBook 文档站

## 架构与约定
- 文档入口由 `mkdocs.yml` 的 `nav` 管理。
- 书源 JS API 主要维护在 `docs/native-to-js.md`。
- 文档默认使用中文，示例代码使用 JavaScript。

## 工作记录
- 2026-06-26：项目缺少 `AGENTS.md` 和 `MEMORY.md`，且 `/Users/xutaoyu/.codex/templates` 不存在，已根据目录结构和 README 创建最小可用项目说明与记忆文件。
- 2026-06-26：书源 JS API 教程位置为 `docs/native-to-js.md`；新增 SHA 哈希文档时应放在 MD5 附近，强调哈希不是加解密，返回小写十六进制字符串。
