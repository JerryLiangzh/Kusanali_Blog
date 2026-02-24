---

title: '博客中的URL配置'
publishDate: 2026-02-24
description: '谈谈建站过程中和url打过的交道'
author: 'Jerry Liangzh'
tags: ["建站"]
featured: false

---

本月和URL打了不少交道，从Waline到Copyright等等，总算弄清楚了一些东西、积累了一点经验，这篇文章就当是备忘录吧。本来是以update形式补充在[建站存档点 - 1](https://kusanali.top/blog/website-archive-point-1)里的，但随着对这个点的深入探索，update的篇幅似乎有点喧宾夺主，除非重构整篇文章，那我还不如聚合在一起好了。

## Waline

首先要谈谈waline，它的统计功能终于启用了。22号那天我在update处提到，从`data-path`入手探索。参考astro theme pure的示例博客以及2位突出贡献者的博客，对他们文章开头的comments右键检查后，`data-path`末尾都是不带`/`的，我也一样；同样地，在`astro.config.ts`里，`trailingSlash`均为`never`。但我的waline后台管理页面中显示的评论所属的博客文章页面都是尾是带`/`的。我追踪到`src\components\waline\Pageview.astro`，可无论把其中的path赋值为默认的`window.location.pathname`还是尾带`.replace(/\/$/,'')`的版本，waline的浏览量、评论数统计功能均不能生效——也就是说`data-path`末尾仍是不带`/`，与waline记录无法匹配，故始终显示**0 views | 0 comments**。于是干脆在`src\components\waline\PageInfo.astro`中把两个`data-path`从`{path}`改成`{path+'/'}`，强行多加一个`/`，就解决了问题。

## 文章URL与Copyright

24号上午在闲逛收藏夹里的各博客时，刷到了绅士喵的这一篇文章，[正确配置用 .html 作为 URL 后缀的博客缓存](https://blog.hentioe.dev/posts/configure-blog-cache-with-html-as-url-suffix.html)，内中提及`/post/post-1`与`/post/post-1/`的对比，并称后者是丑陋的文件夹地址。我虽然对文件夹地址意见不是很大，但如可能我更愿意让我的博客使用前一种地址。我额外参考了本文上一部分提及的那3个参考博客，它们皆是属于前者。

Astro theme pure的Docs并未提及此内容，但Astro的Docs倒是有，也就是涉及到所谓的[build.format](https://docs.astro.build/en/reference/configuration-reference/#buildformat)。和绅士喵文中吐槽的Hugo一样，Astro默认生成的文件结构会让文章以“文件夹”的形式访问，即
```
{
    build: {
        format: 'directory'
    }
}
```
此举会让astro为每个页面都生成一个内含index.html的文件夹，具体到文章URL就是尾带`/`。如果要不带，可以改为`format: 'file'`，此时astro会为每个页面路由创建同名html文件。

这样修改后，waline那边就出了问题——这是必然的。但如果只把`Pageview.astro`和`PageInfo.astro`都改回默认设置，waline统计功能依旧是失效的，因为这时的`data-path`变成了`/blog/website-archive-point-1.html`——真是按下葫芦浮起瓢。这时可以对`PageInfo.astro`里的path动手脚，从`const path = Astro.url.pathname`改成`const path = Astro.url.pathname.replace(/(index)?\.html$/, '');`，以此修复`data-path`。

在改为`format: 'file'`的同时，文末的Copyright部分也陷入了麻烦，从文件式URL变成了尾带`.html`版本，鉴于`packages\pure\components\pages\Copyright.astro`中对应代码是
```
{/* title & link */}
  <div class='flex flex-col'>
    <div class='font-medium text-foreground'>{title}</div>
    <div class='text-sm'>{Astro.url}</div>
  </div>
```
不妨改为：先在前面来一行`const sharelink = 'https://kusanali.top'+Astro.url.pathname.replace(/(index)?\.html$/, ''); `，然后
```
{/* title & link */}
  <div class='flex flex-col'>
    <div class='font-medium text-foreground'>{title}</div>
    <div class='text-sm'>{sharelink}</div>
  </div>
```
需要注意的是，如果sharelink没有前半部分的硬编码，Copyright部分生成的字段就会从URL退化成路径，比如`/blog/website-archive-point-1`。当然，`Copyright.astro`中不止这一处`Astro.url`，但其他部分都是涉及创建分享到第三方平台的链接，比如`http://service.weibo.com/share/share.php?url=https://kusanali.top/blog/happy-birthday.html&title=%E7%94%9F%E6%97%A5%E5%BF%AB%E4%B9%90%EF%BC%81&pic=`，不影响功能，就懒得去改了。