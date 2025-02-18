---
date: '2025-02-18T10:40:07Z'
draft: true # remember to modify!
title: '尝试不通过 Play Store 安装 Notion'
# weight: 1
# aliases: ["/first"]
tags: ['Android', 'Tricks']
author: "NekoRect"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: ""
#canonicalURL: "https://canonical.url/to/page"
disableHLJS: false # to disable highlightjs
disableShare: false
hideSummary: true
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
# ShowPostNavLinks: false # at bottom nav to next or prev post, useless for blog
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: false

# Image
#cover:
#    image: "<image path/url>" # image path/url
#    alt: "<alt text>" # alt text
#    caption: "<text>" # display caption under cover
#    relative: false # when using page bundles set this to true
#    hidden: true # only hide on current single page


#editPost:
#    URL: "https://github.com/<path_to_repo>/content"
#    Text: "Suggest Changes" # edit text
#    appendFilePath: true # to append file path to Edit link
---

无论是在 Notion 的官方页面还是常见的 Android 软件下载站上，都可以看到一个个指向了 Play Store 的链接。即便是在 ApkMirror 上，此应用也被标记为 “被 DMCA 移除”。但是 Notion Android 本身并不依赖 GMS 框架，这就给了我们在国内 de-googled 环境下用上原生 Notion 的机会。

# 下载安装包

Google 大力推行 Split Apks 机制，因而能在网上下载到的也大多是 Split Apks 和少部分安装后重新提取的 Apk 文件。个人建议从 [Aurora Store](https://aurorastore.org/) （Google Play Store 的开源替代）上搜索并下载。

如果可以正常安装的话，那么就不存在后续的步骤。但是在我的实际测试中，似乎 Aurora Store 并不能很好的处理 Notion Split Apks 的安装。其表现为安装总是意外终止，没有错误报告，桌面上也不存在新的图标。  
因而我们还需要安装 [Split Apks Installer (SAI)](https://github.com/Aefyr/SAI) 来处理安装工作。不过在此之前，我们需要先把下载好的 Notion 安装包提取出来。

1. 在 Aurora Store 的下载页面中，在 Notion 下载项目上长按
2. 在弹出菜单中选择 “保存 App Bundle”，如图所示
3. 【可选】随后在 `/sdcard/Downloads/` 目录下，确认导出的 Split Apks zip 文件

# 安装 Split Apks

在下载安装完成 SAI 后，先进入其设置。改动以下选项：

- 安装》专业模式：关闭
- 安装》安装模式：Root（存疑）
- 安装》签名APK：关闭
- 安装》从压缩包中提取APK：开启

随后选择先前提取的 APks zip文件，保持安装选项默认进行安装即可。

安装后的 Notion 在使用中不会报 Google Play 的相关错误，与从 Play Store 下载的版本无异。