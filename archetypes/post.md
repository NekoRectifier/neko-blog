---
date: '{{ .Date }}'
draft: true # remember to modify!
title: '{{ replace .File.ContentBaseName `-` ` ` | title }}'
# weight: 1
# aliases: ["/first"]
tags: []
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
