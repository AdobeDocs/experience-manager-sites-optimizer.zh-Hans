---
title: Sitemap问题机会文档
description: 了解Sitemap问题机会以及如何使用它改善流量获取。
badgeTrafficAcquisition: label="流量获取" type="Caution" url="../../opportunity-types/traffic-acquisition.md" tooltip="流量获取"
source-git-commit: d812a49e4b49d329717586b9f3c7a235aff3e69a
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 1%

---


# Sitemap问题机会

![Sitemap问题机会](./assets/sitemap-issues/hero.png){align="center"}

完整、准确的站点地图可帮助搜索引擎高效地爬网和索引网站页面，从而确保在搜索结果中更好地可见。 Sitemap机会可识别Sitemap的潜在问题。 修复这些问题可以显着改善搜索引擎索引和您网站上的内容可发现性。

页面顶部会显示摘要，包括问题及其对网站和业务影响的摘要。

* **预计的流量丢失** — 由于站点地图问题而预计的流量丢失。
* **预计流量值** — 丢失流量的预计值。

## 自动识别

可以使用以下条件过滤Sitemap问题：

* **有问题的站点地图** — 分析的站点地图URL包含潜在问题。
* **问题类型** — 站点地图中识别的问题类型：
   * **客户端错误** — 不返回`200 Success`响应的条目。
   * **重定向** — 重定向错误或配置错误。

>[!BEGINTABS]

>[!TAB 客户端错误]

![自动识别Sitemap客户端错误](./assets/sitemap-issues/auto-identify-client-errors.png){align="center"}

如果站点地图中的URL返回这些内容，则搜索引擎可能会假定您的站点地图已过期或页面被错误地删除。 客户端指示来自客户端（浏览器或爬网程序）的请求无效。 常见问题包括：

* **404未找到** — 请求的页面不存在。
* **403禁止访问** — 服务器拒绝访问请求的页面。
* **410不存在** — 该页面已被刻意删除，将不会返回。
* **401未授权** — 需要身份验证，但未提供。

这些错误可能会损害SEO，尤其是当重要页面返回&#x200B;**404或410**&#x200B;时，因为搜索引擎可能会对这些页面取消索引。

每个问题都显示在表中，**Page**&#x200B;列标识受影响的站点地图条目：

* **页面** — 存在问题的站点地图条目的URL。

>[!TAB 重定向]

![自动识别Sitemap客户端错误](./assets/sitemap-issues/auto-identify-redirects.png){align="center"}

Sitemap应仅包含最终目标URL，而不包含重定向的URL。 重定向旨在将用户和爬网程序引导到正确的位置，但如果配置错误，则可能会导致出现问题：

* **302已找到（临时重定向）** — 如果错误地使用而非&#x200B;**301**，则可能会导致SEO问题。
* **307临时重定向** — 类似于302，但保留HTTP方法。
* **重定向循环** — 页面重定向回自身或创建无限循环时。
* **中断的重定向** — 重定向指向不存在的页面或4xx页面时。

每个问题都显示在表中，**Page**&#x200B;列标识受影响的站点地图条目：

* **页面** — 存在问题的站点地图条目的URL。

>[!ENDTABS]

## 自动建议

满足筛选条件](#auto-identify)的每个站点地图问题[都列在具有以下列的表中：

* **页面** — 存在问题的站点地图条目的URL。
* **建议** — 针对此问题推荐的修复。

建议通常包括更新的站点路径以更正站点地图条目。 在某些情况下，它们可能还会提供更详细的说明，例如指定正确的重定向目标。

## 自动优化[!BADGE Ultimate]{type=Positive tooltip="Ultimate"}


![自动优化站点地图问题](./assets/sitemap-issues/auto-optimize.png){align="center"}

Sites Optimizer Ultimate添加了部署Sitemap自动优化的功能。

>[!BEGINTABS]

>[!TAB 部署优化]

{{auto-optimize-deploy-optimization-slack}}

>[!TAB 请求审批]

{{auto-optimize-request-approval}}

>[!ENDTABS]
