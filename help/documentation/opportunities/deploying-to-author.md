---
title: 部署到创作文档
description: 了解AEM Sites Optimizer如何将所选优化部署到创作环境，以及如何随后跟踪它们。
product_v2: id: fd1f54a9-f50c-467d-8956-cebbaf4f3eb8
topic_v2: id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: 1d55c607aab6c820d014b9a57bfae20b8170c672
workflow-type: tm+mt
source-wordcount: 245
ht-degree: 6%

---

# 部署到创作文档

<!--![Deploying to author](./assets/deploying-to-author/hero.png){align="center"}-->

在AEM Sites Optimizer发现商机并提出优化建议后，您可以查看和部署所选优化以执行进一步操作。

## 部署到作者

从机会列表中选择一个或多个建议，然后单击&#x200B;**部署到作者**&#x200B;以部署您的选择，或单击&#x200B;**全部部署到作者**&#x200B;以一次部署每个可用的建议。 AEM Sites Optimizer仅将所选优化应用于创作环境 — 它不会将更改发布到您的实时网站。 然后，AEM作者可以从内容管理系统(CMS)中查看和发布更改，这些更改与每个机会自己的[自动优化](/help/documentation/opportunities/missing-alt-text.md#auto-optimize)工作流一致。

当您没有部署权限或站点未完全配置为可部署（例如，尚未连接代码存储库）时，将禁用此操作。 无论属于哪种情况，Sites Optimizer都会在“已禁用”按钮旁边解释原因。

## 跟踪已部署的优化

<!--![Deployed tab](./assets/deploying-to-author/deployed-tab.png){align="center"}-->

部署所选优化后，您可以管理它们，并从商机的详细信息页面上的&#x200B;**已部署**&#x200B;选项卡以及&#x200B;**当前**&#x200B;和&#x200B;**已忽略**&#x200B;选项卡中执行后续步骤。

具体的部署机制（包括如何将更新应用于Edge Delivery Services、AEM as a Cloud Service或数字资产管理）因机会类型而异。 有关详细信息，请参阅该机会自己的&#x200B;**自动优化**&#x200B;部分。

## 另请参阅

* [缺少替代文本机会](/help/documentation/opportunities/missing-alt-text.md#auto-optimize)
* [Core Web Vitals 机会](/help/documentation/opportunities/core-web-vitals.md#auto-optimize)
* [中断的反向链接机会](/help/documentation/opportunities/broken-backlinks.md#auto-optimize)
