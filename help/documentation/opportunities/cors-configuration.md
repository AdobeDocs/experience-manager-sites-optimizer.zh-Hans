---
title: CORS 配置机会文档
description: 了解 CORS 配置机会，并识别和修复网站安全漏洞。
badgeSecurityPosture: label="安全态势" type="Caution" url="../../opportunity-types/security-posture.md" tooltip="安全态势"
source-git-commit: cb64a34b758de8f5dcea298014ddd0ba79a24c17
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 87%

---


# CORS 配置机会

![CORS 配置机会](./assets/cors-configuration/hero.png){align="center"}

正确配置跨源资源共享（CORS）对于保护 Web 应用程序免受未经授权的数据访问至关重要。如果 `Access-Control-Allow-Origin` 标头设置为 `*`，那么任何域都可以请求和接收响应，从而可能将敏感信息暴露给攻击者。此功能提供了一个机会，可以通过实施受信任域的受控允许列表或在不需要时禁用CORS来加强安全性。 确保 CORS 设置的安全性有助于保护私人内容，同时保持授权用户的无缝访问。

## 自动识别

![自动识别 CORS 配置机会](./assets/cors-configuration/auto-identify.png){align="center"}

自动识别功能可扫描您的网站，查找 CORS 错误配置，并检测容易受到未经授权访问影响的 URL。这些 URL 连同以下详细信息均列在顶部表中：

* **页面前缀**——易受到 CORS 错误配置影响的 URL 路径前缀。
* **页面示例**——易受未经授权访问影响的 URL 示例。

## 自动建议

![自动建议 CORS 配置机会](./assets/cors-configuration/auto-suggest.png){align="center"}

自动建议提供可能设置了不严谨 CORS 策略的待审查&#x200B;**应用程序代码文件**&#x200B;及其&#x200B;**行**。


## 自动优化

[!BADGE Ultimate]{type=Positive tooltip="Ultimate"}

>[!BEGINTABS]

>[!TAB 部署优化]

{{auto-optimize-deploy-optimization-slack}}

>[!TAB 请求审批]

{{auto-optimize-request-approval}}

>[!ENDTABS]
