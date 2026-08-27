# Org 原文如何变成 Aster 界面

[返回首页](../README.md)

这页是 Aster 最核心的语义对照表。每个例子都包含可复制的 Org 原文、Aster 的实际分类、出现位置和适用场景。

## 先看分类规则

| Org 特征 | Aster 分类 | 主要出现位置 |
| --- | --- | --- |
| 没有 Workflow 关键字，有活动时间戳或可识别日期规划 | Event | Agenda |
| 有未完成 Workflow 关键字，没有具体钟点 | Task | TODOs、Perspective |
| 有未完成 Workflow 关键字，并有具体钟点 | Timed Task | Agenda |
| Workflow 关键字被配置为 Treat as Project | Project | TODOs、Perspective；可显示进度 |
| 未完成 Workflow + `STYLE=habit` + 重复的 `SCHEDULED` | Habit | Agenda/TODOs、Perspective；显示习惯历史 |
| 没有 Workflow 和日期，但有正文 | Note | Files、Search、Journal（若在 Journal 源中） |
| 没有 Workflow 和直接正文，只有子标题 | Container | 作为结构层级，不伪装成 Task |

文件名、文件夹名、标签或英文单词本身不会单独决定类型。例如 `PROJECT` 只是默认约定；用户也可以把 `PRO` 配置为 Project。

## 1. 全天 Event

### Org 原文

```org
* Team offsite :team:
<2026-09-03 Thu>
```

### Aster 实际效果

- 没有 TODO 关键字，因此不是 Task。
- 在 Agenda 的 2026/9/3 作为全天 Event 显示。
- 使用接近节假日和纪念日的紧凑条状样式，不出现任务状态切换。
- Aster 新建 Event 时写入这种普通活动时间戳，而不是 `SCHEDULED:`。

### 适用场景

全天会议、旅行、纪念日、发布日或不需要完成状态的日期信息。

## 2. 有时间范围的 Event

### Org 原文

```org
* Design review :meeting:
<2026-09-03 Thu 14:00-15:00>
```

### Aster 实际效果

- 在 Agenda 时间线上显示 `14:00–15:00`，时间在标题上方。
- 没有 Workflow 图标和 TODO 状态。
- 新建 Event 时如果只输入 `14:00`，Aster 默认写成一小时区间 `14:00-15:00`。
- 如果明确输入 `14:00-15:30`，Aster 保留给定区间。

### 适用场景

会议、课程、预约、出行区间或其他有开始和结束的事件。

> 兼容说明：已有文件中没有 Workflow 关键字、但使用 `SCHEDULED:` 或 `DEADLINE:` 的日期标题仍可作为 Event 读取。Aster 自己创建 Event 时使用普通活动时间戳，避免给 Event 引入 Task 规划语义。

## 3. 无日期 Task

### Org 原文

```org
* TODO Write release notes :release:
```

### Aster 实际效果

- 出现在 TODOs 和匹配的 Perspective。
- 不出现在有日期的 Agenda 时间线。
- 显示当前 Workflow 关键字、配置的图标、标签和优先级。
- 可以在详情中添加 Schedule，也可以继续保持无日期。

### 适用场景

Inbox、下一步行动、以后处理但还没有日期的工作。

## 4. 只有日期的 Task

### Org 原文

```org
* TODO Submit expense report :finance:
DEADLINE: <2026-09-03 Thu>
```

### Aster 实际效果

- 仍然属于 TODOs，因为没有具体钟点。
- 在条目元数据中显示 Deadline 日期。
- 到期日过去且状态仍未完成时，显示 Overdue 状态。
- 不会为了“有日期”就伪装成全天 Event。
- 默认不创建通知；开启“Remind me on a day”后才写入 Aster 提醒属性和提醒时刻。

### 适用场景

某天之前完成即可、不需要占据具体时间段的任务。

## 5. 有具体时刻的 Task

### Org 原文

```org
* NEXT Call supplier :phone:
SCHEDULED: <2026-09-03 Thu 09:15>
```

### Aster 实际效果

- 因为有未完成 Workflow 关键字，所以仍是 Task。
- 因为有具体钟点，所以显示在 Agenda 时间线，而不是 TODOs 列表。
- 时间是一个精确时刻 `09:15`，不是 Event 的开始—结束区间。
- 未完成且时刻已过时，时间本身变红。
- 保存这种条目时，Aster 会按标准 Org 规划时间创建一次本地提醒，不需要第二个 Notify 开关。

### 适用场景

要在某个时刻执行或收到提醒的行动。

## 6. Project

Project 不是由文件名或固定的 `PROJECT` 单词硬编码出来的。它由 Workflow 设置中的 **Treat as Project** 决定。

### Org 原文

```org
#+TODO: TODO NEXT PROJECT | DONE CANCELLED

* PROJECT Release 1.0 [2/3] :release:
:PROPERTIES:
:COOKIE_DATA: todo
:END:
** DONE Prepare metadata
** DONE Upload build
** NEXT Invite testers
```

### Aster 实际效果

- 主标题使用该关键字配置的 Project 身份。
- 显示 `2/3 · 67%` 进度和进度条。
- 子任务保持各自的 Workflow 状态，并可打开独立详情。
- Project 默认仍在 TODOs 或匹配的 Perspective 中，不存在硬编码 Projects 页面。

### 进度数据源

| 选择 | Org 写入 | 统计范围 |
| --- | --- | --- |
| Automatic | 不写 `COOKIE_DATA` | 只有来源唯一时自动使用 |
| Checklist | `:COOKIE_DATA: checkbox` | 当前标题下的复选框 |
| Direct subtasks | `:COOKIE_DATA: todo` | 直接子任务 |
| All subtasks | `:COOKIE_DATA: todo recursive` | 所有后代任务 |

当复选框和子任务同时存在且没有明确数据源时，Aster 不猜测，会要求选择。

## 7. Habit

标准 Habit 身份需要同时满足：未完成 Workflow、`STYLE=habit`、带 Repeater 的 `SCHEDULED`。

### Org 原文

```org
* NEXT Daily walk :wellbeing:
SCHEDULED: <2026-09-03 Thu 07:30 .+2d/4d>
:PROPERTIES:
:STYLE: habit
:REPEAT_TO_STATE: NEXT
:END:
:LOGBOOK:
- State "DONE" from "NEXT" [2026-09-01 Tue 07:30]
:END:
```

### Aster 实际效果

- 仍显示真实的 `NEXT` Workflow 身份；Habit 是次级类型，不替代关键字。
- 显示习惯标签、`每 2–4 日` 节奏和最近 28 天完成图。
- 蓝色表示尚未到期，绿色表示可执行，黄色表示到期，红色表示逾期。
- `*` 表示 LOGBOOK 中有完成记录，`!` 表示今天。
- 完成动作按 Org Repeater 规则推进源时间，并把状态恢复到配置的进行中状态。

### 适用场景

周期性复盘、锻炼、写作、清理 Inbox 等需要完成历史和宽限窗口的行动。

## 8. 普通 Note 与 Container

### 有正文的 Note

```org
* Reading notes :research:
Aster keeps prose, structure, and references in plain Org files.
```

Aster 把它当作文档内容：显示在 Files、Org Preview 和 Search，不会出现在 Agenda 或 TODOs。

### 只有结构的 Container

```org
* Work
** Project A
Some notes.
** Project B
More notes.
```

`Work` 没有直接正文或任务语义，只组织子标题，因此是 Container。Aster 保留它的层级，但不会制造一条名为 Work 的任务。

## 9. 优先级、标签和标题顺序

标准标题顺序是：Workflow、Priority、标题、仅 Diary 使用的时间、标签。

```org
* WAITING [#B] Weekend review 10:00-11:30 :review:ios:
```

- Workflow：保留完整配置关键字。
- Priority：`[#A]` 到 `[#Z]` 或数字等源值保持字面意义；Aster 常用选择为 None、A、B、C。
- Diary 时间：普通时间写在 Timestamp 中；只有 Diary 表达式没有具体时间，因此可把 `HH:mm` 或 `HH:mm-HH:mm` 放在标题末尾、标签之前。
- Tags：编辑时自动保留 Org Attach 使用的 `ATTACH` 标签。

## 10. 标准重复

```org
* TODO Daily review
SCHEDULED: <2026-09-03 Thu 08:00 ++1d>
```

| Org Cookie | 含义 |
| --- | --- |
| `+1d` | 从源日期推进一个间隔 |
| `++1d` | 保持节奏并追赶到未来 |
| `.+1d` | 从完成时重新计算下次日期 |

单位支持 `h`、`d`、`w`、`m`、`y`，间隔必须为正数。Task/Habit 在完成时推进源时间；Event 的重复活动时间戳产生虚拟未来实例，不重写原时间戳。

## 11. Weekday Diary

```org
* Weekend Review 21:00-21:20 :review:
SCHEDULED: <%%(memq (calendar-day-of-week date) '(0 6))>
```

- `0` 是星期日，`6` 是星期六。
- Aster 只识别明确支持的有界形式，不执行任意 Lisp。
- Diary 负责“哪几天”，标题中的时间负责“几点”。
- 没有 Workflow 关键字时作为重复 Event；加上 Workflow 关键字后仍是 Task。

## 12. Aster 提醒属性

提醒属性独立于 Org 的 `SCHEDULED`、`DEADLINE` 和 Repeater。

```org
* TODO Submit report
DEADLINE: <2026-09-03 Thu>
:PROPERTIES:
:ID: 00000000-0000-0000-0000-000000000001
:ASTER_REMINDER: once
:ASTER_REMINDER_TIME: 09:05
:END:
```

日期型 Task 需要显式开启“Remind me on a day”；有具体规划时刻的 Task 已经使用该时刻提醒，不再重复写 `ASTER_REMINDER_TIME`。

持续提醒还可写入：

```org
:ASTER_REMINDER: persistent
:ASTER_AUTO_SNOOZE: 15m
:ASTER_ALERT_UNTIL: done
```

支持的重复提示间隔是 `5m`、`10m`、`15m`、`30m` 和 `60m`。

## 13. 回写范围

Aster 的语义编辑不会重新格式化整个文件。一次状态、日期、优先级、标签、提醒、Note 或附件操作只修改自己拥有的标题行、规划行、属性、LOGBOOK 或附件链接；未知属性、正文、子树和其他源文本保持原样。

详情页尾部的 Source 入口可以查看当前保存的完整 Org 子树；Files 中的 Edit 可以查看和编辑整个原文件。
