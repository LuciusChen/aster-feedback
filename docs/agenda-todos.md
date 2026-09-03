<p align="right">🌐 <strong>简体中文</strong> · <a href="en/agenda-todos.md">English</a></p>

# Agenda 与 TODOs

[返回首页](../README.md) · [查看 Org 分类总表](org-and-aster.md)

Agenda 和 TODOs 读取同一批 Org 文件，但回答不同问题：

- **Agenda**：什么时候发生？显示 Event、有具体钟点的未完成 Task，以及今天可执行的 Habit（无论是否带具体钟点）。
- **TODOs**：还有什么未完成？显示无日期或只有日期、没有具体钟点的非 Habit Workflow 条目。

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

TODOs 保留所有未完成且没有具体钟点的非 Habit Workflow 条目，包括：

- 完全无日期的 Task。
- 只有日期的 `SCHEDULED` Task。
- 只有日期的 `DEADLINE` Task。
- 配置为 Project 的 Workflow 条目。

Habit 是例外：到达可执行窗口后，它只在今天的 Agenda 时间线出现；没有具体钟点时也不会被移入或重复显示在 TODOs。尚未到达可执行窗口的 Habit 两边都不显示。

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
- Event 可以分别选择开始日期和结束日期；同一天仍写一个活动时间戳，跨天则写标准 Org 日期区间：

  ```org
  * 出差
  <2026-08-28 Fri>--<2026-08-30 Sun>
  ```

- 只输入 `9:00`：默认写成 `09:00-10:00`。
- 输入 `9:00-10:15`：保留指定区间。
- 跨天并带时刻时，同样写一个标准活动时间戳区间，例如
  `<2026-08-28 Fri 09:00>--<2026-08-30 Sun 18:00>`。
- 已经过的隐式时间会推到下一天；明确输入的日期保持用户选择。

### Task

- 不打开 Schedule：写入无日期 Task，进入 TODOs。
- 只选择日期：写入日期型 `SCHEDULED` 或 `DEADLINE`，留在 TODOs。
- 打开 At a time：写入一个具体时刻，进入 Agenda，并按该时刻提醒。
- Task 不写结束时刻；它不是 Event 区间。

切换到 Task 后，横向时间轴始终保留。日期型 Task 在新建页中显示覆盖当天的蓝色预览，但这只是类型与日期反馈，不会改变 Org 语义；只有打开 At a time 后，它才以具体时刻进入 Agenda。

## 新建页可直接设置什么

新建页在同一个可滚动界面中直接提供：

- Task Workflow 状态
- `SCHEDULED` 或 `DEADLINE`
- Event 的开始/结束日期和可选区间时刻，或 Task 的单个日期和可选具体时刻
- Priority
- Tags
- 标准 Org Repeat 或 Weekday Diary
- 待添加附件
- 一条写入 LOGBOOK 的 Note
- 被动显示的 Event & Task Inbox 目标

标题和这些字段都直接位于同一个滚动界面，不再藏在 Show More/Show Less 中，也不使用额外的自定义卡片背景。长标题会按手机可用宽度自然换行，完整内容需要多少行就显示多少行，不设行数上限，也不会把展示换行写入 Org 标题；横向时间轴始终只显示一行，超出条带宽度时以省略号结尾，不会撑宽页面。WebDAV URL、文件与目录名、Journal 文件名规则、Capture 路径与提示回答、Agenda 标签和 Perspective 过滤器等长值编辑也遵循同一规则。时间轴只保留帮助辨识时间范围的淡色平面底。点击 Add 后，标题、规划、属性、Note、链接和附件复制仍作为一次事务写入。

## 日历与时间线

- 第一次打开时默认月视图；之后会记住最后选择的 Week、Month 或 Year，即使结束并重新打开 Aster 也会恢复。下方 Grabber 每次只切换一个相邻级别。
- Agenda 和 Journal 的 Week 复用同一个五列事件日期轨道，不是普通七日页。Agenda 提供当前选中日、含有非 Habit Agenda 条目的日期和外部全天日历日期；Journal 提供当前选中日、已有日记的日期和同一外部日历覆盖。任一方向没有内容日期时都会补一个相邻空日期，因此可以继续向过去或未来翻，不会停在最早或最晚记录。跨度很长的来源只提取当前浏览位置附近所需的日期，不会把整段日期全部展开；重叠横条只有在整段都空闲时才占用一行，达到固定显示上限后会省略多余横条而不会互相压住。被省略的日期在交界处显示一条对齐的撕口，周末沿用日历灰底，每列分界一直延伸到轨道底部。单日和跨日条目只显示来源色横条，不在横条内塞文字；Agenda 中有 Workflow 关键字的日期在居中的星期文字旁叠加一个不占布局的标志。每次有效横向松手都固定前进或后退五个日历日，不会因拖动距离改变步长，并使用和 Month、Year 相同的整页方向切换动态；只有拖动时，最左列里的内容才会在一层清晰、无模糊的透明折射区下随拖动渐进放大，最高约 2.5%，并产生轻微位移。放大始终限制在该列的固定玻璃遮罩内，折射区只露出固定的右侧光学边界，背景、周末灰底和日期边界保持不动。静止时不存在可见玻璃层、色差、模糊、渐变、边线或重影。
- Week 松手后只完成一次五日整页方向切换，不会再叠加第二次吸附或弹簧回弹。iPad 横屏左栏使用和 iPhone 相同的五列轨道，右栏仍保留同步月历。
- 日历固定，下面时间线独立滚动，并可向过去和未来继续扩展。
- 首屏仍把今天顶格显示，但过去日期已经位于其上方；第一次拖动就是原生滚动，不需要先拖一次来“解锁”时间线。
- 时间线顶部日期变化时，日历选中日同步跟随。
- iPhone 上方日期、日历和时间线共用左右各 18pt 的固定内容边距；自定义字体或很长的全天条目只会在自己的圆角条内省略，不会把整页向右撑偏。即使先进入 Files、Journal、Search 或 Settings 再返回 Agenda，后台保留的页面也不会再把 Agenda 与根操作栏撑偏。
- Week、Month 和 Year 使用相同的整页方向动画；开启 Reduce Motion 后三者都会减少空间运动。
- iPad 横屏把完整时间线和五列 Week 轨道放左侧、同步月历放右侧；TODOs 和 Perspective 不保留无意义的大日历。

## 条目操作

- 轻点任意 Task/Event 行：打开同一个详情页。
- 长按 Task：打开 Workflow 和 Actions 分组菜单。
- 长按 Event：打开可用的日程、移动/归档、Note 和删除操作。
- Agenda 和 TODOs 不使用左右滑动改变 Workflow，避免与滚动和系统手势冲突。
- 详情页关闭时把当前有效草稿一次写回源文件；没有额外 Save 按钮。

详情页的主要顺序是：Item/History → Schedule/Repeat/Reminder → Content → Attachments → Notes → Subtasks → Delete。Delete 为红色，并在底部导航之上保留足够滚动空间。

## 自定义视图入口

打开 Views 左侧的三横线二级菜单可以选择：

- Agenda
- TODOs
- Journal
- 已保存 Perspective

Views 始终是一级入口，二级菜单只切换它内部的内容。Aster 不提供固定 Projects 页面；不同用户可按 Workflow、Tags、来源、日期状态等组合 Perspective。**设置 → Tasks & Workflow → Views** 里的 Projects 与 Anniversaries 是可选模板，开启后也只会创建普通、可编辑的 Perspective。
