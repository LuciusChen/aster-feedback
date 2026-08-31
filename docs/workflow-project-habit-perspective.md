<p align="right">🌐 <strong>简体中文</strong> · <a href="en/workflow-project-habit-perspective.md">English</a></p>

# Workflow、Project、Habit 与 Perspective

[返回首页](../README.md) · [Org 分类总表](org-and-aster.md)

Aster 把“状态”“项目”“习惯”和“视图”拆成四层语义：Workflow 来自 Org，Project 是某些进行中状态的角色，Habit 是带重复计划的任务特征，Perspective 则只是一个已保存的筛选与排列方式。

## Workflow 以 Org 为准

最小 Workflow：

```org
#+TODO: TODO | DONE
```

更完整的例子：

```org
#+TODO: TODO(t) NEXT(n) PROJECT(p) WAITING(w@/!) | DONE(d!) CANCELLED(c@)
```

- `|` 左侧是 Process，右侧是 Terminal。
- 括号内可以包含快捷键和进入/离开状态时的日志规则。
- Aster 保存完整关键字，不把自定义状态压成固定的 TODO/DONE。
- 文件内 `#+TODO:` 或 `#+SEQ_TODO:` 优先于应用的全局默认值。
- `#+STARTUP:` 和最近子树的 `LOGGING` 属性可继续覆盖完成日志行为。

在 **设置 → Tasks & Workflow → Workflow** 中可调整顺序、Process/Terminal、图标、颜色、快捷键和状态历史。界面配置仍应对应标准 Org 声明。

### 多套并行 Task Flow 与切换

Org 允许同一个文件并行定义多套互不相同的状态序列：

```org
#+TODO: TODO(t) | DONE(d)
#+SEQ_TODO: REPORT(r) BUG(b) | FIXED(f)
#+TODO: | CANCELED(c)

* TODO Prepare release
* BUG Crash when opening preview
* CANCELED Retired experiment
```

- 每个关键字只能属于一套序列；当前标题上的关键字决定它正在使用哪套流程。
- `|` 的一侧可以为空。`TODO |` 是只有 Process 的流程，`| CANCELED` 是只有 Terminal 的流程，二者都属于标准 Org 语法。
- 对这类单侧流程使用普通完成/重开操作时，如果目标侧本来就没有关键字，Aster 会保持原文不变，绝不会补写未声明的 `DONE` 或 `TODO`；如需改变状态，应从状态选择器明确切到另一套流程。
- 在 Aster 的状态选择器中，只有一套有效流程时继续显示紧凑的 Process/Terminal；存在多套时则按 **Task Flow 1 / 2 / 3** 分组，一次点击即可从 `TODO` 切到 `REPORT`、`FIXED` 或 `CANCELED`。
- 切换只把标题行里的当前关键字替换掉，例如 `* TODO Title` 变成 `* REPORT Title`。Aster 不删除旧流程、不修改其他标题，也不会写入私有的 sequence ID。
- 切到另一套流程的 Terminal 状态时，完成判断、`CLOSED:` 和进入/离开日志仍来自相应的 Org 状态定义。
- 如果标题带 Repeater，从 `TODO` 直接完成为另一套流程的 `FIXED`，Aster 会像 `org-todo` 一样推进重复日期，并回到原流程的 `REPEAT_TO_STATE`、配置目标或第一个 Process 状态，而不是擅自改成 `REPORT`。

文件内定义始终只影响该文件；没有文件内定义时才使用 **设置 → Tasks & Workflow → Workflow** 的全局 Task Flow。高级 Org Syntax 可直接编辑每一行的完整 token，结构化编辑器则用于常见的状态、样式与日志配置。

## Project 不是一个固定英文关键字

在某个 Process 状态上开启 **Treat as Project** 后，该状态的标题才具有 Project 角色。默认的 `PROJECT` 只是方便开箱使用；`PRO`、`ACTIVE` 或中文关键字同样可以被配置为 Project。

```org
* PROJECT Ship TestFlight [2/3]
:PROPERTIES:
:COOKIE_DATA: todo
:END:
** DONE Register identifiers
** DONE Upload build
** NEXT Invite testers
```

Project 仍是 Workflow 条目，因此默认出现在 TODOs 和匹配的 Perspective，而不是一个硬编码的 Projects 页面。**设置 → Tasks & Workflow → Views** 中可以开启内置 Projects 模板；它只是按 Treat as Project 角色创建一条普通 Perspective，之后仍可编辑或关闭，不会改变 Org 原文。

### 进度由谁统计

在 Project 详情中选择一个数据源：

| 选择 | 写入 | 适合场景 |
| --- | --- | --- |
| Automatic | 不写 `COOKIE_DATA` | 只有一种清晰来源 |
| Checklist | `:COOKIE_DATA: checkbox` | 正文中的 `[ ]` / `[X]` |
| Direct subtasks | `:COOKIE_DATA: todo` | 只统计直接子任务 |
| All subtasks | `:COOKIE_DATA: todo recursive` | 统计全部后代任务 |

Checklist 示例：

```org
* PROJECT Publish article [1/3]
:PROPERTIES:
:COOKIE_DATA: checkbox
:END:
- [X] Draft
- [ ] Review
- [ ] Publish
```

如果一个标题同时有复选框和子任务，Automatic 无法唯一判断时，Aster 会要求明确选择，不会偷偷采用一个数据源。

## Habit 是 Task 的附加语义

```org
* NEXT Daily writing :writing:
SCHEDULED: <2026-09-03 Thu 07:30 .+1d>
:PROPERTIES:
:STYLE: habit
:REPEAT_TO_STATE: NEXT
:END:
```

一个标准 Habit 需要：

1. 未完成的 Workflow 关键字。
2. 重复的 `SCHEDULED` 时间。
3. `:STYLE: habit`。

因此 Habit 行仍显示真实的 `NEXT`、`TODO` 或其他关键字。Habit 图表、节奏和完成历史是附加信息，不会把关键字替换成一个假的 Habit 状态。

完成时，Aster 根据 Org Repeater 推进日期、把状态恢复到 `REPEAT_TO_STATE` 或配置的进行中状态，并写入标准 LOGBOOK 历史。

## Perspective 是可配置视图

Perspective 不修改 Org 文件，也不会创建新类型。它保存的是过滤和展示规则：

- 包含 Task、Project、Habit、Event、Anniversary、Day Counter 中的一种或多种。
- 匹配任一 Workflow 关键字。
- 匹配标签。
- 标签匹配模式。
- Priority。
- 日期状态。
- 排序方式。
- 是否保留父子 Outline。

在 **设置 → Tasks & Workflow → Views** 新建；之后打开 Views 左侧的三横线二级菜单，就能与 Agenda、TODOs、Journal 一起选择。

### 可选的内置模板

Views 顶部提供两个开关。它们不是新的 Org 类型，也不是固定导航页面；开启后只会创建一条普通、可编辑的 Perspective：

- **Projects**：匹配所有被标记为 Treat as Project 的进行中状态，保留 Outline。
- **Anniversaries**：匹配标准 `org-anniversary` 年度日期，并按下一次日期排列；可选 Property 决定显示已完成年数或累计天数。

关闭开关只删除对应模板创建的 Perspective，不会删除、移动或改写任何 Org 条目。自己新建的同名 Perspective 也不会被误删。

### 场景：我的项目

- Include：Project
- Workflow：留空，匹配所有被配置为 Project 的状态
- Preserve outline：开启

这样不要求所有用户都使用字面 `PROJECT`。

### 场景：本周重要行动

- Include：Task、Habit
- Priority：A
- Dates：选择对应日期范围
- Sort：按日期

### 场景：等待中的工作

- Include：Task、Project
- Workflow：`WAITING DELEGATED`
- Preserve outline：按需要开启

### 场景：纪念日与累计天数

先在 Views 中开启 **Anniversaries**。年度纪念日使用：

```org
%%(org-anniversary 2020 8 22) Aster 已经 %d 年
```

如果 Aster 需要显示从源日期起累计经过的天数：

```org
* 结婚纪念日
:PROPERTIES:
:ASTER_ANNIVERSARY_DISPLAY: elapsed-days
:END:
%%(org-anniversary 2022 11 2) 结婚纪念日
```

`ASTER_ANNIVERSARY_DISPLAY` 可显式写 `years` 或 `elapsed-days`。只有显式受支持的值才启用 Aster 的派生摘要：`years` 计算已完成年数，`elapsed-days` 计算累计天数，两者都计算距下次周年还有多少天。Org Preview 与 Anniversaries 视图紧凑显示为 `N 天 · 下次 M 天后`，日期 Agenda 保留更完整的上下文提示。省略 Property 或写入不支持的值时保持普通年度 Anniversary，不会统一套用计算摘要。Aster 只使用同一条 `org-anniversary`，不会生成每天一条的 Agenda 数据；Org Agenda 仍只显示真正的周年日。`org-cyclic` 则始终是周期 Event：它使用稳定的 `间隔天数 年 月 日` 顺序，间隔 `1` 会每天出现，间隔 `100` 只会在百日边界出现，`%d` 是完成的周期数而不是总天数。Aster 不执行其他 Lisp，编辑时也只替换对应 Diary 原文。

## 哪些东西不属于 Perspective

- Org 的 Repeat/Diary 规则仍写在源文件中。
- 通知由条目自身的标准带时刻规划派生；兼容旧版的持续提醒属性也不属于 Perspective。
- Apple Reminders 桥接是独立集成。
- Perspective 不复制条目、不改变 Workflow，也不是一个专有项目数据库。
