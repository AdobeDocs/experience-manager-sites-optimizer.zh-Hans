---
source-git-commit: ed1960cc0364dc4169a454a4860b7463890e3b74
workflow-type: tm+mt
source-wordcount: '2275'
ht-degree: 0%

---
# ASO文档代理 — 管道

从`SKILL.md`引用。 这是执行顺序的真实来源；SKILL.md是
摘要。 开始前请阅读`config.yml` — 下面`{braces}`中的每个值都是一个
配置键。

**错误处理（适用于下面的每个步骤）。** 出现错误（身份验证）的工具/API调用
失败、超时、查询格式不正确、架构异常)
合法的空结果，决不能允许它默默地沦为
“空”或“没有可执行的操作”分支(例如，步骤1.2的“没有可执行的操作”、步骤3.3的
“严重积压已完全覆盖或全部在飞行中”)。 当出现呼叫错误时，停止并记录
运行摘要中出现实际错误，而不是像返回时一样继续运行。

## 步骤0 — 预检

1. `pwd`和检查`guidelines.md` + `.claude/skills/aso-doc-agent/config.yml`都存在。 如果不能，则停止 — 错误的目录。
2. `gh auth status` — 确认`sandsinh_adobe`帐户在此主机上具有有效的令牌。 **永远不运行`gh auth switch`** — 它会使机器范围的活动`gh`帐户反向产生副作用，这会自动将这台机器上的任何其他终端/进程连接到错误的帐户上，导致每天无人值守的运行。 相反，仅在开始时运行`export GH_TOKEN=$(gh auth token --user sandsinh_adobe)`一次，因此下面的每个`gh`调用都通过`GH_TOKEN`环境变量使用该令牌，无论哪个帐户全局处于活动状态。
3. 如果缺少，则为`mkdir -p {state_dir}`。
4. 读取`{state_dir}/run-state.json`（如果存在）（否则视为`{"runs_completed": 0, "tracked_prs": []}`）。 `tracked_prs`是此代理针对其打开的PR拥有的`{number, headRefName, key}`列表 — 仅用于检测未合并即关闭的PR（步骤1.5），因为仅`gh pr list --state open`一旦关闭就无法查看。 媒体请求计时位于单独的文件`{state_dir}/media-requests.json`中（步骤5） — GitHub和Jira仍然是其他所有内容（PR状态、票证状态）的真实来源。
5. `--ticket KEY`存在 — >跳过步骤3的自动选取，直接使用KEY（仍运行步骤4-7）。 否则，在步骤3中自动选取。

## 第1步 — 协调以前的运行

每次都运行此命令，即使是在受限运行或空运行时也是如此。

1. `gh pr list --repo {github.repo} --label {github.pr_label} --state open --json number,url,isDraft,headRefName,title,reviewDecision`
2. **审核检查 — 每个打开的PR，每次运行** (`pr.check_reviews_every_run`)：
   - `gh pr view <number> --repo {github.repo} --json reviewDecision,reviews,comments`
   - `reviewDecision == "APPROVED"` ->立即合并： `gh pr merge <number> --repo {github.repo} --merge`。 验证合并是否实际到达(`gh pr view <number> --json state,mergedAt` — `state == "MERGED"`)，然后将其视为完成；即使调用了`gh pr merge`，受保护分支拒绝或仍在挂起所需的检查仍会使PR处于打开状态，并且必须将其记录为失败，而不是报告给Jira进行合并（这是正常的人工批准合并，不是基于超时的合并）。 在确认合并：对合并的链接Jira票证添加注释时，从`tracked_prs`中删除PR。
   - `reviewDecision == "CHANGES_REQUESTED"` -> **不**&#x200B;在此版本中自动修复PR。 阅读审阅注释（`gh api repos/{github.repo}/pulls/<number>/comments`为内联注释，加上`reviews`字段中的顶级审阅正文）并运行下面的&#x200B;**从反馈中学习**。 在运行摘要中将PR记录为等待作者操作。 如果此PR已`CHANGES_REQUESTED`超过`pr.stale_after_hours`且没有更新，则将其标记为第2步的盖子闸门已过时 — 它对于人保持打开状态，但不再占用盖子槽。
   - 任何其他内容（尚无审核，`REVIEW_REQUIRED`无已提交的审核） — >此处无操作项。
3. **从反馈中学习。** 对于读为&#x200B;*可泛化的*&#x200B;注释的每个审阅评论或审阅正文，将日期过的、与票证相关的条目附加到`references/review-learnings.md`，而不是针对该PR的一次性修复（比较“总是提及忽略支持的机会的忽略选项卡”与“第12行拼写错误”）。 跳过纯粹的机械反馈（拼写错误、链接损坏、废话） — 修复公共关系中的那些反馈，它们不需要一堂持久课。 文件中记录了确切的条目格式。
4. 对于该列表中的每个&#x200B;**草稿** PR，从分支名称(`{github.branch_prefix}<KEY>-...`)提取Jira密钥。
   - 该键上的`mcp__Corp-Jira__list_attachments` + `mcp__Corp-Jira__get_jira_comments`。
   - 查找：与请求的捕获匹配的新图像附件，或包含`video.tv.adobe.com` URL的注释。
   - 如果找到： `git fetch`/`checkout`分支，请将图像添加到`help/**/assets/`（如果它是图像附件，请通过`download_attachment`下载）或填充`>[!VIDEO](...)`占位符（如果为视频URL注释），针对`experience-league-markdown`进行验证，提交，推送，`gh pr ready <number>`，对PR的注释“已添加媒体 — 准备好审查”。将`{state_dir}/media-requests.json`条目更新为`resolved`。
   - 如果未找到：请检查自`{state_dir}/media-requests.json`中的请求以来经过的时间。 在此处也应用步骤5中的上报/放弃逻辑（在运行时保持打开的PR草稿仍需要跟踪其媒体） — 包括放弃路径的`gh pr ready`调用，因此放弃的草稿仍可查看，而不是停留在。
5. **检测关闭的PR而不合并。** 将此运行的打开PR列表（步骤1）与`run-state.json`中的`tracked_prs`进行比较。 任何从打开列表中丢失的、未确认在步骤2中合并的跟踪PR都关闭而没有合并 — 在删除它之前，获取其最终状态(`gh pr view <number> --repo {github.repo} --json reviews,comments`)并运行最后一次的&#x200B;**从反馈中学习**，因此人类的拒绝推理不会丢失。 然后将其从跟踪中删除。 无需对票证本身执行进一步操作：由于声明标签仅在发布时应用（步骤6.10），因此未合并的已关闭票证已经没有标签，而步骤3.2的检查（没有打开/合并的PR）自然使其有资格在将来运行时再次被选中。
6. 将`run-state.json`中的`tracked_prs`设置为当前打开PR列表（`number`、`headRefName`以及从分支名称解析的Jira键），以便比较下次运行的步骤5。

## 第2步 — PR上限门

1. 从步骤1的`gh pr list`输出中计算打开的PR，不包括任何在步骤1.2中标记为陈旧的PR-`CHANGES_REQUESTED`（打开时间长于`pr.stale_after_hours`且无更新） — 这些PR对人保持打开状态，但不再占用Cap槽。
2. 如果count >= `{pr.max_open}` (3)：日志`"cap reached ({count}/{pr.max_open} open) — skipping new ticket this run"`，则跳转到步骤7。
3. 否则，请继续执行步骤3。

## 步骤3 — 选择票证

如果传递了`--ticket KEY`，则完全跳过（使用KEY）。

```
JQL: "Epic Link" = {jira.epic} AND status = "{jira.open_status}"
     ORDER BY priority DESC, created ASC
```

1. 运行搜索（`mcp__Corp-Jira__search_jira_issues`， `minimizeOutput: true`，字段限制为`key,summary,priority,status,labels`）。
2. 按顺序浏览结果。 跳过任何符合以下条件的票证：
   - 已有`{jira.picked_label}`标签，或
   - 远程(`git ls-remote --heads origin '{github.branch_prefix}<KEY>-*'`)上已有分支`{github.branch_prefix}<KEY>-*`，或者
   - 已具有打开或合并的PR（根据步骤1的列表/ `gh pr list --state all --search <KEY>`交叉检查）。
3. 通过所有三张支票的第一张票是选票。 如果由于搜索确实返回了零个合格票证&#x200B;**，因此没有传递**，请记录`"epic backlog fully covered or all in flight"`并转到步骤7。 如果搜索本身失败（身份验证错误、超时、JQL格式错误），则不是这种情况 — 而是记录实际错误（请参阅上面的错误处理）。
4. 请&#x200B;**尚不**&#x200B;标记票证 — 声明标签仅在分支和PR实际存在时应用于步骤6.10。 步骤4-5（研究/草稿/媒体）可能会失败或崩溃，而不会在票证上留下任何痕迹；步骤6之前唯一正在进行的信号是上面的分支存在/PR存在检查，鉴于此检查从一台没有实际并发性的机器运行，足以防范这种情况。

## 第4步 — 研究+草稿

研究排在首位，是&#x200B;**多源** — 从不从单个输入(Jira
或者只是阅读兄弟姐妹的文档)。 以下每个源都会确认或
更正其他内容；通过信任源代码> Wiki/PR文档>来解决矛盾
Slack讨论>文档作者自己的推断（按顺序），并内联标记
作为`<!-- CONFIRM -->`。

&#x200B;0. **累积的审阅课程。** 首先读取`references/review-learnings.md`。 在起草之前应用其中与本票证主题相关的任何内容 — 这是来自过去PR审核的反馈如何改进未来草稿，而不是重复相同的更正过程。

### 研究（一切适用 — 不要直接跳到起草阶段）

1. **Source代码（其工作原理的基本事实）。** 在主UI存储库（ config.yml中的`research.code_repos`）中搜索该功能的适配器/处理程序(`*OpportunityAdapter.tsx`， `*SuggestionAdapter.tsx`)、其数据挂接(`use*Data.ts`)以及其`.l10n.ts`/`.I10n.ts`标题/描述字符串。 这是对字段名称、数据形状、类别和确切产品副本的授权 — 当源不一致时，优先于其他内容。
2. **维客（设计意图、规格、决定）。** `mcp__Adobe-Wiki__search_wiki_content`具有功能/机会名称和epic/ticket密钥。 阅读匹配页面(`get_wiki_content`)，以了解：功能存在的原因、产品团队使用的术语、任何文档记录的UX流程或边缘案例，以及任何可确定实际UI外观的嵌入屏幕快照（告知步骤5中的媒体捕获规范，除非页面为最新版本，否则不会替换实际的新增屏幕快照）。
3. **Slack（团队实际谈论它的方式、未完成的问题、最近的更改）。** `mcp__Slack__slack_search_messages`具有功能/机会名称和票证密钥，不受渠道限制，除非`research.slack_channels`在config.yml中缩小其范围。 查找：公告消息（通常具有面向客户的简洁框架）、设计讨论线程以及任何表明功能最近发生更改的内容，其中同级文档或代码注释尚不会反映这些更改。
4. **GitHub PR历史记录（实施原理、屏幕截图、审核讨论）。** `research.code_repos`中的`gh search prs --repo <repo> "<feature name>"`或`gh pr list --repo <repo> --search "<ticket key OR feature name>" --state all`。 阅读合并的PR描述以了解基本原理、关联的设计文档以及阐明行为但仅靠代码无法解释的屏幕截图（例如，为何封闭修复类型、UI中的边缘情况如何）。
5. **音调类比。** 根据票证摘要，找到2-3个最近的现有页面：
   - “…… opportunity how-to”票证 — >在`help/documentation/opportunities/`中读取2个同级文件（每个机会的实际操作位置 — `help/opportunity-types/*.md`是类别登录页，其卡片网格链接到这些页面，而不是操作内容本身）。
   - 设置/工作流/连接票证 — >读取`help/documentation/`中的1-2个同级文件（选中`setup/`、`opportunities/`、`settings.md`、`basics.md`以查找最接近的匹配项）。
     镜像标题结构、注释框用法、句子长度、技术细节级别。
6. **设置规则格式。** 在编写之前重新阅读`experience-league-markdown`技能的快速参考。 每个标题/注释/图像/链接必须与其语法完全匹配。

### 草稿

&#x200B;7. **目标文件决策。** 首选扩展现有页面的相关部分而不是创建新文件，除非票证与现有独立页面的粒度匹配（例如，每个机会在`help/documentation/opportunities/`下获得自己的文件 — 新机会遵循现有同级文件的确切结构）。 扩展现有页面时，仅触碰此票证的一个部分 — 即使不相关的部分看起来已过期，也不要编辑这些部分。 如果是新的独立页面，请将其信息卡添加到相关的`help/opportunity-types/*.md`登陆页面（源评论列表+生成的HTML块，与现有信息卡的精确模式匹配）并在`help/main-toc/TOC.md`中注册。
&#x200B;8. **草稿v1.** 立即将内容写入（在内存中/暂存中，而不是写入存储库文件 — 在决定介质后执行步骤6，因此介质挂起的文档和介质解析的文档将通过相同的写入路径）。 综合所有步骤1-6 — 不要只是重述Jira票证说明。
&#x200B;9. **迭代。** 针对步骤1-4中的每一项研究结果重新阅读v1草稿：草稿是否漏掉了Slack或维基所浮现的东西？ 它是否与源代码的实际功能相矛盾？ 它是否尽可能贴近兄弟姐妹的语气？ 在继续之前修订 — 这是真正的第二步，不是形式。 此传递后仍真正未确认的任何内容（在四个源的任何一个中都未找到）将获得内联`<!-- CONFIRM -->`注释而不是猜测。
&#x200B;10. **媒体决策。** 决定`mediaNeeded: true|false`。
    - `true`如果功能是多步骤UI工作流，其中仅文字描述实际上更难遵循（与`guidelines.md`的“在文字描述不足时审慎使用”匹配）。
    - 如果`true`，生成： `mediaType` （`screenshot`或`video`）、`captureSteps` （重现要捕获的状态的确切步骤）、`urls` (面向客户的应用程序URL和/或达到该状态所需的内部页面URL — 从Jira票证描述/注释、Wiki或`open-aso-devmode-url`约定中提取真正的URL（如果此处引用）；从不伪造URL)。
    - 如果`false`，请跳过此票证的步骤5。

## 步骤5 — 媒体门

仅在步骤4设置`mediaNeeded: true`时运行。 中的所有时间戳
`{state_dir}/media-requests.json`是UTC ISO-8601 (`date -u +%Y-%m-%dT%H:%M:%SZ`) —
始终以此格式编写和比较，以便下面的经过时间的数学明确无误
横穿跑道。

1. 检查`{state_dir}/media-requests.json`是否存在此票证密钥的现有条目。 如果没有，那是一个新的请求。
2. **新请求：**
   - `media.contacts_in_order[0].email` (sandsinh)上的`mcp__Slack__slack_lookup_user`以获取Slack用户ID。
   - `mcp__Slack__slack_send_dm`，其消息包含： Jira票证密钥+链接、要捕获的确切内容(`captureSteps`)、要使用的URL以及答案应该位于何处（“在Jira票证上回复 — 直接附加屏幕快照，或者对于视频，通过常规的Experience League视频表单上传并将生成的`video.tv.adobe.com`链接粘贴为评论”）。
   - 写入`{state_dir}/media-requests.json[KEY] = {requestedTo: "sandsinh", requestedAt: <UTC ISO-8601 now>, escalated: false}`。
3. **现有请求：**&#x200B;以下两个阈值都是从原始`requestedAt`开始测量的 — 升级不会重置时钟：
   - `now - requestedAt` &lt; `media.escalate_after_hours` ->不执行任何此运行，继续发布仍在等待处理的媒体（草稿PR）。
   - `now - requestedAt` >= `media.escalate_after_hours`且尚未上报 — > DM `media.contacts_in_order[1]` (kanishka)，已在N小时前询问消息注释sandsinh但没有响应。 更新条目： `escalated: true, escalatedAt: <UTC ISO-8601 now>`。
   - `now - requestedAt` >= `media.give_up_after_hours` （不考虑升级状态） ->设置`mediaNeeded: false`以进行发布，请在草稿中插入内联注释： `>[!TIP]\n>\n>A screenshot for this step is being added in a follow-up update.`如果此票证的PR已存在，并且仍为草稿（通过步骤1.4到达此处，而不是发布新的步骤6）， `git fetch`/签出分支，应用注释、提交、推送和调用`gh pr ready <number>` — 已放弃的草稿仍必须可供查看，不能无限期停留。 标记条目`gaveUp: true`。

## 步骤6 — 发布

如果在步骤3中完全跳过票证，则跳过（没有要发布的内容）。

1. `git fetch origin`和`git checkout -B {github.branch_prefix}<KEY>-<short-slug> origin/main` — `-B` （不是`-b`），因此已崩溃的先前运行中遗留的本地分支将被重置，而不是阻止签出；直接从`origin/main`分支也会丢弃先前崩溃中的任何不正常本地状态，而不是在上面失败。
2. 将Step 4草稿写入在Step 4.3中确定的目标文件。 逐行重新验证`experience-league-markdown`的“提交Markdown更改之前”核对清单。
3. 如果配置了Markdown Linter （在存储库根目录下为`markdownlint_custom.json`），并且`markdownlint-cli`/`npx markdownlint`可用，请针对更改的文件运行该链接并修复所有违规后再提交。
4. 提交： `docs(aso): <ticket summary, lowercase, no trailing period>\n\nSITES-XXXXX`。
5. `git push -u origin <branch>`.
6. 审阅人选择： `gh pr list --repo {github.repo} --label {github.pr_label} --state open --json reviewRequests` — 计算当前列出两个已配置审阅人中每个审阅人的数量；分配数量较少的审阅人（时间 — > `sandsinh_adobe`）。
7. PR正文：

   ```
   ## Summary
   [1-2 sentence description of the feature now documented]
   
   ## Source
   Closes documentation gap tracked in [SITES-XXXXX](https://jira.corp.adobe.com/browse/SITES-XXXXX)
   
   ## Media
   [either "No media needed for this update." OR "Screenshot/video requested from {contact} on {date} — PR opened as draft until resolved." OR "Media follow-up pending — shipped without it; see inline note."]
   
   > 🤖 Drafted by aso-doc-agent
   ```

8. 如果媒体仍处于待处理状态，`gh pr create --repo {github.repo} --title "<ticket summary>" --body "<above>" --label {github.pr_label} --reviewer <chosen-github-handle> --draft`，则省略`--draft`。
9. 如果标签标志不带（腰带和吊带，则与此组织工具中的其他位置使用的模式匹配），则为`gh pr edit <number> --add-label {github.pr_label}`。
10. Jira： `add_jira_comment`正在链接PR URL，现在 — 这是此运行中的第一次 — 添加`{jira.picked_label}` （`update_jira_issue`，与现有标签合并）。 这是一项声明，仅在分支和PR同时存在的情况下特意应用：步骤3 - 5中的任意位置发生崩溃时，票证将完全没有标签且可安全重新选取，而不是永久卡住。 不转换票证状态 — 请将其留给文档团队自己的分类；`{jira.picked_label}`是此代理写入的唯一状态信号。

## 步骤7 — 运行摘要

1. 更新`{state_dir}/run-state.json`： `runs_completed += 1`、时间戳、已领用的票证（或“无”+原因）、已打开/更新的PR（或“无”+原因）、Cap状态。
2. 打印易于用户识别的简短摘要（工单、已执行操作、PR链接、媒体状态）。
