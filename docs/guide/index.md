---
outline: deep
---

# 简介

<br>

<span text-brand-yellow text-xl>Shiki</span><span op75>（式，一个日语词汇，意为 [“样式”](https://jisho.org/word/%E5%BC%8F)）</span> 是一款美观而强大的代码语法高亮器，它与 VS Code 的语法高亮引擎一样，基于 TextMate 的语法及主题。Shiki 能为几乎所有主流编程语言提供非常准确且快速的语法高亮。

你不需要维护自定义的正则表达式，不需要维护自定义的 CSS，也不需要维护自定义的 HTML；因为你在 VS Code 中使用的颜色主题一样可以用在 Shiki 上。

顺带一提，如你所想，本文档站点中的所有代码块都是由 Shiki 高亮的。

## 功能

- 所有语法 / 主题 / WASM 都是纯 ESM，可以按需懒加载，对捆绑器友好
- 高度通用，不依赖于 Node.js 的 API 和文件系统，可以在任何现代 JavaScript 运行时上运行
- 默认仅支持 ESM，不过你依然可以 [使用 CDN](/guide/install#cdn-usage) 或 [使用 CJS](/guide/install#cjs-usage)
- [语言与主题的细粒度捆绑](/guide/install#fine-grained-bundle)
- [深浅色模式支持](/guide/dual-themes)
- [`hast` 支持](/guide/transformers#codetohast)
- [转换器 API](/guide/transformers)
- [代码装饰 API](/guide/decorations)
- [TypeScript Twoslash 集成](/packages/twoslash)
- [兼容构建](/guide/compat)

## 演练场

这里为你提供了一个小演练场，可以试用 Shiki 来高亮你的代码。此演练场是在浏览器端渲染的，而本文档站点的其他代码块都是在构建时渲染并静态托管的，其相关主题和语言按需加载。

<ShikiMiniPlayground />

查看 [安装文档](/guide/install) 来在你的项目里使用 Shiki。

## 捆绑包大小

你可以在 [pkg-size.dev/shiki](https://pkg-size.dev/shiki) 上查看详细的捆绑包大小。

截止 `v1.1.6`，2024 年 2 月 22 日的数据如下：

| 捆绑包              | 大小（minified） | 大小（gzip） | 备注                                         |
| ------------------- | ---------------: | -----------: | -------------------------------------------- |
| `shiki`             |           6.9 MB |       1.3 MB | 所有主题和语言的异步块（chunks）             |
| `shiki/bundle/full` |           6.9 MB |       1.3 MB | 与 `shiki` 包相同                            |
| `shiki/bundle/web`  |           4.2 MB |       748 KB | 所有的主题和常用 Web 语言的异步块            |
| `shiki/core`        |           106 KB |        34 KB | 不带任何主题和语言的核心引擎，需要你自己构建 |
| `shiki/wasm`        |           623 KB |       231 KB | 以 BASE64 字符串形式内联的 WASM 二进制文件   |
