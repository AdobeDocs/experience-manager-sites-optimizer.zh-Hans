---
title: 网站漏洞机会文档
description: 了解网站漏洞机会，以及如何使用它来提高您网站的安全性。
badgeSecurityPosture: label="安全态势" type="Caution" url="../../opportunity-types/security-posture.md" tooltip="安全态势"
TQID: https://experienceleague.adobe.com/vCLnRixzZCCqUVVHR0uBUYExdaPOZI60wsfhdRkF1Nc
product_v2:
  - id: fd1f54a9-f50c-467d-8956-cebbaf4f3eb8
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 252f5292d6dc62711b4ebeb8ce5a2707857fd674
workflow-type: tm+mt
source-wordcount: 384
ht-degree: 100%

---

# 网站漏洞机会

![网站漏洞机会](./assets/website-vulnerabilities/hero.png){align="center"}

网站漏洞机会可以识别您的应用程序代码使用的第三方库中的安全漏洞。 恶意攻击者会利用此类漏洞，从而提高风险并削弱您网站的整体安全防护能力。

网站漏洞机会在页面顶部显示摘要，其中包括以下内容：

* **发现的问题**——发现的漏洞数量，按其代表的安全风险分类（低、中、高）。
* **聚合的安全风险**——根据机会发现的漏洞，您网站面临的整体安全风险。

## 自动识别

![自动识别网站漏洞](./assets/website-vulnerabilities/auto-identify.png){align="center"}

**网站漏洞机会**&#x200B;功能会自动识别并列出在您的应用程序代码使用的第三方库中发现的漏洞。 它提供以下详细信息：

* **库**——包含漏洞的第三方库。 单个库可能存在多个漏洞。
* **当前版本**——当前使用的库版本。
* **推荐版本**——解决漏洞的建议版本。
* **分数**——漏洞的严重性评级，也包含在页面顶部的摘要中。
* **漏洞**——提供更多详细信息的漏洞标识符、简短描述以及国家漏洞数据库（NVD）的链接。 单击标识符或描述旁边的链接可访问 NVD 链接。

## 自动建议

![自动建议网站漏洞](./assets/website-vulnerabilities/auto-suggest.png){align="center"}

自动建议提供了 AI 生成的建议，帮助您升级到易受攻击库的&#x200B;**推荐版本**。 每个条目都有一个&#x200B;**分数**，表示其总体严重性，有助于确定最关键漏洞的优先级。

>[!BEGINTABS]

>[!TAB 漏洞详细信息]

每个漏洞都包含一个连接到[国家漏洞数据库（NVD）](https://nvd.nist.gov/)中详细信息的链接。 单击漏洞标识符或描述右侧的链接项，即可进入该漏洞的 NVD 页面。

>[!TAB 忽略条目]

您可以选择忽略漏洞列表中的条目。 选择![删除图标](https://spectrum.adobe.com/static/icons/ui_18/CrossSize500.svg)，从列表中移除该条目。 从机会页面顶部的&#x200B;**已忽略**&#x200B;选项卡中可以重新启动已忽略的条目。<!---right now it does not seem to be implemented, but the page description mentions this functionality-->

>[!ENDTABS]


## 自动优化

[!BADGE Ultimate]{type=Positive tooltip="Ultimate"}

![自动优化网站漏洞](./assets/website-vulnerabilities/auto-optimize.png){align="center"}

Sites Optimizer Ultimate 添加了针对发现的漏洞部署自动优化的功能。

>[!BEGINTABS]

>[!TAB 部署优化]

{{auto-optimize-deploy-optimization-slack}}

>[!TAB 请求审批]

{{auto-optimize-request-approval}}

>[!ENDTABS]