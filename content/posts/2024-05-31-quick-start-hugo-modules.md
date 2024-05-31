---
title: "Hugo Modules 从入门到放弃"
date: "2024-05-31T13:58:39+01:00"
# published: ""
# lastmod: ""
categories: ["编程笔记"]
tags: ["Hugo"]
description: "至少写完了。"
---
所以，我想新起一个 Hugo 网站。假设我保留了几年以来使用 Hugo 的经验，那么，我能做到的最好（快速，简洁，易懂，[DRY](https://zh.wikipedia.org/wiki/%E4%B8%80%E6%AC%A1%E4%B8%94%E4%BB%85%E4%B8%80%E6%AC%A1)……）的方式，该是什么呢？

思考了一下，可能包括（但不限于）以下几点：

- 使用 [Hugo Modules](https://gohugo.io/hugo-modules/) 管理主题及部件
- 把能独立出去的部件（包括 shortcode）全都独立出去以供复用
- 通过设置而不是覆盖文件来自定义站点

以下是一个最低限度的实践例。


## 新建站点

根据 [Quick start | Hugo](https://gohugo.io/getting-started/quick-start/)，先检查一下 Hugo 的版本。

```sh
hugo version
# hugo v0.115.2+extended darwin/amd64 BuildDate=unknown
```

超过要求的 v0.112.0 了，继续。

接下来创建站点。

```sh
hugo new site quickstart
# Congratulations! Your new Hugo site is created in ~/Documents/GitHub/quickstart.

# Just a few more steps and you're ready to go:

# 1. Download a theme into the same-named folder.
#    Choose a theme from https://themes.gohugo.io/ or
#    create your own with the "hugo new theme <THEMENAME>" command.
# 2. Perhaps you want to add some content. You can add single files
#    with "hugo new <SECTIONNAME>/<FILENAME>.<FORMAT>".
# 3. Start the built-in live server via "hugo server".

# Visit https://gohugo.io/ for quickstart guide and full documentation.
```

请注意，这里的提示，以及 [Quick start | Hugo](https://gohugo.io/getting-started/quick-start/) 里接下来的步骤，**不是**我们接下来要做的事情。

接下来 cd 进去看看自动创建的文件。如果您跟我一样，已经用了几年 Hugo 的话，会发现原本叫 `config.yaml`／`config.toml` 的文件改名叫 `hugo` 了。我不是很习惯，但是这样明显更合适。

```sh
cd quickstart
tree -a -L 2 .
# .
# ├── archetypes
# │   └── default.md
# ├── assets
# ├── content
# ├── data
# ├── hugo.toml
# ├── layouts
# ├── static
# └── themes

# 8 directories, 2 files
```

<!-- 
## 测试本地服务器

接下来，我希望能看到自己的站点运行起来，然后再考虑添加个什么主题。本节是 [Really getting started with Hugo | BryceWray.com](https://www.brycewray.com/posts/2022/07/really-getting-started-hugo/) 给出的最简步骤。

第二步，创建 `layouts/index.html`，填写：

```go-html-template
{{ define "main" }}
    {{ .Content }}
  <p>This line is from <code>layouts/index.html</code>.</p>
{{ end }}
```

第一步，创建文件夹 `layouts/_default/`。

第三步，创建 `layouts/_default/baseof.html`，填写：

```go-html-template
<!DOCTYPE html>
<html lang="en" charset="utf-8">
<head>
</head>
<body>
    {{ block "main" . }}
    {{ end }}
</body>
</html>
```

第四步，创建 `layouts/_default/single.html`，填写：

```go-html-template
{{ define "main" }}
    {{ .Content }}
  <p>This line is from <code>layouts/_default/single.html</code>.</p>
{{ end }}
```

第五步，创建 `content/firstpost.md`，填写：

```markdown
---
title: "First post"
---

This line is from `content/firstpost.md`.

[Go to home](/)
```

第六步，创建 `content/firstpost.md`，填写：

```markdown
---
title: "First post"
---

This line is from `content/firstpost.md`.

[Go to home](/)
```

检查此时的文件。

```sh
tree -a -L 2 .
# .
# ├── .hugo_build.lock
# ├── archetypes
# │   └── default.md
# ├── assets
# ├── content
# │   ├── _index.md
# │   └── firstpost.md
# ├── data
# ├── hugo.toml
# ├── layouts
# │   ├── _default
# │   └── index.html
# ├── resources
# │   └── _gen
# ├── static
# └── themes

# 11 directories, 6 files
```

然后，就可以运行本地服务器了：

```sh
hugo server
# Start building sites …
# hugo v0.115.2+extended darwin/amd64 BuildDate=unknown

# WARN  found no layout file for "html" for kind "taxonomy": You should create a template file which matches Hugo Layouts Lookup Rules for this combination.

#                    | EN
# -------------------+-----
#   Pages            |  5
#   Paginator pages  |  0
#   Non-page files   |  0
#   Static files     |  0
#   Processed images |  0
#   Aliases          |  0
#   Sitemaps         |  1
#   Cleaned          |  0

# Built in 23 ms
# Watching for changes in ~/Documents/GitHub/quickstart/{archetypes,assets,content,data,layouts,static}
# Watching for config changes in ~/Documents/GitHub/quickstart/hugo.toml
# Environment: "development"
# Serving pages from memory
# Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
# Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
# Press Ctrl+C to stop
```
 -->


## 添加主题

接下来参考 [Use Hugo Modules | Hugo](https://gohugo.io/hugo-modules/use-modules/) 和 [Hugo Modules: Getting Started ❚ A Scripter's Notes](https://scripter.co/hugo-modules-getting-started/)。

首先，检查一下 Go 的版本。

```sh
go version
# go version go1.22.1 darwin/amd64
```

超过要求的 1.12 了，继续。

接下来把站点转化为一个 Hugo Module，因为只有站点本身是 module 的时候，才能使用别的 module（包括但不限于主题）。

```sh
hugo mod init quickstart
# go: creating new go.mod: module quickstart
# go: to add module requirements and sums:
#         go mod tidy
```

接下来添加一个主题。任何包含了 `go.mod` 文件的主题都可以被用作 theme module，这里用 Quick start 给的 [theNewDynamic/gohugo-theme-ananke](https://github.com/theNewDynamic/gohugo-theme-ananke) 作为例子。

首先检查 [theNewDynamic/gohugo-theme-ananke/theme.toml](https://github.com/theNewDynamic/gohugo-theme-ananke/blob/master/theme.toml)（别的 module 可能是 `hugo.toml`）里的 `min_version` 是否满足。

在 `hugo.toml` 里，添加：

```toml
[module]
  [[module.imports]]
    path = 'github.com/theNewDynamic/gohugo-theme-ananke'
```

然后使用以下命令下载：

```sh
hugo mod tidy
```

最后运行本地服务器，查看结果：

```sh
hugo server
```

检查此时的文件。

```sh
tree -a -L 2 .
# .
# ├── .DS_Store
# ├── .hugo_build.lock
# ├── archetypes
# │   └── default.md
# ├── assets
# ├── content
# ├── data
# ├── go.mod
# ├── go.sum
# ├── hugo.toml
# ├── layouts
# ├── resources
# │   └── _gen
# ├── static
# └── themes

# 10 directories, 6 files
```


## 使用 git（可选）

我有一个攒了很久的 `.gitignore` 文档，这里就直接用了。注意：需要保留 `go.mod` 和 `go.sum`。

```sh
# macOS
.DS_Store
.AppleDouble
.LSOverride

# hugo
.hugo_build.lock
/public/
_gen/
static/pagefind
```

创建 git repo：

```sh
git init
git add --all
```

（可选）在 GitHub 网页上[创建空 repo](https://github.com/new)，然后使用以下命令上传：

```sh
git remote add origin https://github.com/<user>/<site>.git
git branch -M main
git push -u origin main
```


## 部署到 Netlify（可选）

参考 [Host on Netlify | Hugo](https://gohugo.io/hosting-and-deployment/hosting-on-netlify/)，创建 `netlify.toml`。以下是一个最小例子：

```toml
[build]
publish = "public"
command = "hugo --gc --minify"

[build.environment]
HUGO_VERSION = "0.115.2"
HUGO_ENV = "production"
HUGO_ENABLEGITINFO = "true"
```

注意：需要把 `go.mod` 里的 `go 1.22.1` 改成 `go 1.22`（不要最后一个小版本），不然 Netlify 会报错。

以上。


## 优点和缺点

优点：

- 不需要纠结 git submodule
- 文件夹结构更简洁
- 可以使用 [mounts](https://gohugo.io/hugo-modules/configuration/#module-configuration-mounts)

（还有别的吗？）

缺点：无法实时修改和预览主题。作为一个在网站上使用自己修改的主题的人，我装完才发现，啊这，这是个非常大的缺点……所以这篇文章很大可能不会有后续了（。

方案：混合使用传统主题安装方式和 Hugo Module？或许？（好麻烦……）


## 参考链接

- [Really getting started with Hugo | BryceWray.com](https://www.brycewray.com/posts/2022/07/really-getting-started-hugo/)
- [Going Wild with Hugo Modules | CloudCannon](https://cloudcannon.com/blog/going-wild-with-hugo-modules/)
- [Use Docsy as a Hugo Module | Docsy](https://www.docsy.dev/docs/get-started/docsy-as-module/)
- [Quick start | Hugo](https://gohugo.io/getting-started/quick-start/)
- [Use Hugo Modules | Hugo](https://gohugo.io/hugo-modules/use-modules/)
- [Hugo Modules: Getting Started ❚ A Scripter's Notes](https://scripter.co/hugo-modules-getting-started/)
- [Hugo Modules: Importing a Theme ❚ A Scripter's Notes](https://scripter.co/hugo-modules-importing-a-theme/)
