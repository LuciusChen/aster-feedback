<p align="right">🌐 <strong>简体中文</strong> · <a href="en/files-preview-attachments.md">English</a></p>

# Files、预览、编辑与附件

[返回首页](../README.md) · [Org 分类与写回](org-and-aster.md)

Files 是工作区的源文件入口。Agenda、TODOs、Journal 和 Search 提供语义视图，Files 则让你检查目录、打开原文件、阅读 Preview，并在需要时直接编辑 Source。

![iOS/iPadOS Org Preview 示例](../assets/screenshots/org-preview.png)

截图来自 iOS/iPadOS；Android 保持相同的 Org 语义层级，但使用 Android 原生排版、导航和文件打开能力。

## 文件树

Files 显示已连接工作区的真实目录层级，包括本地已有文件和同步索引中的远端项目。

- 文件夹名称后的图标只标记 Agenda、Journal 或 Event & Task Inbox 角色，不改变目录本身。
- 角色路径在 Settings 中配置。文件夹长按菜单还可以把该文件夹添加到根 Files 二级菜单或从中移除；这只是导航快捷方式，不改变文件夹角色。
- 空文件夹仍是一个真实文件夹，不会因为没有子项而显示无意义的展开动作。
- Dropbox 可在文件长按菜单中查看版本历史；其他来源不会显示一个无法工作的占位入口。

### 打开指定文件或标题（iOS/iPadOS build 12）

在文件长按菜单或 Org 文档工具栏选择 **Copy Aster Link**，可以从其他应用直接打开该文件。标题的长按菜单也能复制链接；已有的 `ID` 或 `CUSTOM_ID` 会直接复用。没有标识符时，Aster 会先询问是否添加标准 Org `:ID:`，取消不会修改文件。

文件链接形如 `aster://open?file=journal%2F2026-09-06.org`，路径相对于当前工作区。标题链接优先使用 `aster://open?id=your-heading-id`，在工作区内移动或重命名标题后仍能定位。链接不会上传内容，也不能让另一台没有对应文件的设备访问你的工作区。

手写链接还可以使用 `file=<路径>&custom-id=<CUSTOM_ID>`，或 `file=<路径>&heading=<完整标题>`。标题文字必须唯一；重命名后需要更新文字链接。目标缺失、重名或路径不合法时会提示错误，不会随意打开一个近似结果。

### 聚焦标题（iOS/iPadOS build 12）

在 Org Preview 中长按标题并选择 **Focus**，只查看该标题和它的完整子树。顶部保留文件与祖先路径；点击祖先可以扩大范围，点击 **Full Document** 返回原文并恢复原来的阅读位置和折叠状态。

聚焦不会裁剪或重写 Org 文件。此时选择 Edit 仍然编辑完整源码，光标定位到当前标题。附件和链接继续使用原文件的上下文。标题链接加上 `&focus=true`，也可以直接进入同一个聚焦视图。

轻点标题不进入编辑；长按菜单中的 Edit Source 才打开源码。折叠箭头、标签和链接仍各自响应点击。从 Edit 返回 Preview 时，保留当前子树内读到的位置，而不是回到子树开头。

### Apple 快捷指令（iOS/iPadOS build 12）

在快捷指令中搜索 Aster，可选择 **Open Org File**（打开 Org 文件）或 **Open Org Heading**（打开 Org 标题）。文件动作直接选择本地 Org 文件；标题动作可以按标题或文件路径搜索，并显示祖先路径。标题动作中的 **Focus** 默认关闭，开启后只呈现完整子树。

例如，给工作项目的顶层标题创建一个带 Focus 的快捷指令，放在主屏幕。点击后进入该项目子树，使用 Full Document 可以回到完整文件。查询选项不需要 AI，也不会触发同步或修改 Org；新文件需要先在 Aster 中同步到本地。

如果同一文件中有多个同名标题且没有唯一 ID，请先在 Aster 对目标标题使用 Copy Aster Link 并确认添加 ID。无法唯一识别的标题不会出现在快捷指令选项中，以免打开错误的条目。

给标题补上 ID 不会让已有的标题文字或 CUSTOM_ID 快捷指令失效；Aster 会按原来的链接重新定位。原链接依赖的标题或路径改名后，仍需更新对应选择。单个标题无法生成有效链接时，不会影响其他标题的选择列表。

### 文件排序（iOS/iPadOS build 12）

展开文件夹后，点击名称行最右侧的灰色 **⇅**，即可按名称、文档日期或修改时间正序／倒序排列其中的文件，不需要长按。不同层级的排序按钮都靠右对齐；收起文件夹后按钮隐藏，空文件夹不显示。

工作区根目录展开时也显示 ⇅：只有一个根目录时在页头，多个根目录时在各自的名称行。每个文件夹独立记住选择，关闭 Aster 后仍保留；子文件夹不受影响。

文件夹始终排在文件前面，文件夹之间仍按名称排列。缺少日期的文件始终放在文件末尾；日期相同则按名称排列。切换排序不会修改文件、Journal 时间线或左下角的文件夹快捷菜单。

Files 中的 `journal` 文件夹建议选 **文档日期 · 从新到旧**。它沿用 Journal 已有的日期识别规则；没有 Journal 日期时，再读取 Org 的 `#+DATE`。例如 `#+DATE: [2026-09-03 Thu 09:00]` 对应 9 月 3 日，修改旧文件不会把它排到今天。一个文件包含多个 Journal 日期时，沿用第一个日期根的日期。

**修改时间** 使用本地已有文件的修改时间，不代表日记日期。尚未下载或无法识别日期的文件不会为排序而自动下载，获得可用日期后才进入对应位置。排序偏好只保存在当前设备。

## Preview 与 Edit

打开支持的文本文件后：

- **Preview**：用适合阅读的排版呈现 Org 或 Markdown。
- **Edit**：显示并编辑完整源文本。

Preview 不会成为第二份内容。修改在 Edit 中写回原文件；从 Search、Agenda 详情或预览块跳到 Source 时，Aster 会定位到对应源行。

界面刷新会保留新输入的文字和当前选区。全选替换后，写回的仍是你输入的完整源文本。

### 保存冲突保护（iOS/iPadOS build 12）

Org 和 Markdown 保存前会核对打开时的文件版本。同步或其他操作已改变磁盘内容时，Aster 会保留编辑文字，不覆盖新版本。Org 保存失败会阻止打开另一份 Org；Markdown 保存失败后，在同一次应用运行中重新打开该文件仍能找回编辑内容。

保存失败后，工具栏会出现“重新载入已保存文件”。此操作会先询问是否放弃未保存的修改；取消仍保留原文。请在重新载入或退出应用前，先保存或复制需要保留的文字。这种临时恢复内容不同于持久保存的 Journal 草稿。

## Org Preview 支持什么

Org Preview 按语法节点而不是简单正则扫描渲染，包括：

- 多级标题、段落、标签与部分文件元数据
- 表格、列表和复选框
- 链接、图片、标题与说明
- 属性抽屉、普通抽屉和折叠区域
- Source、Example、Quote 等块
- 动态块与相邻结果
- 脚注和内联 LaTeX

标题的标准 LOGBOOK 状态历史默认收在“历史”一行中，便于 Habit 文件一次浏览更多习惯；点击后可展开完整记录。Preview 不会再把同一批状态记录重复显示为普通正文，源文件内容不会因此改变。

未知或暂不渲染的语法仍会保留在 Edit 源文本中。iOS/iPadOS build 14也会在 Preview 中原样显示不支持的 Diary 表达式和未知独立指令；已被界面呈现的标题、日期、标签等元数据不会重复显示。

Aster 新建 Journal 文件时写入的 `#+ASTER_JOURNAL_DATE:` 也属于日期元数据，不会出现在正文中。它仍保留在 Edit 中并用于识别日期；其他未知的 `ASTER_` 指令不会因此被隐藏。

iOS/iPadOS build 14修正了几类阅读边界：`2 * 3 * 4` 的运算符保持原样，合法强调可以跨一行，分隔线正常显示。数字列表、缩进的星号列表以及带 `[@N]` 的列表都使用同一套复选框状态；行末省略 `|` 的表格仍按表格显示。

跨文件夹的文件和图片链接也会正常定位。例如在 `Notes/index.org` 中，`[[file:../Journal/photo.png]]` 指向工作区内的 `Journal/photo.png`。链接不会修改源文件，也不能越过工作区根目录。

### Dynamic Block

例如：

```org
#+BEGIN: dg-argument-gaps
/Updated: 2025-12-29 16:31/

| Claim        | Structural Gaps |
|--------------+-----------------|
| [[dg:c1][A]] | no-source       |

#+END:
```

在 Preview 中，`#+BEGIN:` 和 `#+END:` 是结构边界，不作为正文重复显示；边界之间已经存在的生成内容会按段落和表格渲染。Aster 不运行生成这个块的 Emacs 函数，更新仍应由原来的 Org/Emacs 工具完成。

### Babel

```org
#+begin_src python
print("hello")
#+end_src

#+RESULTS:
: hello
```

Aster 可以折叠、着色并阅读 Source 与 Results，但绝不执行代码。这样既保留源文件，也避免手机预览意外运行用户脚本。

## 非 Org 文件

Markdown 使用独立的 Markdown 阅读器。远端文件需要先下载到应用工作区后才能预览：iOS/iPadOS 使用系统原生文档能力，Android 使用 Aster 的原生阅读器，并在需要时交给兼容的系统 App。Files 不会因此增加一个通用本地 Import 工作区来源。若某个系统支持的格式无法打开，请在反馈中提供平台、扩展名、来源和最小测试文件。

### Markdown 结构修正（iOS/iPadOS build 11 起）

缩进代码仍按字面代码显示，不会误识别成标题或列表。嵌套列表保留层级，Setext 下划线标题和显式换行也按源文本呈现。

这些阅读变化不会重写 Markdown。从预览进入 Edit 仍定位到原块的准确范围，包括标题下划线和列表缩进。

## Org Attach 对应关系

附件使用标准 Org Attach 目录：

```text
<Org 文件所在目录>/data/<ID 前两位>/<ID 其余部分>/<文件名>
```

例如标题是：

```org
* TODO Review contract :ATTACH:
:PROPERTIES:
:ID: 3d94a625-be28-4144-8158-05fbf1288d5c
:END:
[[attachment:contract.pdf]]
```

实际文件路径是：

```text
data/3d/94a625-be28-4144-8158-05fbf1288d5c/contract.pdf
```

`attachment:` 链接、标题 `ID`、`ATTACH` 标签和 `data/` 文件必须一起保留。它不是只允许 `.txt` 的 Aster 私有附件格式。

## 从哪里添加附件

- 新建 Event/Task：直接使用新建页中的 **Attachments**。
- Event/Task 详情：在 Content 之后的 **Attachments** 区域添加或打开。
- Journal 条目：运行生成标题的日志条目模板时选择媒体或文件。
- Files：打开源文档可查看附件链接和对应文件。

## 删除、移动与同步

- 文件或文件夹的删除、移动会通过当前云端来源执行，而不是只在界面隐藏。
- 删除非空文件夹前会明确确认递归范围。
- 远端操作成功后才更新本地树和角色配置。
- 如果一次移动或删除涉及 Agenda、Journal 或 Event & Task Inbox 路径，请随后在 Settings 检查相应角色是否仍有效。
