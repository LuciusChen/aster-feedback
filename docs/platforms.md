<p align="right">🌐 <strong>简体中文</strong> · <a href="en/platforms.md">English</a></p>

# 平台支持与差异

[返回首页](../README.md) · [快速开始](quick-start.md)

Aster 在 Apple 与 Android 平台共享 Org 解析、分类、写回、同步冲突、Agenda、TODOs、Journal、Search 和附件语义。界面不会逐像素复制另一平台：信息层级与操作结果保持一致，导航、弹层、权限和系统集成使用各自平台的原生方式。

## 当前版本状态

| 平台 | 最低系统 | 当前基线 | 分发状态 |
| --- | --- | --- | --- |
| iPhone / iPad | iOS / iPadOS 17 | 0.1 (14) | 正在准备 TestFlight 上传 |
| Android 手机 / 平板 | Android 8.0（API 26） | 0.1 (9) | Google Play 测试准备中，尚未公开发布 |

两个平台可以分别推进 Build/Version Code。反馈时请写明平台和完整版本；相同营销版本号不表示系统能力或修复进度完全相同。

### 习惯回顾、Org 阅读与时区（iOS/iPadOS build 14）

设置 → 日历与 Agenda 新增默认关闭的「今天显示所有习惯」。Habit Perspective 也可查看尚未到期的习惯；这些展示变化不修改真实计划、通知或角标。iPad 编辑器在键盘弹出和收起时保持打开。

Org 阅读修正了强调、复选框、表格、相邻图片和跨文件夹链接。换时区后刷新日期缓存，保留系统提醒的明确时区，并修正跨夏令时的日期范围。详情见 [Agenda 与 TODOs](agenda-todos.md) 和 [Org 原文示例](org-and-aster.md)。这些变化不代表 Android build 9 已包含相同行为。

### 日期编辑与 Journal 图片（iOS/iPadOS build 13）

Task 的 Scheduled 和 Deadline 现在分别读取和保存，互不替换。失败保存保留当前会话中的输入；元数据、重复日期区间、移动和归档的附件处理也已修正。具体写回规则见 [Org 原文与界面](org-and-aster.md)。

Journal 连续多张图片按固定宽度横向排列，空间够就并排显示，溢出时左右滑动。图片上方显示位置标志，点击查看完整原图，详见 [Journal 与 Capture](journal-capture.md)。这些修正不代表 Android build 9 已包含相同行为。

### iPad Search 修正（iOS/iPadOS build 12）

在宽屏搜索结果中连续选择不同任务，右侧详情会跟随当前选择刷新，不再保留第一个任务的表单。切换前的修改保存回原任务，不会带到下一个任务；搜索词和结果列表仍保留。

文件夹排序也包含在 iOS/iPadOS build 12 中，操作和日期含义见[文件树排序](files-preview-attachments.md)。这些变更不代表 Android 版本已包含相同行为。

### 旧系统操作栏修正（iOS/iPadOS build 12）

build 12 分别处理旧系统和 Liquid Glass 系统的根操作栏布局。iOS 17/18 使用材质胶囊并独立处理底部安全区，避免中间图标上浮；iOS 26 及以后保留系统原生玻璃效果。此项不改变按钮顺序或导航方式。

已在 iOS 18.6 的 iPhone 和 iPad 模拟器上检查浅色、深色布局及导航。用户报告中的 iOS 18.7.8 真机仍需要更新后确认。

## 共同产品边界

- Org 文件和相邻的 Org Attach `data/` 目录始终是事实来源。
- Event、Task、Project、Habit、Note、Container 和自定义 Workflow 由 Org 语义决定，不由平台或固定英文关键字决定。
- Agenda 的日历、时间线和 Overdue 跳转共享同一个选中日期；Journal 的真实来源日期与浏览位置保持明确边界。
- 状态切换只写入用户明确选择的完整关键字。Agenda 与 TODOs 不把左右滑动分配给 Workflow。
- 本地索引、提醒计划和小组件内容都可以从工作区重建，不成为第二份数据源。
- Files 只呈现已连接的云端工作区，不把应用内部工作副本或通用本地 Import 暴露成另一种来源。

iOS/iPadOS build 12 的小组件会在跨日时移除旧的 Habit 当日记录，不再把它累计为普通逾期任务。普通未完成任务的逾期计数不受影响；新的 Habit 内容仍由 Aster 运行后按源文件刷新。

## 平台差异

| 能力 | iOS / iPadOS | Android |
| --- | --- | --- |
| 手机导航 | iPhone 使用底部根操作栏 | Android 手机使用底部导航与平台原生弹层 |
| 大屏导航 | iPad 使用横向根操作栏和主从详情 | Android 平板/横向大屏使用左侧 Navigation Rail；Agenda 的日历与时间线仍位于左侧内容栏 |
| 云端来源 | Dropbox、iCloud Drive、Nutstore、WebDAV | Dropbox、Nutstore、WebDAV；没有 iCloud Drive 或通用本地 Import |
| 通知 | Aster Notifications；可另外启用 Apple Reminders 互操作 | Aster Notifications 使用 Android 通知与闹钟能力；不创建 Apple Reminders 等价桥接 |
| 系统日历 | 读取用户允许的系统日历覆盖 | 读取用户允许的 Android Calendar Provider 覆盖 |
| 文件预览 | 使用 iOS/iPadOS 原生文档能力 | 使用 Android 原生阅读器；必要时交给兼容的系统 App |
| 凭据 | 系统 Keychain | Android Keystore |
| 无障碍 | VoiceOver、Dynamic Type、Reduce Motion | TalkBack、系统字体缩放和系统动画辅助设置 |

## 反馈时为什么要写平台

Org 结果不一致通常应跨平台一起检查；导航位置、弹框、权限、通知投递或文件打开失败则可能只属于一个平台。提交问题时请明确填写平台、设备、系统版本、Aster 版本以及工作区来源。Android 问题还应说明是实体设备还是模拟器、手机布局还是平板/横屏布局。

完整字段见[提交反馈](feedback.md)。
