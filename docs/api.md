# API 参考

## `codeToHast`

你可以使用 `codeToHast` 方法，通过 `hast` 进行自定义渲染，而无需将其序列化为 HTML 代码。你还可以进一步将 AST 与 [unified](https://github.com/unifiedjs) 生态系统集成。

```ts twoslash
import { getHighlighter } from 'shiki'

const highlighter = await getHighlighter({
  themes: ['nord', 'min-light'],
  langs: ['javascript'],
})
// ---cut---
const root = highlighter.codeToHast(
  'const a = 1',
  { lang: 'javascript', theme: 'nord' }
)

console.log(root)
```

<!-- eslint-skip -->

```ts
{
  type: 'root',
  children: [
    {
      type: 'element',
      tagName: 'pre',
      properties: {
        class: 'shiki vitesse-light',
        style: 'background-color:#ffffff;color:#393a34',
        tabindex: '0'
      },
      children: [
        {
          type: 'element',
          tagName: 'code',
          properties: {},
          children: [
            {
              type: 'element',
              tagName: 'span',
              properties: { class: 'line' },
              children: [
                {
                  type: 'element',
                  tagName: 'span',
                  properties: { style: 'color:#AB5959' },
                  children: [ { type: 'text', value: 'const' } ]
                },
                {
                  type: 'element',
                  tagName: 'span',
                  properties: { style: 'color:#B07D48' },
                  children: [ { type: 'text', value: ' a' } ]
                },
                {
                  type: 'element',
                  tagName: 'span',
                  properties: { style: 'color:#999999' },
                  children: [ { type: 'text', value: ' =' } ]
                },
                {
                  type: 'element',
                  tagName: 'span',
                  properties: { style: 'color:#2F798A' },
                  children: [ { type: 'text', value: ' 1' } ]
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```
