<p align="right">🌐 <strong>简体中文</strong> · <a href="en/quick-start.md">English</a></p>

# 快速开始

[返回首页](../README.md)

## 1. 测试前准备

- 使用 iOS 或 iPadOS 17 及以上版本。
- 先备份准备连接的 Org 工作区。
- 如果使用 Org Attach，确认备份同时包含 Org 文件旁的 `data/` 目录。
- 第一次测试建议使用单独的测试文件夹，确认同步和回写符合预期后再连接主要工作区。

## 2. 连接云端工作区

打开 **设置 → Cloud Workspace**，选择一个来源：

- Dropbox
- iCloud Drive
- Nutstore
- WebDAV

选择工作区根目录，并决定是否包含其子目录。Aster 会把云端内容同步到应用本地工作区，再从本地的真实文件建立 Agenda、TODOs、Journal、Files 和 Search 索引。

连接成功后，先在 **Files** 中检查：

1. 目录层级是否完整。
2. `.org` 文件是否可以预览和编辑。
3. 图片、PDF 和设备可预览的其他文件是否可以打开。
4. 云端尚未下载的文件，等待同步完成后是否会自动打开。

## 3. 指定三个工作路径

工作区根目录只回答“文件在哪里”。下面三个角色要分别配置，但都会显示已选择的实际相对路径：

| 设置 | 作用 | 示例 |
| --- | --- | --- |
| Agenda Sources | 哪些 Org 文件参与日程、TODOs、Perspective 和提醒索引 | `agenda/` |
| Journal folder | 每日 Journal 文件保存和读取的位置 | `denote/daily/` |
| Event & Task Inbox | 新建 Event 与 Task 默认写入的 Org 文件 | `agenda/inbox.org` |

路径可以位于任意已连接的工作区根目录下。Agenda Sources 也可以直接选择“工作区根目录”：关闭“包含子文件夹”时只索引根目录中的 Org 文件，打开后才递归索引该根目录。它们不要求使用固定的文件夹名称。

## 4. 确认 Workflow

打开 **设置 → Tasks & Workflow**，检查 Process 和 Terminal 关键字是否与自己的 Org 文件一致。

例如：

```org
#+TODO: TODO NEXT WAITING PROJECT | DONE CANCELLED
```

这里的 `TODO`、`NEXT`、`WAITING` 和 `PROJECT` 是进行中状态，`DONE` 和 `CANCELLED` 是终止状态。Aster 会保留每个关键字的完整文本，并允许为关键字配置图标、颜色和是否“Treat as Project”。

如果文件自身有 `#+TODO:` 或 `#+SEQ_TODO:`，文件内声明优先于全局设置。

## 5. 创建第一个条目

根操作栏分为三部分：

- 一级 Tab 是 **Views、Files、Settings**。
- 左侧三横线按钮切换当前二级内容。Views 中有 Agenda、TODOs、Journal 和自定义 Perspective；Files 中有 Workspace、角色文件夹和你从文件树固定的文件夹；Settings 中有“工作区与同步”“外观”“日历与日程”“任务与工作流”“新建与存储”和“日志”六个常用分类。“关于 Aster”仍留在 Settings 首页。iPad 上选择分类会直接切换现有右侧详情，iPhone 上则直接进入该分类，不会连续叠加多层页面。
- 右侧是“新建”和“搜索”。轻点新建会按当前内容创建：Agenda 新建 Event、TODOs/Perspective 新建 Task、Journal 新建日志条目、Files 新建 Org 文档；长按可查看全部四种类型。
- 搜索打开后会自动聚焦；轻点 Cancel 会关闭搜索并回到进入前的页面。

iPhone 上三组控件位于同一条底栏；一级 Tab 的可用空间足够宽时显示图标和文字，包括 iPhone 15 在内的较紧凑宽度会只显示图标，三个入口的选中状态和无障碍名称仍然独立可用，不会互相重叠。进入设置子页面、条目详情或文档阅读页后，页面最后一行文字和控件都能继续滚动到这条底栏上方。iPad 上二级按钮位于整个窗口左侧，一级 Tab 严格相对整个窗口居中，新建和搜索位于最右侧；三组控件共用同一个垂直中心，也不会增加全局侧边栏。

## 6. 验证写回

建议完成一次最小闭环：

1. 在 TODOs 新建一条 `TODO`。
2. 打开详情，修改优先级、日期或标签。
3. 返回 Files，打开对应 `.org` 文件的编辑页。
4. 确认标题行、规划时间、属性和标签已写入真实源文本。
5. 等待同步后，在另一个客户端或 Emacs 中打开同一文件确认结果。

## 7. 第一次遇到同步错误时

- 不要连续重复提交同一编辑。
- 先打开 Files，确认本地文件内容是否已经保存。
- 查看设置中的连接状态和同步状态。
- 如果提示重新连接，先确认云端账号本身可以正常访问，再在 Aster 中重新授权。
- 若出现冲突，比较两份内容后明确选择保留哪一份，不要把同步冲突当成普通网络重试。

更完整的说明见 [同步、数据安全与冲突](sync-and-safety.md)。
