---
title: 多语言模式
linktitle: 多种语言 和 国际化
description: Hugo支持并排创建多语言网站.
date: 2017-01-10
publishdate: 2017-01-10
lastmod: 2017-01-10
categories: [content management]
keywords: [multilingual,i18n, internationalization]
menu:
  docs:
    parent: "content-management"
    weight: 150
weight: 150	#rem
draft: false
aliases: [/content/multilingual/,/tutorials/create-a-multilingual-site/]
toc: true
---

你应该在你的站点配置了`languages`部分定义可用的语言。

> 另请参见[Hugo多语言第1部分：内容翻译](https://regisphilibert.com/blog/2018/08/hugo-multilingual-part-1-managing-content-translation/)

## 配置语言

下面是一个多语种Hugo项目站点得配置例子:

{{< code-toggle file="config" >}}
DefaultContentLanguage = "en"
copyright = "Everything is mine"

[params]
[params.navigation]
help  = "Help"

[languages]
[languages.en]
title = "My blog"
weight = 1
[languages.en.params]
linkedin = "https://linkedin.com/whoever"

[languages.fr]
title = "Mon blogue"
weight = 2
[languages.fr.params]
linkedin = "https://linkedin.com/fr/whoever"
[languages.fr.params.navigation]
help  = "Aide"
{{< /code-toggle >}}

在 `languages` 块没有定义的所有内容将回退到该key的全局值 (e.g., 版权为英语`en`语言).
这也适用于`params`, 如上`help`证明: 您将获得在法语值`Aide` 和 `Help`在所有语言中没有这个参数设置.

通过上面的配置中，所有的内容，网站地图，RSS提要，paginations，和分类页`/`后面以英文(默认内容语言)呈现接着`/fr`后以法语呈现.

当与前面的问题`Params`在[单一页面模板][singles]工作, 忽略`params`在翻译的值.

`defaultContentLanguage` 设置项目的默认语言. 如果没有设置，默认语言为`en`。

如果默认语言需要低于它自己的语言代码(`/en`)被渲染和其他人一样, 设置 `defaultContentLanguageInSubdir: true`.

只有明显的非全局选项可以为每个语言来覆盖。 全局选项的例子是`baseURL`，`buildDrafts`等。

### 禁用语言

您可以禁用一个或更多的语言。 在一个新的翻译工作时，这可能是有用的。

```toml
disableLanguages = ["fr", "ja"]
```

请注意，您不能禁用默认内容语言。

我们一直在这个作为一个独立的设置，使其通过[OS环境](/getting-started/configuration/#configure-with-environment-variables)更易于设置:

```bash
HUGO_DISABLELANGUAGES="fr ja" hugo
```
如果您已经禁用语言的`config.toml`列表, 你可以在这样的发展使他们:

```bash
HUGO_DISABLELANGUAGES=" " hugo server
```


### 配置多语种多主机

从 **Hugo 0.31** 我们支持多主机配置多种语言。 看到[这个问题](https://github.com/gohugoio/hugo/issues/4027)的详细信息。

这意味着，你现在可以配置每`language`一个`baseURL`:


> 如果`baseURL`被设置在`language`水平，那么所有的语言都必​​须有一个，他们都必须是不同的。

例:

{{< code-toggle file="config" >}}
[languages]
[languages.fr]
baseURL = "https://example.fr"
languageName = "Français"
weight = 1
title = "En Français"

[languages.en]
baseURL = "https://example.com"
languageName = "English"
weight = 2
title = "In English"
{{</ code-toggle >}}

通过上述，两个网站将生成到`public`用自己的根:

```bash
public
├── en
└── fr
```

**所有URL(i.e `.Permalink` etc.)都会从根产生。 所以，上面的英文主页上都会有`.Permalink`设置成`https://example.com/`。**

当你运行 `hugo server` 我们会启动多个HTTP服务器。
您通常会看到这样的事情在控制台:

```bash
Web Server is available at 127.0.0.1:1313 (bind address 127.0.0.1)
Web Server is available at 127.0.0.1:1314 (bind address 127.0.0.1)
Press Ctrl+C to stop
```

服务器之间的实时重载和 `--navigateToChanged` 按预期方式工作。

### 分类和黑色星期五

分类和黑色星期五[配置][config]也可以按语言设定:


{{< code-toggle file="config" >}}
[Taxonomies]
tag = "tags"

[blackfriday]
angledQuotes = true
hrefTargetBlank = true

[languages]
[languages.en]
weight = 1
title = "English"
[languages.en.blackfriday]
angledQuotes = false

[languages.fr]
weight = 2
title = "Français"
[languages.fr.Taxonomies]
plaque = "plaques"
{{</ code-toggle >}}

## 翻译您的内容

有来管理你的内容翻译两种方式。
这两个确保每个页面分配一个语言，并链接到其对应的翻译。

### 按文件名翻译

考虑下面的例子:

1. `/content/about.en.md`
2. `/content/about.fr.md`

第一个文件指定的英语语言被链接到第二位。
第二个文件被指定法语，并链接到第一。

Their language is __assigned__ according to the language code added as a __suffix to the filename__.

By having the same **path and base filename**, the content pieces are __linked__ together as translated pages.

{{< note >}}
If a file has no language code, it will be assigned the default language.
{{</ note >}}

### 通过内容目录翻译

该系统采用了每种语言的不同内容目录。每种语言的内容目录使用`contentDir`参数组。

{{< code-toggle file="config" >}}

languages:
  en:
    weight: 10
    languageName: "English"
    contentDir: "content/english"
  fr:
    weight: 20
    languageName: "Français"
    contentDir: "content/french"

{{< /code-toggle >}}

`contentDir`的值可以是任何有效路径 - 甚至绝对路径引用。
唯一的限制是，内容目录不能重叠。

考虑到与上述配置相结合下面的例子:

1. `/content/english/about.md`
2. `/content/french/about.md`

第一个文件指定的英语语言被链接到第二位。
第二个文件被指定法语，并链接到第一。

Their language is __assigned__ according to the content directory they are __placed__ in.

By having the same **path and basename** (relative to their language content directory), the content pieces are __linked__ together as translated pages.

### 绕过默认的链接。

任何网页在前面的问题共享相同的`translationKey`集将与作为翻译的页面，无论基本名称或位置.

考虑下面的例子:

1. `/content/about-us.en.md`
2. `/content/om.nn.md`
3. `/content/presentation/a-propos.fr.md`

```yaml
# set in all three pages
translationKey: "about"
```

By setting the `translationKey` front matter param to `about` in all three pages, they will be __linked__ as translated pages.


### 本地化固定链接

由于路径和文件名是用来处理连接，所有翻译的页面将共享相同的URL (除了语言子目录).

To localize the URLs, the [`slug`]({{< ref "/content-management/organization/index.md#slug" >}}) or [`url`]({{< ref "/content-management/organization/index.md#url" >}}) front matter param can be set in any of the non-default language file.

For example, a French translation (`content/about.fr.md`) can have its own localized slug.

{{< code-toggle >}}
Title: A Propos
slug: "a-propos"
{{< /code-toggle >}}


At render, Hugo will build both `/about/` and `/fr/a-propos/` while maintaining their translation linking.

{{% note %}}
If using `url`, remember to include the language part as well: `/fr/compagnie/a-propos/`.
{{%/ note %}}

### 捆绑页

为了避免重复文件的负担，每个页面捆绑继承其链接翻译的页面束除了内容文件的资源（降价文件，HTML文件等）。

Therefore, from within a template, the page will have access to the files from all linked pages' bundles.

If, across the linked bundles, two or more files share the same basename, only one will be included and chosen as follows:

* File from current language bundle, if present.
* First file found across bundles by order of language `Weight`.

{{% note %}}
Page Bundle resources follow the same language assignment logic as content files, both by filename (`image.jpg`, `image.fr.jpg`) and by directory (`english/about/header.jpg`, `french/about/header.jpg`).
{{%/ note %}}

## 引用所翻译的内容

要创建的链接翻译内容的列表，请使用类似下面的模板:

{{< code file="layouts/partials/i18nlist.html" >}}
{{ if .IsTranslated }}
<h4>{{ i18n "translations" }}</h4>
<ul>
    {{ range .Translations }}
    <li>
        <a href="{{ .Permalink }}">{{ .Lang }}: {{ .Title }}{{ if .IsPage }} ({{ i18n "wordCount" . }}){{ end }}</a>
    </li>
    {{ end }}
</ul>
{{ end }}
{{< /code >}}

The above can be put in a `partial` (i.e., inside `layouts/partials/`) and included in any template, whether a [single content page][contenttemplate] or the [homepage][]. It will not print anything if there are no translations for a given page.

The above also uses the [`i18n` function][i18func] described in the next section.

### 列出所有可用的语言

在`Page`上`.AllTranslations`可以用来列出所有的翻译, 包括页面本身. 在主页上可以用来建立一个语言导航:


{{< code file="layouts/partials/allLanguages.html" >}}
<ul>
{{ range $.Site.Home.AllTranslations }}
<li><a href="{{ .Permalink }}">{{ .Language.LanguageName }}</a></li>
{{ end }}
</ul>
{{< /code >}}

## 字符串翻译

Hugo 使用 [go-i18n][] 支持字符串翻译. [查看该项目的源代码库][go-i18n-source]找工具，这将帮助您管理您的翻译工作流程.

Translations are collected from the `themes/<THEME>/i18n/` folder (built into the theme), as well as translations present in `i18n/` at the root of your project. In the `i18n`, the translations will be merged and take precedence over what is in the theme folder. Language files should be named according to [RFC 5646][] with names such as `en-US.toml`, `fr.toml`, etc.

{{% note %}}
从 **Hugo 0.31** 您不再需要使用一个有效的语言代码。它可以是任何东西。

查看 https://github.com/gohugoio/hugo/issues/3564

{{% /note %}}

从你的模板中，这样使用`i18n`功能:

```
{{ i18n "home" }}
```

这将使用一个像这样的在`i18n/en-US.toml`里定义 :

```
[home]
other = "Home"
```

通常你想在翻译的字符串使用页面变量。 要做到这一点, 当调用 `i18n` 传 "." 上下文:

```
{{ i18n "wordCount" . }}
```

这将使用像这个在`i18n/en-US.toml`里定义  :

```
[wordCount]
other = "This article has {{ .WordCount }} words."
```
单数和复数形式的例子:

```
[readingTime]
one = "One minute to read"
other = "{{.Count}} minutes to read"
```
然后在模板:

```
{{ i18n "readingTime" .ReadingTime }}
```

## 自定义日期

在写这篇文章的时候，Go还没有为国际日期语言环境的支持，但如果你做了一些工作，你可以模拟它。
例如，如果你想使用法语月份名称， 你可以用这个内容添加一个数据文件如 ``data/mois.yaml``:

~~~yaml
1: "janvier"
2: "février"
3: "mars"
4: "avril"
5: "mai"
6: "juin"
7: "juillet"
8: "août"
9: "septembre"
10: "octobre"
11: "novembre"
12: "décembre"
~~~

...随后指数在模板的非英语日期名像现在这样:

~~~html
<time class="post-date" datetime="{{ .Date.Format '2006-01-02T15:04:05Z07:00' | safeHTML }}">
  Article publié le {{ .Date.Day }} {{ index $.Site.Data.mois (printf "%d" .Date.Month) }} {{ .Date.Year }} (dernière modification le {{ .Lastmod.Day }} {{ index $.Site.Data.mois (printf "%d" .Lastmod.Month) }} {{ .Lastmod.Year }})
</time>
~~~

This technique extracts the day, month and year by specifying ``.Date.Day``, ``.Date.Month``, and ``.Date.Year``, and uses the month number as a key, when indexing the month name data file.

## 菜单

您可以定义菜单为各自独立的语言。
Creating multilingual menus works just like [creating regular menus][menus], except they're defined in language-specific blocks in the configuration file:

```
defaultContentLanguage = "en"

[languages.en]
weight = 0
languageName = "English"

[[languages.en.menu.main]]
url    = "/"
name   = "Home"
weight = 0


[languages.de]
weight = 10
languageName = "Deutsch"

[[languages.de.menu.main]]
url    = "/"
name   = "Startseite"
weight = 0
```

The rendering of the main navigation works as usual. `.Site.Menus` will just contain the menu in the current language. Note that `absLangURL` below will link to the correct locale of your website. Without it, menu entries in all languages would link to the English version, since it's the default content language that resides in the root directory.

```
<ul>
    {{- $currentPage := . -}}
    {{ range .Site.Menus.main -}}
    <li class="{{ if $currentPage.IsMenuCurrent "main" . }}active{{ end }}">
        <a href="{{ .URL | absLangURL }}">{{ .Name }}</a>
    </li>
    {{- end }}
</ul>

```

## 缺少翻译

如果字符串没有翻译当前语言，Hugo将使用值从默认语言。 如果没有默认值设置为空字符串将被显示。

While translating a Hugo website, it can be handy to have a visual indicator of missing translations. The [`enableMissingTranslationPlaceholders` configuration option][config] will flag all untranslated strings with the placeholder `[i18n] identifier`, where `identifier` is the id of the missing translation.

{{% note %}}
Hugo will generate your website with these missing translation placeholders. It might not be suitable for production environments.
{{% /note %}}

For merging of content from other languages (i.e. missing content translations), see [lang.Merge](/functions/lang.merge/).

To track down missing translation strings, run Hugo with the `--i18n-warnings` flag:

```
 hugo --i18n-warnings | grep i18n
i18n|MISSING_TRANSLATION|en|wordCount
```

## 多语言支持主题

为了支持多语言模式，你的主题，一些注意事项必须采取模板中的URL。
如果有一种以上的语言，URL必须符合下列条件：

* 从内置`.Permalink`或`.RelPermalink`来吧
* 可与[`relLangURL` template function][rellangurl] 或者 [`absLangURL` template function][abslangurl]构建  **要么** 与`{{ .LanguagePrefix }}`前缀

如果定义一种以上的语言，在`LanguagePrefix`变量将等于`/en`（或任何你`CurrentLanguage`是）。
如果未启用，这将是一个空字符串（因此对于单一语言Hugo网站是无害的）。

[abslangurl]: /functions/abslangurl
[config]: /getting-started/configuration/
[contenttemplate]: /templates/single-page-templates/
[go-i18n-source]: https://github.com/nicksnyder/go-i18n
[go-i18n]: https://github.com/nicksnyder/go-i18n
[homepage]: /templates/homepage/
[i18func]: /functions/i18n/
[menus]: /content-management/menus/
[rellangurl]: /functions/rellangurl
[RFC 5646]: https://tools.ietf.org/html/rfc5646
[singles]: /templates/single-page-templates/
