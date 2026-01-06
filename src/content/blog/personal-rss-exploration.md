---

title: '个人向RSS折腾记录'
publishDate: 2026-01-06
description: '一些RSS探索经历'
author: 'Jerry Liangzh'
tags: ["RSS"]
featured: false

---

本博客曾经历了一段不长不短的技术选型时期，彼时我将收集到的博客，在Edge收藏夹中按SSG&SSR类型分类，比如Jekyll、Hexo、Astro等等，各框架之下又有按Theme细分的子类。这些博客及其分类为当时的选型与持续至今的主题风格渐进改造提供了不小的帮助。但在重心转向内容产出与维护的现在，如果能充分关注、仔细阅读这些博客的内容，加之收藏夹中繁多的其他非博客内容产出站点，以及必要的筛选手段，应该是大有裨益的。于是，我想到了RSS。

接下来的内容不会涉及太多“为什么需要RSS”或“RSS在生活中扮演了或可以扮演什么样的角色”。毕竟这样的讨论难避主观，因人而异，还不如去问各LLM。本文仅对我的RSS探索经历做一点记录，以及补充一些必要的信息。

## 什么是RSS

RSS，全称是Really Simple Syndication，即“简易信息聚合”，基于XML，可以描述和同步网站内容，本质上是一种方便高效的信息传播格式。RSS订阅，类似于微博、微信公众号等平台，都只是信息获取渠道，只是RSS订阅为渠道的自主可控创造了条件。

## 如何订阅RSS

RSS的订阅方式有很多。

### 网站自行提供

有些网站与博客会自行提供RSS订阅源，比如[少数派](https://sspai.com)、[超能网](https://www.expreview.com)、[CWorld Site](https://cworld0.com)以及[Simon Willison’s Weblog](https://simonwillison.net)等等。在这些网站或博客的页面角落，一般能发现一个橙底+内嵌白色的、酷似右转90°的WiFi图案，此即它们所提供的RSS订阅源。

如果没有找到，也不代表网站并没有提供。以[HLTV](https://hltv.org)为例，页面并无相关图标。此时就需要[RSS Radar](https://github.com/DIYgod/RSSHub-Radar)，一个浏览器扩展，可以帮助发现、生成相应的RSS URL。在该扩展中，也有一些网站的RSS订阅源已被汇总，可供取用。

### RSSHub

"Everything is RSSible."[RSSHub](https://github.com/DIYgod/RSSHub)通过爬取网页内容生成RSS流，以此订阅原本不支持RSS的站点或信源，比如B站Up主动态、特朗普的推特等等。当然，其效果很受目标网站反爬策略的影响。受限于一些原因，我并没有部署RSSHub。具体可移步这位博主的教程———[重新捡起 RSS：RSSHub + FreshRSS 建立我的信息流](https://blog.l3zc.com/2023/07/rsshub-freshrss-information-flow/)。

## RSS Readers

有了RSS订阅源，现在应该考虑如何阅读它们了。一般而言，RSS阅读器可分为2类，一是在线阅读器，以[Folo](https://github.com/RSSNext/Folo)、[Inoreader](https://www.inoreader.com)、[Feedly](https://feedly.com)与[Readwise Reader](https://readwise.io)为代表，自带服务器同步，只需要注册账号，即可在各种平台/设备间无缝阅读。当然，这类阅读器也有basic plan或free plan限制，比如上限订阅100/150个源、隐藏订阅需要更高级的套餐等等。

二是本地阅读器，以[Fluent Reader](https://github.com/yang991178/fluent-reader)、[ReadYou](https://github.com/ReadYouApp/ReadYou)和[RSS Guard](https://github.com/martinrotter/rssguard)等为代表。这些阅读器支持本地账号，通过手动添加RSS订阅源或OPML文件引入RSS信息流。当然，它们不少也支持在线账号登录，比如Inoreader等等。

## 自建RSS订阅源

如果希望实现数据的完全掌握，不妨自建订阅源。这方面的服务有[FreshRSS](https://github.com/FreshRSS/FreshRSS)、[minuflux](https://github.com/miniflux/v2)与[Tiny Tiny RSS](https://tt-rss.org)。通过自托管服务，还可以实现跨设备间的自动同步，包括但不限于新信息的同步和信息已读状态的同步。这里我选择的是FreshRSS。它的部署相当简单，可以手动Docker部署，也可以一键部署。由于[ClawCloud Run](https://run.claw.cloud)对满足条件的新用户有每月5美元的优惠，而通过ClawCloud部署FreshRSS每天只需0.11美元，故可称绰绰有余。这里搬运了FreshRSS GitHub文档中的Automated install。

| | | |
|-|-|-|
| [<img src="https://www.docker.com/wp-content/uploads/2022/03/horizontal-logo-monochromatic-white.png" width="160" alt="Docker" />](./Docker/) | [<img src="https://install-app.yunohost.org/install-with-yunohost.png" width="160" alt="YunoHost" />](https://install-app.yunohost.org/?app=freshrss) | [<img src="https://elest.io/images/logos/deploy-to-elestio-btn.png" width="160" alt="Elestio" />](https://elest.io/open-source/freshrss) |
| [<img src="https://cloudron.io/img/button.svg" width="160" alt="Cloudron" />](https://cloudron.io/button.html?app=org.freshrss.cloudronapp) | [<img src="https://www.pikapods.com/static/run-button-34.svg" width="160" alt="PikaPods" />](https://www.pikapods.com/pods?run=freshrss) | [<img src="https://zeabur.com/button.svg" width="160" alt="Zeabur" />](https://zeabur.com/templates/MD4TRW) |
| [<img src="https://raw.githubusercontent.com/ClawCloud/Run-Template/refs/heads/main/Run-on-ClawCloud.svg" width="160" alt="ClawCloud" />](https://template.run.claw.cloud/?openapp=system-fastdeploy%3FtemplateName%3Dfreshrss) | [<img src="https://assets.hostinger.com/vps/deploy.svg" width="160" alt="Hostinger" />](https://www.hostinger.com/vps/docker-hosting?compose_url=https://github.com/FreshRSS/FreshRSS/blob/edge/Docker/freshrss/docker-compose.yml) | |

部署完后，可以访问ClawCloud提供的Public access，创建账户与密码等等。
<未完待续>