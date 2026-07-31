---
title: 预检设置
description: 了解如何为AEM Sites Optimizer设置Preflight。
TQID: https://experienceleague.adobe.com/GfLmEEBoSP2481ZZUjRyyfMjExGgI0l9yMAqTF8ObcY
product_v2:
  - id: fd1f54a9-f50c-467d-8956-cebbaf4f3eb8
source-git-commit: 14f10c231373992c49a8bb93c043556305b6280d
workflow-type: tm+mt
source-wordcount: 785
ht-degree: 52%

---

# 预检设置

运行Preflight需要在创作环境中进行设置。 您可以为通用编辑器、基于文档的创作、AEM Sites页面编辑器或Adobe Managed Services设置Preflight，以便在发布页面之前对页面运行Preflight审核。

## 启用用户访问权限

要使用Preflight，请确保您的用户至少已分配到[Adobe Admin Console](https://adminconsole.adobe.com)中的以下AEM Sites Optimizer产品配置文件之一：

* AEM Sites Optimizer - 自动建议用户
* AEM Sites Optimizer - 自动优化用户

## 启用Preflight

>[!BEGINTABS]

>[!TAB 通用编辑器]

若要在通用编辑器中设置预检功能，请执行以下步骤：

1. 打开 **Extension Manager**：
   [https://experience.adobe.com/#/@org/aem/extension-manager/universal-editor](https://experience.adobe.com/#/@org/aem/extension-manager/universal-editor)
1. 找到 **AEM Sites Optimizer Preflight** 扩展。
1. 该组织的系统管理员需要启用此扩展。
1. 扩展启用后，在 **通用编辑器** 中打开某个页面，例如：
   `https://author-p12345-e123456.adobeaemcloud.com/ui#/@org/aem/universal-editor/canvas/author-p12345-e123456.adobeaemcloud.com/content/en/example/home.html`
1. **Preflight 扩展**&#x200B;会显示在&#x200B;**侧边栏**&#x200B;中。
1. 从侧边栏中选择&#x200B;**Preflight扩展**&#x200B;以打开当前页面的Preflight。

>[!TAB 基于文档的创作]

要为基于文档的创作设置预检功能，请执行以下步骤：

1. 在 Edge Delivery Services 项目的 GitHub 存储库中，将以下配置添加到 `/tools/sidekick/config.json` 文件：

   ```json
   {
     "plugins": [
       {
         "id": "preflight",
         "titleI18n": {
           "en": "Preflight"
         },
         "environments": ["preview"],
         "event": "preflight"
       }
     ]
   }
   ```

1. 新建文件 `/tools/sidekick/aem-sites-optimizer-preflight.js`，并添加以下内容：

   ```javascript
   (function () {
     let isAEMSitesOptimizerPreflightAppLoaded = false;
     function loadAEMSitesOptimizerPreflightApp() {
       const script = document.createElement('script');
       script.src = 'https://experience.adobe.com/solutions/OneAdobe-aem-sites-optimizer-preflight-mfe/static-assets/resources/sidekick/client.js?source=plugin';
       script.onload = function () {
         isAEMSitesOptimizerPreflightAppLoaded = true;
       };
       script.onerror = function () {
         console.error('Error loading AEMSitesOptimizerPreflightApp.');
       };
       document.head.appendChild(script);
     }
   
     function handlePluginButtonClick() {
       if (!isAEMSitesOptimizerPreflightAppLoaded) {
         loadAEMSitesOptimizerPreflightApp();
       }
     }
   
     // Sidekick V1 extension support
     const sidekick = document.querySelector('helix-sidekick');
     if (sidekick) {
       sidekick.addEventListener('custom:preflight', handlePluginButtonClick);
     } else {
       document.addEventListener('sidekick-ready', () => {
         document.querySelector('helix-sidekick')
           .addEventListener('custom:preflight', handlePluginButtonClick);
       }, { once: true });
     }
   
     // Sidekick V2 extension support
     const sidekickV2 = document.querySelector('aem-sidekick');
     if (sidekickV2) {
       sidekickV2.addEventListener('custom:preflight', handlePluginButtonClick);
     } else {
       document.addEventListener('sidekick-ready', () => {
         document.querySelector('aem-sidekick')
           .addEventListener('custom:preflight', handlePluginButtonClick);
       }, { once: true });
     }
   }());
   ```

1. 在 `/scripts/scripts.js` 文件中更新 `loadLazy()` 函数，以导入适用于预览 URL 的预检功能脚本：

   ```javascript
   if (window.location.href.includes('.aem.page')) {
      import('../tools/sidekick/aem-sites-optimizer-preflight.js');
   }
   ```

1. 打开需要审核的页面的预览 URL（`*.aem.page`）。
1. 在&#x200B;**Sidekick**&#x200B;中，单击&#x200B;**Preflight**&#x200B;按钮以打开当前页的Preflight。

>[!TAB AEM Sites 页面编辑器]

如果您的创作环境运行[AEM 2026.7.0 （版本27083）](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/maintenance/2026/2026-7-0#release-27083)或更高版本，则AEM Sites页面编辑器中会内置Preflight，无需使用小书签。 执行以下步骤：

1. 在 **AEM Sites 页面编辑器**&#x200B;中打开您想审核的页面。
1. 在编辑器工具栏中，选择&#x200B;**预检**&#x200B;图标（下面高亮显示的播放按钮）以打开当前页面的“预检”面板。

   ![AEM Sites页面编辑器工具栏中的“印前检查”图标](./assets/setup/toolbar-preflight-button.png){align="center"}

>[!NOTE]
>
>在工具栏中看不到&#x200B;**预检**&#x200B;图标？ 检查以下各项：
>
>* **支持的版本** — “集成”按钮需要AEM 2026.7.0 （版本27083）或更高版本。 在早期版本中，请使用下面的小书签方法。
>* **转出** — 正在分阶段为组织启用集成按钮，因此可能尚未到达您的组织，即使在受支持的版本上也是如此。 在此之前，请使用下面的小书签方法，或联系Adobe或您的管理员。
>* **页面访问** — 只有当您具有页面的编辑访问权限时，才会显示按钮。
>* **用户访问权限** — 确认已为您的用户分配&#x200B;**AEM Sites Optimizer — 自动建议用户**&#x200B;或&#x200B;**AEM Sites Optimizer — 自动优化用户**&#x200B;配置文件。 请参阅[启用用户访问权限](#enable-user-access)。

要在AEM Sites早期版本的AEM页面编辑器中使用Preflight，您可以在Web浏览器中创建小书签。 执行以下步骤：

1. 在 Web 浏览器中显示您的&#x200B;**书签栏**：

   * 在 Windows 上按 **Ctrl+Shift+B**，或在 Mac 上按 **Cmd+Shift+B**。

1. 在浏览器中创建一个新书签：

   * 右键单击书签栏，选择&#x200B;**新建页面**&#x200B;或&#x200B;**添加书签**。
   * 在&#x200B;**地址（URL）**&#x200B;字段中粘贴以下代码：

   ```javascript
   javascript:(function(){const script=document.createElement('script');script.src='https://experience.adobe.com/solutions/OneAdobe-aem-sites-optimizer-preflight-mfe/static-assets/resources/sidekick/client.js?source=bookmarklet&target-source=aem-cloud-service';document.head.appendChild(script);})();
   ```

1. 将该书签命名为&#x200B;**预检**（或其他任意名称）。
1. 在 **AEM Sites 页面编辑器**&#x200B;中打开需要审核的页面的预览 URL（`*.aem.page`）。
1. 单击书签栏中的&#x200B;**Preflight**&#x200B;书签以打开当前页面的预检。

>[!TAB Adobe Managed Services]

>[!IMPORTANT]
>
>仅支持使用 Adobe 的身份标识提供程序 (IMS) 对 AEM 作者进行身份验证的 Adobe Managed Services (AMS) 环境。 如果您的组织使用任何其他身份标识提供程序进行 AMS 身份验证，Preflight 就无法工作。

要在AMS环境的AEM Sites页面编辑器中使用Preflight，请在Web浏览器中创建小书签，请执行以下步骤：

1. 在 Web 浏览器中显示&#x200B;**书签栏**：

   * 在 Windows 上按 **Ctrl+Shift+B**，或在 Mac 上按 **Cmd+Shift+B**。

1. 在浏览器中创建一个新书签：

   * 右键单击书签栏，选择&#x200B;**新建页面**&#x200B;或&#x200B;**添加书签**。
   * 在&#x200B;**地址（URL）**&#x200B;字段中粘贴以下代码：

   ```javascript
   javascript:(function(){const script=document.createElement('script');script.src='https://experience.adobe.com/solutions/OneAdobe-aem-sites-optimizer-preflight-mfe/static-assets/resources/sidekick/client.js?source=bookmarklet&target-source=ams';document.head.appendChild(script);})();
   ```

1. 将该书签命名为&#x200B;**预检**（或其他任意名称）。
1. 在 **AEM Sites 页面编辑器**&#x200B;中打开您想审核的页面。
1. 单击书签栏中的&#x200B;**Preflight**&#x200B;书签以打开当前页面的预检。

>[!ENDTABS]

## 最佳实践

在运行预检审计时，请注意以下准则：

* 请务必在发布到生产环境之前，先在&#x200B;**暂存环境或预览页面**&#x200B;上运行审计。
* 优先解决&#x200B;**高影响问题**，例如链接失效、缺少 H1 标记或不安全的链接。
* 在运行审核之前，确保&#x200B;**已启用身份验证**，确保受保护的暂存环境。
* 查看并应用&#x200B;**元标记建议**，以提升 SEO 性能。
