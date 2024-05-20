---
author: loikein
# published: "2021-04-15 18:07:02.530 +0200"
# lastmod: "2021-04-15 20:24:48.363 +0200"
slug: playground
description: "有些例子还挺魔性的，为了心理健康建议别看全文。"
featured_image: "/post-img/background-sky.jpg"
enableMathJax: true
categories:
- 独立页面
title: 实验田
_build:
  list: never
---
## Custom Shortcodes

### Quote

Adapted from: [loikein/hugo-tufte/layouts/shortcodes/blockquote.html](https://github.com/loikein/hugo-tufte/blob/main/layouts/shortcodes/blockquote.html), original author: [shawnohare (Shawn O'Hare)](https://github.com/shawnohare).

Including at least one of `author` or `cite` will begin the footnote, and `detail` is optional.

{{< quote
author=`[Somebody famous](#)`
cite=`[Some book famous](#)`
detail=`pp.256-257` >}}
Hello world.
{{< /quote >}}

Usage:

```html
{{</* quote
author=`[Somebody famous](#)`
cite=`[Something famous](#)`
detail=`pp.256-27` */>}}
Hello world.
{{</* /quote */>}}
```

### diffcode

Adapted from: [CloudCannon/alto-hugo-template/layouts/shortcodes/diffcode.html](https://github.com/CloudCannon/alto-hugo-template/blob/main/layouts/shortcodes/diffcode.html) and [CloudCannon/alto-hugo-template/layouts/partials/diffcode.html](https://github.com/CloudCannon/alto-hugo-template/blob/main/layouts/partials/diffcode.html).

{{< diffcode >}}
```sh
[submodule "something"]
    path = something
    url = https://github.com/something/something.git
+    ignore = untracked
-    branch = new-branch
```
{{< /diffcode >}}

Usage: \(add `+` or `-` \(no space\) before the lines to be highlighted\)

````html
{{</* diffcode */>}}
```sh
[submodule "something"]
    path = something
    url = https://github.com/something/something.git
+    ignore = untracked
-    branch = new-branch
```
{{</* /diffcode */>}}
````

### Two-column layout \(new version\)

{{< cols "zh-Hans,en,ja" >}}
你好世界
||
Hello world

Hello world [test](#)[^5]
||
こんにちは
{{< /cols >}}

[^5]: test

Usage:

```html
{{</* cols "zh-Hans,en,ja" */>}}
你好世界
||
Hello world
||
こんにちは
{{</* /cols */>}}
```

### Ruby

{{< ruby "你好世界" "hello world" >}}　

Usage:

```html
{{</* ruby "你好世界" "hello world" */>}}
```

### kbd

{{< kbd I hate `typing` >}}　

Usage:

```html
{{</* kbd I hate `typing` */>}}
```

### Summary \& Nested shortcodes

{{< fold  "我是谁？`我在哪？`" >}}
{{< md >}}Hello world{{< /md >}}

{{< highlight text >}}
0 Oo Ii Ll 1 | 2 Z 5 s 8 Bb 6 # * ^ ~ () {} [] . , : ; “ ‘ ’ `
{{< /highlight >}}
{{< /fold >}}

Usage:

````html
{{</* fold  "我是谁？`我在哪？`" */>}}
{{</* md */>}}Hello world{{</* /md */>}}

{{</* highlight text */>}}
0 Oo Ii Ll 1 | 2 Z 5 s 8 Bb 6 # * ^ ~ () {} [] . , : ; “ ‘ ’ `
{{</* /highlight */>}}
{{</* /fold */>}}
````

### Figure

Local picture:

{{< figure
folder="sticker"
name="shocked.png"
alt="I did not understand but I was shocked"
h="200px" >}}

Usage:

```html
{{</* figure
folder="sticker"
name="shocked.png"
alt="I did not understand but I was shocked"
h="200px" */>}}
```

Remote picture:

{{< figure 
src="https://raw.githubusercontent.com/dracula/sublime/master/images/html.png"
alt="Dracula theme HTML demo for Sublime Text"
link="https://github.com/dracula/sublime/blob/master/README.md" >}}

Usage:

```html
{{</* figure 
src="https://raw.githubusercontent.com/dracula/sublime/master/images/html.png"
alt="Dracula theme HTML demo for Sublime Text"
link="https://github.com/dracula/sublime/blob/master/README.md" */>}}
```

### Video

{{< video name="scream" folder="sticker" mute="false" w="50%" >}}

Usage:

```html
{{</* video name="scream" folder="sticker" mute="false" w="50%" */>}}
```

### Emoji sticker

Static:

{{< sticker name="css-is-awesome.png" h=120 animated="false" >}}

Usage:

```html
{{</* sticker name="css-is-awesome.png" h=120 animated="false" */>}}
```

Animated: not looping vs. looping

{{< sticker name="nice" h=200 loop="false" >}}
{{< sticker name="nice" h=200 >}}

Usage:

```html
{{</* sticker name="nice" h=200 loop="false" */>}}
{{</* sticker name="nice" h=200 */>}}
```

### Link card

{{< preview "https://blog.ypertex.com/articles/useful-hugo-templating/" >}}

Usage:

```html
{{</* preview "https://blog.ypertex.com/articles/useful-hugo-templating/" */>}}
```

### Fediverse post

Currently only supports Mastodon, because I do not have accounts on other types of servers.

No content warning (CW):

{{< mstdn mastodon.social 106070125652504343 >}}

Has CW:

{{< mstdn "mastodon.social" "106353678279784184" >}}

Usage:

```html
{{</* mstdn mastodon.social 106070125652504343 */>}}
```

### Twitter tweets

~~Currently waiting for [gohugoio/hugo Issue #5617](https://github.com/gohugoio/hugo/issues/5617) to be implemented.~~

It will never happen due to changes in the Twitter API v2.

<!-- {{< twitter 877500564405444608 >}} -->

{{< twitter 1621026986784337922 >}}

Usage:

```html
{{</* twitter 1621026986784337922 */>}}
```

### Bilibili

{{< bilibili 52046593 >}}

Usage: \(supports both AV and BV\)

```html
{{</* bilibili 52046593 */>}}
```

### YouTube-nocookie

{{< youtube id=kF8I_r9XT7A title="How to Become Pope" >}}

Usage:

```html
{{</* youtube id=kF8I_r9XT7A title="How to Become Pope" */>}}
```

***

## Regular Markdown Stuff

Begin [test file](https://gist.github.com/loikein/27ef6913386b206d1b3c18b8e93c5768)…

### Formatting

<span lang="en">**Bold**, __bold__</span>, **加粗**

<span lang="en">*Italic*, _italic_</span>, *斜体*

<u>Underline</u>, <underline>underline</underline>

<del>Strike</del>, <s>strike</s>, ~~strike~~, ~strike~, --strike--

<mark>Highlight</mark>, ==highlight==

<!-- Comments-->

Footnote[^1], footnote[^2]

[^1]: The linked footnote appears at the end of the document.

[^2]: New lines

---


### Lists

- `ul`
- Unordered list

1. `ol`
1. Ordered list

`dl`
:   `dt`
:   Description list

- [x] Task list
- [ ] Task list

### Code

Inline `code`, `` `escape` ``, and <kbd>keystroke</kbd>

```html
<h1>hello world</h1>
```

```js
console.log("hello world");
alert("hello world");
```

{{< highlight go "linenos=table,hl_lines=8 15-17,linenostart=199" >}}
// GetTitleFunc returns a func that can be used to transform a string to
// title case.
//
// The supported styles are
//
// - "Go" (strings.Title)
// - "AP" (see https://www.apstylebook.com/)
// - "Chicago" (see https://www.chicagomanualofstyle.org/home.html)
//
// If an unknown or empty style is provided, AP style is what you get.
func GetTitleFunc(style string) func(s string) string {
  switch strings.ToLower(style) {
  case "go":
    return strings.Title
  case "chicago":
    return transform.NewTitleConverter(transform.ChicagoStyle)
  default:
    return transform.NewTitleConverter(transform.APStyle)
  }
}
{{< /highlight >}}

### Font

> 我能体に傷つけないで吞下 259 ml glass。

> Four score and seven years ago our fathers brought forth upon this continent, a new nation, conceived in Liberty, and dedicated to the proposition that all men are created equal.

0 Oo Ii Ll 1 | 2 Z 5 s 8 Bb 6 # * ^ ~ \(\) {} \[\] . , : ; “ ‘ ’ \`

```
0 Oo Ii Ll 1 | 2 Z 5 s 8 Bb 6 # * ^ ~ () {} [] . , : ; “ ‘ ’ `
```

### Inline HTML

ref: https://burk.io/2020/let-there-be-dark

<div title="#282a36" style="height: 50px; width: 100px; background-color: #282a36; display: inline-block; border-style: solid; border-color: black; color:white; padding:10px;">#282a36</div>

<div title="#f8f8f2" style="height: 50px; width: 100px; background-color: #f8f8f2; margin-right: 5px; display: inline-block; border-style: solid; border-color: black; color:black; padding:10px;">#f8f8f2</div>

### LaTeX & Table

$\LaTeX{}$

$$R_1 \begin{cases} >\mu_{2} \\ \leq \mu_{2} \end{cases}$$

| Message to agent 1 | $M_1$          |
| ------------------ | -------------- |
| Agent 1's action   | $a_1$          |
| New finding        | $R_1 \begin{cases} >\mu_{2} \\ \leq \mu_{2} \end{cases}$ |
