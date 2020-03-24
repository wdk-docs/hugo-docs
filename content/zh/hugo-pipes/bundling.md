---
title: Asset 捆绑
description: Hugo 管道可以捆绑任何数量的资产在一起.
date: 2018-07-14
publishdate: 2018-07-14
lastmod: 2018-07-14
categories: [asset management]
keywords: []
menu:
  docs:
    parent: "pipes"
    weight: 60
weight: 60
sections_weight: 60
draft: false
---


Asset files of the same MIME type can be bundled into one resource using `resources.Concat` which takes two arguments, a target path and a slice of resource objects.


```go-html-template
{{ $plugins := resources.Get "js/plugins.js" }}
{{ $global := resources.Get "js/global.js" }}
{{ $js := slice $plugins $global | resources.Concat "js/bundle.js" }}
```
