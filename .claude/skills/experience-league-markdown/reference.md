---
source-git-commit: 14f10c231373992c49a8bb93c043556305b6280d
workflow-type: tm+mt
source-wordcount: '1030'
ht-degree: 0%

---
# Experience League Markdown — 完整语法参考

摘自https://experienceleague.adobe.com/en/docs/authoring-guide/using/markdown/markdown-syntax （最后针对“上次更新日期：2026年6月17日”页面确认）。 如果此处某些内容似乎已过期，请重新获取实时页面。

## 前言和标题

```markdown
---
title: Title for search optimization
description: This is the article description used for search optimization.
---
# Article title
```

紧接在结束`---`之后的行（和一个空白行）必须是`# Title` — 并且它应与frontmatter中的`title:`匹配。

## 基本文本格式

- 粗体： `**bold**`
- 斜体： `*italic*`
- 粗体+斜体： `***both***`
- 转义格式化字符： `\*not italic\*`
- 段落不需要特殊语法 — 它们之间只需要一行空白。

## 标题

```markdown
# This is level 1 (article title)
## This is level 2 (mini-TOC entry)
### This is level 3
```

- `#` (H1) =文章标题，必须匹配前件`title`。
- 默认情况下，`##` (H2) =显示在mini-TOC中（前件中的`mini-toc-levels: 3`显示更多级别）。
- 从不跳过级别（`##` → `####`无效）。
- 每个标题后面的&#x200B;**和**&#x200B;前需要空白行。
- 最大标题长度：69个字符(EN)，120（本地化）。
- 标题ID/锚点： `## Creating processing rules {#processing-rules}` — 小写，带连字符。 如果标题文本以数字（例如，年）开头，则此为必填字段。 如果没有明确的ID，默认锚点为自动缩放的标题文本。

## 注释/告诫

标准类型： `NOTE`、`TIP`、`IMPORTANT`、`WARNING`。 较新的EXL专用类型： `ADMIN`、`AVAILABILITY`、`PREREQUISITES`、`INFO`、`ERROR`、`SUCCESS`。

```markdown
>[!NOTE]
>
>This is a standard NOTE block.
>
>It can include multiple paragraphs.
```

块的每一行都以`>`开头。 在类型标记后面包含一个纯`>`行。

## 选项卡

```markdown
>[!BEGINTABS]

>[!TAB iOS]

Content for the iOS tab.

>[!TAB Android]

Content for the Android tab.

>[!ENDTABS]
```

- 不能在选项卡集中嵌套选项卡集，也不能在列表中嵌套选项卡集。
- 选项卡标题是逐字渲染的 — `>[!TAB ...]`中没有标记格式。
- 在一个页面上可以使用多个选项卡集。

## 视频

```markdown
>[!VIDEO](https://video.tv.adobe.com/v/27069/?learn=on&enablevpops)
```

- 视频必须已托管在`video.tv.adobe.com` (Adobe TV/MPC)上 — 不支持原始视频文件链接或`<video>`标记。
- 建议的查询参数： `?learn=on&enablevpops` （此存储库中的每个嵌入使用的规范表单）。 添加`&autoplay=true`以进行自动播放。
- 成绩单：将`{transcript=true}`添加到短代码，或在`TOC.md`/`metadata.md`中设置整个指南/存储库的`auto-video-transcripts: true`。

## 徽章

内联徽章（在放置的位置渲染）：

```markdown
[!BADGE Beta]{type=Informative url="https://www.example.com" tooltip="Go to example.com"}
```

元数据徽章（在H1上方呈现） — 在前端内容中：

```yaml
badgePremium: label="Premium" type="Positive" url="https://www.premium-product.com" tooltip="Download Premium"
```

- `type` （不区分大小写）： `Informative` （默认/蓝色）、`Positive` （绿色）、`Negative` （红色）、`Neutral` （深灰色）、`Caution` （黄色）。
- 只有标签是必需的；`type`/`url`/`tooltip`是可选的。
- 每篇文章最多&#x200B;**两个**&#x200B;元数据徽章（可配置，但在依赖异常之前询问）。
- 元数据标记值必须加引号。 必须引用内联徽章`url`/`tooltip`。
- 从`TOC.md`使用的徽章URL必须是根相对(`/help/guide/article.md`)，而不是相对 — TOC条目适用于多个文件夹。
- `before-title="false"`将元数据徽章移动到H1下方。
- 添加`newtab=true`以在新选项卡中打开徽章URL。

## 图像

```markdown
![alt text](assets/logo.png "Hover text"){width="300" align="center"}
```

- `align`：仅`center`或`right` — 无`left`，无`valign`。
- `width`：像素(`"300"`)或视图区域的百分比(`"50%"`)。
- `zoomable="yes"`使图像点击放大（不要与同时是一个链接的图像结合 — 链接成功）。
- 共享图像的根相对路径： `/help/assets/imagename.png`。
- 限制：100 MB硬限制(GitHub)，5 MB（开始关注之前），20 MB会触发验证错误。 每篇文章最多100张图像（EDS渲染限制）。

## 链接和交叉引用

- 外部： `[Adobe](https://www.adobe.com)`
- 作为链接的裸URL： `<https://www.adobe.com>` — 未包装的裸URL **不**&#x200B;自动链接。
- 相对交叉引用： `[Overview](collaborative-doc-instructions/overview.md)` — 从&#x200B;*源*&#x200B;文件的位置解析；支持`./`、`../`、`../../`。
- 根相对交叉引用： `[Overview](/help/using/docile-rules/introduction.md)` — 从存储库中的任何文件工作，而不管源位置如何。
- 指向标题的深层链接：目标需要`{#heading-id}`；与`[Text](file.md#heading-id)`的链接（或对于同一页面仅需`#heading-id`）。
- 在新选项卡中打开： `[See What's new](whats-new.md){target="_blank"}`。

## 列表

```markdown
1. This is step 1.
1. This is the next step.
   1. Sub-step (indent 3 spaces for numbered lists)
   1. Sub-step
```

```markdown
* First item.
* Second item.
```

- 编号列表：始终写入`1.`（或始终写入`1)`） — GitHub渲染真实序列。 选择一种样式（`.`与`)`）并在文章中保持一致。
- 项目符号列表：选择`*`、`-`、`+`之一并保持一致 — 在同一篇文章中混合使用它们是一个验证错误。 大多数存储库中的约定： `*`。
- 任何列表之前和之后均需要空白行。
- 列表项（图像、表格、注释）之间的内容必须缩进到文本开头（编号列表为3个空格，项目符号列表为2个空格），否则会破坏列表。 过度缩进（6个空格）会将其转换为代码块。

## 代码块

内联： `` `code` `` — 或者内联包含三个反撇号（如果您需要在其中使用字面反撇号）。

受防护：

&grave;&grave;&grave;&grave;markdown

```javascript
var x = 1;
```

&grave;&grave;&grave;&grave;

- 始终为语法突出显示+复制按钮指定语言。
- 隔离块的上方和下方需要空白行。
- 行号： `` ``&#x200B;`html {line-numbers="true"} `&#x200B;&grave;
- 开始在其他位置编号： `` ``&#x200B;`html {line-numbers="true" start-line="7"} `&#x200B;&grave;
- 突出显示行： `` ``&#x200B;`html {line-numbers="true" start-line="7" highlight="11-13, 16"} `&#x200B;&grave;
- 代码块内容从未本地化（除`!UICONTROL`/`!DNL`标记外，这些标记在发布时会被去除）。
- 在代码块中不能使用Markdown/HTML格式（如`<i>`） — 请使用尖括号或纯文本作为占位符。

## 表格

- 标准GFM管表适用于简单情况。
- 特殊情况下允许HTML表（例如，没有标题行的表） — 否则首选标记。
- Markdown表单元格中允许有限的HTML：`<p>`、`<br>`、`<ul>`、`<ol>`。
- 可以将表设置为自动或固定渲染 — 如果需要该级别控制，请参阅语法指南中链接的“表”一文。

## 可折叠部分

```markdown
+++See details

This is text inside a collapsible section.

* Bullet one
* Bullet two

+++
```

- 不要嵌套可折叠的部分 — 它们将无法正确呈现（并且验证不会失败，因此错误将以静默方式发出）。
- 与任何其他位置一样，在部分中的内部列表/代码块周围需要空白行。

## 文本突出显示

```markdown
This sentence is normal. <span class="preview">This text is highlighted.</span>
```

使用`<span class="preview">`进行内联/段落突出显示，`<div class="preview">`用于多个段落/组件。

## 代码片段和

- 从存储库的`help/snippets.md`共享了H2锚点：引用了`{{anchor-id}}`。
- 来自`help/_includes/*.md`的共享包含文件：引用了`{{$include /help/_includes/filename.md}}`。

## 评论

```markdown
<!-- standard comment code -->
```

- 从不使用`<!--> bad comment syntax <-->`（缺少短划线） — 它以可视方式呈现，而不是隐藏文本。
- 评论在渲染的文档中不可见，但&#x200B;**对GitHub**&#x200B;上查看原始.md的任何人都可见 — 没有机密或机密信息。
- 避免在项目符号列表内添加注释（可以中断列表渲染）。 在`TOC.md`中，仅注释掉文件末尾的行，从不注释掉列表中间的。

## 空行解决方法

源中的额外空白行由渲染器折叠。 要强制使用可见的垂直空间，请将`<br>&nbsp;`放在您希望间隙的位置上。

## 转义字符

- 可避免使用反斜杠的字符： `` # { } [ ] * + - . ! `` — 例如`\# not a heading`。
- 对于尖括号(`<placeholder>`)，反斜杠不起作用 — 使用内联代码块(`` `<placeholder>` ``)或HTML实体(`&lt;placeholder&gt;`)。
- 代码块中的HTML实体&#x200B;**不是**&#x200B;转换回字符 — `&gt;`在此保留文字文本。
- 元数据(YAML frontmatter)有自己的转义规则 — 如果某个值以特殊字符（如`:`或`[`）开头，请引整个值： `title: "Processing rules: A new beginning"`。

## 受限制的允许列表

Markdown中的任意位置仅允许使用这些HTML标记；任何其他内容均为验证错误：

```
table  tbody  td  tfoot  thead  th  tr  col  colgroup
p  ul  ol  li  br
b  i  strong  u  s  em  sub  sup  span
caption  a  img  div
pre  code  codeblock
```

与HTML相比，Markdown语法更适合任何能够执行这项工作的位置 — HTML实际上仅适用于无标头表等边缘情况。

## 明确不支持（即使本地预览呈现它们，也不要使用）

- 水平规则(`***`， `<hr>`)
- 表情符号短码(`:bowtie:`)
- 任务列表(`- [x] done`)
- 超出注释/制表符/视频短代码的块引用&#x200B;*组件*（纯`>`块引用呈现为引用，而不是样式化组件）
- Markdown定义列表语法（请改用手动粗体+短划线格式： `**Frog** - An amphibious green creature.`）
- `valign`图像

## 值得了解的文件大小/计数限制

| 内容 | 限制 |
|---|---|
| 图像/下载文件大小 | 验证警告为5 MB，错误为20 MB，硬GitHub上限为100 MB |
| 每篇文章的图像数 | 100（EDS渲染限制） |
| 每篇文章的元数据徽章 | 2（默认） |
