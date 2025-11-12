---
title: 预检设置
description: 了解如何为 AEM Sites Optimizer 设置预检扩展。
source-git-commit: 210acc5337796707ced10f2b84d473503fc06088
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 100%

---


# 预检设置

要启用 AEM Sites Optimizer 的预检功能，需要在通用编辑器、基于文档的预览，或 AEM Cloud Service 中设置预检功能扩展，以便在页面发布前执行预检审计。

## 启用用户访问权限

要使用预检扩展，请确保您的用户在 [Adobe Admin Console](https://adminconsole.adobe.com) 中被分配到以下 AEM Sites Optimizer 产品轮廓中的至少一个：

* AEM Sites Optimizer - 自动建议用户
* AEM Sites Optimizer - 自动优化用户

## 启用预检功能扩展

>[!BEGINTABS]

>[!TAB 通用编辑器]

若要在通用编辑器中设置预检功能，请执行以下步骤：

1. 打开 **Extension Manager**：
   [https://experience.adobe.com/#/@org/aem/extension-manager/universal-editor](https://experience.adobe.com/#/@org/aem/extension-manager/universal-editor)
1. 找到 **AEM Sites Optimizer** 预检功能扩展，并提交启用请求。
1. **Adobe AEM 团队**&#x200B;将会审核并为您的组织启用该扩展。
1. 扩展启用后，在 **通用编辑器** 中打开某个页面，例如：
   `https://author-p12345-e123456.adobeaemcloud.com/ui#/@org/aem/universal-editor/canvas/author-p12345-e123456.adobeaemcloud.com/content/en/example/home.html`
1. **预检功能扩展**&#x200B;将在&#x200B;**侧边栏**&#x200B;中显示。
1. 从侧边栏中选择&#x200B;**预检功能扩展**，即可开始对当前页面执行&#x200B;**预检审计**。

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

1. 打开需要审计页面的预览 URL（`*.aem.page`）。
1. 在 **Sidekick** 中点击&#x200B;**预检**&#x200B;功能按钮，即可开始对当前页面进行审计。

>[!TAB AEM Sites 页面编辑器]

要在 AEM Sites 页面编辑器中使用预检功能，您可以在 Web 浏览器中创建一个书签小程序。执行以下步骤：

1. 在 Web 浏览器中显示&#x200B;**书签栏**：

   * 在 Windows 上按 **Ctrl+Shift+B**，或在 Mac 上按 **Cmd+Shift+B**。

1. 在浏览器中创建一个新书签：

   * 右键单击书签栏，选择&#x200B;**新建页面**&#x200B;或&#x200B;**添加书签**。
   * 在&#x200B;**地址（URL）**&#x200B;字段中粘贴以下代码：

   ```javascript
   javascript:(function(){const script=document.createElement('script');script.src='https://experience.adobe.com/solutions/OneAdobe-aem-sites-optimizer-preflight-mfe/static-assets/resources/sidekick/client.js?source=bookmarklet&target-source=aem-cloud-service';document.head.appendChild(script);})();
   ```

1. 将该书签命名为&#x200B;**预检**（或其他任意名称）。
1. 在 **AEM Sites 页面编辑器**&#x200B;中打开需要审计页面的预览 URL（`*.aem.page`）。
1. 在书签栏中点击&#x200B;**预检**&#x200B;书签，即可开始对当前页面进行审计。

>[!ENDTABS]

## 最佳实践

在运行预检审计时，请注意以下准则：

* 请务必在发布到生产环境之前，先在&#x200B;**暂存环境或预览页面**&#x200B;上运行审计。
* 优先解决&#x200B;**高影响问题**，例如链接失效、缺少 H1 标记或不安全的链接。
* 在受保护的暂存环境中运行审计前，请确保已启用&#x200B;**身份验证**。
* 查看并应用&#x200B;**元标记建议**，以提升 SEO 性能。
