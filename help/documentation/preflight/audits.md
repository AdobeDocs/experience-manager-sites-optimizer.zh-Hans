---
title: 在 Preflight 中运行审核
description: 了解如何在您的页面上启动 Preflight 审核。
product_v2:
  - id: fd1f54a9-f50c-467d-8956-cebbaf4f3eb8
    internal-label: Experience Manager
source-git-commit: ae16300fdfd525dc5efead2f165d5a238b5ea6e1
workflow-type: tm+mt
source-wordcount: '469'
ht-degree: 17%
---

# Preflight 中的审核

Preflight 会审核您的页面，在发布之前识别出增强内容的机会。 与自动扫描不同，您可以选择运行审核的时间，以便在准备就绪时分析页面。

![带有“分析”页面按钮的“预检”登陆屏幕](./assets/audits/hero.png){align="center"}

要为页面运行 Preflight 审核，请执行以下操作：

1. 在您的[创作环境](./access-preflight.md)（通用编辑器、基于文档的创作或 AEM Sites 页面编辑器）中打开要审核的页面。
1. 打开 [Preflight 面板](./access-preflight.md)。 预检打开到&#x200B;**运行性能准备审核**&#x200B;登陆屏幕。
1. 如果需要，请打开&#x200B;**设置**（齿轮图标）以在内部链接审核中包含网站导航链接。 默认情况下，此设置处于关闭状态，在通用编辑器中不可用。 根据您的环境，**设置**&#x200B;显示在标题或&#x200B;**更多操作** (**...**)中。 请参阅[包含网站导航链接](./opportunities/seo/internal-links.md#include-site-navigation-links)。
1. 选择&#x200B;**分析页面**。 印前检查在当前页面上运行其所有审核并打开就绪仪表板，其中显示已通过的审核百分比以及找到的机会（按类别分组）。

要了解预览结果并识别优化机会，请参阅[Preflight中的审核结果](./audit-results.md)。

## 使用集成的预检按钮

如果您的创作环境运行[AEM 2026.7.0 （发行版27083）](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/release-notes/maintenance/2026/2026-7-0#release-27083)或更高版本，则AEM Sites页面编辑器工具栏中内置了Preflight。 选择&#x200B;**印前检查**&#x200B;图标（播放按钮）以打开当前页面的面板，然后选择&#x200B;**分析页面**&#x200B;以运行审核。

>[!VIDEO](https://video.tv.adobe.com/v/3496629?learn=on&enablevpops)

## 继续上一个会话

Preflight会记住您最近运行的情况，因此，如果您离开并返回，则无需重新运行审核。

* 如果您在&#x200B;**同一浏览器选项卡**&#x200B;中重新打开“印前检查”面板（包括在刷新后），“印前检查”将自动加载您上次运行的结果。
* 如果您在新选项卡中或在关闭浏览器&#x200B;**后返回**，登陆屏幕将在&#x200B;**分析页面**&#x200B;旁边显示&#x200B;**继续上一个会话**&#x200B;按钮。 选择&#x200B;**继续上一个会话**&#x200B;以重新加载您最近的结果，或选择&#x200B;**分析页面**&#x200B;以开始新运行。

印前检查会单独跟踪每个页面的最新运行，因此&#x200B;**继续上一个会话**&#x200B;始终为您所在的页面重新加载上次运行。

当您重新加载以前的运行时，标题会显示该运行执行的时长，例如&#x200B;*2分钟前*&#x200B;或&#x200B;*昨天*，以便您一眼就能看出结果的当前程度。 标签会随着时间的推移而更新，并在就绪控制面板和审核详细信息页面之间移动时保持可见。

审核完成并显示结果后，在工具栏中选择&#x200B;**重新分析**&#x200B;以放弃结果并重新运行每次审核。 查看Preflight](./audit-results.md#toolbar)中的[审核结果。
