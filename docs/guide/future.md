# 未来规划

我们计划在未来的主要版本中移除一些已废弃的 API 并优化代码除屑优化（tree-shaking）。
具体计划如下：

- 👉 `v1.x`：仍支持已废弃的 API，仅在类型层面标记。可选择性开启运行时警告。
- `v2.0`：无破坏性更改，但默认开启运行时废弃警告。
- `v3.0`：移除已废弃的 API，包含破坏性更改。

在当前版本（从 v1.19.0 起），你可以通过在应用程序开始时调用 `enableDeprecationWarnings()` 来选择开启运行时警告。

```ts
import { enableDeprecationWarnings, getHighlighter } from 'shiki'
enableDeprecationWarnings() // [!code hl]
// 调用已废弃的用法将会显示警告：
// [SHIKI DEPRECATED]: Use `createHighlighter` instead
const shiki = await getHighlighter(/* ... */)
```

这将帮助你更好地为未来的变更做准备，实现平稳升级。

## 重要的废弃项

### `getHighlighter` -> `createHighlighter`

这不涉及功能变更，主要是为了避免混淆而进行的命名修正。这应该是一个简单的查找替换操作。

### WASM 相关 API

自 v0.16 引入[引擎系统](/guide/regex-engines)以来，WebAssembly 相关依赖不再是硬性要求。为了使代码树摇更容易，并将引擎与核心解耦，我们提取出了两个包：`@shikijs/engine-oniguruma` 和 `@shikijs/engine-javascript`。这些包也分别从主包的 `shiki/engine/oniguruma` 和 `shiki/engine/javascript` 重新导出。

你可能需要更改导入路径：

```ts
import { loadWasm } from 'shiki' // [!code --]
import { loadWasm } from 'shiki/engine/oniguruma' // [!code ++]
```

`getHighlighter` 中的 `loadWasm` 字段被 `engine` 字段替代：

```ts
import { createHighlighter } from 'shiki'
import { createOnigurumaEngine } from 'shiki/engine/oniguruma' // [!code ++]
const shiki = await createHighlighter({
  // ...
  loadWasm: () => import('shiki/wasm'), // [!code --]
  engine: createOnigurumaEngine(() => import('shiki/wasm')), // [!code ++]
})
```
