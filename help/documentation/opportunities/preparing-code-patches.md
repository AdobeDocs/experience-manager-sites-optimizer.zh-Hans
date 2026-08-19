---
title: 准备代码修补程序文档
description: 了解AEM Sites Optimizer如何为Core Web Vitals修复准备代码修补程序以及随后如何跟踪它们。
product_v2:
  - id: fd1f54a9-f50c-467d-8956-cebbaf4f3eb8
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: a86d83ee226055e6401b13fd421b40d449b96fa8
workflow-type: tm+mt
source-wordcount: 248
ht-degree: 2%

---

# 准备代码修补程序文档

<!--![Preparing code patches](./assets/preparing-code-patches/hero.png){align="center"}-->

对于[核心Web虚拟机会](/help/documentation/opportunities/core-web-vitals.md)，AEM Sites Optimizer会为已识别的性能问题生成代码级修复。 您可以查看和准备作为代码修补程序的这些修补程序，而不是直接部署它们。

## 准备代码修补程序

从Core Web Vitals列表中选择一个或多个问题，然后单击&#x200B;**准备代码修补程序**&#x200B;以准备您的选择，或单击&#x200B;**准备所有代码修补程序**&#x200B;以一次准备每个可用的修补程序。 AEM Sites Optimizer会为每个修复创建一个标记的GitHub问题，并自动打开一个包含代码更改的链接拉取请求，以供您的团队审查、测试和合并。

如果您没有准备代码修补程序的权限，或者站点未针对它进行完全配置（例如，未连接任何代码存储库或修补程序生成仍在进行中），将禁用此操作。 对于每种情况，Sites Optimizer都会在“已禁用”按钮旁边解释原因。

## 跟踪准备的代码修补程序

准备好代码修补程序后，您可以从Core Web Vitals详细信息页面的&#x200B;**已部署**&#x200B;选项卡以及&#x200B;**当前**&#x200B;和&#x200B;**已忽略**&#x200B;选项卡中管理这些修补程序并执行后续步骤。 该处的修补程序状态反映其拉取请求是否已合并，而不仅仅是生成 — 仅当修补程序实际合并到代码库后，问题才会变为&#x200B;**已部署**。

## 另请参阅

* [Core Web Vitals 机会](/help/documentation/opportunities/core-web-vitals.md#auto-optimize)
* [部署到创作文档](/help/documentation/opportunities/deploying-to-author.md)
