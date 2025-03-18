---
title: CORS配置机会文档
description: 了解CORS配置机会以及识别和修复站点安全漏洞。
badgeSecurityPosture: label="安全态势" type="Caution" url="../../opportunity-types/security-posture.md" tooltip="安全态势"
source-git-commit: ab2d75b1d986d83e3303e29a25d2babd1598394a
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 3%

---


# CORS配置机会

![CORS配置机会](./assets/cors-configuration/hero.png){align="center"}

正确配置跨源资源共享(CORS)对于保护Web应用程序免受未经授权的数据访问至关重要。 当`Access-Control-Allow-Origin`标头设置为`*`时，任何域都可以请求和接收响应，从而可能向攻击者公开敏感信息。 这提供了通过实施受信任域的受控允许列表或在不需要时禁用CORS来加强安全性的机会。 确保安全的CORS设置有助于保护私有内容，同时保持授权用户的无缝访问。

## 自动识别

![自动识别CORS配置机会](./assets/cors-configuration/auto-identify.png){align="center"}

自动识别可扫描您的网站以查找CORS错误配置，并检测容易遭受未经授权访问的URL。 顶表中列出了这些URL以及以下详细信息：

* **Page prefix** — 容易出现CORS错误配置的URL路径前缀。
* **页面示例** — 示例URL容易遭受未经授权的访问。

## 自动建议

![自动建议CORS配置机会](./assets/cors-configuration/auto-suggest.png){align="center"}

自动建议提供&#x200B;**要审阅的应用程序代码文件**&#x200B;及其&#x200B;**行**，这些文件可能正在设置松散的CORS策略。


## 自动优化[!BADGE Ultimate]{type=Positive tooltip="Ultimate"}



>[!BEGINTABS]

>[!TAB 部署优化]

{{auto-optimize-deploy-optimization-slack}}

>[!TAB 请求审批]

{{auto-optimize-request-approval}}

>[!ENDTABS]
