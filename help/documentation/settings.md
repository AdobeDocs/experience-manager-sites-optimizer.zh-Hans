---
title: Sites Optimizer 设置
description: 了解如何配置 Sites Optimizer 设置，并与其他工具集成。
source-git-commit: b71d5510162864ee76931cf754164ea637cadd92
workflow-type: tm+mt
source-wordcount: '749'
ht-degree: 12%

---


# Sites Optimizer 设置

![Sites Optimizer 设置](./assets/settings/hero.png){align="center"}

Sites Optimizer设置是配置Sites Optimizer体验的中心枢纽。

## Google Search Console

Google Search Console的![Sites Optimizer设置](./assets/settings/google-search-console.png){align="center"}

AEM Sites Optimizer 中的 Google Search Console 设置连接器可以分析关键 SEO 量度，例如搜索排名、点进率和 Core Web Vitals。 通过保持与 Google Search Console 的连接，您可以利用 JSON 分析来发现优化机会，并提高网站性能。

要设置此连接器，您必须拥有该域 Google Search Console 管理访问权限的凭据。

## 连接到AEM Sites

以下指南介绍如何将您现有的Edge Delivery Services (EDS)站点连接到AEM Sites Optimizer。 在开始之前，请确保您的EDS站点已设置并正在运行 — 此连接专门用于AEM Sites Optimizer访问您的内容。

连接需要两个步骤：

1. 提供代码存储库URL和内容源URL。
2. 授予AEM Sites Optimizer访问您的内容源的权限。

### 步骤1 — 链接代码存储库和内容源

在AEM Sites Optimizer中，转到&#x200B;**设置→连接到AEM Sites**&#x200B;并输入以下内容：

- **代码存储库URL** — EDS网站的GitHub URL，例如：
  `https://github.com/owner/repo`

- **内容Source URL** — 支持EDS站点的SharePoint文件夹或Google驱动器文件夹的URL，例如：
  `https://drive.google.com/drive/folders/...` 或 `https://myorg.sharepoint.com/...`

输入Content Source URL后，AEM Sites Optimizer将检测您的内容源类型并显示以下相关访问说明。

### 步骤2 — 授予对内容源的访问权限

遵循与您的内容源匹配的部分。

#### SharePoint — Adobe域

![连接到AEM Sites对话框显示Adobe SharePoint域无需任何操作](./assets/settings/connect-content-and-drive.png){align="center"}

如果您的内容Source URL使用Adobe SharePoint域，则无需进一步操作。 已配置访问。 单击&#x200B;**保存**&#x200B;以完成连接。

#### SharePoint — 自定义域

如果您的Content Source URL使用您组织自己的SharePoint域，则需要注册Azure应用程序并将其凭据提供给AEM Sites Optimizer。

##### 您将需要什么

- 在Azure Portal中注册应用程序的权限，或可以代表您注册应用程序的联系人。
- 授予API同意的租户管理员权限，或可以为您批准API同意的管理员。

##### 步骤2a — 在Azure中注册应用程序

1. 转到&#x200B;**Azure Portal → Microsoft Entra ID →应用程序注册→新注册**。
2. 为其命名，例如： `AEM Sites Optimizer`。
3. 保留所有其他默认值，然后单击&#x200B;**注册**。
4. 在&#x200B;**概述**&#x200B;页面上，记下：
   - **应用程序（客户端）ID**
   - **目录（租户） ID**

##### 步骤2b — 添加API权限

1. 转到&#x200B;**API权限→添加Microsoft Graph →应用程序权限→权限**。
2. 添加以下两项：
   - `Sites.Selected` — 对特定SharePoint网站集的作用域访问权限。
   - `Files.SelectedOperations.Selected` — 在没有登录用户的情况下访问文件。
3. 单击&#x200B;**授予管理员同意**。

![Azure API权限显示Sites.Selected和Files.SelectedOperations.Selected已授予](./assets/settings/app-permissions.png){align="center"}

>[!NOTE]
>
>授予管理员同意需要租户管理员权限。 如果您没有此权限，请让IT或Azure管理员完成此步骤后再继续。

##### 步骤2c — 创建客户端密码

应用程序注册的![Azure证书和密钥页](./assets/settings/create-credentials.png){align="center"}

1. 转到&#x200B;**证书和密码→新客户端密码**。
2. 设置说明和到期，然后单击&#x200B;**添加**。
3. 立即复制机密值 — 它只显示一次。

##### 步骤2d — 授予应用程序访问SharePoint网站的权限

您可以使用Microsoft Graph Explorer、PowerShell或直接图形API调用授予应用程序访问权限。

导航到[Microsoft Graph Explorer](https://developer.microsoft.com/graph/graph-explorer)，使用您的Microsoft帐户登录，然后运行以下请求：

1. 查找您的网站ID：

```
GET https://graph.microsoft.com/v1.0/sites/{tenant}.sharepoint.com:/sites/{site-name}
```

1. 从响应中复制`id`，然后授予站点级访问权限：

```
POST https://graph.microsoft.com/v1.0/sites/{siteId}/permissions
```

正文：

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

##### 步骤2e — 在AEM Sites Optimizer中输入凭据

![连接到AEM Sites对话框显示SharePoint凭据字段](./assets/settings/add-sharepoint-credentials.png){align="center"}

返回&#x200B;**连接到AEM Sites**&#x200B;对话框，在&#x200B;**通过SharePoint进行内容存储库连接**&#x200B;下输入以下内容：

- **租户ID (Azure AD)** — 来自应用程序注册→概述。
- **客户端ID（应用程序注册）** — 从应用程序注册→概述。
- **客户端密钥** — 在步骤2c中创建。

单击&#x200B;**验证连接**&#x200B;以确认访问，然后单击&#x200B;**保存**。

#### Google通道

![连接到AEM Sites对话框显示用于共享访问权限的Google驱动器服务帐户](./assets/settings/validate-eds-google.png){align="center"}

1. 在Google驱动器中，右键单击支持您的EDS网站的文件夹，然后选择&#x200B;**共享**。
2. 在&#x200B;**添加人员和组**&#x200B;字段中，输入&#x200B;**连接到AEM Sites**对话框中显示的服务帐户电子邮件：
   `experience-success-studio@helix-225321.iam.gserviceaccount.com`
3. 将权限级别设置为&#x200B;**编辑者**。
4. 取消选中&#x200B;**通知联系人**，然后单击&#x200B;**共享**。

共享完成后，在对话框中单击&#x200B;**验证连接**，然后单击&#x200B;**保存**。
