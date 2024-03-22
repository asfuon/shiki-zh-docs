# 主题

## 捆绑的主题

以下列出的主题来自 [`tm-themes`](https://github.com/antfu/textmate-grammars-themes/tree/main/packages/tm-themes) 。

<ThemesList />

这些主题的许可证由其代码库覆盖，它们的许可证是宽松的（Apache-2.0、MIT 等），你可以在 [此通知](https://github.com/antfu/textmate-grammars-themes/blob/main/packages/tm-grammars/NOTICE) 中查看。

如果你要加载自定义语言，请参考 [这个指南](/guide/load-theme)。

## 特殊的主题

你可以将主题设置为 `none` 以绕过高亮显示。这可以作为处理用户指定了不可用主题时的回滚效果，例如：

```ts twoslash theme:none
import { codeToHtml } from 'shiki'

const html = codeToHtml('console.log("Hello World")', {
  lang: 'javascript',
  theme: 'none', // [!code hl]
})
```
