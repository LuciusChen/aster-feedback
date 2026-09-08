<p align="right">🌐 <strong>简体中文</strong> · <a href="en/org-and-aster.md">English</a></p>

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
| 未完成 Workflow + `STYLE=habit` + 重复的 `SCHEDULED` | Habit | 默认今天可执行时进入 Agenda；Perspective 可回顾未到期习惯（iOS/iPadOS build 14），不进入 TODOs；显示习惯历史 |
| `org-anniversary` 年度 Diary | Anniversary；可选 Property 可显示 Day Counter | Agenda；可选 Anniversaries Perspective |
| `org-cyclic` / `diary-cyclic` 周期 Diary | Cyclic Event | Agenda；可选 Perspective |
| 没有 Workflow 和日期，但有正文 | Note | Files、Search、Journal（若在 Journal 源中） |
| 没有 Workflow 和直接正文，只有子标题 | Container | 作为结构层级，不伪装成 Task |

文件名、文件夹名、标签或英文单词本身不会单独决定类型。例如 `PROJECT` 只是默认约定；用户也可以把 `PRO` 配置为 Project。

这里的 Container 是 Agenda/TODO 等语义视图的分类，不会让 Journal 隐藏真实标题。Journal 会保留日期根标题下面的完整 Org 层级，以免日记的章节语义丢失。

## 解析与原文保真

Aster 会先为每次文件修订建立一棵保留原文范围的 Org 语法树，再从这棵树投影 Preview、Agenda、Journal 和 Search。官方默认的元素和对象类别都有明确结构，包括 Diary sexp、inline task、标准与自定义 block、嵌套列表、行内或多行脚注、inline source/Babel call、citation reference，以及 regular/plain/angle link。

- 匿名脚注不会被误当成多行定义并吞掉后文；列表续行和子列表归属于真正的父项。
- `SCHEDULED`、`DEADLINE` 和 Property 只有位于标题元数据位置时才取得对应结构语义；普通正文中的相同文字不会变成 Task planning。
- 未识别的扩展语法仍按原字节保留。Aster 不执行 Babel，也不执行任意 Diary Lisp；Agenda 只解释文档中列出的安全子集。

### 阅读边界示例（iOS/iPadOS build 14）

以下阅读示例得到修正，不会改变 Org 文件：

```org
* 阅读检查
2 * 3 * 4

1. [ ] 待完成
  * [X] 已完成

| 名称 | 数量
| 苹果 | 2

-----

%%(org-calendar-holiday) 自定义提醒
```

Preview 保留算式符号，显示列表复选框、表格和分隔线。最后一行按原文显示，不会被执行，也不会因此生成 Agenda 提醒。

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
- 不创建通知；需要在某个时刻提醒时，打开 `At a time`，把时刻写进标准 `SCHEDULED` 或 `DEADLINE` 后再保存。

### 适用场景

某天之前完成即可、不需要占据具体时间段的任务。

### 同时有计划和截止日期

```org
* TODO Submit expense report :finance:
SCHEDULED: <2026-09-07 Mon> DEADLINE: <2026-09-11 Fri -2d>
```

这个任务计划在 9 月 7 日处理，9 月 11 日截止。两个字段没有具体时刻，仍在 TODOs 中；`-2d` 表示截止前两天开始显示到期提示。

在 iPhone 或 iPad 详情里选 Deadline，只会显示并修改 9 月 11 日。改成 9 月 12 日后，源文件保留 `SCHEDULED: <2026-09-07 Mon>`，只把 Deadline 更新为 `<2026-09-12 Sat -2d>`。关闭 Deadline 开关也只删除 Deadline，不删除 Scheduled 或正文。

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
- Project 默认仍在 TODOs 或匹配的 Perspective 中，不存在硬编码 Projects 页面。可选的 Projects 内置模板只是按 Treat as Project 角色生成普通 Perspective。

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

iOS/iPadOS build 14提供未到期习惯的回顾方式，见[今天显示所有习惯与 Perspective](agenda-todos.md#查看尚未到期的习惯iosipados-build-14)。展示范围和真实计划日期是两回事；开关不会改变以下 Org 内容。

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
* WAITING [#B] Weekend review 10:00-11:30 :review:mobile:
```

- Workflow：保留完整配置关键字。
- Priority：`[#A]` 到 `[#Z]` 或数字等源值保持字面意义；Aster 常用选择为 None、A、B、C。
- Diary 时间：普通时间写在 Timestamp 中；只有 Diary 表达式没有具体时间，因此可把 `HH:mm` 或 `HH:mm-HH:mm` 放在标题末尾、标签之前。
- Tags：编辑时自动保留 Org Attach 使用的 `ATTACH` 标签。

### Agenda 可见性与规划显示 Cookie

下面两棵子树会保留在 Files、Org Preview、Edit 和 Search 中，但默认不会进入 Agenda、TODOs、Perspective、小组件或本地提醒：

```org
* TODO COMMENT 暂时不参与计划
** TODO 这个子任务也被排除

* 已归档资料 :ARCHIVE:
** TODO 这个子任务也被排除
```

`COMMENT` 是标题中位于 Workflow 与 Priority 之后的标准标记；`ARCHIVE` 是精确的特殊标签。Aster 不删除也不改写这些内容。

标准的 Deadline warning 与 Scheduled delay cookie 也会保留：

```org
* TODO Submit release
DEADLINE: <2026-09-10 Thu -5d>

* TODO Prepare environment
SCHEDULED: <2026-09-01 Tue -2d>

* TODO First setup only
SCHEDULED: <2026-09-01 Tue --2d>
```

- `DEADLINE ... -5d` 从截止日前 5 天开始显示简洁的到期提示；`-0d` 表示只在截止当天显示，没有显式 cookie 时 Aster 使用 14 天预警窗口。
- `SCHEDULED ... -2d` 把每次派生的 Agenda 显示推迟 2 天；`--2d` 只推迟重复任务的第一次，第一次按标准 Workflow 完成并留下状态日志后不再推迟。
- 这些 cookie 只改变 Agenda 的派生显示，不会改写 Org 原日期，也不会推迟由具体时刻产生的本地通知。编辑同一种规划的日期时会保留 cookie；切换为不兼容的规划类型时会移除它。
- Category 只来自解析确认的 `CATEGORY` Property、文件级 `#+CATEGORY` 或文件名回退；代码块或普通正文里的同名字样不会被误当作 Agenda 元数据。

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

## 12. Anniversary、累计天数与周期事件

年度纪念日：

```org
%%(org-anniversary 2020 8 22) Aster 已经 %d 年
```

如果希望 Aster 在 Anniversaries 中显示累计天数，用标准 Anniversary 加一个可选 Property：

```org
* 结婚纪念日
:PROPERTIES:
:ASTER_ANNIVERSARY_DISPLAY: elapsed-days
:END:
%%(org-anniversary 2022 11 2) 结婚纪念日
```

- `org-anniversary` 的参数是 `年 月 日`，`%d` 显示发生年份与源年份之差。
- Aster 始终按当前时区的公历解释这三个数字，即使设备显示佛历、伊斯兰历等其他日历，也不会改变源日期或累计结果。
- `ASTER_ANNIVERSARY_DISPLAY` 支持 `years` 与 `elapsed-days`，而且必须显式写入受支持的值才会启用 Aster 的派生计算：`years` 计算已完成年数，`elapsed-days` 计算累计天数，两者都会计算距下次周年的天数。Org Preview 仍使用紧凑文案 `N 天 · 下次 M 天后`。在 Anniversaries 中，源日期还没到时显示 `N 天后开始 · 月日`，源日期当天显示 `今天开始`，不会把它误称为“1 周年”；满一年以后才显示编号周年与下一日期，例如 `4 周年`、`还有 65 天 · 11 月 2 日`。显式累计天数会保留累计值，并在第二行说明同一个开始日期或下一编号周年。关键词为空的年度 Org 时间戳也进入同一套日期轨，但只显示普通的年度重复含义，不推造周年编号。全部条目使用同一套无卡片的日期轨与分隔线，第一条不会获得渐变或圆角背景。省略 Property 或写入不支持的值时不会启用累计计算；该 Property 不复制日期，Org Agenda 仍只在真正的周年日显示一次。
- 推荐的 `org-cyclic` 参数是 `间隔天数 年 月 日`。它始终表示周期 Event；间隔为 `1` 就是每天显示一次，`%d` 表示已经完成的周期数。
- 如果只需要每 100 天显示一次，写作 `%%(org-cyclic 100 2023 8 28) 结婚第 %d 个百日`。此时 `%d` 表示完成了多少个 100 天周期。
- Aster 兼容 `diary-cyclic` 的 `间隔天数 月 日 年` 顺序；编辑时会保留原函数和对应参数顺序，但新示例优先使用不受 `calendar-date-style` 影响的 `org-cyclic`。
- 两者都是没有 Workflow 关键字的源条目，不会获得 TODO 状态。
- 开启 **设置 → Tasks & Workflow → Views → Anniversaries** 后，可以在一个可选 Perspective 中集中查看年度纪念日；只有带上述显式 Property 的条目才额外显示已完成年数或累计天数与下一周年倒计时。
- Aster 不执行任意 Diary Lisp；不支持的表达式会原样保留在 Org 文件中。

## 13. 兼容旧版 Aster 提醒属性

当前版本的新建页和详情页只通过标准 Org 的 `SCHEDULED` 或 `DEADLINE` 具体时刻创建通知，不再新建私有提醒属性。下面的属性只用于兼容旧版本已经写入的文件：

```org
* TODO Submit report
DEADLINE: <2026-09-03 Thu>
:PROPERTIES:
:ID: 00000000-0000-0000-0000-000000000001
:ASTER_REMINDER: once
:ASTER_REMINDER_TIME: 09:05
:END:
```

旧版日期型 Task 的提醒仍可读取。打开含有效 `ASTER_REMINDER_TIME` 的日期型 Task 时，详情页会把该时刻带入普通的 `At a time` 编辑；保存后写成标准带时刻的规划时间，并移除过时的私有提醒属性。Aster 不会仅因加载工作区就在后台改写源文件。

旧文件也可能包含持续提醒属性：

```org
:ASTER_REMINDER: persistent
:ASTER_AUTO_SNOOZE: 15m
:ASTER_ALERT_UNTIL: done
```

支持的重复提示间隔是 `5m`、`10m`、`15m`、`30m` 和 `60m`。

## 14. 回写范围

Aster 的语义编辑不会重新格式化整个文件。一次状态、日期、优先级、标签、提醒、Note 或附件操作只修改自己拥有的标题行、规划行、属性、LOGBOOK 或附件链接；未知属性、正文、子树和其他源文本保持原样。

详情页顶部工具栏的 Source 入口可以查看当前保存的完整 Org 子树；Files 中的 Edit 可以查看和编辑整个原文件。

以下元数据、日期区间、附件移动和失败编辑修正包含在 iOS/iPadOS build 13 中，不代表 Android build 9 已包含相同行为。

添加 Note、完成重复任务或使用 plain 模板向前插入正文时，规划日期与属性抽屉仍保留在标题元数据区。代码块里的 `CLOSED`、属性或 LOGBOOK 示例不会被当作真实元数据修改。LF、CRLF 和末尾没有换行的文件都支持新增日期。

为条目添加 Event 日期或将 Task 改为 Event 时，新活动时间戳放在规划日期和属性抽屉之后、正文之前。原有 ID、自定义属性和附件引用仍可识别；再次修改日期只更新对应的时间戳。

普通关键字菜单与详情状态修改会同时更新父项进度。重复规划日期区间会一起推进起点和终点，不会只移动开始日期。在详情中调整已有规划区间的日期，也会保留原有时长；新建 Task 仍使用时间点。

跨文件夹移动或归档时，Aster 会复制父项和子项的 ID 附件，并调整相对文件链接，让它们仍指向原来的文件。原附件保留，供其他引用使用。附件未下载、目标存在同名不同内容文件或写入失败时，操作会报错，原条目不会被移除。

详情页点返回时，只有保存成功才会退出。保存失败会保留输入；若切换主页面离开，重新打开同一条目也能恢复当前会话中的失败编辑。取消完成备注不会提交完成状态。这不是持久化的 Journal 草稿，退出应用前仍需保存或复制内容。

## 15. 项目入口与聚焦阅读（iOS/iPadOS build 12）

### Org 原文

```org
* Writing project
:PROPERTIES:
:ID: writing-project-example
:END:
Project notes.
** Draft
Draft text.
** References
Reading notes.
* Another project
Unrelated material.
```

### Aster 实际效果与场景

`aster://open?id=writing-project-example&focus=true` 打开 Writing project 及其 Draft、References 子标题，不显示 Another project。标题改名或在当前工作区内移动后，标准 ID 仍可定位；请不要把这个示例 ID 复制给多个标题。

可以在 Apple 快捷指令中添加 Open Org Heading，选择 Writing project 并打开 Focus，再把快捷指令放到主屏幕。这里没有新增日期、TODO 状态或私有属性，也不会改变 Agenda 分类。

点击 Full Document 恢复完整阅读范围。点击 Edit 始终打开完整文件，不会把另一个项目从源码中移除。更多操作见[文件与标题链接](files-preview-attachments.md)。

## 16. 时区与跨地区提醒（iOS/iPadOS build 14）

### 普通 Org 跟随当地时间

```org
* TODO Morning review
SCHEDULED: <2026-09-08 Tue 09:00>
```

在上海是当地 09:00，换到纽约后仍是当地 09:00。重新解析不会修改这段原文；仅日期的值也保留原来的日历日期。旅行后可用它核对 Agenda、通知和角标是否已经刷新。

### 系统提醒保留固定时刻

下面是托管 `apple-reminders.org` 的结构示例。实际 ID 由系统提供，不应把示例 ID 当成一个可同步的新提醒。

```org
* TODO New York reminder
DEADLINE: <2026-09-08 Tue 15:00>
:PROPERTIES:
:APPLE_REMINDER_ID: example-provider-id
:APPLE_REMINDER_DEADLINE_TIME_ZONE: America/New_York
:END:
```

Aster 在上海显示 9 月 9 日 03:00，在纽约显示 9 月 8 日 15:00。两者是同一个时刻；改标题或完成状态不会把提醒提前或推迟。独立的开始日期使用 `APPLE_REMINDER_SCHEDULED_TIME_ZONE`。

这些属性只用于已标识的 Apple Reminders 条目，不是普通 Org 的通用时区语法。Emacs 等忽略这些属性的阅读器看到的是原文中的纽约 15:00，不会自动换算成上海时间。没有指定时区的系统提醒继续使用当地时间。

即使尚未完成同步，删除日期后再添加，也会正确使用该字段保留的时区。例如保留上述纽约时区属性，在上海选 9 月 9 日 04:00，写回的是纽约 9 月 8 日 16:00，而不是把上海的钟点误当成纽约时间。

仅日期的值及其范围始终按当地日历处理，保留的时区属性不会让 11 月 7–9 日变成 7–8 日。重新开启钟点时，写回仍使用保留的时区，但不会重新解释旧范围的日数。

如果来源从固定时区改为跟随当地时间，或反过来，Aster 也会更新通知规则，即使更改当下的显示时间相同。

### 夏令时前后的重复范围

```org
* TODO Two-day review
SCHEDULED: <2026-03-07 Sat 09:00 +1w>--<2026-03-09 Mon 09:00>
```

在纽约完成一次后，范围推进为 3 月 14 日 09:00 到 3 月 16 日 09:00。跨越夏令时不会让结束日期少一天，或把结束时刻改成 08:00。

短范围也遵循相同规则。例如 2026 年 3 月 8 日 01:30–03:30 的每周条目，下一次仍是 3 月 15 日 01:30–03:30，不会缩短为 01:30–02:30。手动修改日期也保留这段钟面时间差。

按小时重复则保留实际经过的时长。同一标题中有多个时间戳时，各自使用自己的重复规则。旧版本已经写错的范围不会被猜测性修复。
