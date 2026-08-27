# Agenda 与 TODOs

[返回首页](../README.md) · [查看 Org 分类总表](org-and-aster.md)

Agenda 和 TODOs 读取同一批 Org 文件，但回答不同问题：

- **Agenda**：什么时候发生？只显示 Event 和有具体钟点的未完成 Task。
- **TODOs**：还有什么未完成？显示无日期或只有日期、没有具体钟点的 Workflow 条目。

![Agenda 示例](../assets/screenshots/agenda.png)

## Agenda 显示什么

| 条目 | 是否进入 Agenda | 显示方式 |
| --- | --- | --- |
| 无 Workflow 的全天 Event | 是 | 紧凑全天条 |
| 无 Workflow 的时间范围 Event | 是 | 时间范围在上、标题在下 |
| 有 Workflow 的日期型 Task | 否 | 留在 TODOs |
| 有 Workflow 的定时 Task | 是 | 精确时刻在上、Workflow 条目在下 |
| Habit 到达可执行窗口 | 是 | Workflow + Habit 节奏和历史 |
| 普通 Note/Container | 否 | 留在 Files/Search |

## TODOs 显示什么

![TODOs 示例](../assets/screenshots/todos.png)

TODOs 保留所有未完成且没有具体钟点的 Workflow 条目，包括：

- 完全无日期的 Task。
- 只有日期的 `SCHEDULED` Task。
- 只有日期的 `DEADLINE` Task。
- 配置为 Project 的 Workflow 条目。
- 目前无需在时间线上显示的 Habit。

列表按真实文件和 Org 层级组织。可见父 Task 下的可见子 Task 会保持嵌套；纯结构 Container 不会被错误显示为 Task。

## Overdue 的规则

- 只有未完成的 Workflow 条目才会成为 Overdue。
- 日期型 Task 过期后在 TODOs 显示日期和红色状态，但不会被搬到今天。
- 定时 Task 在当天时刻已过时，Agenda 中的时间变红。
- 今天顶部可以显示历史 Overdue 数量；点击会以滚动动画前往最近的 Overdue 原日期。
- 多条 Overdue 仍保留在各自日期，用户可继续向上查看，不合并成一条伪造任务。
- 没有 Workflow 关键字的 Event 不使用 Task 的 Overdue 语义。

## 新建时的 Event/Task 选择

Agenda 的 `+` 打开快速新建。上方两段选择只决定 Org 类型，不会因为输入日期或时间而自动改变：

| 选择 | Org 语义 | 时间精度 |
| --- | --- | --- |
| 日历图标 | Event，无 Workflow 关键字 | 全天或开始—结束区间 |
| 勾选图标 | Task，使用选定 Workflow 关键字 | 无日期、只有日期或一个具体时刻 |

### Event

- 不输入时间：写入全天活动时间戳。
- 只输入 `9:00`：默认写成 `09:00-10:00`。
- 输入 `9:00-10:15`：保留指定区间。
- 已经过的隐式时间会推到下一天；明确输入的日期保持用户选择。

### Task

- 不打开 Schedule：写入无日期 Task，进入 TODOs。
- 只选择日期：写入日期型 `SCHEDULED` 或 `DEADLINE`，留在 TODOs。
- 打开 At a time：写入一个具体时刻，进入 Agenda，并按该时刻提醒。
- Task 不写结束时刻；它不是 Event 区间。

## Show More 包含什么

默认新建页只显示标题、日期、类型和必要的时间轴，以降低输入压力。展开 Show More 后可以设置：

- Task Workflow 状态
- `SCHEDULED` 或 `DEADLINE`
- 日期和时间
- Priority
- Tags
- 标准 Org Repeat 或 Weekday Diary
- 待添加附件
- 一条写入 LOGBOOK 的 Note
- 被动显示的 Capture Inbox 目标

收起 Show More 不会丢弃已经填写的值。点击 Add 后，标题、规划、属性、Note、链接和附件复制作为一次事务写入。

## 日历与时间线

- 月视图是默认入口；下方 Grabber 每次只在 Week、Month、Year 中切换一个相邻级别。
- 日历固定，下面时间线独立滚动，并可向过去和未来继续扩展。
- 时间线顶部日期变化时，日历选中日同步跟随。
- 跨周、月或年时，日历页面使用有方向的动画；开启 Reduce Motion 后减少运动。
- iPad 横屏把完整时间线放左侧、同步月历放右侧；TODOs 和 Perspective 不保留无意义的大日历。

## 条目操作

- 轻点任意 Task/Event 行：打开同一个详情页。
- 长按 Task：打开 Workflow 和 Actions 分组菜单。
- 长按 Event：打开可用的日程、移动/归档、Note 和删除操作。
- Agenda 和 TODOs 不使用左右滑动改变 Workflow，避免与滚动和系统手势冲突。
- 详情页关闭时把当前有效草稿一次写回源文件；没有额外 Save 按钮。

详情页的主要顺序是：Item/History → Schedule/Repeat/Reminder → Content → Attachments → Notes → Subtasks → Delete。Delete 为红色，并在底部导航之上保留足够滚动空间。

## 自定义视图入口

长按底部 Agenda 标签可以选择：

- Agenda
- TODOs
- 已保存 Perspective

选择后，底部标签的名称和图标跟随当前视图。Aster 不提供固定 Projects 视图；不同用户可按 Workflow、Tags、来源、日期状态等组合 Perspective。
