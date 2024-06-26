# 加载自定义主题

请先查看 [所有内建主题](/themes)。

你可以通过向 `themes` 数组传递一个 `Theme` 对象来载入自定义主题。

```ts twoslash
import { createHighlighter } from 'shiki'

const myTheme = {
  name: 'my-theme',
  settings: [
    {
      scope: ['comment'],
      settings: {
        foreground: '#888'
      }
    },
    // ...
  ]
}

const highlighter = await createHighlighter({
  themes: [myTheme],
  langs: [],
})

const code = `console.log('hello')`
const html = highlighter.codeToHtml(code, {
  lang: 'javascript',
  theme: 'my-theme'
})
```

同样的，你也可以在高亮器创建后载入自定义主题。

```ts twoslash
// @noErrors
import { createHighlighter } from 'shiki'

// 从文件、网络请求或其他任何地方载入主题对象
const myTheme = JSON.parse(fs.readFileSync('my-theme.json', 'utf8'))

const highlighter = await createHighlighter({
  langs: ['javascript'],
  themes: [],
})

await highlighter.loadTheme(myTheme) // <--

const code = `console.log('hello')`
const html = highlighter.codeToHtml(code, {
  lang: 'javascript',
  theme: 'my-theme'
})
```

主题是一个 JavaScript 对象形式的 TextMate 主题，[它看起来像这样](https://github.com/antfu/textmate-grammars-themes/blob/main/packages/tm-themes/themes/dark-plus.json)。
