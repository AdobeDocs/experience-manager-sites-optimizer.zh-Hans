---
title: 印前检查基础知识
description: 了解Preflight的基础知识以及如何使用其界面。
source-git-commit: 85b592d5486ed5197d7bd8214c31732465c41f78
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---


# 印前检查基础知识

![预检](./assets/overview/hero.png){align="center"}

Preflight可帮助您识别在发布网页之前增强网页功能的机会。 Preflight扩展通过对您的内容运行审核来识别商机，并将结果显示在面板中，这样您就可以在发布之前处理这些商机。

## 显示Preflight的位置

印前检查功能可在不同的创作环境中使用：

* **通用编辑器** - Preflight扩展显示在&#x200B;**侧边栏**&#x200B;中。 选择该选项以开始审核当前页面。
* **基于文档的创作** — 通过Sidekick或小书签对预览的页面内容运行预检工具以查看机会列表。
* **AEM Sites页面编辑器** — 使用浏览器中的Preflight小书签开始审核。

有关设置说明，请参阅[预检设置](./setup.md)。

## 开始审核

要运行Preflight，请执行以下操作：

1. 在创作环境(通用编辑器、基于文档的预览或AEM Sites页面编辑器)中打开要审核的页面。
2. 打开“印前检查”面板 — 从侧边栏中选择“印前检查扩展”，或单击Sidekick中的“印前检查”按钮。
3. 印前检查分析页面并显示发现可增强页面的机会。

## 审核结果

审核完成后， Preflight将显示找到的业务机会。 每个机会都按类型进行整理，并包含有关如何解决问题的详细信息。

At the top of the AEM Preflight dialog is a User Progress bar that reflects overall audit results. 它显示未出现任何问题的已传递机会百分比，以及在所有机会中找到的问题总数。 用户进度栏可帮助作者快速衡量总体页面运行状况。 The bar is color-coded -- red for less than one third of opportunities complete, orange for one third to two thirds complete, and green for more than two thirds complete. While audits are still running the progress bar is shown in blue.

## 关于印前检查机会

Preflight会评估内容的多个方面，包括辅助功能、元数据、链接和可读性。 有关可用机会类型的完整列表以及如何解决它们，请参阅[印前检查机会](./overview.md)。