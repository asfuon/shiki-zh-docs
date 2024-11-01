# 主题颜色控制

## 多种颜色值支持

通常，TextMate 主题要求标签的颜色值是有效的十六进制颜色值，这个限制来自于 [`vscode-textmate`](https://github.com/microsoft/vscode-textmate)。然而，在 shiki v1.0 中，我们引入了一个自动解决的方法，通过用占位符替换非十六进制颜色值，并在标签化时将其替换回来。这样可以让你使用具有多种颜色值的主题进行渲染，而不必担心其技术细节：

```ts twoslash
import { createHighlighter } from 'shiki'

const highlighter = await createHighlighter({
  langs: ['javascript'],
  themes: [
    {
      name: 'my-theme',
      settings: [
        {
          scope: ['comment'],
          settings: {
            // 使用 `rgb`、`hsl`、`hsla`，// [!code hl:3]
            // 或者任何你的渲染器支持的颜色。
            foreground: 'rgb(128, 128, 128)'
          }
        },
        {
          scope: ['string'],
          settings: {
            foreground: 'var(--code-string)' // CSS 变量 // [!code hl:1]
          }
        },
        // 更多
      ],
      // 背景和前景颜色 // [!code hl:3]
      bg: 'var(--code-bg)',
      fg: 'var(--code-fg)'
    }
  ]
})

const html = highlighter.codeToHtml('const foo = "bar"', { lang: 'javascript', theme: 'my-theme' })
```

::: info 注意
谨慎使用，这将与 TextMate 主题不兼容。

这也可能会使主题与非 Web 用例不兼容，例如 [`shiki-cli`](/packages/cli) 和 [`shiki-monaco`](/packages/monaco)。
:::

了解如何 [载入主题](./load-theme)。

## 颜色替换

你还可以使用 `colorReplacements` 选项来替换主题的颜色值。它在你想要使用具有不同颜色的主题时非常有用。它在主题对象、 `codeToHast` 以及 `codeToHtml` 的选项上可用。

`colorReplacements` 对象需要符合颜色与颜色一对一的形式，键是你想要替换的原颜色，值是新的颜色：

```js
const html = await codeToHtml(
  code,
  {
    lang: 'js',
    theme: 'min-dark',
    colorReplacements: {
      '#ff79c6': '#189eff'
    }
  }
)
```

此外，`colorReplacements` 可以包含制定作用域的替换。对于多主题的情况下，这样可以很方便的替换指定主题的颜色：

```js
const html = await codeToHtml(
  code,
  {
    lang: 'js',
    themes: { dark: 'min-dark', light: 'min-light' },
    colorReplacements: {
      'min-dark': {
        '#ff79c6': '#189eff'
      },
      'min-light': {
        '#ff79c6': '#defdef'
      }
    }
  }
)
```

该选项只在 `colorReplacements` 上可用，在主题对象上则不可用。

## CSS 变量主题

shiki 提供了一个工厂函数助手（Factory Function Helper）`createCssVariablesTheme`，用于更方便地创建使用 CSS 变量的主题。请注意，这个主题形式比大多数其他主题的细粒度要低，并且需要在你的应用程序中定义 CSS 变量。这是为了更方便地从 Shiki 的 [`css-variables` 主题](https://github.com/shikijs/shiki/blob/main/docs/themes.md#theming-with-css-variables) 迁移而提供的。但为了更好的显示效果，我们建议使用 [多种颜色值支持](#多种颜色值支持) 或 [颜色替换](#颜色替换) 来覆盖现有的主题。

此主题形式**不包含在默认设置**中，必须显式注册：

```ts twoslash
import { createCssVariablesTheme } from 'shiki/core'
import { createHighlighter } from 'shiki'

// 创建一个自定义的 CSS 变量主题，以下是默认值。
const myTheme = createCssVariablesTheme({ // [!code hl:6]
  name: 'css-variables',
  variablePrefix: '--shiki-',
  variableDefaults: {},
  fontStyle: true
})

const highlighter = await createHighlighter({
  langs: ['javascript'],
  themes: [myTheme] // 注册主题 // [!code hl]
})

const html = highlighter.codeToHtml('const foo = "bar"', {
  lang: 'javascript',
  theme: 'css-variables' // 使用主题 // [!code hl]
})
```

CSS 变量的示例如下：

```css
:root {
  --shiki-foreground: #eeeeee;
  --shiki-background: #333333;
  --shiki-token-constant: #660000;
  --shiki-token-string: #770000;
  --shiki-token-comment: #880000;
  --shiki-token-keyword: #990000;
  --shiki-token-parameter: #aa0000;
  --shiki-token-function: #bb0000;
  --shiki-token-string-expression: #cc0000;
  --shiki-token-punctuation: #dd0000;
  --shiki-token-link: #ee0000;

  /* Only required if using lang: 'ansi' */
  --shiki-ansi-black: #000000;
  --shiki-ansi-black-dim: #00000080;
  --shiki-ansi-red: #bb0000;
  --shiki-ansi-red-dim: #bb000080;
  --shiki-ansi-green: #00bb00;
  --shiki-ansi-green-dim: #00bb0080;
  --shiki-ansi-yellow: #bbbb00;
  --shiki-ansi-yellow-dim: #bbbb0080;
  --shiki-ansi-blue: #0000bb;
  --shiki-ansi-blue-dim: #0000bb80;
  --shiki-ansi-magenta: #ff00ff;
  --shiki-ansi-magenta-dim: #ff00ff80;
  --shiki-ansi-cyan: #00bbbb;
  --shiki-ansi-cyan-dim: #00bbbb80;
  --shiki-ansi-white: #eeeeee;
  --shiki-ansi-white-dim: #eeeeee80;
  --shiki-ansi-bright-black: #555555;
  --shiki-ansi-bright-black-dim: #55555580;
  --shiki-ansi-bright-red: #ff5555;
  --shiki-ansi-bright-red-dim: #ff555580;
  --shiki-ansi-bright-green: #00ff00;
  --shiki-ansi-bright-green-dim: #00ff0080;
  --shiki-ansi-bright-yellow: #ffff55;
  --shiki-ansi-bright-yellow-dim: #ffff5580;
  --shiki-ansi-bright-blue: #5555ff;
  --shiki-ansi-bright-blue-dim: #5555ff80;
  --shiki-ansi-bright-magenta: #ff55ff;
  --shiki-ansi-bright-magenta-dim: #ff55ff80;
  --shiki-ansi-bright-cyan: #55ffff;
  --shiki-ansi-bright-cyan-dim: #55ffff80;
  --shiki-ansi-bright-white: #ffffff;
  --shiki-ansi-bright-white-dim: #ffffff80;
}
```

如果你是从 Shiki 的 v0 版本迁移来的，以下是一些对 Shiki 的 `css-variables` 条目的更名供你参考：

| Shiki v0                   | Shiki v1.0           |
| -------------------------- | -------------------- |
| `--shiki-color-text`       | `--shiki-foreground` |
| `--shiki-color-background` | `--shiki-background` |
| `--shiki-color-ansi-*`     | `--shiki-ansi-*`     |
