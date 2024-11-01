# 同步使用

`await createHighlighter()` 和 `highlighter.codeToHtml()` 已经是将异步和同步部分分离的尝试。对于大多数情况，您应该能够在初始化阶段解决异步部分，并在之后同步地使用高亮器。

在某些极端情况下，如果您需要完全同步地运行 Shiki，从 v1.16 开始，我们提供了核心 API 的同步版本。您可以使用 `createHighlighterCoreSync` 同步地创建一个高亮器实例。

```ts
import { createHighlighterCoreSync } from 'shiki/core'
import { createJavaScriptRegexEngine } from 'shiki/engine/javascript'
import js from 'shiki/langs/javascript.mjs'
import nord from 'shiki/themes/nord.mjs'

const shiki = createHighlighterCoreSync({
  themes: [nord],
  langs: [js],
  engine: createJavaScriptRegexEngine()
})

const html = shiki.highlight('console.log(1)', { lang: 'js', theme: 'nord' })
```

在这样做时，要求所有的 `themes` 和 `langs` 作为普通对象提供。此外，还需要显式提供 `engine`。通过新的[实验性 JavaScript 正则表达式引擎](/guide/regex-engines#javascript-regexp-engine-experimental)，您也可以同步创建引擎实例。

[Oniguruma 引擎](/guide/regex-engines#oniguruma-engine)只能异步创建，因此您需要在创建同步高亮器之前解析引擎的 Promise。

```ts
import { createHighlighterCoreSync } from 'shiki/core'
import { createOnigurumaEngine } from 'shiki/engine/oniguruma'
import js from 'shiki/langs/javascript.mjs'
import nord from 'shiki/themes/nord.mjs'

// 在之前某个地方加载
const engine = await createOnigurumaEngine(import('shiki/wasm'))

const shiki = createHighlighterCoreSync({
  themes: [nord],
  langs: [js],
  engine, // 如果传入已解析的引擎，其他部分仍然可以同步执行。
})

const html = shiki.highlight('console.log(1)', { lang: 'js', theme: 'nord' })
```
