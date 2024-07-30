---
date: '2024-07-27T12:22:03Z'
draft: false
title: 'Hugo 常用命令与使用技巧'
# weight: 1
# aliases: ["/first"]
tags: ["Hugo", "Guide"]
author: "NekoRect"
showToc: false
TocOpen: false
draft: false
hidemeta: false
comments: false
description: ""
#canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
hideSummary: true
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: false
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
#editPost:
#    URL: "https://github.com/<path_to_repo>/content"
#    Text: "Suggest Changes" # edit text
#    appendFilePath: true # to append file path to Edit link
---

最近切换到 Hugo 来了，原因 Hexo 在非 x86 平台的操作系统上部署体验过于繁琐，先是 nodejs 在通过 npm 下载 hexo 二进制文件。相比 Hugo 仅仅使用已经打包好了的二进制，Hexo 不得不说是输在了多平台的方便上。（本博文使用 AOSP Android 上的 Termux 运行的 code－server 完成）

旧 blog 的内容将在近日开始迁移，不过考虑下从有想法到成功的 Hugo 页面部署所用的时间。我想迁移所需要的时间也不会少。

# 常用命令

1. 新建 post 页面
  `hugo new content [path_of_post_file]` -k [archetype_name]  
  示例：`hugo new content posts/[name].md -k post`
  > “content” 也可以不用添加，直接写文件路径不会有区别。
  
没了，想到哪里写哪里吧。
