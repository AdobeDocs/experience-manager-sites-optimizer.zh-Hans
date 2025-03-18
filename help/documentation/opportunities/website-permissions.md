---
title: 网站权限机会文档
description: 了解网站权限机会以及如何使用它提高网站的安全性。
badgeSecurityPosture: label="安全态势" type="Caution" url="../../opportunity-types/security-posture.md" tooltip="安全态势"
source-git-commit: 5d1ae616ddde74f69b73ba5b44297c14b2dea36b
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 2%

---


# 网站权限机会

![网站权限机会](./assets/website-permissions/hero.png){align="center"}

网站权限机会可优化网站权限，这对于维护安全可管理的AEM环境至关重要。 此机会允许您通过删除过于宽泛的权限（例如`/`或`/content`等通用路径上的`jcr:all`）并根据最小权限原则调整用户访问来优化访问控制。 通过简化权限和消除冗余，您可以降低安全风险、改进可维护性并防止未来出现配置错误。 在AEM安全权限控制台或代码存储库中查看和更新权限，从而采取相应措施，确保服务用户仅拥有他们真正需要的访问权限。

## 自动识别

![自动识别网站权限](./assets/website-permissions/auto-identify.png){align="center"}

**网站权限机会**&#x200B;功能会自动标识和列出

* **用户** — 具有可疑权限的用户帐户。
* **路径** - AEM中受权限影响的路径。
* **权限** — 可疑权限。
* **问题** — 指示影响权限的问题类型。

## 自动建议

![自动建议网站漏洞](./assets/website-permissions/auto-suggest.png){align="center"}

自动建议在&#x200B;**建议的权限**&#x200B;字段中提供AI生成的推荐，允许您使用安全的替代项替换任何标记的权限。

## 自动优化[!BADGE Ultimate]{type=Positive tooltip="Ultimate"}

![自动优化网站权限](./assets/website-permissions/auto-optimize.png){align="center"}

Sites Optimizer Ultimate添加了为找到的漏洞部署自动优化的功能。

>[!BEGINTABS]

>[!TAB 部署优化]

{{auto-optimize-deploy-optimization-slack}}

>[!TAB 请求审批]

{{auto-optimize-request-approval}}

>[!ENDTABS]
