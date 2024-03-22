# 加载自定义语言

请先查看 [所有内建语言](/languages)。

你可以通过将 TextMate 语法对象传递到 `langs` 数组中来加载自定义语言。

```ts twoslash
// @noErrors
import { getHighlighter } from 'shiki'

const myLang = JSON.parse(fs.readFileSync('my-lang.json', 'utf8'))

const highlighter = await getHighlighter({
  langs: [myLang],
  themes: ['vitesse-light']
})

const html = highlighter.codeToHtml(code, {
  lang: 'my-lang',
  theme: 'vitesse-light'
})
```

同样的，你也可以在高亮器创建后载入自定义语言。

```ts twoslash
// @noErrors
import { getHighlighter } from 'shiki'

const myLang = JSON.parse(fs.readFileSync('my-lang.json', 'utf8'))

const highlighter = await getHighlighter({
  langs: [],
  themes: ['vitesse-light'],
})

await highlighter.loadLanguage(myLang) // <--

const html = highlighter.codeToHtml(code, {
  lang: 'my-lang',
  theme: 'vitesse-light'
})
```

## 从 v0.14 迁移

自从 1.0 开始，`shiki` 是与环境无关的，所以我们无法访问文件系统。这意味着 `shiki@0.14` 中的 `path` 属性在 1.0 中已经不受支持，你必须手动读取文件并传入对象。

例如，以下代码不会工作：

```ts
const highlighter = await getHighlighter({
  langs: [
    {
      name: 'vue-vine',
      scopeName: 'source.vue-vine',
      // ‼️ 这不会工作！
      path: join(__dirname, './vine-ts.tmLanguage.json'),
      embeddedLangs: [
        'vue-html',
        'css',
        'scss',
        'sass',
        'less',
        'stylus',
      ],
    },
  ],
  themes: []
})
```

所以，请手动加载文件（通过 `fs`，`import()` 或 `fetch()` 等）:

```ts
const vineGrammar = JSON.parse(fs.readFileSync(join(__dirname, './vine-ts.tmLanguage.json'), 'utf8'))

const highlighter = await getHighlighter({
  langs: [
    {
      name: 'vue-vine',
      scopeName: 'source.vue-vine',
      embeddedLangs: [
        'vue-html',
        'css',
        'scss',
        'sass',
        'less',
        'stylus',
      ],
      ...vineGrammar
    },
  ],
  themes: []
})
```

## 自定义语言别名

你可以使用 `langAlias` 选项注册自定义的语言别名。例如：

```ts twoslash
import { getHighlighter } from 'shiki'

const highlighter = await getHighlighter({
  langs: ['javascript'],
  langAlias: { // [!code hl:3]
    mylang: 'javascript',
  },
  themes: ['nord']
})

const code = highlighter.codeToHtml('const a = 1', {
  lang: 'mylang', // [!code hl]
  theme: 'nord'
})
```
