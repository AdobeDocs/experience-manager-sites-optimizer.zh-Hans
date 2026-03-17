---
solution: Experience Manager
product: adobe experience manager
landing-page-name: experience-manager
landing-page-breadcrumb-title: AEM
type: Documentation
description: AEM Sites Optimizer文档。
index: true
git-repo: https://github.com/AdobeDocs/experience-manager-sites-optimizer.zh-Hans
feature-set: Experience Manager Assets,Experience Manager Sites,Experience Manager, Experience Manager Forms, Experience Manager Cloud Manager
cloud: Experience Cloud
recommendations: noDisplay
source-git-commit: 4cf02d5c9d44ed00bb3b284330b2d553d54ba8d3
workflow-type: tm+mt
source-wordcount: '85'
ht-degree: 2%

---


# 元数据供内部使用

GitHub创作系统使用以下递增的引用级别，按层级组织元数据。

1. metadata.md
1. ToC
1. 文章

在metadata.md文件中定义的元数据应用到整个存储库，但可以在ToC和文章级别覆盖。 任何覆盖元数据的操作应在尽可能最低的级别进行。

`experience-manager-cloud-service.en`存储库中的元数据是最低要求。

metadata.md

* `product`
* `git-repo`
* `index`
* `solution-title`
* `solution-hub-url`
* `getting-started-title`
* `getting-started-url`
* `tutorials-title`
* `tutorials-url`

ToCs

* `sub-product`
* `user-guide-title`

文章

* `title`
* `description`
* `contentOwner` （仅在`/help/assets`下的核心资源内容上）

