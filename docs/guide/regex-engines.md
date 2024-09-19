---
outline: deep
---

# 正则表达式引擎

TextMate 语法基于正则表达式来匹配标记。通常，我们使用 [Oniguruma](https://github.com/kkos/oniguruma)（一个用 C 语言编写的正则表达式引擎）来解析语法。为了使其在 JavaScript 中运行，我们将 Oniguruma 编译为 WebAssembly，以便在浏览器或 Node.js 中运行。

自 v1.15 版本起，我们为用户提供了切换正则表达式引擎并提供自定义实现的功能。

在 `createHighlighter` 和 `createHighlighterCore` 中添加了 `engine` 选项。例如：

```ts
import { createHighlighter } from 'shiki'

const shiki = await createShiki({
  themes: ['nord'],
  langs: ['javascript'],
  engine: { /* 自定义引擎 */ }
})
```

Shiki 提供了两个内置引擎：

## Oniguruma 引擎

这是默认的引擎，使用编译后的 Oniguruma WebAssembly。它是最准确和最强大的引擎。

```ts
import { createHighlighter } from 'shiki'
import { createWasmOnigEngine } from 'shiki/engine/oniguruma'

const shiki = await createShiki({
  themes: ['nord'],
  langs: ['javascript'],
  engine: createWasmOnigEngine(import('shiki/wasm'))
})
```

## JavaScript 正则表达式引擎（实验性）

::: warning 实验性
此功能为实验性，可能会在不遵循语义化版本管理的情况下发生变化。
:::

这个实验性引擎使用 JavaScript 的原生正则表达式。由于 TextMate 语法的正则表达式是基于 Oniguruma 语法风格，可能包含 JavaScript 正则表达式不支持的语法，因此我们使用 [`oniguruma-to-js`](https://github.com/antfu/oniguruma-to-js) 来简化语法并尝试使其与 JavaScript 的正则表达式兼容。

```ts {2,4,9}
import { createHighlighter } from 'shiki'
import { createJavaScriptRegexEngine } from 'shiki/engine/javascript'

const jsEngine = createJavaScriptRegexEngine()

const shiki = await createHighlighter({
  themes: ['nord'],
  langs: ['javascript'],
  engine: jsEngine
})

const html = shiki.codeToHtml('const a = 1', { lang: 'javascript', theme: 'nord' })
```

请查看[兼容性表](/references/engine-js-compat)以了解您使用的语言的支持状态。

如果可以接受不匹配，并且您希望无论如何都能得到结果，您可以启用 `forgiving` 选项以抑制转换过程中发生的任何错误：

```ts
const jsEngine = createJavaScriptRegexEngine({ forgiving: true })
// ...使用该引擎
```

::: info
如果您在 Node.js 上运行 Shiki（或在构建时），我们仍然建议使用 Oniguruma 引擎以获得最佳效果，因为大多数情况下，包大小或 WebAssembly 支持并不是问题。

JavaScript 引擎更适合在某些情况下在浏览器中运行，特别是在您希望控制包大小时。
:::
