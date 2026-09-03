<p align="right">🌐 <strong>简体中文</strong> · <a href="en/journal-capture.md">English</a></p>

# Journal 与日志条目模板

[返回首页](../README.md) · [快速开始](quick-start.md)

Journal 是每日 Org 文件的日历和时间线；日志条目模板决定“一次新日志写入什么”，目标始终是 Today's Journal。Event/Task 是平级的新建流程，写入 Event & Task Inbox；Org 文档则写入单独命名的文件。

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

Journal 与 Agenda 共用同一个会持久化的 Week/Month/Year 日历尺寸，也共用 **设置 → Calendar & Agenda → 农历** 开关。开启或关闭该开关会同时为两个日历添加或移除相同的紧凑农历注释；只读节日日历覆盖也保持一致。日历固定在可独立滚动的时间线上方；下方 Grabber 每次完成拖动只切换一个相邻尺寸，继续拉动时步进会逐渐减小而不是突然停住。Week 每次有效左右松手固定切换五个日历日，Month 每次切换一个月，Year 每次切换一年；三者使用相同的整页方向切换动态，开启 Reduce Motion 后会到达相同结果但减少空间运动。Journal 初次打开会和 Agenda 一样高亮今天，即使最近一篇日志早于今天；时间线和 iPad 横屏详情仍从最近一篇真实日志开始。有日志的日期会显示标记；点击有标记的日期可把时间线移动并选中到当天，没有标记的日期只用于浏览，不会替换当前选中的真实 Journal 日。明确选择或继续滚动时间线后，日历选中日期和当前显示周期会保持同步；如果选中日仍然存在，后台刷新也会保留用户另外浏览到的日历周期。时间线沿用 Agenda 的紧凑日期标题和细分隔线，直接显示时间、标题与元数据；每日文件名不再重复占一行。

Week 不是普通七日页，而是和 Agenda 共用的五列事件日期轨道。它按已有日记日期和外部全天日历日期排列，并在内容边界外保留相邻空日期；一次有效松手固定切换五个日历日，并使用和 Month、Year 相同的单次整页方向动画，不会再叠加第二次吸附或弹簧回弹。Week 中的空日期也只移动当前浏览位置，不会清除最后一个有真实来源的 Journal 选中日。iPad 横屏左栏沿用同一五列轨道，点到空日期后右侧仍显示此前选中日期的真实日志内容。

Year 总览也会使用这些相同输入：Journal 日期、传统节日和已选择的只读系统节日共同形成密度，不会在切到 Year 后丢失农历/节日语义。

**设置 → Journal** 中同样写作 Week/Month/Year 的选项只限制 Journal 要准备的历史范围、日期标记和 Journal 搜索内容，不会改变页面当前的日历尺寸，也不会在 Journal 页面增加第二个范围选择器。

## 从哪里新建日志条目

在 Journal 轻点“新建”，会打开已经配置的日志条目模板；没有模板时，Aster 会引导到 **设置 → 新建与存储 → 日志条目模板**。Journal 设置中也有同一入口。
模板保持与 Emacs 一致的“先选择、再填写”顺序，并以紧凑列表行显示；行内 Org 预览没有额外卡片背景。选择模板后的填写页同样使用紧凑原生分区：输入默认从一行开始，实时预览和待上传附件不再嵌套卡片，照片、相机和文件入口保留完整点击范围但不使用大块按钮底。

长按全局“新建”也可以从其他页面选择 Journal。Agenda 的 Event/Task 新建是独立写入器，不会混入日志模板。

## 一个最小日志条目模板

例如把一段带时间的笔记写入当天 Journal：

```org
** %<%H:%M> %?
```

建议配置为：

| 字段 | 值 |
| --- | --- |
| Type | Entry |
| Destination | Today's Journal（固定） |
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

日志条目模板的目标始终是 **Today's Journal**：运行时解析配置的 Journal 文件夹、每日日志文件名、日期根标题和可选 Journal section，不提供任意工作区文件路径。如果每日日志文件不存在，模板可以引用 **Org Document Template** 初始化它。初始化只运行一次，条目正文每次都会展开。

## Org Document Template 与占位符

Org Document Template 只在 Aster 第一次创建 `.org` 文件时生成文件头。可在 **设置 → 新建与存储 → Org 文档模板** 中新建和编辑，并在日志条目模板的 **New file template** 中选择。

| 占位符 | 展开结果 |
| --- | --- |
| `{title}` | 新建文件时提供的单行文档标题 |
| `{date}` | 当前公历日期，格式为 `YYYY-MM-DD` |
| `{created}` | 含当前时分的非活动 Org 时间戳，例如 `[2026-08-28 Fri 09:30]` |
| `{identifier}` | 新生成的小写 UUID |

例如，把 Emacs Denote 的 Journal 文件头改写为 Aster 文件模板：

```org
#+title:      {title}
#+date:       {created}
#+filetags:
#+identifier: {identifier}
#+startup: indent
#+ARCHIVE: journal.org::
```

`#+filetags:` 在这里是一个留空的普通 Org 字段；Aster 当前没有 `{filetags}` 占位符。也可以直接在模板中填写固定标签。Emacs 的 `org-download-image-dir`、`.dir-locals.el` 和 Lisp 表达式不是文件头内容，不能放进该模板；Aster 添加的媒体使用 Org Attach 管理。

## Inline 与 Template File

- **Inline**：条目正文直接保存在 Aster 的设备设置中。
- **Template File**：每次新建日志条目时读取工作区中的文本模板文件，适合由 Emacs 和 Aster 共用模板正文。

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

任何生成标题的日志条目模板都可以同时添加照片、视频或文件。Aster 会：

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

## 写回与连续日志条目

- 日志条目以一次持久事务写入当日日志文件。
- 第一次保存完成后，即使界面索引尚未刷新，立即新建第二条日志也会基于最新已保存文本继续追加。
- 保存期间不会重复接受同一次提交。
- 同步在本地写入成功后进行；Journal、Files 和 Search 都只是同一份源文件的不同投影。
