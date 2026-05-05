---
title: 中断的反向链接机会文档
description: 了解中断的反向链接机会，以及如何使用它来提高流量获取。
badgeTrafficAcquisition: label="流量获取" type="Caution" url="../../opportunity-types/traffic-acquisition.md" tooltip="流量获取"
source-git-commit: 97e61d3061fb68225eece98ba0f94affb08be9e3
workflow-type: ht
source-wordcount: '655'
ht-degree: 100%

---


# 中断的反向链接机会

<!--![Broken backlinks opportunity](./assets/broken-backlinks/hero.png){align="center"}-->

>[!VIDEO](https://video.tv.adobe.com/v/3483250/?learn=on&enablevpops)

中断的反向链接机会可识别出指向网站上不存在 (404) 页面的外部链接。这些链接会导致损失引荐流量，降低 SEO 价值，因为搜索引擎依靠反向链接来评估相关性和权威性。当 URL 改变、内容被移除或页面不再可用却没有进行正确的重定向时，就会出现这些问题。AEM Sites Optimizer 可识别所有中断的反向链接，提供特定的 AI 建议，启用一键式部署修复它们，所有这些功能都能在一个集中视图中完成。

## 自动识别

<!--![Auto-identify broken backlinks](./assets/broken-backlinks/auto-identify.png){align="center"}-->

AEM Sites Optimizer 会持续扫描外部数据源，检测那些指向网站上不存在的 404 页面的反向链接。数据从多个来源聚合，包括 Google Search Console、[运行遥测](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/sites/operational-telemetry-for-aem-as-a-cloud-service)和第三方 SEO 平台。自动识别机会可识别出链接到损坏 URL 的外部域，并根据造成的影响，包括域权威性、预期流量以及链接权益损失，对其进行优先级排序。

此机会列出了所有发现的问题，包含以下详细信息：

* **反向链接域和页面**——包含损坏链接的外部页面或域。
* **优先级**——高、中或低优先级，表示损坏的链接对 SEO 过程的影响。
* **损坏的目标 URL**——链接到您网站上不存在的 URL。

## 自动建议

<!--![Auto-suggest broken backlinks](./assets/broken-backlinks/auto-suggest.png){align="center"}-->

对于每个发现的损坏反向链接，AEM Sites Optimizer 都会推荐最合适的恢复流量和 SEO 价值的目标。它通过进行以下分析来确定反向链接的意图：

* URL 结构和令牌
* 锚点文本
* 反向链接页面的标题和上下文

将这个意图与现有的网站内容进行匹配，以确定最相关的目标页面。每个损坏的 URL 都会映射到一个准确的替换页面或者最相关的页面。如果无法确定合适的目标，就会将问题显示出来，以进行手动审阅。

>[!BEGINTABS]

>[!TAB AI 原理]

<!--![AI rationale on autosuggestion of broken backlinks](./assets/broken-backlinks/auto-suggest-ai-rationale.png){align="center"}-->

选择&#x200B;**信息**&#x200B;图标，查看所建议 URL 的 AI 原理。 该原理解释了为什么 AI 认为所建议的 URL 最适合中断的链接。 它可以帮助您了解 AI 的决定过程，并在了解相关信息的情况下决定接受或拒绝该建议。

>[!TAB 编辑目标 URL]

<!--![Edit suggested URL of broken backlinks](./assets/broken-backlinks/edit-target-url.png){align="center"}-->

如果您不同意 AI 生成的建议，可以选择&#x200B;**编辑图标**&#x200B;来编辑所建议的 URL。 通过编辑，您可以手动输入您认为最适合替代断链的 URL。 Sites Optimizer 还会列出您网站上它认为可能适合中断链接的任何其他 URL。

>[!TAB 忽略条目]

<!--![Ignore broken backlinks](./assets/broken-backlinks/ignore.png){align="center"}-->

您可以选择忽略包含目标中断 URL 的条目。 选择![删除图标或忽略图标](https://spectrum.adobe.com/static/icons/ui_18/CrossSize500.svg)，将中断的反向链接从机会列表中移除。 从机会页面顶部的&#x200B;**已忽略**&#x200B;选项卡中可以重新启动已忽略的中断反向链接。

>[!ENDTABS]

## 自动优化

<!--[!BADGE Ultimate]{type=Positive tooltip="Ultimate"}-->

建议被审阅和批准之后，您可以点击&#x200B;**部署优化**。然后，AEM Sites Optimizer 会根据您的实施中管理重定向的方式，将修复应用到创作环境中。然后，AEM 作者可以从内容管理系统 (CMS) 发布这些更改。

根据配置情况，修复将以内容更改或代码更改的形式应用到现有的部署工作流中。优化过程包括以下步骤：

* **验证**——确保更改内容按预期工作，在部署之前不会引入回归。
* **部署**——通过现有流程应用更改，例如在 AEM 中进行内容更新或者通过 CI/CD 管道进行代码部署。
* **权限检查**——验证用户是否具有部署更改的适当权限。如果没有，会提供替代输出，例如可下载的重定向列表或代码补丁。

此过程可确保重定向能够准确实施，在发布之前经过验证，并与现有的配置和治理流程保持一致。
