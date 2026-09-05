<p align="right">🌐 <strong>简体中文</strong> · <a href="en/platforms.md">English</a></p>

# 平台支持与差异

[返回首页](../README.md) · [快速开始](quick-start.md)

Aster 在 Apple 与 Android 平台共享 Org 解析、分类、写回、同步冲突、Agenda、TODOs、Journal、Search 和附件语义。界面不会逐像素复制另一平台：信息层级与操作结果保持一致，导航、弹层、权限和系统集成使用各自平台的原生方式。

## 当前版本状态

| 平台 | 最低系统 | 当前基线 | 分发状态 |
| --- | --- | --- | --- |
| iPhone / iPad | iOS / iPadOS 17 | 0.1 (11) | TestFlight 测试 |
| Android 手机 / 平板 | Android 8.0（API 26） | 0.1 (9) | Google Play 测试准备中，尚未公开发布 |

两个平台可以分别推进 Build/Version Code。反馈时请写明平台和完整版本；相同营销版本号不表示系统能力或修复进度完全相同。

## 共同产品边界

- Org 文件和相邻的 Org Attach `data/` 目录始终是事实来源。
- Event、Task、Project、Habit、Note、Container 和自定义 Workflow 由 Org 语义决定，不由平台或固定英文关键字决定。
- Agenda 的日历、时间线和 Overdue 跳转共享同一个选中日期；Journal 的真实来源日期与浏览位置保持明确边界。
- 状态切换只写入用户明确选择的完整关键字。Agenda 与 TODOs 不把左右滑动分配给 Workflow。
- 本地索引、提醒计划和小组件内容都可以从工作区重建，不成为第二份数据源。
- Files 只呈现已连接的云端工作区，不把应用内部工作副本或通用本地 Import 暴露成另一种来源。

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
