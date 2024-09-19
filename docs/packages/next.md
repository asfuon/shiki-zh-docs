# Next.js

Shiki 并未提供 [Next.js](https://nextjs.org) 的官方集成，但在 Next.js 应用中使用 Shiki 相对简单。

::: info
在 Edge Runtime 中使用 Shiki 可能会引发意想不到的问题，因为 Shiki 依赖于懒加载导入语言和主题。

推荐使用 Serverless Runtime。
:::

## React 服务器组件

由于服务器组件仅在服务器上运行，您可以使用打包好的高亮器，而无需担心包的大小。

```tsx
import { codeToHtml } from 'shiki'

export default function Page() {
  return (
    <main>
      <CodeBlock />
    </main>
  )
}

async function CodeBlock() {
  const out = await codeToHtml('console.log("Hello World")', {
    lang: 'ts',
    theme: 'github-dark'
  })

  return <div dangerouslySetInnerHTML={{ __html: out }} />
}
```

### 自定义组件

您还可以调用 `codeToHast` 获取 HTML 抽象语法树，并使用 [`hast-util-to-jsx-runtime`](https://github.com/syntax-tree/hast-util-to-jsx-runtime) 将其渲染。通过此方法，您可以渲染自己的 `pre` 和 `code` 组件。

```tsx
import { toJsxRuntime } from 'hast-util-to-jsx-runtime'
import { Fragment } from 'react'
// @ts-expect-error -- untyped
import { jsx, jsxs } from 'react/jsx-runtime'
import { codeToHast } from 'shiki'

export default function Page() {
  return (
    <main>
      <CodeBlock />
    </main>
  )
}

async function CodeBlock() {
  const out = await codeToHast('console.log("Hello World")', {
    lang: 'ts',
    theme: 'github-dark'
  })

  return toJsxRuntime(out, {
    Fragment,
    jsx,
    jsxs,
    components: {
      // 自定义 `pre` 元素
      pre: props => <pre data-custom-codeblock {...props} />
    },
  })
}
```

## React 客户端组件

对于客户端组件，它们会在服务器上预渲染，并在客户端进行水合和渲染。
我们可以从创建一个客户端的 `CodeBlock` 组件开始。

创建 `shared.ts` 文件，用于高亮器：

```ts
import { toJsxRuntime } from 'hast-util-to-jsx-runtime'
import { Fragment } from 'react'
// @ts-expect-error -- untyped
import { jsx, jsxs } from 'react/jsx-runtime'
import { codeToHast } from 'shiki/bundle/web'

export async function highlight(code: string) {
  const out = await codeToHast(code, {
    lang: 'ts',
    theme: 'github-dark'
  })

  return toJsxRuntime(out, {
    Fragment,
    jsx,
    jsxs,
  })
}
```

在 `codeblock.tsx` 文件中：

```tsx
'use client'
import { useLayoutEffect, useState } from 'react'
import { highlight } from './shared'

export function CodeBlock({ initial }: { initial?: JSX.Element }) {
  const [nodes, setNodes] = useState(initial)

  useLayoutEffect(() => {
    void highlight('console.log("Rendered on client")').then(setNodes)
  }, [])

  return nodes ?? <p>Loading...</p>
}
```

`initial` 属性可以通过服务器组件传递，用于在服务器上预渲染代码块。

在 `page.tsx` 中：

```tsx
import { CodeBlock } from './codeblock'
import { highlight } from './shared'

export default async function Page() {
  // `initial` 是可选的。
  return (
    <main>
      <CodeBlock initial={await highlight('console.log("Rendered on server")')} />
    </main>
  )
}
```

::: info
上述示例使用 `shiki/bundle/web` 包。您可以更改为 [精细化包](/guide/bundles#fine-grained-bundle) 以完全控制打包的语言和主题。
:::

### 性能

Shiki 会按需加载请求的语言和主题，Next.js 的打包工具可以自动处理懒加载导入。
导入 `shiki` 或其 Web 包对于大多数 Next.js 应用来说足够高效，精细化包不会显著影响包的大小。

此外，您可以使用 `createHighlighter` API 预加载特定语言和主题。
请参阅 [高亮器使用](/guide/install#highlighter-usage) 以了解更多详情。

### 高亮器实例

如果您将高亮器（未使用 `await`）定义为全局变量，您可以在服务器和客户端组件中直接引用它。

```ts
import { createHighlighter } from 'shiki'

const highlighter = createHighlighter({
  themes: ['nord'],
  langs: ['javascript'],
})

// 在异步服务器组件或客户端的 `useEffect` 中
const html = (await highlighter).codeToHtml('const a = 1', {
  lang: 'javascript',
  theme: 'nord'
})
```
