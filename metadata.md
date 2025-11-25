---
solution: Experience Manager
product: adobe experience manager
type: Documentation
description: AEM Sites Optimizer文档。
index: y
git-repo: https://github.com/AdobeDocs/experience-manager-sites-optimizer.zh-Hans
feature-set: Experience Manager Assets,Experience Manager Sites,Experience Manager, Experience Manager Forms, Experience Manager Cloud Manager
cloud: Experience Cloud
recommendations: noDisplay
source-git-commit: 2f4ef1c6f44d602bfe365a52eb692fe7faa7f05f
workflow-type: tm+mt
source-wordcount: '79'
ht-degree: 29%

---


# 内部使用的元数据

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

ToC

* `sub-product`
* `user-guide-title`

文章

* `title`
* `description`
* `contentOwner` （仅在`/help/assets`下的核心资源内容上）

