---
description: 将内部ASO Sprint发行说明转换为面向客户的Experience League格式，并将其附加到发行说明页面。
source-git-commit: 5f400c37283d1a3d8285b4d2ac5246761a7275e6
workflow-type: tm+mt
source-wordcount: '1029'
ht-degree: 0%

---


# 发行说明转换器

将内部sprint发行说明（从Slack `#aem-sites-optimizer-announcements`渠道或`.cursor/commands/release-notes`光标输出）转换为面向客户的条目，并将其附加到`help/documentation/release-notes.md`。

## 用途

调用此技能，然后在出现提示时粘贴内部发行说明内容。 该技能将：

1. 应用以下准则进行筛选和色调规则。
2. 解析内部发行说明（表情符号分类的部分：✨功能、🚀增强功能、🤖人工智能优先、🔧修复、🏢后台）。
3. 按照准则筛选掉所有排除的类别（AI工具、后台、本地化链接、E2E测试、SitesInternal-only项目）。
4. 使用以下音调示例作为参考，重写面向客户的音调中的其余项目。
5. 按功能区域（而不是按团队或存储库）对相关项目进行分组。
6. 在下面的页面结构模板之后格式化为新版本条目。
7. 将新条目附加到`help/documentation/release-notes.md`之前（位于上一个最新条目之上，位于页面介绍段落下方）。
8. 打印摘要表，其中显示：保留项目、重写项目、删除项目（每个删除项目的原因）。

## 准则

### 核心原则

1. **客户利益优先。** 每个条目都应该回答“我以前做不到的，现在可以做什么，或者做得更好？”  — 不是“我们送了什么？” 以值开头，而不是以实施开头。

2. **领导色调。** 为决策者撰稿：成果和能力，而非技术机制。 数字体验的副总裁应该立即了解更新为什么重要。

3. **没有内部行话。** 替换所有团队内部简写：
   - “PLG”→“试用用户”或“新客户”
   - “BackOffice”→完全省略（仅用于基础架构的更改）
   - “MSM”→“AEM多站点管理器”
   - “SHM”→“站点运行状况监视器”
   - “OrcaFix”、“Cursor commands”、“AGENTS.md”→完全忽略
   - “EDS”→“Edge Delivery Services”

4. **个短条目。** 一句&#x200B;*什么*，一句&#x200B;*为什么它很重要*。 如果两个都适合一个句子，就做吧。

5. **范围准确。** 仅包括客户将在产品UI中看到的更改或在其工作流中的体验。 不包括基础架构、工具和开发人员体验更改。

6. **标记早期访问功能。** 如果某个功能在功能标记后附带，而此功能标记默认处于关闭状态（每个组织/站点的选择加入，例如通过LaunchDarkly `FeatureGate`/`isEnabledByDefault={false}`），请将`(Early Access)`附加到粗体功能名称中 — 镜像用于已分级功能的现有`(General Availability)`约定。 如有疑问，请检查是否默认对所有客户都开启该功能；如果不能，则为抢先体验。 验证代码中的功能标志默认设置 — 请勿猜测。

### 页面结构模板

每个版本条目都遵循以下结构：

```markdown
## [Month Start]–[Day End], [Year]

### New Features

- **[Feature Name]** — [One-sentence benefit statement. One sentence of business context if needed.] (append `(Early Access)` or `(General Availability)` to the feature name when the feature's availability status is notable)

### Enhancements

- **[Enhancement Name]** — [One-sentence improvement statement.]

### Bug Fixes

- [Short description of what was fixed and why it matters to users.]
```

**规则：**
- 日期范围格式： `May 11–22, 2026` （短划线，缩写月份，四位数年份）。
- 按时间倒序排列：页面顶部的最新版本。
- 仅包含具有内容的部分。 如果为空，请省略“增强功能”或“错误修复”。
- 错误修复条目不使用粗体功能名称 — 它们是简单的项目符号。
- 仅当有3个或多个用户可见的修复值得注意时，才包括错误修复。

### 要包含的内容与排除的内容

**包括：**

| 类别 | 示例 |
|---|---|
| 新的机会类型 | 广告意图不匹配，无法将CTA放在折页上方 |
| 新视图或工作流 | “已部署”选项卡、CSV导出、Jira链接 |
| 试用/载入改进 | 引导式设置流程，非现场已载入状态 |
| 设置改进 | 审核目标URL、投放类型配置 |
| 有意义的UX修复 | 计数不正确、导航中断、影响决策的显示问题 |
| 新的数据/集成 | Ahrefs数据在自然搜索中，依赖关系树在安全性中 |
| “部署到作者”功能 | 支持直接部署的新机会类型 |

**排除：**

| 类别 | 原因 |
|---|---|
| AI工具（OrcaFix、Cursor命令、AGENTS.md、Claude Code rules） | 内部开发人员工具，对客户不可见 |
| 本地化线人/预提交挂接 | 工程流程，而不是产品功能 |
| 后台/基础架构更改 | 除非它们更改最终用户行为，否则在UI中不可见 |
| React Spectrum版本升级 | 内部依赖关系，用户不可见 |
| E2E测试改进 | 工程质量，而非产品功能 |
| 发布管道自动化 | 内部流程 |
| Sites仅内部功能 | 不提供给客户 |

### 色调示例

| 内部措辞 | 面向客户的用语 |
|---|---|
| “为手动验证工作流引入了REJECTED状态” | “您现在可以将建议标记为已拒绝，以表明它们不适用于您的网站，从而使您的机会列表专注于可操作项目。” |
| “Canonical和Hreflang机会的部署视图（按日期分组）” | “对Canonical和Hreflang销售机会所做的更改现在按部署日期分组到了一个已部署的选项卡中，使您能够清楚地了解修复的内容和时间。” |
| “Alt Text Autofix V2 —‘检查修复性’预检评估” | “在部署替换文本修复程序之前，您可以运行预检检查，以验证修复程序能否成功应用于您的内容。” |
| “针对SHM指标实现了96%的存储优化” | 忽略 — 仅基础结构 |
| “AGENTS.md，带有正式的代理角色和安全护栏” | 省略 — 内部AI工具 |
| “E2E测试性能优化（~6分钟→~5分钟）” | 省略 — 工程流程 |

### 分组规则

- **按功能区域**&#x200B;分组，而不是按团队或存储库分组。 例如，所有替换文本改进（功能、增强功能和修复）都属于同一区域 — 不会将其分散到多个部分中。
- **将密切相关的修复**&#x200B;合并为单个项目符号，而不是单独列出每个项目符号（例如，“付费流量、可访问性和安全性机会中的多个显示和布局改进”）。
- **错误修复的阈值部分**：仅在有3个或多个用户可见的修复值得调用时包含此部分。 应忽略低于此阈值的微小或纯外观修复。

## 步骤

1. 应用本文件中的准则 — 将所有原则内部化，包括/排除规则、音调示例和分组规则。
2. 询问用户涵盖的日期范围（例如，“2026年5月11日至22日”）（如果尚未提供）。
3. 要求用户粘贴内部发行说明内容（或接受文件路径）。
4. 处理内容：
   - **分析**&#x200B;每个节(✨/🚀/🤖/🔧/🏢)及其项目符号。
   - 根据上面的“排除”表&#x200B;**筛选器**。 用原因标记每个已删除的项目。
   - **重写**&#x200B;以客户口吻保留项目：利益优先，无行话，简短条目。
   - **按功能区域分组**，其中有多个相关项。
   - **阈值检查**：如果存在3个以上用户可见的修复，则仅包含“错误修复”部分。
5. 使用上面的页面结构模板设置新条目的格式。
6. 读取`help/documentation/release-notes.md`的当前内容。
7. 在页面介绍段落之后立即插入新条目（在前一个最新`##`日期标题之前）。
8. 编写更新的文件。
9. 打印摘要表。

## 输入格式

该技能接受标准团队格式的内部发行说明：

```
*ASO UI Release Notes — [Date Range]*
Collaborators: [teams]

✨ *Features*
• [Feature description]

🚀 *Enhancements*
• [Enhancement description]

🤖 *AI-First Development*
• [AI tooling items — will be dropped]

🔧 *Fixes & UX Improvements*
• [Fix description]

🏢 *BackOffice*
• [BackOffice items — will be dropped]
```

## 输出

技能输出：

1. 带格式的面向客户的条目（用于在编写之前进行审阅）。
2. 修改`release-notes.md`之前的确认提示。
3. 写入后：保留/重写/已删除项目的摘要表。
