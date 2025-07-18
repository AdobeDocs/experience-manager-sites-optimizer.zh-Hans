---
title: 跨站点脚本机会文档
description: 了解跨站点脚本机会，并识别和修复网站安全漏洞。
badgeSecurityPosture: label="安全态势" type="Caution" url="../../opportunity-types/security-posture.md" tooltip="安全态势"
source-git-commit: cb64a34b758de8f5dcea298014ddd0ba79a24c17
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 78%

---


# 跨站点脚本机会

![跨站点机会](./assets/cross-site-scripting/hero.png){align="center"}

跨站点脚本机会可识别网站代码中的漏洞。 然后，它修复了攻击者可能利用漏洞将恶意脚本插入其他用户查看的网页中的问题。 这些脚本可能窃取敏感信息，例如会话 Cookie；或者代表用户执行操作，例如更改用户的密码。

## 自动识别

![自动识别跨站点机会](./assets/cross-site-scripting/auto-identify.png){align="center"}

* **易受攻击的代码**——任何容易受到跨站点脚本攻击的代码。
* **重现链接**——发现漏洞的页面的链接。

## 自动建议

![自动建议跨站点机会](./assets/cross-site-scripting/auto-suggest.png){align="center"}

* **建议的修复**——AI 生成的有关如何修复漏洞的建议。

## 自动优化

[!BADGE Ultimate]{type=Positive tooltip="Ultimate"}

>[!BEGINTABS]

>[!TAB 部署优化]

{{auto-optimize-deploy-optimization-slack}}

>[!TAB 请求审批]

{{auto-optimize-request-approval}}

>[!ENDTABS]
