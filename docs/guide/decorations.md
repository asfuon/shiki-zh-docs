# 代码装饰

我们提供了一个代码装饰的 API，用来将渲染后指定位置的代码元素包裹在自定义类或前缀中。

```ts twoslash
import { codeToHtml } from 'shiki'

const code = `
const x = 10
console.log(x)
`.trim()

const html = await codeToHtml(code, {
  theme: 'vitesse-light',
  lang: 'ts',
  decorations: [ // [!code hl:8]
    {
      // line 和 character 都是从 0 开始索引的
      start: { line: 1, character: 0 },
      end: { line: 1, character: 11 },
      properties: { class: 'highlighted-word' }
    }
  ]
})
```

渲染的结果是是（已使用 CSS 样式化）：

```ts
// @decorations:[{"start":{"line":1,"character":0},"end":{"line":1,"character":11},"properties":{"class":"highlighted-word"}}]
const x = 10
console.log(x)
```

你也可以用以 0 作为索引的代码相对位置偏移来指定位置：

```ts twoslash
import { codeToHtml } from 'shiki'

const code = `
const x = 10
console.log(x)
`.trim()
// ---cut---
const html = await codeToHtml(code, {
  theme: 'vitesse-light',
  lang: 'ts',
  decorations: [ // [!code hl:7]
    {
      start: 21,
      end: 24,
      properties: { class: 'highlighted-word' }
    }
  ]
})
```

这渲染出：

```ts
// @decorations:[{"start":21,"end":24,"properties":{"class":"highlighted-word"}}]
const x = 10
console.log(x)
```

## 在转换器中使用代码装饰

对于一些高级的用例，你可以使用 [转换器 API](./transformers.md) 来完全控制标签和 HAST 树。

当然，如果你想在转换器中添加代码装饰，你可以使用以下的方法：

```ts twoslash
/* eslint-disable import/no-duplicates */
import { DecorationItem } from 'shiki'
function doSomethingWithCode(code: string): DecorationItem[] {
  return []
}
const code: string = ''

// ---cut---
import { ShikiTransformer, codeToHtml } from 'shiki'

const myTransformer: ShikiTransformer = {
  name: 'my-transformer',
  preprocess(code, options) {
    // 用某种方式生成代码装饰
    const decorations = doSomethingWithCode(code)

    // 确保装饰数组存在
    options.decorations ||= []
    // 添加代码装饰
    options.decorations.push(...decorations)
  }
}

const html = await codeToHtml(code, {
  theme: 'vitesse-light',
  lang: 'ts',
  transformers: [
    myTransformer
  ]
})
```

请注意，你只能在 `preprocess` 钩子时添加代码装饰，在之后的钩子中，对装饰数组的修改会被忽略。
