---
title: 静态网站生成器的好处
linktitle: 静态的好处
description: 改进的性能，安全性和易用性是短短的原因静态网站生成器如此吸引人。
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
keywords: [ssg, static, performance, security]
menu:
  docs:
    parent: "about"
    weight: 30
weight: 30
sections_weight: 30
draft: false
aliases: []
toc: false
---

The purpose of website generators is to render content into HTML files. Most are "dynamic site generators." That means the HTTP server---i.e., the program that sends files to the browser to be viewed---runs the generator to create a new HTML file every time an end user requests a page.

Over time, dynamic site generators were programmed to cache their HTML files to prevent unnecessary delays in delivering pages to end users. A cached page is a static version of a web page.

Hugo takes caching a step further and all HTML files are rendered on your computer. You can review the files locally before copying them to the computer hosting the HTTP server. Since the HTML files aren't generated dynamically, we say that Hugo is a _static site generator_.

This has many benefits. The most noticeable is performance. HTTP servers are _very_ good at sending files---so good, in fact, that you can effectively serve the same number of pages with a fraction of the memory and CPU needed for a dynamic site.

## 更多关于静态网站生成器

- ["An Introduction to Static Site Generators", David Walsh][]
- ["Hugo vs. Wordpress page load speed comparison: Hugo leaves WordPress in its dust", GettingThingsTech][hugovwordpress]
- ["Static Site Generators", O'Reilly][]
- [StaticGen: Top Open-Source Static Site Generators (GitHub Stars)][]
- ["Top 10 Static Website Generators", Netlify blog][]
- ["The Resurgence of Static", dotCMS][dotcms]

["an introduction to static site generators", david walsh]: https://davidwalsh.name/introduction-static-site-generators
["static site generators", o'reilly]: https://www.oreilly.com/web-platform/free/files/static-site-generators.pdf
["top 10 static website generators", netlify blog]: https://www.netlify.com/blog/2016/05/02/top-ten-static-website-generators/
[hugovwordpress]: https://gettingthingstech.com/hugo-vs.-wordpress-page-load-speed-comparison-hugo-leaves-wordpress-in-its-dust/
[staticgen: top open-source static site generators (github stars)]: https://www.staticgen.com/
[dotcms]: https://dotcms.com/blog/post/the-resurgence-of-static
