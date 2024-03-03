# Nuxt

[Nuxt](https://nuxt.com) 是一个通用的元框架，你可以用它完全控制你的应用程序。你可以安装 Shiki 并使用 ([markdown-it](/packages/markdown-it) 或 [rehype](/packages/rehype)) 集成，或根据需要手动导入。

此外，Nuxt 还为 Shiki 提供了一些模块集成以方便使用：

- [`nuxt-shiki`](https://github.com/pi0/nuxt-shiki) - 客户端和服务端的 Shiki 集成，包括组件的使用方法。
- [`@nuxt/content`](https://github.com/nuxt/content) - Nuxt Content 内置了用于 Markdown 文件的 Shiki 集成。[你可以使用 `highlight` 选项来启用](https://content.nuxt.com/get-started/configuration#highlight)。
  - [`@nuxtjs/mdc`](https://github.com/nuxt-modules/mdc) - 为 `@nuxt/content` 提供 MDC 语法的底层模块，支持 Shiki 语法高亮。

## TwoSlash 集成

- [`nuxt-content-twoslash`](https://github.com/antfu/nuxt-content-twoslash) - 向 Nuxt Content 添加 Twoslash 支持的 Nuxt 模块。Twoslash 相关信息在构建时计算。
