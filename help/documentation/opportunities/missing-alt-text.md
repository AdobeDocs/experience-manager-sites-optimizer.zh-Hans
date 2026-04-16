---
title: 缺少替代文本文档
description: 了解缺少替代文本机会，以及如何使用它来提高您网站上的参与度。
badgeEngagement: label="参与度" type="Caution" url="../../opportunity-types/engagement.md" tooltip="参与度"
source-git-commit: 8052c94f778829012f023fe470411dfe77ef46b9
workflow-type: tm+mt
source-wordcount: '689'
ht-degree: 37%

---


# 缺少替代文本机会

![缺少替代文本机会](./assets/missing-alt-text/hero.png){align="center"}

缺少替换文本机会可识别网站上没有描述性替换文本的图像。 如果没有替换文本，依赖屏幕阅读器的用户将无法解读视觉内容，从而造成辅助功能障碍。 它还限制了搜索引擎理解和索引图像的方式，降低了内容可发现性和搜索性能。 AEM Sites Optimizer识别缺少的替换文本问题，提供特定的AI建议，并允许一键式部署修复这些问题，所有这些都在单个集中式视图中。

## 自动识别

![自动识别缺少替代文本](./assets/missing-alt-text/auto-identify.png){align="center"}

AEM Sites Optimizer通过使用一种多步骤审核来扫描您的网站，该审核将网站抓取、真实用户流量数据和AI分析整合在一起，以识别需要替换文本但未定义替换文本的图像。 它还会评估页面上的图像，以根据Web内容无障碍准则(WCAG)确定是否需要替换文本，不包括装饰性或非信息性图像。 根据图像在页面中的角色和相关性分析图像，确定对辅助功能和SEO影响最大的修复的优先级。

此机会提供已发现问题的列表，包括：

* **页面**——包含缺少替代文本的页面路径。
* **图像**——缺失描述性替代文本的图像。

## 自动建议

![自动建议缺少替代文本](./assets/missing-alt-text/auto-suggest.png){align="center"}

对于每个已识别的问题，AEM Sites Optimizer会为图像建议一个描述性替换文本。 它使用AI视觉模型分析图像并生成反映其在页面中的内容和角色的描述。 推荐非常简洁、相关，并且符合辅助功能最佳实践。 每个建议都可以在应用之前进行审查和编辑。

>[!BEGINTABS]

>[!TAB 编辑缺少替代文本]

![编辑缺少替代文本](./assets/missing-alt-text/edit-alt-text-value.png){align="center"}

如果您不同意 AI 生成的建议，可以选择&#x200B;**编辑图标**&#x200B;来编辑所建议的替代文本。 使用该功能可以手动调整您认为最适合图像的文字。 编辑窗口包含以下内容：

* **页面路径**——只读字段，显示出现缺少替代文本问题的页面路径。 单击路径旁边的箭头，打开相应的页面。
* **图像**——需要替代文本的图像的只读预览。
* **目标替代文本**——可编辑字段，您可以在其中手动输入图像的描述性替代文本。 确保替代文本简洁明了地传达图像的内容和目的。 自然地包含相关的关键词，不要过多。

>[!TAB 忽略条目]

您可以选择忽略机会列表中的条目。 选择![删除图标](https://spectrum.adobe.com/static/icons/ui_18/CrossSize500.svg)，从列表中移除该条目。 从机会页面顶部的&#x200B;**已忽略**&#x200B;选项卡中可以重新启动已忽略的条目。

>[!ENDTABS]

## 自动优化

[!BADGE Ultimate]{type=Positive tooltip="Ultimate"}

>[!VIDEO](https://video.tv.adobe.com/v/3483274/?captions=chi_hans&learn=on&enablevpops)

审查并批准建议后，您可以单击&#x200B;**部署优化**。 然后，AEM Sites Optimizer会根据在您的实施中管理替换文本的方式，将修复应用到创作环境中。 然后，AEM作者可以从内容管理系统(CMS)发布更改。

根据配置，更新可以直接应用于页面内容、资产元数据或支持内容模型。 优化过程包括以下步骤：

* **验证** — 确保在不影响现有功能的情况下安全应用更新。
* **部署** — 通过现有流程应用更新，例如AEM中的内容更新或与内容API的集成。
* **权限检查** — 验证用户是否具有应用更改的适当权限。 如果没有，则可以使用替代输出（例如可下载的更新）进行切换。

更新在支持的地方进行版本控制，提供可视性和回滚容量。 这可确保准确地应用替换文本更新，与现有实施保持一致，并与治理和可访问性标准保持一致。

AEM Sites Optimizer会根据您的设置自动应用替换文本更新，如下所示：

>[!BEGINTABS]

>[!TAB Edge Delivery Services]

更新源文档（例如，Google Docs或SharePoint）。

>[!TAB AEM as a Cloud Service]

通过内容API直接写入更新，并提供版本控制和回退支持。

>[!TAB 数字资产管理（可选）]

在适用的地方更新资产级别的替换文本。

>[!ENDTABS]
