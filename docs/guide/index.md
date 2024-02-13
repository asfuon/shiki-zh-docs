---
outline: deep
---

# 简介

<br>

<span text-brand-yellow text-xl>Shiki</span><span op75>（式，一个日文词汇，意为 [“样式”](https://jisho.org/word/%E5%BC%8F)）</span> 是一款美观而强大的语法高亮器，它基于 TextMate 语法及主题，与 VS Code 的语法高亮引擎相同。它能为几乎所有主流编程语言提供非常准确且快速的语法高亮显示。

没有需要维护的自定义正则表达式，没有需要维护的自定义 CSS，也没有需要维护的自定义 HTML；你在 VS Code 中使用的主题一样可以用在 Shiki 上。

哦对了，如你所愿，本文档中的所有代码块都是由 Shiki 高亮的。

## 功能

- 所有语法 / 主题 / WASM 都以纯 ESM 的形式提供，按需懒加载且对捆绑器友好
- 通用便携，不依赖于 Node.js API 和文件系统，可以在任何现代 JavaScript 运行时上运行
- 默认仅支持 ESM，不过你依然可以 [使用 CDN](/guide/install#cdn-usage) 或 [使用 CJS](/guide/install#cjs-usage)
- [语言与主题的细粒度捆绑](/guide/install#fine-grained-bundle)
- [浅色与深色主题支持](/guide/dual-themes)
- [`hast` 支持](/guide/transformers#codetohast)
- [转换器 API](/guide/transformers)
- [代码装饰 API](/guide/decorations)
- [TypeScript Twoslash 集成](/packages/twoslash)
- [兼容构建](/guide/compat)

## 演练场

这里有一个小演练场供你尝试如何使用 Shiki 高亮显示你的代码。本文档的其他代码块都是在构建时渲染并静态托管的，而此演练场是在浏览器侧客户端上渲染的，其相关主题和语言按需加载。

<ShikiMiniPlayground />

在你的项目中 [安装 Shiki](/guide/install)。

## 谁在使用？

依赖 Shiki 的项目（按字母顺序排序）：

- [Astro](https://docs.astro.build/en/guides/markdown-content/#syntax-highlighting)
- [Expressive Code](https://expressive-code.com/)
- [JSX email](https://jsx.email/)
- [Lobe UI](https://github.com/lobehub/lobe-ui)
- [Nuxt Content](https://content.nuxt.com/usage/markdown#code-highlighting)
- [Slidev](https://sli.dev/custom/highlighters.html#highlighters)
- [VitePress](https://vitepress.dev/guide/markdown#syntax-highlighting-in-code-blocks)
- [Vocs](https://github.com/wevm/vocs)

## 捆绑包大小

你可以在 [pkg-size.dev/shiki](https://pkg-size.dev/shiki) 上查看详细的捆绑包大小。

截止 `v0.9.11`，2023 年 12 月 21 日的数据如下：

| 捆绑包              | 大小（minified） | 大小（gzip） | 备注                                             |
| ------------------- | ---------------: | -----------: | ------------------------------------------------ |
| `shiki`             |           6.4 MB |       1.2 MB | 所有主题和语言的异步块（chunks）                 |
| `shiki/bundle/full` |           6.4 MB |       1.2 MB | 与 `shiki` 包相同                                |
| `shiki/bundle/web`  |           3.8 MB |       695 KB | 所有主题和常用 Web 语言的异步块                  |
| `shiki/core`        |           100 KB |        31 KB | 不带任何主题和语言的核心引擎，需要你自己构建它们 |
| `shiki/wasm`        |           623 KB |       231 KB | 以 BASE64 字符串形式内联的 WASM 二进制文件       |
