---
name: experience-league-video-upload
description: 当用户想要将视频提交/上传到Experience League（video.tv.adobe.com / KT视频提交）以通过>[！VIDEO]嵌入在此存储库的Markdown中时使用 — 涵盖使用浏览器自动化填写提交表单、此存储库的默认设置以及绝不能自动执行的操作。
source-git-commit: 14f10c231373992c49a8bb93c043556305b6280d
workflow-type: tm+mt
source-wordcount: '840'
ht-degree: 1%

---


# Experience League视频上传

## 概述

此存储库中未托管Experience League视频 — 通过单独的提交表单上传本地`.mp4`，该表单返回您随后嵌入到`>[!VIDEO](...)`的`video.tv.adobe.com` URL（请参阅[[experience-league-markdown]]）。 该技能可通过浏览器自动化填充该表单，最上层是（不包括）附加文件和提交。

表单： https://81368-exlmpcvideoupload.adobeio-static.net/#/

## 视频文件推荐

在用户记录或选择剪辑之前，建议使用&#x200B;**16:9宽高比**，最大分辨率为&#x200B;**1920 x 1080像素** — 这是表单自己规定的要求，而不仅仅是样式首选项。 主动提及它（例如，当用户表示他们将为此捕获屏幕录制时），而不仅仅是在被询问时。

## 硬规则：从不附加文件或提交

提交会创建一个真正的KT Jira票证，并将其上传到生产视频平台 — 这是一个面向外部、难以撤消的操作。 **始终停止**&#x200B;填写完其他字段后停止并向用户交回视频文件和最终提交点击，即使他们下次不重复该指令。 这是此技能的默认设置，不是每个请求都需要重新确认的内容 — 仅当用户明确表示要在同一请求中提交时，才跳过此停止。

## 先决条件

需要`chrome-devtools` MCP服务器，该服务器&#x200B;**不是**&#x200B;提交到此存储库（不应对每个参与者强制使用浏览器自动化MCP）。 如果未加载：

1. 在存储库根目录下创建`.mcp.json`：

   ```json
   {
     "mcpServers": {
       "chrome-devtools": {
         "command": "npx",
         "args": ["-y", "chrome-devtools-mcp@latest", "--accept-insecure-certs", "--no-usage-statistics"]
       }
     }
   }
   ```
2. 将`.mcp.json`添加到`.gitignore`（个人工具，未共享）。
3. 在`.claude/settings.local.json`中，添加`"enableAllProjectMcpServers": true`和`"enabledMcpjsonServers": ["chrome-devtools"]`。
4. 告诉用户重新启动Claude代码（或运行`/mcp`） — MCP服务器仅在启动时加载，无法在会话期间执行此操作。

## 此存储库的默认值

除非用户另有说明，否则请使用：

| 字段 | 默认 | 原因 |
|---|---|---|
| 云 | `Experience Cloud` | — |
| 产品 | `AEM` | 用户为此存储库指定的默认值（该表单还列出了`AEM as a Cloud Service` — 除非询问，否则不要替换它） |
| 子产品 | `AEM Sites` | 最接近的匹配项；该表单没有“Sites Optimizer”条目 |
| 角色 | `User` | 预检/Sites Optimizer内容针对作者/营销人员，而非管理员/开发人员，除非视频明确面向技术受众 |
| 技能级别 | `Beginner` | 除非显示的工作流具有真正的先决条件， |
| 视频语音性别 | `No voices` | 仅用于静默屏幕录制 — 询问剪辑中是否包含旁白 |
| 视频类型 | 询问或推断内容 | 实时选项为`Event` / `Feature` / `Technical` / `Value` — UI演练通常为`Feature` |
| 电子邮件 | 任何预填的 | 表单会自动填写登录用户的Adobe电子邮件；不会覆盖它 |

## 步骤

1. `mcp__chrome-devtools__new_page`到表单URL。
2. `mcp__chrome-devtools__take_snapshot`并等待（`"Title"`上的`mcp__chrome-devtools__wait_for`）直到表单数据完成加载 — 它开始于“正在加载表单数据……” 旋转木马。
3. 填充&#x200B;**标题**&#x200B;和&#x200B;**描述** — 描述是可内容编辑的富文本框，而不是纯`<textarea>`。 `fill`/`fill_form`上无提示的no-ops（该值不接受，并保留“必需”错误）。 相反：`click`它集中显示，然后`mcp__chrome-devtools__type_text`包含文本。
4. 下拉列表（**视频类型**、**视频语音**、**云**、**产品**、**子产品**、**事件名称**）是自定义列表框按钮，而不是本机`<select>`。 对于每个： `click`用于打开它的按钮，从快照中读取实际选项（这些选项已加载API — 不要假定默认表的确切选项拼写仍为最新版本），然后`click`匹配的`option`。
5. **产品**&#x200B;和&#x200B;**子产品**&#x200B;在设置其父字段之前处于禁用状态（产品需要云；子产品需要产品） — 按顺序填写它们。
6. **角色**&#x200B;和&#x200B;**技能级别**&#x200B;是复选框组 — `uid`的复选框上带有`"value": "true"`的`fill_form`在此工作正常（与说明字段不同）。
7. 停下。 截取屏幕快照，总结设置的内容和原因（尤其是任何被替换的默认内容，如产品/子产品），并告诉用户附加视频并自行提交。
8. 用户表示已提交后，请向他们询问生成的Adobe MPC视频URL（上传后显示在表单上，例如`https://video.tv.adobe.com/v/3496629?learn=on`）。 无论此视频走到哪里，都使用它来填充`>[!VIDEO](...)`短代码 — 请勿自己伪造或猜测该URL/ID。

## 验证返回的视频URL

当用户向您提供要嵌入的视频URL时（上述步骤8或任何其他时间）：

- **拒绝任何不在`video.tv.adobe.com`上的内容。** 必须按[[experience-league-markdown]]在该处托管视频 — 指向YouTube、文件主机或任何其他域的链接不是有效的`>[!VIDEO]`目标。 告知用户它需要先完成此存储库的上传流程；请勿嵌入它。
- **如果它是缺少`&enablevpops`的有效`video.tv.adobe.com` URL，请在嵌入之前添加它**（与此存储库中其他`>[!VIDEO]`已使用的约定匹配 — 请参阅`help/home.md`、`help/documentation/trial.md`等）。 如果已经有`?`，则附加`&enablevpops`，否则`?enablevpops`。

## 常见错误

- 在“描述”字段中尝试`fill`/`fill_form`并在错误横幅仍显示“需要描述”时继续。  — 在每个步骤之后检查错误列表，而不仅仅是在末尾。
- 从内存中猜测下拉列表选项文本而不是打开下拉列表 — 实际值（例如，语音性别为`No voices`，视频类型为`Feature`/`Technical`/`Value`，产品下的AEM/AEM-as-a-Cloud-Service拆分）不可猜测，并且独立于此文档更改。
- 单击&#x200B;**上传视频**/附加文件“以保存用户一个步骤”。 请勿 — 请参阅上面的“硬规则”。
