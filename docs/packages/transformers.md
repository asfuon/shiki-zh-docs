---
outline: deep
---

# @shikijs/transformers

<Badges name="@shikijs/transformers" />

受 [shiki-processor](https://github.com/innocenzi/shiki-processor) 启发，为 Shiki 设计的常用转换器（Transformers）的集合。

## 安装

```bash
npm i -D @shikijs/transformers
```

## 使用方法

```ts twoslash
// [!code highlight:5]
import {
  transformerNotationDiff,
  // ...
} from '@shikijs/transformers'
import {
  codeToHtml,
} from 'shiki'

const code = `console.log('hello')`
const html = await codeToHtml(code, {
  lang: 'ts',
  theme: 'nord',
  transformers: [
    transformerNotationDiff(), // [!code highlight]
    // ...
  ],
})
```

## 无样式

转换器只应用到类，并不带有样式，你可以提供自己的 CSS 规则来样式化它们。

## 转换器

### `transformerNotationDiff`

使用 `[!code ++]` 和 `[!code --]` 来标记增删的行。

````md
```ts
console.log('hewwo') // [\!code --]
console.log('hello') // [\!code ++]
console.log('goodbye')
```
````

渲染成（已样式化）：

```ts
console.log('hewwo') // [!code --]
console.log('hello') // [!code ++]
console.log('goodbye')
```

- `// [!code ++]` 输出：`<span class="line diff add">`
- `// [!code --]` 输出：`<span class="line diff remove">`
- 外围的 `<pre>` 标签修改为：`<pre class="has-diff">`

::: details HTML 输出

```html
<!-- Output (stripped of `style` attributes for clarity) -->
<pre class="shiki has-diff"> <!-- Notice `has-diff` -->
  <code>
    <span class="line"></span>
    <span class="line"><span>function</span><span>()</span><span></span><span>{</span></span>
    <span class="line diff remove">  <!-- Notice `diff` and `remove` -->
      <span></span><span>console</span><span>.</span><span>log</span><span>(</span><span>&#39;</span><span>hewwo</span><span>&#39;</span><span>) </span>
    </span>
    <span class="line diff add">  <!-- Notice `diff` and `add` -->
      <span></span><span>console</span><span>.</span><span>log</span><span>(</span><span>&#39;</span><span>hello</span><span>&#39;</span><span>) </span>
    </span>
    <span class="line"><span></span><span>}</span></span>
    <span class="line"><span></span></span>
  </code>
</pre>
```

:::

---

### `transformerNotationHighlight`

使用 `[!code highlight]` 来高亮显示行：

````md
```ts
console.log('Not highlighted')
console.log('Highlighted') // [\!code highlight]
console.log('Not highlighted')
```
````

渲染成（已样式化）：

```ts
console.log('Not highlighted')
console.log('Highlighted') // [!code highlight]
console.log('Not highlighted')
```

- `// [!code highlight]` 输出：`<span class="line highlighted">`
- 外围的 `<pre>` 标签被修改为：`<pre class="has-highlighted">`

你也可以使用单个注释来高亮多行：

````md
```ts
// [\!code highlight:3]
console.log('Highlighted')
console.log('Highlighted')
console.log('Not highlighted')
```
````

渲染为：

```ts
// [!code highlight:3]
console.log('Highlighted')
console.log('Highlighted')
console.log('Not highlighted')
```

---

### `transformerNotationWordHighlight`

使用 `[!code word:Hello]` 在接下来的代码高亮所有的 `Hello`。

````md
```ts
// [\!code word:Hello]
const message = 'Hello World'
console.log(message) // prints Hello World
```
````

渲染成（已样式化）：

```ts
// [!code word:Hello]
const message = 'Hello World'
console.log(message) // prints Hello World
```

输出：匹配到的单词为 `<span class="highlighted-word">Hello</span>`

你还可以指定高亮显示的次数，例如 `[!code word:Hello:2]` 会高亮最近的那个 `Hello`。

````md
```ts
// [\!code word:Hello:1]
const message = 'Hello World'
console.log(message) // prints Hello World
```
````

渲染为：

```ts
// [!code word:Hello:1]
const message = 'Hello World'
console.log(message) // prints Hello World
```

---

### `transformerNotationFocus`

使用 `[!code focus]` 来聚焦显示行：

````md
```ts
console.log('Not focused');
console.log('Focused') // [\!code focus]
console.log('Not focused');
```
````

渲染成（已样式化）：

```ts
console.log('Not focused')
console.log('Focused') // [!code focus]
console.log('Not focused')
```

- 输出：`<span class="line focused">`
- 外围的 `<pre>` 标签被修改为：`<pre class="has-focused">`

你也可以使用单个注释聚焦多行：

````md
```ts
// [\!code focus:3]
console.log('Focused')
console.log('Focused')
console.log('Not focused')
```
````

渲染为：

```ts
// [!code focus:3]
console.log('Focused')
console.log('Focused')
console.log('Not focused')
```

---

### `transformerNotationErrorLevel`

使用 `[!code error]` 和 `[!code warning]` 来指定行的日志等级：

````md
```ts
console.log('No errors or warnings')
console.error('Error') // [\!code error]
console.warn('Warning') // [\!code warning]
```
````

- 输出：错误为 `<span class="line highlighted error">`
- 输出：警告为 `<span class="line highlighted warning">`
- 外围的 `<pre>` 标签被修改为：`<pre class="has-highlighted">`

加上一些额外的 CSS 规则，它们可以看起来像这样：

```ts
console.log('No errors or warnings')
console.error('Error') // [!code error]
console.warn('Warning') // [!code warning]
```

---

### `transformerRenderWhitespace`

将空白字符（Tab 和空格）渲染为单独的标签（具有 `tab` 或 `space` 类名）。

使用一些 CSS，可以使其看起来像这样：

<div class="language-js vp-adaptive-theme"><button title="Copy Code" class="copy"></button><span class="lang">js</span><pre v-pre class="shiki shiki-themes vitesse-light vitesse-dark vp-code" style="--shiki-light:#393a34;--shiki-dark:#dbd7caee;--shiki-light-bg:#ffffff;--shiki-dark-bg:#121212;" tabindex="0"><code><span class="line"><span style="--shiki-light:#AB5959;--shiki-dark:#CB7676;">function</span><span class="space"> </span><span style="--shiki-light:#59873A;--shiki-dark:#80A665;">block</span><span style="--shiki-light:#999999;--shiki-dark:#666666;">(</span><span class="space"> </span><span style="--shiki-light:#999999;--shiki-dark:#666666;">)</span><span class="space"> </span><span style="--shiki-light:#999999;--shiki-dark:#666666;">{</span></span>
<span class="line"><span class="space"> </span><span class="space"> </span><span style="--shiki-light:#59873A;--shiki-dark:#80A665;">space</span><span style="--shiki-light:#999999;--shiki-dark:#666666;">(</span><span class="space"> </span><span style="--shiki-light:#999999;--shiki-dark:#666666;">)</span></span>
<span class="line"><span class="tab">&#9;</span><span class="tab">&#9;</span><span style="--shiki-light:#59873A;--shiki-dark:#80A665;">tab</span><span style="--shiki-light:#999999;--shiki-dark:#666666;">(</span><span class="space"> </span><span style="--shiki-light:#999999;--shiki-dark:#666666;">)</span><span style="--shiki-light:#393A34;--shiki-dark:#DBD7CAEE;"> </span></span>
<span class="line"><span style="--shiki-light:#999999;--shiki-dark:#666666;">}</span></span></code></pre></div>

::: details 示例 CSS

```css
.vp-code .tab,
.vp-code .space {
  position: relative;
}

.vp-code .tab::before {
  content: '⇥';
  position: absolute;
  opacity: 0.3;
}

.vp-code .space::before {
  content: '·';
  position: absolute;
  opacity: 0.3;
}
```

:::

---

### `transformerMetaHighlight`

根据代码片段上提供的[元字符串](/guide/transformers#元)，高亮显示行。

````md
```js {1,3-4}
console.log('1')
console.log('2')
console.log('3')
console.log('4')
```
````

渲染成（已样式化）：

```js {1,3-4}
console.log('1')
console.log('2')
console.log('3')
console.log('4')
```

- 输出：包含的行为 `<span class="line highlighted">`

### `transformerMetaWordHighlight`

根据代码片段中提供的元字符串，高亮显示词。

````md
```js /Hello/
const msg = 'Hello World'
console.log(msg)
console.log(msg) // 打印 Hello World
```
````

渲染成（已样式化）：

```js /Hello/
const msg = 'Hello World'
console.log(msg) // 打印 Hello World
```

输出：匹配的词为 `<span class="highlighted-word">Hello</span>`

---

### `transformerCompactLineOptions`

在 `shiki` 中删除的对 `shiki` v0 的 `lineOptions` 的支持。

---

### `transformerRemoveLineBreak`

删除 `<span class="line">` 之间的换行符。当你在 CSS 中将 `display: block` 设置为 `.line` 时这有可能用。

---

### `transformerRemoveNotationEscape`

将 `// [\!code ...]` 转换为 `// [!code ...]`.
避免按原样呈现转义符号语法。
