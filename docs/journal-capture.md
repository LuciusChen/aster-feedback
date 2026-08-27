# Journal 与 Capture

[返回首页](../README.md) · [快速开始](quick-start.md)

Journal 是对每日 Org 文件的时间线阅读；Capture 是把一段结构化内容写入 Org 的通用入口。两者经常一起使用，但不是同一件事：Journal 决定“今天的文件和条目如何显示”，Capture Template 决定“每次写入什么、写到哪里”。

![Journal 示例](../assets/screenshots/journal.png)

## 每日 Journal 文件

Aster 创建的每日文件带有稳定的日期标记和一级日期标题：

```org
#+TITLE: Thursday, 27 Aug 2026
#+ASTER_JOURNAL_DATE: 2026-08-27

* Thursday, 27 Aug 2026
** 09:10 Morning notes
Today I want to finish the release checklist.
```

- `#+ASTER_JOURNAL_DATE` 用于稳定识别日期，不依赖显示语言。
- 已有、没有该标记的 Journal 仍可通过标题、一级日期标题或 Denote 文件名识别。
- Journal 的年份、月份和日期使用当前时区的公历；农历只是一种可选显示信息，不改变文件身份。
- 日范围、条目数量和文件命名方式在 **设置 → Journal** 中配置。

## Capture 从哪里进入

在 Journal 轻点底部 `+`，会打开已经配置的 Capture Template；没有模板时，Aster 会引导到 **设置 → Org 与 Capture → Templates → Capture templates**。

长按全局 `+` 也可以从其他页面直接选择 Capture。Agenda 的快速 Event/Task 新建是一个受限、专门的写入器，不会混入通用 Capture 模板。

## 一个最小 Capture Template

例如把一段带时间的笔记写入当天 Journal：

```org
** %<%H:%M> %?
```

建议配置为：

| 字段 | 值 |
| --- | --- |
| Type | Entry |
| Destination | Today's Journal |
| Journal section | 留空，或填写 `Notes` |
| Source | Inline |
| Prepend | 按自己的阅读顺序选择 |

运行时，`%?` 是主要输入位置，`%<%H:%M>` 展开为当前时刻。保存后得到普通 Org 源文本，不会生成 Aster 专有的影子记录。

## Template 类型

### Entry

Entry 用于标题树。模板没有开头星号时，Aster 会补一个标题；写入已有 Outline 时会调整标题层级。

```org
*** TODO %? :inbox:
%U
```

### Plain

Plain 把展开后的文本直接插入目标正文，不自动包成标题。空白 Plain 模板可以像一个普通文本 Capture 缓冲区一样，在运行时输入完整内容。

## 目标位置

Capture 有两类目标：

- **Today's Journal**：运行时解析当天 Journal 文件、日期根标题和可选 Journal section。
- **Workspace Org file**：写入一个工作区相对路径，例如 `agenda/inbox.org`，并可指定 Outline 路径。

如果目标文件不存在，Capture 可以引用一个 **Org File Template** 先初始化文件。文件模板只运行一次；Capture 的正文模板每次运行都会展开。两者不要混淆。

## Inline 与 Template File

- **Inline**：模板正文直接保存在 Aster 的设备设置中。
- **Template File**：每次 Capture 时读取工作区中的文本模板文件，适合由 Emacs 和 Aster 共用模板正文。

Template File 只提供“这一次插入什么”，不会替代新文件初始化模板。

## 支持的运行时字段

| 字段 | 含义 |
| --- | --- |
| `%?` | 主要编辑位置 |
| `%i` | 初始文本 |
| `%a` | 来源；当前没有来源上下文时会询问 |
| `%t` / `%T` | 仅日期 / 含时间的活动时间戳 |
| `%u` / `%U` | 仅日期 / 含时间的非活动时间戳 |
| `%<…>` | 按指定格式展开当前时间，例如 `%<%Y-%m-%d>` |
| `%^{Prompt\|default\|choice}` | 运行时问题、默认值和可选值 |
| `%%` | 一个字面 `%` |

Aster 不执行 `%(...)`、任意 Emacs Lisp、`.dir-locals.el` 或外部函数。像“今天的 Journal”这种常见动态目标由安全的原生目标承担，而不是执行用户代码。

## 媒体与附件

任何生成标题的 Capture 都可以同时添加照片、视频或文件。Aster 会：

1. 为新标题写入或复用 `ID`。
2. 保留标题的 `ATTACH` 标签。
3. 把文件复制到标准 Org Attach 目录。
4. 在标题正文中写入相对附件链接。

附件与文本在同一次提交中处理；如果源文件写入失败，本次新复制的附件不会被假装为成功结果。

## 常见场景

### 快速日志

```org
** %<%H:%M> %?
```

目标选 Today's Journal，适合日常流水记录。

### 读书摘录

```org
** %^{Book} — %?
%U
Source: %a
```

运行时补书名和来源，可同时添加封面或截图。

### 写入任务 Inbox

```org
* TODO %? :inbox:
%U
```

目标选 `agenda/inbox.org`。这属于通用 Capture；如果只是新建普通 Task，TODOs 的 `+` 更直接。

## 写回与连续 Capture

- Capture 以一次持久事务写入目标文件。
- 第一次保存完成后，即使界面索引尚未刷新，立即进行第二次 Capture 也会基于最新已保存文本继续追加。
- 保存期间不会重复接受同一次提交。
- 同步在本地写入成功后进行；Journal、Files 和 Search 都只是同一份源文件的不同投影。
