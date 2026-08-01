---
name: experience-league-markdown
description: 在Adobe Experience League/Adobe-Enterprise-Docs存储库(help/**/*.md)中编写或编辑Markdown文件时使用 — 可管理前台内容、标题、注释(NOTE/TIP/IMPORTANT/WARNING/etc.)、选项卡(BEGINTABS/TAB/ENDTABS)、视频嵌入、徽章、图像、链接/交叉引用、表、列表、代码块和Experience League验证管道强制实施的HTML受限制标记。
source-git-commit: 14f10c231373992c49a8bb93c043556305b6280d
workflow-type: tm+mt
source-wordcount: '659'
ht-degree: 1%

---


# Experience League Markdown

## 概述

Experience League文档使用GitHub风格的Markdown以及一组自定义扩展（基于块格式的快捷键、徽章、选项卡、视频嵌入）。 创作管道&#x200B;**验证**&#x200B;这些文件 — 使用不受支持的语法（原始`<video>`标记、`<hr>`、任务列表、混合项目符号字符、跳过的标题级别、过大的图像）会导致生成/验证错误，而不仅仅是样式错误。

Source真相： https://experienceleague.adobe.com/en/docs/authoring-guide/using/markdown/markdown-syntax （如果本地reference.md似乎已失效 — “上次更新”日期在顶部，请获取此页面）。

每个短代码和规则的完整语法引用： [reference.md](reference.md)。 在编写任何非琐碎内容（选项卡、视频、徽章、表格和HTML）之前阅读该文档。

## 快速参考

| 元素 | 语法 | 注释 |
|---|---|---|
| Frontmatter | `---\ntitle: ...\ndescription: ...\n---` | 空白行，则`# Title`必须位于下一个 |
| 标题级别 | `#`, `##`, `###` | `#` =标题（与frontmatter `title`匹配），`##` = mini-TOC条目。 绝不跳过某个级别。 前/后空白行。 最多69个字符(EN) |
| 标题Id | `## Heading text {#custom-id}` | 如果标题以/包含数字（如`## 2026 release notes {#2026-release-notes}`），则此为必填字段 |
| 注意/提示/等 | `>[!NOTE]`，然后`>`，然后`>Text`（每行占一行） | 类型：注意、提示、重要、警告、警告、管理员、可用性、先决条件、信息、错误、成功 |
| 选项卡 | `>[!BEGINTABS]` / `>[!TAB Title]` / `>[!ENDTABS]` | 无法嵌套选项卡集；无法在列表中进行嵌套 |
| 视频 | `>[!VIDEO](https://video.tv.adobe.com/v/ID/?learn=on&enablevpops)` | 必须托管在video.tv.adobe.com上 — 没有原始`<video>`/文件链接 |
| 图像 | `![alt text](assets/img.png "hover text"){width="300" align="center"}` | `align`是`center`或仅限`right`（无`left`，无`valign`） |
| 链接（相对） | `[Text](../folder/file.md)` | 源文件位置的帐户 |
| 链接（根） | `[Text](/help/guide/file.md)` | 在存储库中的任意位置工作；TOC.md徽章URL需要 |
| 深层链接 | `[Text](file.md#heading-id)` | 目标标题需要显式`{#heading-id}` |
| 外部链接（纯URL） | `<https://example.com>` | 空URL不会自动链接 — 在`< >`中换行或使用`[text](url)` |
| 项目符号列表 | `* item` （从`*`/`-`/`+`中选择一个，保持一致性） | 列表前/后空白行；混合标记=验证错误 |
| 编号列表 | `1. item` （每行重复`1.`） | GitHub渲染实数 |
| 代码（内联） | `` `code` `` | 对于文件名、命令、值、未验证的示例URL |
| 代码（受防护） | ` `&#x200B;``language ` ... ` ``&#x200B;` ` | 始终指定语言；前/后空行；`{line-numbers="true" start-line="n" highlight="n-m"}`可选 |
| 徽章（内联） | `[!BADGE Beta]{type=Informative url="..." tooltip="..."}` | `type`：信息/正面/负面/中性/警告 |
| 可折叠 | `+++Summary` ... `+++` | 无嵌套的可折叠项；内部列表/代码周围有空白行 |
| 空白行攻击 | `<br>&nbsp;`在其自己的行中 | 呈现器折叠/忽略纯额外空白行 |
| 注释 | `<!-- text -->` | 从不`<!--> text <-->` — 任何查看GitHub上的原始文件的人都可见，因此没有密钥 |

## 常见错误

- **原始`<video>`、`<iframe>`或其他非的HTML**→验证错误。 HTML的允许列表为： `table tbody td tfoot thead th tr col colgroup p ul ol li br b caption i strong u s span sub sup a img div em pre code codeblock`。 任何其他内容（包括`<video>`/`<source>`）均被拒绝 — 请改用`>[!VIDEO]`短代码，这要求视频已在video.tv.adobe.com上托管。
- **`<hr>`/ `***`水平规则，表情符号快捷键(`:bowtie:`)，任务列表(`- [x]`)** — 不支持任何内容；即使本地预览呈现它们，也不要使用它们。
- **混合项目符号字符** （`*`和`-`在同一列表中） — 验证错误。 每篇文章选择一篇。
- **跳过标题级别** （`##`直接到`####`） — 不允许。
- **没有显式ID的数字前导标题** （例如`## 2026 release notes`） — 必须添加`{#some-id}`，否则自动段塞可能会发生冲突/中断。
- **散文** (`Visit https://example.com for more`)中的裸URL将不会呈现为链接。 换行`< >`或使用`[text](url)`。
- **视觉间距的额外空白行** — 由渲染器折叠。 使用`<br>&nbsp;`而不是空的`<br>`或重复的新行。
- **超过~5 MB的图像** — 在5 MB处出现验证警告，在20 MB处出现错误。 一篇文章中有超过100张图像会中断渲染（EDS限制）。
- **frontmatter元数据**&#x200B;中有两个以上的徽章 — 默认情况下不允许使用。
- **转义问题**：反斜杠转义仅适用于`` # { } [ ] * + - . ! ``。 对于`<` `>`，如`<filename>`占位符，请使用内联代码块或HTML实体(`&lt;filename&gt;`)，而不是反斜杠。

## 提交Markdown更改之前

1. Frontmatter存在，`# Title`紧跟在后面（在空白行之后）。
2. 每个标题前后都有一个空白行；没有跳过的级别。
3. 任何视频都是`>[!VIDEO](https://video.tv.adobe.com/...)`，而不是原始`<video>`标记。
4. 任何自定义短代码(`>[!NOTE]`、`>[!BEGINTABS]`、`>[!BADGE ...]`)与[reference.md](reference.md)中的精确语法匹配 — 包括多行块中的空白`>`行。
5. 列表使用一致的项目符号/编号样式，并在整个列表周围使用空白行。
6. 链接：相对链接解析自&#x200B;*源*&#x200B;文件的文件夹；跨存储库链接或TOC/徽章链接使用根相对(`/help/...`)表单。
7. 在上面的常见错误部分的HTML之外，没有任何列入允许列表标记。
