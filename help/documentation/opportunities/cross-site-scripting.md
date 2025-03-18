---
title: 跨站点脚本销售机会文档
description: 了解跨站点脚本编写机会，并识别和修复站点安全漏洞。
badgeSecurityPosture: label="安全态势" type="Caution" url="../../opportunity-types/security-posture.md" tooltip="安全态势"
source-git-commit: ab2d75b1d986d83e3303e29a25d2babd1598394a
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 4%

---


# 跨站点脚本编写机会

![跨站点机会](./assets/cross-site-scripting/hero.png){align="center"}

跨站点脚本机会识别并修复了网站代码中的漏洞，攻击者可以利用这些漏洞将恶意脚本注入其他用户查看的网页。 这些脚本可能会窃取敏感信息（如会话Cookie），或代表用户执行操作（如更改用户的密码）。

## 自动识别

![自动识别跨站点机会](./assets/cross-site-scripting/auto-identify.png){align="center"}

* **易受攻击的代码** — 任何易受跨站点脚本攻击的代码。
* **要再现的链接** — 指向发现漏洞的页面的链接。

## 自动建议

![自动建议跨站点机会](./assets/cross-site-scripting/auto-suggest.png){align="center"}

* **建议的修复** — 有关如何修复漏洞的AI生成的建议。

## 自动优化[!BADGE Ultimate]{type=Positive tooltip="Ultimate"}


>[!BEGINTABS]

>[!TAB 部署优化]

{{auto-optimize-deploy-optimization-slack}}

>[!TAB 请求审批]

{{auto-optimize-request-approval}}

>[!ENDTABS]
