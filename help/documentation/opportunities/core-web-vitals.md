---
title: 核心Web Vitals机会文档
description: 了解核心Web虚拟机会以及如何使用它改善流量获取。
badgeSiteHealth: label="网站健康" type="Caution" url="../../opportunity-types/site-health.md" tooltip="网站健康"
source-git-commit: 81343812472477448fd0ee8be89bd6ae784a9e61
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 1%

---


# 核心Web虚拟机会

![核心Web虚拟机会](./assets/core-web-vitals/hero.png){align="center"}

核心Web虚拟机会可识别可能会降低网页的用户体验和自然搜索性能的问题。 这些问题由一系列因素引起，例如：自定义字体、未优化的JavaScript依赖项、第三方脚本等。 核心Web生命机会指出这些错误元素，并提出可提高网页性能的修复建议。 请注意，只能分析至少具有1000次页面查看次数的页面。

首先，核心Web Vitals机会在页面顶部显示一个摘要，其中包括问题及其对您的网站和业务影响的摘要。

* **预计的流量丢失** — 由于核心Web动态低于性能阈值而导致的预计的流量丢失。
* **预计流量值** — 丢失流量的预计值。

## 自动识别

![自动识别核心Web动态](./assets/core-web-vitals/auto-identify.png){align="center"}

在页面的下部，您有一个包含所有当前问题的列表，这些问题分组为：

* **移动设备问题** — 影响页面移动设备版本的问题列表。
* **桌面问题** — 影响页面的桌面版本的问题列表。

每个问题都显示在表中，**Page**&#x200B;列标识受影响的页面条目。

此外，这些问题还按核心Web虚拟报表的标准性能量度分组：最大内容绘制&#x200B;**LCP**、与下一个绘制&#x200B;**INP**&#x200B;的交互和累积布局偏移&#x200B;**CLS**。

## 自动建议

![自动建议核心Web虚拟机会](./assets/core-web-vitals/auto-suggest.png){align="center"}

核心Web虚拟机会提供人工智能生成的修复建议。 单击“建议”按钮时，将显示一个新窗口，其中包含作为类别的性能指标&#x200B;**LCP**、**INP**&#x200B;和&#x200B;**CLS**。 您可以在这些类别之间切换以查看特定问题的列表。

每个类别都可能包含几个问题，因此请务必向下滚动以查看问题和建议的完整列表。  此外，每个量度都有两个适用于移动设备和台式机的性能标尺。

## 自动优化[!BADGE Ultimate]{type=Positive tooltip="Ultimate"}


![自动优化核心Web虚拟机会](./assets/core-web-vitals/auto-optimize.png){align="center"}

Sites Optimizer Ultimate新增了针对核心web虚拟机会发现的问题部署自动优化的功能。<!--- TBD-need more in-depth and opportunity specific information here. What does the auto-optimization do?-->

>[!BEGINTABS]

>[!TAB 部署优化]

{{auto-optimize-deploy-optimization-slack}}

>[!TAB 请求审批]

{{auto-optimize-request-approval}}

>[!ENDTABS]

