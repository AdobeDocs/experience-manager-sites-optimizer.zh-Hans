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
source-git-commit: eda941a0096e32f61b45d69d89a3ee5b1a0c7e4b
workflow-type: tm+mt
source-wordcount: '82'
ht-degree: 53%

---


# 内部使用的元数据

GitHub创作系统中的元数据是分层的，并定义了以下相对于前一项的递增级别。

1. metadata.md
1. ToC
1. 文章

在 metadata.md 文件中的定义的元数据应用到整个存储库，但可以在 ToC 和文章级别覆盖。任何覆盖元数据的操作应在尽可能最低的级别进行。

experience-manager-cloud-service.en存储库中的元数据是最低要求。

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

