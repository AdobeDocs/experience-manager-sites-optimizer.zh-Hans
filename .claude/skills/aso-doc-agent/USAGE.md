---
source-git-commit: ed1960cc0364dc4169a454a4860b7463890e3b74
workflow-type: tm+mt
source-wordcount: '879'
ht-degree: 0%

---
# ASO文档代理 — 用法

这是什么，它如何运行，以及需要您时该怎么做。

## 作用

该代理每天从以下位置选择最高优先级的未记录ASO功能：
[SITES-49539](https://jira.corp.adobe.com/browse/SITES-49539)的积压(39张票证，例如
&quot;Canonical opportunity how-to&quot;， &quot;Slack notifications&quot;)，撰写了一篇文档
它以存储库的风格，并打开公关 — 指定两者中的任意一个
已配置的审阅者(`sandsinh_adobe` / `kanishka_adobe`)当前打开的次数较少
复查来自此代理的请求。 如果功能需要屏幕快照或视频，它会要求
在Slack上完成公关。

每次运行还会检查每个打开的PR的审核状态：已批准的PR会被合并
立即，读取更改请求的反馈，当它可泛化时
课程（不是一次性打字错误），这样以后草稿就不会重复相同的错误。

一次运行=一次功能=最多一个PR。 每次运行最多只能处理一张票子，
且一次打开的PR不能超过3个（等待现有规则先合并/关闭）。

## 所有东西都生活在哪里

| 什么 | 路径 |
|---|---|
| 它如何决定要做什么 | `.claude/skills/aso-doc-agent/SKILL.md` |
| 确切的逐步说明 | `.claude/skills/aso-doc-agent/references/pipeline.md` |
| 团队特定的设置（编辑此项以更改审阅人、上限、提升时间） | `.claude/skills/aso-doc-agent/config.yml` |
| 从PR审核反馈中汲取的经验教训（在Git中跟踪，在每个草稿之前阅读） | `.claude/skills/aso-doc-agent/references/review-learnings.md` |
| 本地运行状态（已授权 — 可安全删除，它将重建） | `.claude/skills/aso-doc-agent/state/` |
| 每日计划安装程序 | `.claude/scripts/aso-doc-agent-setup.sh` |
| Headless运行的权限允许列表 | `.claude/settings.local.json` （已授权，计算机本地） |

## 运行它

- **手动，在正常会话中：** `/aso-doc-agent` （或`/aso-doc-agent --ticket SITES-XXXXX`）
- 从存储库根目录&#x200B;**Headless，一次性：** `claude -p "/aso-doc-agent"`
- **每天，无人值守：**&#x200B;已通过`launchctl`安装（见下文） — 每天当地时间07:53运行，无需任何操作

### 安装/更改每日计划

```bash
bash .claude/scripts/aso-doc-agent-setup.sh
```

安装`launchd`作业(`~/Library/LaunchAgents/com.sandsinh.aso-doc-agent.plist`)
每天从此存储库运行`claude -p "/aso-doc-agent"`。 可随时重新运行脚本
编辑其中的计划（默认为：07:53 local）。 仅当计算机处于以下状态时才有效：
此时处于打开和唤醒状态 — launchd不会以追溯方式运行丢失的作业，但会运行
正常安排的下一个时间。

```bash
launchctl list | grep com.sandsinh.aso-doc-agent   # confirm it's loaded
launchctl start com.sandsinh.aso-doc-agent         # trigger a run right now, don't wait for 07:53
launchctl unload ~/Library/LaunchAgents/com.sandsinh.aso-doc-agent.plist  # stop it
```

中每个计划运行土地的日志 `.claude/skills/aso-doc-agent/state/launchd.out.log`
和`launchd.err.log`。

## 要求您执行的操作

- **来自代理的Slack DM**(作为您发送，发送给您 — 先发送，后发送给kanishka
escalation)询问屏幕快照或视频，以及准确的捕获步骤和要捕获的URL
使用。 **在链接的Jira票证上回复，而不是在Slack中**：直接附加屏幕快照，
对于视频，请通过常规的Experience League视频表单上传它
（`experience-league-video-upload`技能）并粘贴结果 `video.tv.adobe.com`
链接为Jira注释。 下次运行会自动选取它。
- 如果无人在&#x200B;**5天**&#x200B;内响应，则请求会从sandsinh提升到kanishka
自动。 在&#x200B;**10天**&#x200B;之后，如果两者都没有响应，代理将发送文档
添加内联注释。 没有基于超时的自动合并 — PR
仍然在等待真正的人类检查，无论这要花多长时间。
- **要审阅的PR** — 分配给你们中较少代理打开的PR
当前正在等待审阅。 草拟的公示文件意味着媒体仍在等待处理；它们翻转到
资产显示后自动可供审查。 批准代理并合并
它将在下次运行时运行 — 不需要您执行单独的合并步骤。
- **如果您请求更改**，代理将在下次运行时读取您的备注。 可推广
反馈（不是打字错误/链接修复）写入`references/review-learnings.md`，因此
未来的PR不需要重复相同的校正。

## 调整行为

编辑`.claude/skills/aso-doc-agent/config.yml`(在Git中跟踪 — 更改影响每个
以后在该计算机上运行（或其他克隆存储库的人运行）：

- `pr.max_open` — 在代理暂停领取新票证之前打开的PR数（默认为3）
- `pr.stale_after_hours` — `CHANGES_REQUESTED` PR在停止计入`pr.max_open`之前可以停留多长时间（默认336 = 14天）；它保持打开状态，这仅会取消阻止新挑选
- `github.reviewers` — 谁被分配，在哪个余额中
- `media.contacts_in_order` / `escalate_after_hours` （默认为120 = 5天） / `give_up_after_hours` （默认为240 = 10天） — 询问对象、询问顺序以及询问的耐心程度；这两者都是从原始请求中衡量的，因此提升不会超过放弃日期
- `pr.check_reviews_every_run` — 关闭审核检查步骤（不推荐；这是合并和学习的方式）

## 如果它在权限提示时停止

Headless (`claude -p`， launchd)运行没有要提示的终端 — 未列出的工具调用
只会失败而不是挂掉。 如果运行日志显示对命令的权限拒绝
管道需要是合法的，请将其添加到中的`permissions.allow`列表
`.claude/settings.local.json` (未在git中跟踪 — 计算机本地；每个开发人员正在运行
此代理需要其自己的副本和自己的范围允许列表)。

## 如果它完全停止进展

按顺序检查：
1. `gh pr list --repo Adobe-Enterprise-Docs/experience-manager-sites-optimizer.en --label aso-doc-agent --state open` — 如果此项显示3，则表示它正在等待审核，而不是卡住。
2. Jira：是否在SITES-49539下还剩有不为`aso-doc-agent-picked`的合格`New`票证？ 该标签仅会在存在分支+PR时应用（pipeline.md步骤6.10），因此崩溃的运行不应留下已标记但未发布的票证 — 如果您仍然找到已标记但未发布的票证（例如，手动添加标签），请手动删除该标签以使其再次符合条件。
3. `.claude/skills/aso-doc-agent/state/launchd.err.log`表示最近运行的错误。
4. 如果运行摘要显示“严重积压已完全覆盖”或“此处没有任何可执行的操作”，但您知道应该有符合条件的工作，请将其视为可疑 — 这些消息将保留给真正空的结果。 单独记录实际的Jira/GitHub/Slack错误，该错误应在`launchd.err.log`中显示为自己的行，而不是隐藏在这些消息之一后面。
