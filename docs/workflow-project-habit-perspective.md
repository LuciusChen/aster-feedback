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

Project 仍是 Workflow 条目，因此默认出现在 TODOs 和匹配的 Perspective，而不是一个硬编码的 Projects 页面。

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

- 包含 Task、Project、Habit 中的一种或多种。
- 匹配任一 Workflow 关键字。
- 匹配标签。
- 标签匹配模式。
- Priority。
- 日期状态。
- 排序方式。
- 是否保留父子 Outline。

在 **设置 → Tasks & Workflow → Perspectives** 新建；之后长按底部 Agenda，就能与 Agenda、TODOs 一起选择。标签名称和图标会跟随当前视图。

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

## 哪些东西不属于 Perspective

- Org 的 Repeat/Diary 规则仍写在源文件中。
- 本地通知与持续提醒是独立设置。
- Apple Reminders 桥接是独立集成。
- Perspective 不复制条目、不改变 Workflow，也不是一个专有项目数据库。
