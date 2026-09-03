<p align="right">🌐 <strong>简体中文</strong> · <a href="README.en.md">English</a></p>

<p align="center"><img src="assets/aster-logo.png" alt="Aster" width="128"></p>

<h1 align="center">Aster 文档与反馈</h1>

<p align="center">面向 iPhone 与 iPad 的原生 Org mode 工作区</p>

Aster 是一款面向 iPhone 和 iPad 的原生 Org mode 工作区。它把用户已有的 `.org` 文件呈现为日程、待办、日志、文件、搜索、提醒与小组件，同时仍以原始 Org 文件和 Org Attach 目录为数据源。

当前文档已对齐主仓库下一版 TestFlight 的 Unreleased 行为，基线为 0.1 (7)（2026-09-03）。

这个仓库用于两件事：

- 提供与主仓库下一版 TestFlight 行为一致的中英文使用文档。
- 集中收集可复现的问题、兼容性反馈和功能建议。

> Aster 仍处于测试阶段。测试重要数据前，请先备份 Org 文件和同目录下的 `data/` 附件目录。

## 从这里开始

1. [快速开始](docs/quick-start.md)：连接工作区、设置 Agenda、Journal 和 Capture。
2. [Org 原文如何变成 Aster 界面](docs/org-and-aster.md)：Event、Task、Project、Habit、Note 与 Container 的分类和可复制示例。
3. [Agenda 与 TODOs](docs/agenda-todos.md)：日期、时间、Overdue、重复与提醒的显示规则。
4. [Journal 与日志条目模板](docs/journal-capture.md)：固定日历、每日文件、模板、媒体和写回规则。
5. [Files、预览、编辑与附件](docs/files-preview-attachments.md)：原生文档阅读、源文本编辑和 Org Attach。
6. [Workflow、Project、Habit 与 Perspective](docs/workflow-project-habit-perspective.md)：自定义关键字、项目进度、习惯、可选 Projects/Anniversaries 模板和自定义视图。
7. [同步、数据安全与冲突](docs/sync-and-safety.md)：Dropbox、iCloud Drive、Nutstore、WebDAV 与备份建议。
8. [提交反馈](docs/feedback.md)：如何提供足够的信息，让问题可以被稳定复现。

## 四个最重要的原则

### Org 文件是事实来源

Aster 不把任务迁移到专有数据库。Agenda、TODOs、Journal、Search、Widget 和本地通知都由真实 Org 文件派生，支持的编辑会写回原文件。

### 类型由 Org 语义决定

Aster 不依赖文件名或某个固定英文关键字猜测类型。有没有 TODO 关键字、时间是否精确到时刻、是否存在 `STYLE=habit`、关键字是否配置为 Project，都会影响条目的归类。

### 未支持的文本应原样保留

Aster 只修改当前操作拥有的字段或源文本范围。未知属性、抽屉、代码块、注释和其他 Org 语法不会因为一次状态或日期编辑而被重新生成。

### 同步不是备份

云端同步会传播删除和冲突结果。测试前仍应保留独立备份；附件备份必须同时包含 `.org` 文件旁的 `data/` 目录。

## 当前界面示例

以下截图来自 Aster 自带的匿名 Demo Workspace，与文档中的 Org 示例使用同一套数据。

| Agenda | TODOs |
| --- | --- |
| ![Agenda 示例](assets/screenshots/agenda.png) | ![TODOs 示例](assets/screenshots/todos.png) |

| Journal | Org Preview |
| --- | --- |
| ![Journal 示例](assets/screenshots/journal.png) | ![Org Preview 示例](assets/screenshots/org-preview.png) |

## 反馈入口

- 遇到错误、闪退、显示异常或数据未按预期写回：提交 [Bug report](https://github.com/LuciusChen/aster-feedback/issues/new?template=bug.yml)。
- 希望讨论新场景或改进交互：提交 [Feature request](https://github.com/LuciusChen/aster-feedback/issues/new?template=feature.yml)。
- 涉及隐私、凭据或尚未公开的文件内容时，不要粘贴到公开 Issue；请只描述最小复现结构，并移除账号、路径、Token、文件正文和附件。

## 文档边界

文档描述当前公开测试版本已经实现的行为，不把设想当成现成功能。界面或 Org 写回规则变化时，应同时更新相应示例和截图。
