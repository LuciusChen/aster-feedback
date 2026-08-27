# 提交反馈

[简体中文] · [English](en/feedback.md)

[返回首页](../README.md)

好的反馈应让另一台设备可以复现同一个结果，同时不暴露真实工作区内容。

## 选择入口

- 显示错误、闪退、数据未写回、同步异常或明显变慢：使用 [Bug report](https://github.com/LuciusChen/aster-feedback/issues/new?template=bug.yml)。
- 新的使用场景、交互建议或 Org 能力建议：使用 [Feature request](https://github.com/LuciusChen/aster-feedback/issues/new?template=feature.yml)。

提交前先搜索已有 Issue，避免把同一个问题拆成多个无法汇总的线索。

## Bug 最小信息

1. Aster 版本和 Build，例如 `0.1 (1)`。
2. 设备和系统，例如 `iPhone 17 Pro · iOS 26.0`。
3. 工作区来源，例如 Dropbox、iCloud Drive、Nutstore 或 WebDAV。
4. 从哪个页面开始，依次做了什么。
5. 预期结果和实际结果。
6. 最小、脱敏的 Org 源文本。
7. 必要时附截图或短视频。

## 如何提供 Org 最小复现

优先提供一个独立标题，不要粘贴整份文件：

```org
#+TODO: TODO NEXT | DONE

* TODO Example :demo:
SCHEDULED: <2026-09-03 Thu 09:15>
:PROPERTIES:
:STYLE: habit
:END:
```

保留触发问题的语法结构，替换真实标题、标签、链接、ID、路径和正文。如果问题只在层级中发生，再补最少的父/子标题。

### 附件问题

说明：

- 文件扩展名和大致大小。
- 对应标题是否有 `ID`、`ATTACH` 标签和 `attachment:` 链接。
- 问题发生在新建、详情、Files 打开还是同步后。

不需要公开上传私人附件原件；可用一张空白图片或测试 PDF 复现。

### 同步问题

说明错误出现于连接、列表、上传、下载、删除、冲突还是重新授权；同时说明网络环境和最近一次成功同步。不要提供账号密码、Token 或完整授权 URL。

### 性能问题

说明：

- 操作和等待时长，例如“纯文字 Capture 约 10 秒”。
- 文件数量、文件夹数量以及目标文件的大致大小。
- 是每次发生，还是首次/同步后发生。
- 等待期间界面能否滚动和取消。

## Feature request 怎么写

先描述场景和 Org 语义，再描述界面：

```text
场景：我有一个无具体时间的日期型 Task，希望当天 09:00 提醒。
当前：日期进入 TODOs，但提醒设置不容易发现。
期望：在详情中开启“Remind me on a day”后再展开时间。
Org：保留 DEADLINE 日期，并用 Aster reminder properties 表示提醒。
```

这种描述比“加一个按钮”更容易判断是否与 Event、Task、Repeat、Diary 和写回规则冲突。

## 数据安全优先级

如果发现以下情况，请停止继续编辑同一文件并立即反馈：

- 未支持的 Org 正文、抽屉或块被删除。
- 状态/日期修改覆盖了不相关子树。
- 附件复制成功但源链接丢失，或反过来。
- 冲突被静默覆盖，没有留下可恢复的两份内容。
- 页面显示保存成功，但 Files 中的源文件没有变化。

先复制出受影响文件和相邻 `data/` 目录，再尝试恢复或重连。
