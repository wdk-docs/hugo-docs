---
title: 资产压缩
description: Hugo 管道允许压缩任何CSS，JS，JSON，HTML，SVG或XML资源.
date: 2018-07-14
publishdate: 2018-07-14
lastmod: 2018-07-14
categories: [asset management]
keywords: []
menu:
  docs:
    parent: "pipes"
    weight: 50
weight: 50
sections_weight: 50
draft: false
---


Any resource of the aforementioned types can be minifed using `resources.Minify` which takes for argument the resource object.


```go-html-template
{{ $css := resources.Get "css/main.css" }}
{{ $style := $css | resources.Minify }}
```

Note that you can also minify the final HTML output to `/public` by running `hugo --minify`.
