# Files、预览、编辑与附件

[简体中文] · [English](en/files-preview-attachments.md)

[返回首页](../README.md) · [Org 分类与写回](org-and-aster.md)

Files 是工作区的源文件入口。Agenda、TODOs、Journal 和 Search 提供语义视图，Files 则让你检查目录、打开原文件、阅读 Preview，并在需要时直接编辑 Source。

![Org Preview 示例](../assets/screenshots/org-preview.png)

## 文件树

Files 显示已连接工作区的真实目录层级，包括本地已有文件和同步索引中的远端项目。

- 文件夹名称后的图标只标记 Agenda、Journal 或 Capture Inbox 角色，不改变目录本身。
- 角色路径在 Settings 中配置；Files 的长按菜单只保留文件管理操作。
- 空文件夹仍是一个真实文件夹，不会因为没有子项而显示无意义的展开动作。
- Dropbox 可在文件长按菜单中查看版本历史；其他来源不会显示一个无法工作的占位入口。

## Preview 与 Edit

打开支持的文本文件后：

- **Preview**：用适合阅读的排版呈现 Org 或 Markdown。
- **Edit**：显示并编辑完整源文本。

Preview 不会成为第二份内容。修改在 Edit 中写回原文件；从 Search、Agenda 详情或预览块跳到 Source 时，Aster 会定位到对应源行。

## Org Preview 支持什么

Org Preview 按语法节点而不是简单正则扫描渲染，包括：

- 多级标题、段落、标签与部分文件元数据
- 表格、列表和复选框
- 链接、图片、标题与说明
- 属性抽屉、普通抽屉和折叠区域
- Source、Example、Quote 等块
- 动态块与相邻结果
- 脚注和内联 LaTeX

未知或暂不渲染的语法仍会保留在 Edit 源文本中。

### Dynamic Block

例如：

```org
#+BEGIN: dg-argument-gaps
/Updated: 2025-12-29 16:31/

| Claim        | Structural Gaps |
|--------------+-----------------|
| [[dg:c1][A]] | no-source       |

#+END:
```

在 Preview 中，`#+BEGIN:` 和 `#+END:` 是结构边界，不作为正文重复显示；边界之间已经存在的生成内容会按段落和表格渲染。Aster 不运行生成这个块的 Emacs 函数，更新仍应由原来的 Org/Emacs 工具完成。

### Babel

```org
#+begin_src python
print("hello")
#+end_src

#+RESULTS:
: hello
```

Aster 可以折叠、着色并阅读 Source 与 Results，但绝不执行代码。这样既保留源文件，也避免手机预览意外运行用户脚本。

## 非 Org 文件

Markdown 使用独立的 Markdown 阅读器。图片、PDF、音视频和系统支持的文档类型交给 iOS 原生预览能力；远端文件需要先下载到本地。若某个系统支持的格式无法打开，请在反馈中提供扩展名、来源和最小测试文件。

## Org Attach 对应关系

附件使用标准 Org Attach 目录：

```text
<Org 文件所在目录>/data/<ID 前两位>/<ID 其余部分>/<文件名>
```

例如标题是：

```org
* TODO Review contract :ATTACH:
:PROPERTIES:
:ID: 3d94a625-be28-4144-8158-05fbf1288d5c
:END:
[[attachment:contract.pdf]]
```

实际文件路径是：

```text
data/3d/94a625-be28-4144-8158-05fbf1288d5c/contract.pdf
```

`attachment:` 链接、标题 `ID`、`ATTACH` 标签和 `data/` 文件必须一起保留。它不是只允许 `.txt` 的 Aster 私有附件格式。

## 从哪里添加附件

- 新建 Event/Task：展开 **Show More → Attachments**。
- Event/Task 详情：在 Content 之后的 **Attachments** 区域添加或打开。
- Capture：运行生成标题的模板时选择媒体或文件。
- Files：打开源文档可查看附件链接和对应文件。

## 删除、移动与同步

- 文件或文件夹的删除、移动会通过当前云端来源执行，而不是只在界面隐藏。
- 删除非空文件夹前会明确确认递归范围。
- 远端操作成功后才更新本地树和角色配置。
- 如果一次移动或删除涉及 Agenda、Journal 或 Capture 路径，请随后在 Settings 检查相应角色是否仍有效。
