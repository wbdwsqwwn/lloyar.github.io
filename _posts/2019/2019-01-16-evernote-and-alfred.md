---
layout: article
title: 当 Evernote 遇上 Alfred
date: '2019-01-16 11:17:00 +08:00'
key: '2019-01-16_11:17'
tags:
    - Tools
    - macOS
---

**Overview**

Alfred 是 Mac 上当之无愧的效率神器，强大的搜索功能以及可自定义的工作流，可以极大限度的发挥出 macOS 自动化脚本的威力。

而今天的另一个主角，就是个人知识管理界的大佬 Evernote ，国内版名为印象笔记。

当它们俩结合在一起时，会产生怎样神奇的反应？

<!--more-->

## 前言

由于众所周知的原因，很多国外软件来到中国后都会被~~阉割~~修改，并进行一定的本土化改造。一方面，软件语言的本地化有利于国内用户使用，而另一方面，造成的就是功能上的限制与缺失。

对于普通用户来说，功能上的精简其实并不会造成多大麻烦，反而会让用户更加容易上手。但对爱折腾的人来说，阉割就不那么令人愉快了。

但印象笔记国内版也算不上被阉割，虽说和很多社交软件的关联被取消，但是增加了国内社交软件的关联（微博、微信等等）。其实就算不阉割这些国外社交但关联，身处天朝也基本上不能用。

而且国内的团队非常给力，以非常灵敏的反馈速度支持了呼声一直很高的 Markdown 。昨天刚刚发布的 9.0.0 版本又迎来了桌面便签功能。那大洋彼岸的 Evernote 呢？能躺着赚钱为什么要努力，怕是仍旧活在 6102 年吧 ，不思进取啊喂。

其实 Alfred 官方论坛上已经有比较完美的 Evernote 的 Workflow，具体信息请看[这里](https://www.alfredforum.com/topic/840-evernote-workflow-9-beta-3/)，不过很可惜，**你用不了**。 😊

这是因为 Evernote 和印象笔记分家了，成为了互相独立的两款软件。这就导致在 Mac 上两款应用软件的签名不同，Alfred 为 `Evernote.app` 打造的 Workflow 无法正确识别 `印象笔记.app` 。

为了不重复造轮子，可以将脚本中涉及到 `com.evernote.Evernote` 的全部改为 `com.yinxiang.Mac` 即可。

不过原脚本文件必须在安装了 Evernote 的机器上执行过一次才能正常打开源码进行修改，而国内 App Store 没有上架国际版印象笔记，呵呵哒。为此我申请了一个美区的 AppleID 。

为了不让后来者继续这么折腾，在 [这里](/assets/files/yinxiang.alfredworkflow) 把修改过后的 `.alfredworkflow` 文件分享出来。

## 正文 - 使用说明

### 搜索

#### 关键词

| Keywords       | Explain        | e.g.     |
| :------------- | :------------- | :------------- |
| `ens`          | 在所有笔记中搜索  | `ens my query...` |
|`ens @ :`       | 在指定的笔记本中搜索 |`ens @MyFirstNotebook : my query...` |
|`ens # :`   |在指定的标签中搜索  |`ens #tags : my query...` |

你能使用 `ent` （只在标题中搜索）或者 `enr` （在提醒中搜索）或者 `entodo` （在 `to-do` 待办事项的笔记中搜索）或者 `enrec` （搜索最近一周的笔记）或者 `enu` （搜索笔记的原始URL）来代替 `ens` 关键字。

你能选择多个标签进行更加准确的搜索，只需要添加第二个井号并选择或输入一个标签。
e.g. `ens #tag1 #tag2 :my query`

另外，你能选中一个单独的笔记本之后再选中一个标签进行搜索。
e.g. `ent @notebook #tag1 #tag2 :my query`

### 动作

- `Return` 键打开笔记
- `Shift` 键预览笔记
- `Option` 键设置提醒
- `Control` 键向最顶层应用粘贴笔记内容
- `Function` 键打开笔记URL
- `Command` 键添加文本（可以来自剪切板、被选中的文本或者输入的内容）或者在 Finder 中选中文件。按下 `Command` 键后，一个新的 Alfred 窗口将会呈现出来，你将可以选择文本来源和一些动作：
  - Return 键将不会附加日期
  - Option 键将会附加当前日期

提示：你也可以使用 `Command` 键仅向笔记添加一个标签。键入或者选择一个标签并且在冒号后面不要输入任何东西，之后选择 “Type a Note” e.g. `enn #tag :`。[^1]
{:.warning}

[^1]: **Hint**: You can also use the `Command` key to only add tags to a note. To do so, type or select a tag and don’t type anything after the colon then select the source “Type a Note” e.g. `enn #tag :`

请注意，Alfred Fallback Search 需要被支持（你必须在 Alfred 3 的设置里进行添加，Preferences>Features>Default Results，之后选择 Setup fallback results 按钮）。

## 创建

### 关键词 `enn`

![16-keywords-enn](/images/2019/01/16-keywords-enn.png)

Create

你能可选的键入笔记标题，或者效仿下列语法进行更为复杂的笔记创建：

@Notebook #tag1 #tag2 !reminder :Title

- @notebook: after typing @ a list of notebooks will be displayed then select one or type it; the default will be used if omitted
- #tags: after typing # a list of tags will be displayed then select one or type a new one (multiple tags are supported, type each one after a hash sign)
- !reminder: after typing an exclamation point a list of reminder suggestions will be displayed then select one or type a custom reminder such as in 4 days or
05/01/2014 or 05/01/2014 at 2:00
- Title: at the end, after a colon (or the second colon if you are adding time in your reminder)

Note that items of the syntax are optional, however the syntax has to end with a colon, with or without typing the note title e.g. #tag1 :

### 笔记内容来源

- 从剪切板
- 从被选中的文本
- 直接在 Alfred 中键入
- 从 Safari 或 Google Chrome 的 URL
- 从 Mail.app 中被选中的消息（单条或多条）
- 从 Finder.app 中被选中的文件（单个或多个）：你可以创建附带一个文件的单条笔记或者附带所有被选择的文件的单条笔记。Alfred 文件浏览器同样支持。

### 动作

`Return` 键: 创建一条笔记
`Control` 键: 创建一条笔记并打开它
`Command` 键: 附加文本或文件到一条笔记上
`Option` 键: 附加文本到一条笔记上并加上当前日期

### 如何进行追加

1. Highlight one of the note content source e.g. From Clipboard
1. 可选的键入标签和提醒 e.g. `#tag1` `#tag2` `!tomorrow`
1. 按住 `command` 键并敲击 `return` 键
1. 从列表中选择一条笔记（仅在搜索标题时）并敲击 `return` 键

### 邮件

- 消息的主题作为笔记标题
- 消息的回复日期作为笔记的创建日期
- Message Link 作为笔记的源 URL
- A short header (e.g. sender)
- 电子邮件内容的纯文本版本

### 偏好设置

唤出 Alfred 并键入 `enpref` 关键字：

- Search Wildcard: Manual
: 手动输入`*`进行通配符搜索，速度较快

- Search Wildcard: Automatic
: 自动进行通配符搜索，比较耗时

## 结尾

第一次翻译官方文档，好累啊。翻译不好的地方我在页尾引出原文了，不理解的童鞋可以对照原文查看。

虽说功能有这么多，但是我估计大家平时用得上的功能也就是搜索和简单的创建吧。而且现在印象笔记在菜单栏的小工具存在感这么强（有了桌面便签后更是如此），放着这么方便的功能不用，通过 Alfred 命令行进行创建笔记怎么都觉着麻烦呀。

不得不感叹 macOS 的强大，不同 app 直接的联动能做到这种程度，简单又好用，各个软件的功能就像积木一样，可以随意拆开再重新拼凑。这在 Windows 中想都不敢想，我相信 Windows 肯定也支持不同应用之间进行通讯，但是代价肯定相当之高。

Apple 的自动化脚本支持 JavaScript ，加上我一直都想见识一下这个所谓面向原型的“神奇”语言到底是怎样一番风情，所以在未来肯定会学习一下 JavaScript ，这里先挖个**坑**，立个 Flag 🚩 。

当然，我对前端没兴趣，仅仅是好奇什么是面向原型而已。
