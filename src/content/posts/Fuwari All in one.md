---
title: Fuwari All in one
published: 2025-06-17
description: ''
image: ''
tags: []
category: 'Examples'
draft: false 
lang: ''
---

## 嵌入视频

只需从 YouTube 或其他平台复制嵌入代码，然后将其粘贴到 Markdown 文件中即可。

```html
<iframe width="100%" height="468" src="https://www.youtube.com/embed/5gIf0_xpFPI?si=N1WTorLKL0uwLsU_" title="YouTube video player" frameborder="0" allowfullscreen></iframe>
```

### Bilibili

<iframe width="100%" height="468" src="//player.bilibili.com/player.html?bvid=BV1fK4y1s7Qf&p=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" mute> </iframe>

### YouTube

<iframe width="100%" height="468" src="https://www.youtube.com/embed/5gIf0_xpFPI?si=N1WTorLKL0uwLsU_" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## GitHub 仓库卡片

你可以添加链接到 GitHub 存储库的动态卡片，在页面加载时，存储库信息将从 GitHub API 中提取。

::github{repo="siltal/JetbrainsSplashModify"}

使用代码创建 GitHub 存储库卡 `::github{repo="<owner>/<repo>"}`.

```markdown
::github{repo="siltal/JetbrainsSplashModify"}
```

## 提示卡

支持以下类型的提示: `note` `tip` `important` `warning` `caution`

```md lineNumber=false
:::note
突出显示用户应该考虑的信息，即使浏览时也是如此。
:::
```

:::note
突出显示用户应该考虑的信息，即使浏览时也是如此。
:::

:::tip
帮助用户更进一步可选信息。
:::

:::important
用户成功所必需的关键信息。
:::

:::warning
由于存在潜在风险，需要用户立即关注的关键内容。
:::

:::caution
某一行为的潜在负面后果。
:::

### 自定义标题

提示的标题可以自定义。

```md
:::note[我的自定义标题]
This is a note with a custom title.
:::
```

:::note[我的自定义标题]
This is a note with a custom title.
:::

### GitHub 语法

> [!TIP]
> [GitHub 语法](https://github.com/orgs/community/discussions/16925) 也同样支持.

```md

> [!NOTE]
> The GitHub syntax is also supported.

> [!TIP]
> The GitHub syntax is also supported.
```

## 富有表现力的代码

在这里，我们将探索使用 [Expressive Code](https://expressive-code.com/) 的代码块是什么样子。提供的示例基于官方文档，您可以参考该文档了解更多详细信息。

### 语法高亮

[语法高亮](https://expressive-code.com/key-features/syntax-highlighting/)

#### 常规语法高亮

```js
console.log('This code is syntax highlighted!')
```

#### 渲染 ANSI 转义序列

````md
```ansi

```
````

```ansi

ANSI colors:
- Regular: [31mRed[0m [32mGreen[0m [33mYellow[0m [34mBlue[0m [35mMagenta[0m [36mCyan[0m
- Bold:    [1;31mRed[0m [1;32mGreen[0m [1;33mYellow[0m [1;34mBlue[0m [1;35mMagenta[0m [1;36mCyan[0m
- Dimmed:  [2;31mRed[0m [2;32mGreen[0m [2;33mYellow[0m [2;34mBlue[0m [2;35mMagenta[0m [2;36mCyan[0m

256 colors (showing colors 160-177):
[38;5;160m160 [38;5;161m161 [38;5;162m162 [38;5;163m163 [38;5;164m164 [38;5;165m165[0m
[38;5;166m166 [38;5;167m167 [38;5;168m168 [38;5;169m169 [38;5;170m170 [38;5;171m171[0m
[38;5;172m172 [38;5;173m173 [38;5;174m174 [38;5;175m175 [38;5;176m176 [38;5;177m177[0m

Full RGB colors:
[38;2;34;139;34mForestGreen - RGB(34, 139, 34)[0m

Text formatting: [1mBold[0m [2mDimmed[0m [3mItalic[0m [4mUnderline[0m
```

### 编辑器和终端框架

[编辑器和终端框架](https://expressive-code.com/key-features/frames/)

#### 编辑器框架

````md
```js title="file.js"
console.log('A')
```

```html
<!-- src/content/index.html -->
HTML PAGE
```
````

```js title="file.js"
console.log('A')
```

```html
<!-- src/content/index.html -->
HTML PAGE
```

#### 终端框架

`bat`,`batch`,`cmd`,`console`,`powershell`,`ps`,`ps1`,`psd1`,`psm1`,`sh`,`shell`,`shellscript`,`shellsession`,`zsh`

````md
```powershell title="PowerShell terminal example"
C:\User\Siltal\Desktop>
```
````

```powershell title="PowerShell terminal example"
C:\User\Siltal\Desktop>
```

#### 覆盖框架类型

````md
```sh frame="none"
sh frame="none" 
echo "Look ma, no frame!"
```
```ps frame="code" title="PowerShell Profile.ps1"
# 不使用覆盖，这将会是ps终端的样式
function Watch-Tail { Get-Content -Tail 20 -Wait $args }
New-Alias tail Watch-Tail
```

````

```sh frame="none"
sh frame="none" 
echo "Look ma, no frame!"
```

```ps frame="code" title="PowerShell Profile.ps1"
# 不使用覆盖，这将会是ps终端的样式
function Watch-Tail { Get-Content -Tail 20 -Wait $args }
New-Alias tail Watch-Tail
```

### 文本和行标记

[文本和行标记](https://expressive-code.com/key-features/text-markers/)

#### 行标记，区间标记，插入行，删除行，命名标记，给长标签留出单独的空行

````md
```js {1, 3, 5-7} ins={2} del={"删除":4} {"long long marker":9}
// Line 1 - targeted by line number
// Line 2
// Line 3 - targeted by line number
// Line 4 
// Line 5 - targeted by range "5-7"
// Line 6 - targeted by range "5-7"
// Line 7 - targeted by range "5-7"
// Line 8

// Line 10 - targeted by range 
```
````

```js {1, 3, 5-7}  ins={2} del={"-":4} {"very long long marker":9-10}
// Line 1 - targeted by line number
// Line 2
// Line 3 - targeted by line number
// Line 4 
// Line 5 - targeted by range "5-7"
// Line 6 - targeted by range "5-7"
// Line 7 - targeted by range "5-7"
// Line 8

// Line 10 - targeted by long marker
```

#### 使用diff语法

````md
```diff 
+this line will be marked as inserted
-this line will be marked as deleted
this is a regular line
--- a/README.md
+++ b/README.md
@@ -1,3 +1,4 @@
 no whitespace will be removed either
```

````

```diff
+this line will be marked as inserted
-this line will be marked as deleted
this is a regular line
--- a/README.md
+++ b/README.md
@@ -1,3 +1,4 @@
 no whitespace will be removed either
```

#### 使用diff同时指定语言

````md
```diff lang="js"
  function thisIsJavaScript() {
    // This entire block gets highlighted as JavaScript,
    // and we can still add diff markers to it!
-   console.log('Old code to be removed')
+   console.log('New and shiny code!')
  }
```
````

```diff lang="js"
  function thisIsJavaScript() {
    // This entire block gets highlighted as JavaScript,
    // and we can still add diff markers to it!
-   console.log('Old code to be removed')
+   console.log('New and shiny code!')
  }
```

#### 框选给定的文本

````md
```js "given text" ins=/a?re/ del=/\/\//
function demo() {
  // Mark any given text inside lines
  return 'Multiple matches of the given text are supported';
}
```
````

```js "given text" ins=/a?re/ del=/\/\//
function demo() {
  // Mark any given text inside lines
  return 'Multiple matches of the given text are supported';
}
```

### 文本换行

[Word Wrap](https://expressive-code.com/key-features/word-wrap/)

`warp`是默认的，使用`warp=false`关闭换行

`preserveIndent`是默认的，使用`preserveIndent=false`将换行内容顶头写

#### wrap 与 preserveIndent

````md
```js wrap 
// Example with wrap
function getLongString() {
  return 'This is a very long string that will most probably not fit into the available space unless the container is extremely wide'
}
```

```js wrap preserveIndent=false
// Example with preserveIndent=false
function getLongString() {
  return 'This is a very long string that will most probably not fit into the available space unless the container is extremely wide'
}
```

```js wrap=false
// Example with wrap
function getLongString() {
  return 'This is a very long string that will most probably not fit into the available space unless the container is extremely wide'
}
```


````

```js wrap
// Example with wrap
function getLongString() {
  return 'This is a very long string that will most probably not fit into the available space unless the container is extremely wide'
}
```

```js wrap preserveIndent=false
// Example preserveIndent=false
function getLongString() {
  return 'This is a very long string that will most probably not fit into the available space unless the container is extremely wide'
}
```

```js wrap=false
// Example with wrap=false
function getLongString() {
  return 'This is a very long string that will most probably not fit into the available space unless the container is extremely wide'
}
```

## 折叠区域

[折叠区域](https://expressive-code.com/plugins/collapsible-sections/)

````md
```js collapse={"AAAA":1-5, 12-14, 21-24}
// All this boilerplate setup code will be collapsed
import { someBoilerplateEngine } from '@example/some-boilerplate'
import { evenMoreBoilerplate } from '@example/even-more-boilerplate'

const engine = someBoilerplateEngine(evenMoreBoilerplate())

// This part of the code will be visible by default
engine.doSomething(1, 2, 3, calcFn)

function calcFn() {
  // You can have multiple collapsed sections
  const a = 1
  const b = 2
  const c = a + b

  // This will remain visible
  console.log(`Calculation result: ${a} + ${b} = ${c}`)
  return c
}

// All this code until the end of the block will be collapsed again
engine.closeConnection()
engine.freeMemory()
engine.shutdown({ reason: 'End of example boilerplate code' })
```
````

```js collapse={1-5, 12-14, 21-24}
// All this boilerplate setup code will be collapsed
import { someBoilerplateEngine } from '@example/some-boilerplate'
import { evenMoreBoilerplate } from '@example/even-more-boilerplate'

const engine = someBoilerplateEngine(evenMoreBoilerplate())

// This part of the code will be visible by default
engine.doSomething(1, 2, 3, calcFn)

function calcFn() {
  // You can have multiple collapsed sections
  const a = 1
  const b = 2
  const c = a + b

  // This will remain visible
  console.log(`Calculation result: ${a} + ${b} = ${c}`)
  return c
}

// All this code until the end of the block will be collapsed again
engine.closeConnection()
engine.freeMemory()
engine.shutdown({ reason: 'End of example boilerplate code' })
```

## 行号

[行号](https://expressive-code.com/plugins/line-numbers/)

### 显示行号

````md
```js showLineNumbers
// 显示行号
console.log('Greetings from line 2!')
console.log('I am on line 3')
```

```js showLineNumbers=false
// 不显示行号
console.log('Hello?')
console.log('Sorry, do you know what line I am on?')
```
````

```js showLineNumbers
// 显示行号
console.log('Greetings from line 2!')
console.log('I am on line 3')
```

```js showLineNumbers=false
// 不显示行号
console.log('Hello?')
console.log('Sorry, do you know what line I am on?')
```

### 改变起始行号

````md
```js showLineNumbers startLineNumber=5
console.log('Greetings from line 5!')
console.log('I am on line 6')
```
````

```js showLineNumbers startLineNumber=5
console.log('Greetings from line 5!')
console.log('I am on line 6')
```
