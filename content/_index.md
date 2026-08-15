---
title: '我的 Hugo 站点'
layout: hextra-home
---

<div class="hx:mt-6 hx:mb-6">
{{< hextra/hero-headline >}}
  嗨，我是洋洋 👋<br class="hx:sm:block hx:hidden" />技术笔记 · 工具文档 · 生活随笔
{{< /hextra/hero-headline >}}
</div>

<div class="hx:mb-12">
{{< hextra/hero-subtitle >}}
  这里是我的个人站点：记录技术踩坑、分享工具使用心得，<br class="hx:sm:block hx:hidden" />以及一些折腾路上的笔记。
{{< /hextra/hero-subtitle >}}
</div>

<div class="hx:mb-6">
{{< hextra/hero-button text="阅读文章" link="blog" >}}
{{< hextra/hero-button text="工具文档" link="docs/tools" style="outline" >}}
{{< hextra/hero-button text="关于我" link="about" style="outline" >}}
</div>

<div class="hx:mt-6"></div>

{{< hextra/feature-grid >}}
  {{< hextra/feature-card
    title="技术笔记"
    subtitle="Hugo、GitHub Pages、Linux 运维等踩坑与经验总结。"
    class="hx:aspect-auto hx:md:aspect-[1.1/1] hx:max-md:min-h-[240px]"
  >}}
  {{< hextra/feature-card
    title="工具文档"
    subtitle="AI 工具、搜索技巧、自动化工作流的使用笔记与配置速查。"
    class="hx:aspect-auto hx:md:aspect-[1.1/1] hx:max-md:min-h-[240px]"
  >}}
  {{< hextra/feature-card
    title="随时搜索"
    subtitle="内置全文搜索，按 Ctrl+K 快速找到想要的内容。"
    class="hx:aspect-auto hx:md:aspect-[1.1/1] hx:max-md:min-h-[240px]"
  >}}
{{< /hextra/feature-grid >}}
