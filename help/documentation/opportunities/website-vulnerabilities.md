---
title: 网站漏洞机会文档
description: 了解网站漏洞机会以及如何使用它提高网站的安全性。
badgeSecurityPosture: label="安全态势" type="Caution" url="../../opportunity-types/security-posture.md" tooltip="安全态势"
source-git-commit: ab2d75b1d986d83e3303e29a25d2babd1598394a
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 1%

---


# 网站漏洞机会

![网站漏洞机会](./assets/website-vulnerabilities/hero.png){align="center"}

网站漏洞机会可识别您的应用程序代码使用的第三方库中的安全漏洞。 恶意攻击者可能会利用这些漏洞，从而增加风险并降低网站的安全状态。

网站漏洞机会在页面顶部显示摘要，包括以下内容：

* **发现的问题** — 找到的漏洞数，按它们代表的安全风险进行分类（低、中、高）。
* **聚合安全风险** — 根据机会发现的漏洞，网站面临的整体安全风险。

## 自动识别

![自动识别网站漏洞](./assets/website-vulnerabilities/auto-identify.png){align="center"}

**网站漏洞机会**&#x200B;功能会自动识别和列出在您的应用程序代码使用的第三方库中找到的漏洞。 它提供了以下详细信息：

* **库** — 包含漏洞的第三方库。 单个库可能具有多个漏洞。
* **当前版本** — 当前正在使用的库的版本。
* **建议的版本** — 解决该漏洞的建议版本。
* **得分** — 此漏洞的严重性等级，也在页面顶部进行了总结。
* **漏洞** — 漏洞标识符、简短描述以及指向国家漏洞数据库(NVD)的链接以了解更多详细信息。 通过单击描述旁边的标识符或链接访问NVD链接。

## 自动建议

![自动建议网站漏洞](./assets/website-vulnerabilities/auto-suggest.png){align="center"}

自动建议为您应升级到的易受攻击库的&#x200B;**推荐版本**&#x200B;提供人工智能生成的建议。 每个条目都有一个&#x200B;**分数**，用于指示其整体严重性，从而帮助排定最关键漏洞的优先级。

>[!BEGINTABS]

>[!TAB 漏洞详细信息]

每个漏洞都包含指向[国家漏洞数据库(NVD)](https://nvd.nist.gov/)中详细信息的链接。 单击该漏洞标识符或说明右侧的链接项，将会转到该漏洞的NVD页面。

>[!TAB 忽略条目]

您可以选择忽略漏洞列表中的条目。 选择&#x200B;**忽略图标**&#x200B;会从列表中删除该条目。 可以从机会页面顶部的&#x200B;**Ignored**&#x200B;选项卡重新参与已忽略的条目。<!---right now it does not seem to be implemented, but the page description mentions this functionality-->

>[!ENDTABS]


## 自动优化[!BADGE Ultimate]{type=Positive tooltip="Ultimate"}

![自动优化网站漏洞](./assets/website-vulnerabilities/auto-optimize.png){align="center"}

Sites Optimizer Ultimate添加了为找到的漏洞部署自动优化的功能。

>[!BEGINTABS]

>[!TAB 部署优化]

{{auto-optimize-deploy-optimization-slack}}

>[!TAB 请求审批]

{{auto-optimize-request-approval}}

>[!ENDTABS]