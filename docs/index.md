---
layout: home

hero:
  name: "Shiki 式"
  text: "语法高亮器"
  tagline: 美观而强大的代码高亮器
  image:
    src: /logo.svg
    alt: Shiki Logo
  actions:
    - theme: brand
      text: 快速开始
      link: /guide/
    - theme: alt
      text: 安装
      link: /guide/install/

features:
  - title: 精确 & 美观
    icon: 🌈
    details: 由 TextMate 驱动，和编辑器一样准确，且共用语法与主题。
  - title: 零运行时
    icon: ⏱️
    details: 预构建，不载入任何 JavaScript 代码就能获得完美的语法高亮。
  - title: 自定义
    icon: 🧩
    details: 使用基于 HAST 的高度可定制化插件和转换器功能。
  - title: ESM & 通用
    icon: 🎄
    details: 高度可除屑优化（Tree-shaking）的 ESM，可以在任何 JavaScript 运行时上运行，如浏览器、Node.js、Cloudflare Worker 等。
---

<HomeDemo />
