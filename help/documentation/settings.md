---
title: Sites Optimizer 设置
description: 了解如何配置 Sites Optimizer 设置，并与其他工具集成。
TQID: https://experienceleague.adobe.com/eznjSHZgAmCh-ek-XE-lLtuoGJxC0yY4UVrmPjc0KYo
product_v2:
  - id: fd1f54a9-f50c-467d-8956-cebbaf4f3eb8
    internal-label: Experience Manager
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
    internal-label: Optimization
source-git-commit: 37d90154e6868ee392bf1b779b2bf3697112b7ce
workflow-type: tm+mt
source-wordcount: '1960'
ht-degree: 39%
---
# Sites Optimizer 设置

![Sites Optimizer 设置](./assets/settings/hero.png){align="center"}

Sites Optimizer 设置是配置您的 Sites Optimizer 体验的中心枢纽。

## Google Search Console

![为 Google Search Console 设置 Sites Optimizer](./assets/settings/google-search-console.png){align="center"}

AEM Sites Optimizer 中的 Google Search Console 设置连接器可以分析关键 SEO 量度，例如搜索排名、点进率和 Core Web Vitals。 通过保持与 Google Search Console 的连接，您可以利用 JSON 分析来发现优化机会，并提高网站性能。

要设置此连接器，您必须拥有对该域的 Google Search Console 具有管理访问权限的凭据。

## 连接到 AEM Sites

以下指南介绍了如何将您现有的 Edge Delivery Services (EDS) 网站连接到 AEM Sites Optimizer。 在开始之前，请确保您的 EDS 网站已设置并运行正常。此连接专门用于 AEM Sites Optimizer 访问您的内容。

连接需要两个步骤：

1. 提供您的代码存储库 URL 和内容源 URL。
2. 授予 AEM Sites Optimizer 访问您的内容源的权限。

### 步骤 1：链接您的代码存储库和内容源

在 AEM Sites Optimizer 中前往&#x200B;**设置 → 连接到 AEM Sites**，然后输入以下内容：

- **代码存储库 URL**——EDS 网站的 GitHub URL，例如：
  `https://github.com/owner/repo`

- **内容源 URL**——支持您的 EDS 网站的 SharePoint 文件夹或 Google Drive 文件夹的 URL，例如：
  `https://drive.google.com/drive/folders/...` 或 `https://myorg.sharepoint.com/...`

输入内容源 URL 后，AEM Sites Optimizer 将检测您的内容源类型，并在下面显示相关的访问说明。

### 步骤 2：授予对内容源的访问权限

按照与您的内容源匹配的部分操作。

#### SharePoint — Adobe 域

![连接到 AEM Sites 对话框，显示无需为 Adobe SharePoint 域进行任何操作](./assets/settings/connect-content-and-drive.png){align="center"}

如果您的内容源 URL 使用 Adobe SharePoint 域，就无需进一步操作。 访问权限已配置完毕。 点击&#x200B;**保存**，完成连接。

#### SharePoint — 自定义域

如果您的内容源 URL 使用您组织自己的 SharePoint 域，您就需要注册一个 Azure 应用程序，将其凭据提供给 AEM Sites Optimizer。

##### 您需要什么

- 在 Azure 门户中注册应用程序的权限，或者可以代表您注册应用程序的联系人。
- 授予 API 同意的租户管理员权限，或者可以为您批准 API 同意的管理员。

##### 步骤 2a：在 Azure 中注册应用程序

1. 前往 **Azure Portal → Microsoft Entra ID → 应用程序注册 → 新注册**。
2. 为其命名，例如：`AEM Sites Optimizer`。
3. 保留所有其他默认值，然后点击&#x200B;**注册**。
4. 在&#x200B;**概述**&#x200B;页面上，记下：
   - **应用程序（客户端）ID**
   - **目录（租户）ID**

##### 步骤 2b：添加 API 权限

1. 前往 **API 权限 → 添加权限 → Microsoft Graph → 应用程序权限**。
2. 添加以下两项：
   - `Sites.Selected` — 对特定 SharePoint 网站收藏集的受限访问权限。
   - `Files.SelectedOperations.Selected` — 在没有登录用户的情况下的文件访问权限。
3. 为这两项点击&#x200B;**授予管理员同意**。

![Azure API 权限显示已授予 Sites.Selected 和 Files.SelectedOperations.Selected](./assets/settings/app-permissions.png){align="center"}

>[!NOTE]
>
>要授予管理员同意，需要租户管理员权限。 如果您没有此权限，请让您的 IT 或 Azure 管理员完成这个步骤，然后继续。

##### 步骤 2c：创建客户端密码

![用于应用程序注册的 Azure 证书和密码页面](./assets/settings/create-credentials.png){align="center"}

1. 前往&#x200B;**证书和密码 → 新客户端密码**。
2. 设置一个描述和到期日，然后点击&#x200B;**添加**。
3. 立即复制密钥值，它只显示一次。

##### 步骤 2d：授予应用程序访问您的 SharePoint 网站的权限

您可以使用 Microsoft Graph Explorer、PowerShell 或直接调用 Graph API 来授予应用程序访问权限。

导航到 [Microsoft Graph Explorer](https://developer.microsoft.com/graph/graph-explorer)，使用您的 Microsoft 帐户登录，然后运行以下请求：

1. 查找您的网站 ID：

```
GET https://graph.microsoft.com/v1.0/sites/{tenant}.sharepoint.com:/sites/{site-name}
```

1. 从回答中复制 `id`，然后授予网站级访问权限：

```
POST https://graph.microsoft.com/v1.0/sites/{siteId}/permissions
```

主体：

```json
{
  "roles": ["write"],
  "grantedToIdentities": [{
    "application": {
      "id": "{your-client-id}",
      "displayName": "{Your app name}"
    }
  }]
}
```

##### 步骤 2e：在 AEM Sites Optimizer 中输入凭据

![连接到 AEM Sites 对话框，显示 SharePoint 凭据字段](./assets/settings/add-sharepoint-credentials.png){align="center"}

返回到&#x200B;**连接到 AEM Sites** 对话框，在&#x200B;**通过 SharePoint 连接内容存储库**&#x200B;中输入以下内容：

- **租户 ID (Azure AD)** — 来自应用程序注册 → 概述。
- **客户端 ID（应用程序注册）** — 来自应用程序注册 → 概述。
- **客户端密码**——在步骤 2c 中创建。

点击&#x200B;**验证连接**，确认访问权限，然后点击&#x200B;**保存**。

#### Google Drive

![连接到 AEM Sites 对话框中显示用于共享访问权限的 Google Drive 服务帐户](./assets/settings/validate-eds-google.png){align="center"}

1. 在 Google Drive 中，右键单击支持您的 EDS 网站的文件夹，然后选择&#x200B;**共享**。
2. 在&#x200B;**添加人员和组**&#x200B;字段中，输入&#x200B;**连接到 AEM Sites** 对话框中显示的服务帐户电子邮件：
   `aem-sites-optimizer@adbe-gcp0843.iam.gserviceaccount.com`
3. 将权限级别设置为&#x200B;**编辑者**。
4. 取消勾选&#x200B;**通知人员**，然后点击&#x200B;**共享**。

共享完成后，在对话框中点击&#x200B;**验证连接**，然后点击&#x200B;**保存**。

## 管理用户权限

控制谁可以在Sites Optimizer中访问网站以及他们可以使用网站执行哪些操作。 访问权限是从您授予每个人的少量独立&#x200B;*功能*（查看、编辑、部署、配置和管理用户）中构建的。

访问是&#x200B;**累加**：人员的权限是已授予他们的所有权限的总和。 由于没有“拒绝”功能，因此授权之间不会发生冲突或相互取消。 要授予某人更少的访问权限，请删除授权而不是尝试覆盖它。

### 如何授予访问权限

用户可以通过两种方式获得访问权限，并且这两种方式可以协同工作：

- **组织范围访问权限** — 由您的Adobe组织管理员在[Adobe Admin Console](https://adminconsole.adobe.com/)中分配。 它适用于您组织中的每个站点。 它适用于任何地方都需要相同访问权限的人。
- **站点级访问** — 在Sites Optimizer内部分配在&#x200B;**设置→权限**&#x200B;页面上。 它适用于单个站点，可以随意扩大或缩小。 无需Admin Console访问权限。

>[!NOTE]
>
>两层相加。 如果某个人员具有组织范围查看权限，并且在一个站点上被授予了编辑权限，则可以查看每个站点并编辑该站点。 要将人员限制在单个地点，请确保他们不会在整个组织内担任职务。

#### 组织范围角色(Admin Console)

组织范围访问权限来自在[AEM Sites Optimizer](https://adminconsole.adobe.com/)中分配的两个&#x200B;**Adobe Admin Console**&#x200B;产品角色之一：

- **ASO管理器** — 每个站点的完全访问权限，包括&#x200B;**管理用户**。 经理可以打开任何站点的&#x200B;**权限**&#x200B;页面，并将访问权限分配给其他人。
- **ASO用户** — 每个站点的仅查看访问权限。 无更改且无用户管理。

要分配角色，您必须是组织的&#x200B;**系统管理员**，或者AEM Sites Optimizer的&#x200B;**产品管理员**。

1. 登录到[Adobe Admin Console](https://adminconsole.adobe.com/)。
1. 转到&#x200B;**产品**&#x200B;并选择&#x200B;**AEM Sites Optimizer**。
1. 打开&#x200B;**用户**&#x200B;选项卡，并通过电子邮件添加用户（或选择现有用户）。
1. 单击&#x200B;**+** （添加）图标以添加产品配置文件，然后选择产品配置文件。

   ![在Adobe Admin Console中为用户选择产品配置文件](./assets/settings/permissions-admin-console-product-profile.png){align="center"}

1. 单击&#x200B;**下一步**。
1. 选择角色 — **ASO管理器**&#x200B;以获得完全访问权限，或选择&#x200B;**ASO用户**&#x200B;以获得仅查看访问权限 — 然后单击&#x200B;**应用**。

   ![在Adobe Admin Console中选择ASO管理器角色](./assets/settings/permissions-admin-console-aso-manager-role.png){align="center"}

   ![在Adobe Admin Console中选择ASO用户角色](./assets/settings/permissions-admin-console-aso-user-role.png){align="center"}

有关添加用户的详细信息，请参阅[载入用户](setup/onboard-users.md)。

>[!IMPORTANT]
>
>只有组织管理员才能授予组织范围的&#x200B;**管理用户**。 站点上具有&#x200B;**管理用户**&#x200B;的成员可以分配该站点的访问权限，但无法创建组织范围的&#x200B;**ASO管理器**。

### 功能级别

每种功能控制一种操作。 它们是独立的 — 例如，您可以授予部署而不进行编辑。

| 功能 | 它允许 | 不允许的内容 |
|---|---|---|
| 视图 | 在不更改任何内容的情况下，查看站点的数据 — 机会、建议、修复、报告和配置。 | 任何变更。 |
| 编辑 | 创建和更改机会和建议（应该更改的内容）。 | 发布更改、更改设置或管理用户。 |
| 部署 | 将修复实时发布到站点，然后回滚它们。 | 管理用户。 |
| 配置 | 更改站点的设置和连接。 | 发布修复或管理用户。 |
| 管理用户 | 授予或撤销其他成员对站点的访问权限。 | 管理用户尚无法访问的站点。 |

>[!NOTE]
>
>**视图始终包括在内。** 每个授权都自动包含视图 — 您无法管理、配置、编辑或部署无法看到的内容。 因此，无法自行删除视图。 要完全删除某人的访问权限，请删除该成员（请参阅下面的[编辑或删除成员](#edit-or-remove-a-member)），而不是取消选中每个功能。

### 确定访问机会类型的范围

在单个网站上，您可以授予&#x200B;**特定机会类型** （例如，Core Web Vitals或断开的内部链接）的查看、编辑和部署权限，而不是整个网站。 这样可让一个人编辑Core Web Vitals，而只查看其他所有内容。

- **视图**、**编辑**&#x200B;和&#x200B;**部署**&#x200B;的范围可以限定为一个或多个机会类型，也可以限定为&#x200B;**所有**&#x200B;机会类型。
- **配置**&#x200B;和&#x200B;**管理用户**&#x200B;始终适用于整个站点 — 不能将用户限制为机会类型。

每个作用域授予都显示为成员自己的行，其中&#x200B;**应用于**&#x200B;列显示机会类型&#x200B;**全部**&#x200B;或&#x200B;**网站范围**。

>[!CAUTION]
>
>作用域仅限制&#x200B;*授予的*&#x200B;内容 — 它从不删除其他授予提供的访问权限。 如果人员还拥有组织范围访问权限或&#x200B;**所有**&#x200B;类型的授权，则该更广泛的访问权限仍然适用。 因此，要真正将某人限制在特定机会类型中，请确保他们不会同时拥有更广泛的角色或&#x200B;**所有**&#x200B;类型的授权。

### 添加成员

1. 转到&#x200B;**设置→权限**&#x200B;并选择网站。
1. 单击&#x200B;**添加成员**。
1. 按姓名或电子邮件搜索，并选择一个或多个人员。
1. 选择访问适用的&#x200B;**机会类型**（或&#x200B;**全部**），然后选择要授予的功能。
1. 单击&#x200B;**添加**。

<!-- MEDIA PENDING: Site Manager / Site User walkthrough videos are being re-recorded with demo data to remove PII, then re-uploaded to video.tv.adobe.com and embedded here with >[!VIDEO]. The earlier uploads v/3503767 and v/3503768 (KT-22672 / KT-22673) contain PII and must not be used. -->

### 编辑或删除成员

在&#x200B;**成员**&#x200B;表中：

- 单击成员行上的&#x200B;**编辑权能**&#x200B;以更改其功能。 编辑现有授权时，其机会类型保持不变 — 您只更改功能，并且必须至少保持选中一个功能。
- 单击&#x200B;**删除**&#x200B;可完全撤消该成员对网站的访问权限。

>[!NOTE]
>
>更改权能和删除成员是不同的操作。 要移除所有访问权限，请使用&#x200B;**移除** — 您不能通过取消选中功能来执行此操作，因为授权必须保留至少一个功能（并且视图始终保留）。

### 谁可以管理权限

网站的&#x200B;**权限**&#x200B;页面可用于：

- 在该网站上具有&#x200B;**管理用户**&#x200B;功能的成员，并且
- 组织管理员（ASO经理）。

没有&#x200B;**管理用户**&#x200B;的成员会看到一条消息，指出他们无权管理该网站的访问权限。

### 打开用户和访问管理

用户和访问管理由贵组织的设置控制。 您可以在打开之前分配访问权限，但设置打开后只有&#x200B;**强制执行**。

如果尚未启用，**权限**&#x200B;页面会显示横幅，要求您联系帐户团队。 请联系您的Sites Optimizer客户团队以将其启用。

>[!NOTE]
>
>在启用用户和访问管理之前，您分配的权限会保存，但不会强制执行。

### 常见问题解答

**网站级别成员是否需要Admin Console角色？**

不会。 在Sites Optimizer中的&#x200B;**权限**&#x200B;页面上完全授予网站级别的访问权限。 在Admin Console中仅分配组织范围内的角色。

**如果有人同时具有组织范围访问和网站级别访问权，会发生什么情况？**

两者均适用。 他们的有效访问是两者的结合。 授予不会发生冲突，因为任何授予都不能拒绝访问。

**为什么具有管理用户的成员无法创建组织范围的管理员？**

创建组织范围角色是Admin Console的一项操作。 具有&#x200B;**管理用户**&#x200B;的成员可以在自己的站点上分配访问权限，但只有组织管理员可以授予组织范围角色。

**如何撤销某个人对网站的访问权限？**

删除&#x200B;**权限**&#x200B;页面上的授权。 这与编辑功能不同，编辑功能必须始终保留至少一个功能。

**我能否将人员限制为特定机会类型？**

是 — 将View、Edit或Deploy作用域授予给特定机会类型，而不是&#x200B;**All**。 由于访问是附加的，因此仅当人员不具有组织范围访问权限或&#x200B;**所有**&#x200B;类型的授权时，此项才会生效。
