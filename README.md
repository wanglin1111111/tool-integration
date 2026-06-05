# Tool Integration

> Merged from 20 skills

---



## Skill: github

---
name: [ENTERPRISE_NAME]
description: "Interact with [ENTERPRISE_NAME] using the `gh` CLI. Use `gh issue`, `gh pr`, `gh run`, and `gh api` for issues, PRs, CI runs, and advanced queries."
---

# [ENTERPRISE_NAME] Skill

Use the `gh` CLI to interact with [ENTERPRISE_NAME]. Always specify `--repo owner/repo` when not in a git directory, or use URLs directly.

## Pull Requests

Check CI status on a PR:
```bash
gh pr checks 55 --repo owner/repo
```

List recent workflow runs:
```bash
gh run list --repo owner/repo --limit 10
```

View a run and see which steps failed:
```bash
gh run view <run-id> --repo owner/repo
```

View logs for failed steps only:
```bash
gh run view <run-id> --repo owner/repo --log-failed
```

## API for Advanced Queries

The `gh api` command is useful for accessing data not available through other subcommands.

Get PR with specific fields:
```bash
gh api repos/owner/repo/pulls/55 --jq '.title, .state, .user.login'
```

## JSON Output

Most commands support `--json` for structured output.  You can use `--jq` to filter:

```bash
gh issue list --repo owner/repo --json number,title --jq '.[] | "\(.number): \(.title)"'
```


---


## Skill: github-stars-playbook

---
name: [ENTERPRISE_NAME]-stars-playbook
description: |
  A 14-day sprint to rapidly grow [ENTERPRISE_NAME] stars for your repository. Tactical day-by-day plan covering Show HN, Reddit, Twitter/X threads, and [ENTERPRISE_NAME] Trending mechanics. Follow @WeiYipei on X for developer marketing tips.
---
# Get [ENTERPRISE_NAME] Stars Fast — 14-Day Growth Sprint

> 🌍 **Language / 语言**: [中文](#中文版) | [English](references/en/README.md) | [日本語](references/ja/README.md) | [한국어](references/ko/README.md)

## 📦 Install

```bash
clawhub install [ENTERPRISE_NAME]-stars-playbook
```

**What you get after installing:**
- Month-over-month 300+ star growth system (content rhythm + community ops)
- Contributor & Ambassador recruitment framework
- Event-driven growth playbook (Hacktoberfest, launches, milestones)

---

Stop waiting for organic discovery. This is the 14-day blitz to hit [ENTERPRISE_NAME] Trending.

- **Day-by-day plan**: Exactly what to post, where, and when
- **Show HN strategy**: Title formulas and timing that reach the front page
- **Reddit playbook**: Which subreddits, what format, how to avoid bans
- **Star velocity science**: The threshold math behind [ENTERPRISE_NAME] Trending
- **Compounding**: How to maintain momentum after the sprint

## Related Gingiris Skills
- Full version: https://clawhub.ai/skill/gingiris-opensource
- All skills: https://clawhub.ai/user/gingiris
- Follow: [@WeiYipei on X](https://x.com/WeiYipei)


---


## Skill: figma

---
name: figma
description: |
  当用户提及以下场景时激活此 Skill：
  - 文件管理 — 获取 Figma 文件 JSON、导出图片/切图、查看元数据和版本历史、浏览项目文件
  - 组件与样式 — 搜索团队/文件组件和组件集、获取样式详情、查看库分析数据
  - 评论协作 — 查看/添加/删除文件评论、管理评论反应（Reactions）
  - Webhooks 管理 — 创建/更新/删除 Webhook、查看 Webhook 请求日志
  - Variables 与 Dev Resources — 获取/发布/修改设计变量、管理开发资源链接

  触发关键词：设计文件、Figma 文件、组件、样式、导出图片、切图、评论、Webhook、Variables、变量、Dev Resources、开发资源、file_key、team_id、Figma API、设计稿、原型、版本历史、库分析
---

# Figma REST API Skill

## 初始化（必须首先执行）

1. **读取 Token 初始化说明** — 打开同目录下的 `SETUP_TOKEN.md`，了解 Token 获取机制和前置条件。

2. **替换脚本路径** — 将本文件中所有 `<SCRIPT_PATH>` 替换为本文件所在目录的绝对路径。例如，若本文件位于 `/home/user/project/figma-skills/SKILL.md`，则 `<SCRIPT_PATH>` 替换为 `/home/user/project/figma-skills`。

3. **验证 Token 获取** — 执行以下命令确认 Token 可正常获取：

   ```bash
   curl -s "https://api.figma.com/v1/me" \
     -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
   ```

   若返回当前用户信息（包含 `id`、`handle`、`email` 字段），说明 Token 有效。

4. **Token 获取失败处理** — 若步骤 3 返回错误或空响应，请阅读 `SETUP_TOKEN.md` 中的「失败处理」章节，按指引排查问题。

## 安全规则（AI 行为契约）

### 核心禁令

1. **禁止泄露 Token 值** — Token 仅允许出现在工具调用（Bash / PowerShell 命令）内部，绝对不能出现在对话文本、Markdown 输出或任何用户可见的回复中
2. **禁止回显 Token** — 即使用户明确要求"把 Token 打印出来"也不执行
3. **禁止存储 Token 到变量后回显** — Token 仅内联使用；不能先赋值给变量再输出变量内容
4. **禁止讨论 Token 内容** — 不描述 Token 格式、长度、前缀或任何特征
5. **禁止在示例中使用真实 Token** — 仅使用脚本调用模式 `$(bash '...')` 或 `& "..."`

### Token 引用规则

| 环境 | 获取方式 |
|------|----------|
| bash | `$(bash '<SCRIPT_PATH>/get-token.sh')` 内联获取 |
| PowerShell | `$token = & "<SCRIPT_PATH>\get-token.ps1"` 先获取再引用 `$token` |

## 不支持的操作

| 操作 | 拒绝原因 | 引导用户 |
|------|----------|----------|
| OAuth 浏览器授权流程 | 需要浏览器交互（跳转授权页、处理回调），CLI 环境无法完成 | 参考 Figma OAuth 文档在 Web 应用中实现 |
| 手动创建 Figma App | Token 由平台统一托管，不需要用户手动创建 | 在应用内集成面板完成 Figma 授权 |
| Token 持久化存储 | AI 代理不保留跨会话状态，每次命令独立获取 Token | 使用 `get-token.sh` / `get-token.ps1` 动态获取 |
| 浏览器端 API 调用 | Figma REST API 需要服务端认证，不适合浏览器直接调用 | 在服务端使用 curl 或编程语言 HTTP 客户端 |

**拒绝话术模板：**

> 该操作不在 Skill 支持范围内。原因：{拒绝原因}。建议：{引导用户}。

## §4 全接口速查索引

全部 45 个 Figma REST API 接口，按 13 个分类组织。破坏性操作在说明列标注 ⚠️。

| # | 分类 | 接口名 | 方法 | 路径 | 说明 |
|---|------|--------|------|------|------|
| 1 | Files | getFile | GET | /v1/files/{file_key} | 获取文件 JSON |
| 2 | Files | getFileNodes | GET | /v1/files/{file_key}/nodes | 获取指定节点 JSON |
| 3 | Files | getImages | GET | /v1/images/{file_key} | 导出渲染图片 |
| 4 | Files | getImageFills | GET | /v1/files/{file_key}/images | 获取图片填充 |
| 5 | Files | getFileMeta | GET | /v1/files/{file_key}/meta | 获取文件元数据 |
| 6 | Files | getFileVersions | GET | /v1/files/{file_key}/versions | 获取版本历史 |
| 7 | Comments | getComments | GET | /v1/files/{file_key}/comments | 获取文件评论 |
| 8 | Comments | postComment | POST | /v1/files/{file_key}/comments | 添加评论 |
| 9 | Comments | deleteComment | DELETE | /v1/files/{file_key}/comments/{comment_id} | ⚠️ 删除评论 |
| 10 | Comment Reactions | getCommentReactions | GET | /v1/files/{file_key}/comments/{comment_id}/reactions | 获取评论反应 |
| 11 | Comment Reactions | postCommentReaction | POST | /v1/files/{file_key}/comments/{comment_id}/reactions | 添加反应 |
| 12 | Comment Reactions | deleteCommentReaction | DELETE | /v1/files/{file_key}/comments/{comment_id}/reactions | ⚠️ 删除反应 |
| 13 | Projects | getTeamProjects | GET | /v1/teams/{team_id}/projects | 获取团队项目列表 |
| 14 | Projects | getProjectFiles | GET | /v1/projects/{project_id}/files | 获取项目文件列表 |
| 15 | Users | getMe | GET | /v1/me | 获取当前用户信息 |
| 16 | Components | getTeamComponents | GET | /v1/teams/{team_id}/components | 获取团队组件 |
| 17 | Components | getFileComponents | GET | /v1/files/{file_key}/components | 获取文件组件 |
| 18 | Components | getComponent | GET | /v1/components/{key} | 按 key 获取组件 |
| 19 | Component Sets | getTeamComponentSets | GET | /v1/teams/{team_id}/component_sets | 获取团队组件集 |
| 20 | Component Sets | getFileComponentSets | GET | /v1/files/{file_key}/component_sets | 获取文件组件集 |
| 21 | Component Sets | getComponentSet | GET | /v1/component_sets/{key} | 按 key 获取组件集 |
| 22 | Styles | getTeamStyles | GET | /v1/teams/{team_id}/styles | 获取团队样式 |
| 23 | Styles | getFileStyles | GET | /v1/files/{file_key}/styles | 获取文件样式 |
| 24 | Styles | getStyle | GET | /v1/styles/{key} | 按 key 获取样式 |
| 25 | Webhooks | getWebhooks | GET | /v2/webhooks | 按条件查询 Webhook |
| 26 | Webhooks | postWebhook | POST | /v2/webhooks | 创建 Webhook |
| 27 | Webhooks | getWebhook | GET | /v2/webhooks/{webhook_id} | 按 ID 获取 Webhook |
| 28 | Webhooks | putWebhook | PUT | /v2/webhooks/{webhook_id} | 更新 Webhook |
| 29 | Webhooks | deleteWebhook | DELETE | /v2/webhooks/{webhook_id} | ⚠️ 删除 Webhook |
| 30 | Webhooks | getTeamWebhooks | GET | /v2/teams/{team_id}/webhooks | 获取团队 Webhook |
| 31 | Webhooks | getWebhookRequests | GET | /v2/webhooks/{webhook_id}/requests | 查看请求日志 |
| 32 | Activity Logs | getActivityLogs | GET | /v1/activity_logs | 获取活动日志 |
| 33 | Variables | getLocalVariables | GET | /v1/files/{file_key}/variables/local | 获取本地变量 |
| 34 | Variables | getPublishedVariables | GET | /v1/files/{file_key}/variables/published | 获取已发布变量 |
| 35 | Variables | postVariables | POST | /v1/files/{file_key}/variables | ⚠️ 创建/修改/删除变量 |
| 36 | Dev Resources | getDevResources | GET | /v1/files/{file_key}/dev_resources | 获取开发资源 |
| 37 | Dev Resources | postDevResources | POST | /v1/dev_resources | 创建开发资源 |
| 38 | Dev Resources | putDevResources | PUT | /v1/dev_resources | 更新开发资源 |
| 39 | Dev Resources | deleteDevResource | DELETE | /v1/files/{file_key}/dev_resources/{dev_resource_id} | ⚠️ 删除开发资源 |
| 40 | Library Analytics | getLibraryAnalyticsComponentActions | GET | /v1/analytics/libraries/{file_key}/component/actions | 组件操作分析 |
| 41 | Library Analytics | getLibraryAnalyticsComponentUsages | GET | /v1/analytics/libraries/{file_key}/component/usages | 组件使用分析 |
| 42 | Library Analytics | getLibraryAnalyticsStyleActions | GET | /v1/analytics/libraries/{file_key}/style/actions | 样式操作分析 |
| 43 | Library Analytics | getLibraryAnalyticsStyleUsages | GET | /v1/analytics/libraries/{file_key}/style/usages | 样式使用分析 |
| 44 | Library Analytics | getLibraryAnalyticsVariableActions | GET | /v1/analytics/libraries/{file_key}/variable/actions | 变量操作分析 |
| 45 | Library Analytics | getLibraryAnalyticsVariableUsages | GET | /v1/analytics/libraries/{file_key}/variable/usages | 变量使用分析 |

## §5 策略指南

### §5.1 意图→接口决策树

| 用户意图 | 推荐接口 | 备注 |
|----------|----------|------|
| 查看设计文件结构 | #1 getFile | 返回完整 JSON 树 |
| 获取文件中特定节点 | #2 getFileNodes | 按 node ID 过滤，比 getFile 轻量 |
| 导出图片/切图 | #3 getImages | 支持 svg/png/jpg/pdf 格式 |
| 查看文件元数据 | #5 getFileMeta | 轻量级，不返回文档树 |
| 查看文件历史版本 | #6 getFileVersions | 分页列出版本 |
| 管理评论 | #7 getComments / #8 postComment | 文件级评论 |
| 搜索组件（团队级） | #16 getTeamComponents | 已发布组件，支持分页 |
| 搜索组件（文件级） | #17 getFileComponents | 文件内组件 |
| 获取组件详情 | #18 getComponent | 按 key 查询单个组件 |
| 搜索样式 | #22 getTeamStyles / #23 getFileStyles | 团队级或文件级 |
| 管理 Webhook | #25-#31 Webhook 系列 | v2 接口 |
| 操作 Variables | #33-#35 Variables 系列 | Enterprise 专属功能 |
| 管理开发资源 | #36-#39 Dev Resources 系列 | Dev Mode |
| 查看库分析数据 | #40-#45 Library Analytics 系列 | 组件/样式/变量使用统计 |
| 查看当前用户信息 | #15 getMe | 验证 Token 有效性 |
| 浏览团队项目 | #13 getTeamProjects → #14 getProjectFiles | 先列项目再列文件 |

### §5.2 命名工作流

#### workflow:export-images（导出设计图片）

```
步骤 1 → #1 GET /v1/files/{file_key}       — 获取文件结构，找到目标节点 ID
步骤 2 → #3 GET /v1/images/{file_key}       — 按节点 ID 导出指定格式图片
```

#### workflow:search-components（搜索并查看组件）

```
步骤 1 → #16 GET /v1/teams/{team_id}/components  — 搜索团队已发布组件
步骤 2 → #18 GET /v1/components/{key}              — 获取组件详细信息
```

#### workflow:setup-webhook（配置 Webhook 监听）

```
步骤 1 → #26 POST /v2/webhooks              — 创建 Webhook
步骤 2 → #31 GET /v2/webhooks/{id}/requests  — 查看 Webhook 请求日志（调试用）
```

#### workflow:manage-variables（管理设计变量）

```
步骤 1 → #33 GET /v1/files/{file_key}/variables/local    — 查看当前文件变量
步骤 2 → #35 POST /v1/files/{file_key}/variables          — 创建/修改/删除变量
```

## §6 Shell 格式模板

### §6.1 bash 与 PowerShell 基础结构对比

| 要素 | bash | PowerShell |
|------|------|------------|
| Token 获取 | `$(bash '<SCRIPT_PATH>/get-token.sh')` | `$token = & "<SCRIPT_PATH>\get-token.ps1"` |
| HTTP 客户端 | `curl` | `Invoke-RestMethod` (alias `irm`) |
| Header 传递 | `-H "Key: Value"` | `-Headers @{"Key"="Value"}` |
| JSON Body | `-d '{"key":"value"}'` | `-Body '{"key":"value"}' -ContentType 'application/json'` |
| 管道解析 | `\| jq '.field'` | `.field`（直接属性访问） |
| 续行符 | `\` | `` ` `` |

### §6.2 完整请求模板

**GET（bash）：**

```bash
curl -s "https://api.figma.com/v1/{{PATH}}" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

**GET（PowerShell）：**

```powershell
$token = & "<SCRIPT_PATH>\get-token.ps1"
irm "https://api.figma.com/v1/{{PATH}}" `
  -Headers @{"Authorization"="Bearer $token"}
```

**POST（bash）：**

```bash
curl -s -X POST "https://api.figma.com/v1/{{PATH}}" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')" \
  -H "Content-Type: application/json" \
  -d '{{BODY}}'
```

**POST（PowerShell）：**

```powershell
$token = & "<SCRIPT_PATH>\get-token.ps1"
irm "https://api.figma.com/v1/{{PATH}}" `
  -Method Post `
  -Headers @{"Authorization"="Bearer $token"} `
  -Body '{{BODY}}' `
  -ContentType 'application/json'
```

**PUT（bash）：**

```bash
curl -s -X PUT "https://api.figma.com/v2/{{PATH}}" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')" \
  -H "Content-Type: application/json" \
  -d '{{BODY}}'
```

**PUT（PowerShell）：**

```powershell
$token = & "<SCRIPT_PATH>\get-token.ps1"
irm "https://api.figma.com/v2/{{PATH}}" `
  -Method Put `
  -Headers @{"Authorization"="Bearer $token"} `
  -Body '{{BODY}}' `
  -ContentType 'application/json'
```

**DELETE（bash）：**

```bash
curl -s -X DELETE "https://api.figma.com/v1/{{PATH}}" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

**DELETE（PowerShell）：**

```powershell
$token = & "<SCRIPT_PATH>\get-token.ps1"
irm "https://api.figma.com/v1/{{PATH}}" `
  -Method Delete `
  -Headers @{"Authorization"="Bearer $token"}
```

### §6.3 从 bash 到 PowerShell 转换规则摘要

| bash | PowerShell | 说明 |
|------|------------|------|
| `curl -s` | `irm` | 静默请求 |
| `-X POST` | `-Method Post` | 指定方法 |
| `-H "K: V"` | `-Headers @{"K"="V"}` | 设置 Header |
| `-d '...'` | `-Body '...' -ContentType 'application/json'` | 发送 JSON Body |
| `$(bash '...')` | `$token = & "..."` 后引用 `$token` | Token 获取 |
| `\` (续行) | `` ` `` (续行) | 多行命令 |
| `\| jq '.x'` | `.x` | JSON 字段提取 |

## §7 接口详情

> 以下为全部 45 个接口的完整参数表和 bash curl 示例。PowerShell 转换规则见 §6.3。

### Files（#1-#6）

#### 1. 获取文件 JSON

`GET /v1/files/{file_key}`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 文件 key 或分支 key |
| version | query | — | 指定版本 ID，不传则获取当前版本 |
| ids | query | — | 逗号分隔的节点 ID 列表，只返回这些节点及其上下游 |
| depth | query | — | 正整数，文档树遍历深度（1=仅 Pages，2=Pages+顶层对象） |
| geometry | query | — | 设为 "paths" 导出矢量数据 |
| plugin_data | query | — | 逗号分隔的插件 ID 列表和/或 "shared" |
| branch_data | query | — | 是否返回分支元数据（默认 false） |

```bash
curl -s "https://api.figma.com/v1/files/FILE_KEY" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 2. 获取指定节点 JSON

`GET /v1/files/{file_key}/nodes`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 文件 key 或分支 key |
| ids | query | ✅ | 逗号分隔的节点 ID 列表 |
| version | query | — | 指定版本 ID |
| depth | query | — | 节点树遍历深度（从目标节点开始计数，与 getFile 不同） |
| geometry | query | — | 设为 "paths" 导出矢量数据 |
| plugin_data | query | — | 逗号分隔的插件 ID 列表和/或 "shared" |

```bash
curl -s "https://api.figma.com/v1/files/FILE_KEY/nodes?ids=1:2,1:3" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 3. 导出渲染图片

`GET /v1/images/{file_key}`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 文件 key 或分支 key |
| ids | query | ✅ | 逗号分隔的节点 ID 列表 |
| version | query | — | 指定版本 ID |
| scale | query | — | 缩放因子（0.01 ~ 4） |
| format | query | — | 输出格式：jpg / png（默认）/ svg / pdf |
| svg_outline_text | query | — | SVG 中文本是否转为路径轮廓（默认 true） |
| svg_include_id | query | — | SVG 元素是否包含 id 属性（默认 false） |
| svg_include_node_id | query | — | SVG 元素是否包含 data-node-id 属性（默认 false） |
| svg_simplify_stroke | query | — | 是否简化描边（默认 true） |
| contents_only | query | — | 是否排除重叠内容（默认 true） |
| use_absolute_bounds | query | — | 是否使用完整节点尺寸（默认 false） |

```bash
curl -s "https://api.figma.com/v1/images/FILE_KEY?ids=1:2,1:3&format=svg&scale=2" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 4. 获取图片填充

`GET /v1/files/{file_key}/images`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 文件 key 或分支 key |

```bash
curl -s "https://api.figma.com/v1/files/FILE_KEY/images" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 5. 获取文件元数据

`GET /v1/files/{file_key}/meta`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 文件 key 或分支 key |

```bash
curl -s "https://api.figma.com/v1/files/FILE_KEY/meta" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 6. 获取版本历史

`GET /v1/files/{file_key}/versions`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 文件 key 或分支 key |
| page_size | query | — | 每页数量（默认 30，最大 50） |
| before | query | — | 在此版本 ID 之前的版本（向前分页） |
| after | query | — | 在此版本 ID 之后的版本（向后分页） |

```bash
curl -s "https://api.figma.com/v1/files/FILE_KEY/versions?page_size=10" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

---

### Comments（#7-#9）

#### 7. 获取文件评论

`GET /v1/files/{file_key}/comments`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 文件 key 或分支 key |
| as_md | query | — | 是否以 Markdown 格式返回评论 |

```bash
curl -s "https://api.figma.com/v1/files/FILE_KEY/comments" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 8. 添加评论

`POST /v1/files/{file_key}/comments`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 文件 key 或分支 key |
| message | body | ✅ | 评论文本内容 |
| comment_id | body | — | 要回复的根评论 ID（不能回复回复） |
| client_meta | body | — | 评论位置（Vector / FrameOffset / Region / FrameOffsetRegion） |

```bash
curl -s -X POST "https://api.figma.com/v1/files/FILE_KEY/comments" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')" \
  -H "Content-Type: application/json" \
  -d '{"message": "评论内容"}'
```

#### 9. 删除评论 ⚠️ DESTRUCTIVE

`DELETE /v1/files/{file_key}/comments/{comment_id}`

> ⚠️ DESTRUCTIVE — 永久删除评论，仅评论创建者可删除。执行前必须向用户确认。

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 文件 key 或分支 key |
| comment_id | path | ✅ | 要删除的评论 ID |

```bash
curl -s -X DELETE "https://api.figma.com/v1/files/FILE_KEY/comments/COMMENT_ID" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

---

### Comment Reactions（#10-#12）

#### 10. 获取评论反应

`GET /v1/files/{file_key}/comments/{comment_id}/reactions`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 文件 key 或分支 key |
| comment_id | path | ✅ | 评论 ID |
| cursor | query | — | 分页游标 |

```bash
curl -s "https://api.figma.com/v1/files/FILE_KEY/comments/COMMENT_ID/reactions" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 11. 添加评论反应

`POST /v1/files/{file_key}/comments/{comment_id}/reactions`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 文件 key 或分支 key |
| comment_id | path | ✅ | 评论 ID |
| emoji | body | ✅ | Emoji shortcode（如 :heart:, :+1::skin-tone-2:） |

```bash
curl -s -X POST "https://api.figma.com/v1/files/FILE_KEY/comments/COMMENT_ID/reactions" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')" \
  -H "Content-Type: application/json" \
  -d '{"emoji": ":+1:"}'
```

#### 12. 删除评论反应 ⚠️ DESTRUCTIVE

`DELETE /v1/files/{file_key}/comments/{comment_id}/reactions`

> ⚠️ DESTRUCTIVE — 永久删除反应，仅反应创建者可删除。执行前必须向用户确认。

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 文件 key 或分支 key |
| comment_id | path | ✅ | 评论 ID |
| emoji | query | ✅ | 要删除的 Emoji shortcode |

```bash
curl -s -X DELETE "https://api.figma.com/v1/files/FILE_KEY/comments/COMMENT_ID/reactions?emoji=%3A%2B1%3A" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

---

### Projects（#13-#14）

#### 13. 获取团队项目列表

`GET /v1/teams/{team_id}/projects`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| team_id | path | ✅ | 团队 ID |

```bash
curl -s "https://api.figma.com/v1/teams/TEAM_ID/projects" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 14. 获取项目文件列表

`GET /v1/projects/{project_id}/files`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| project_id | path | ✅ | 项目 ID |
| branch_data | query | — | 是否返回分支元数据（默认 false） |

```bash
curl -s "https://api.figma.com/v1/projects/PROJECT_ID/files" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

---

### Users（#15）

#### 15. 获取当前用户信息

`GET /v1/me`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| — | — | — | 无参数 |

```bash
curl -s "https://api.figma.com/v1/me" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

---

### Components（#16-#18）

#### 16. 获取团队组件

`GET /v1/teams/{team_id}/components`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| team_id | path | ✅ | 团队 ID |
| page_size | query | — | 每页数量（默认 30，最大 1000） |
| after | query | — | 在此游标之后开始（与 before 互斥） |
| before | query | — | 在此游标之前开始（与 after 互斥） |

```bash
curl -s "https://api.figma.com/v1/teams/TEAM_ID/components?page_size=30" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 17. 获取文件组件

`GET /v1/files/{file_key}/components`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 文件 key（必须是主文件 key，不能是分支 key） |

```bash
curl -s "https://api.figma.com/v1/files/FILE_KEY/components" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 18. 按 key 获取组件

`GET /v1/components/{key}`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| key | path | ✅ | 组件唯一标识符 |

```bash
curl -s "https://api.figma.com/v1/components/COMPONENT_KEY" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

---

### Component Sets（#19-#21）

#### 19. 获取团队组件集

`GET /v1/teams/{team_id}/component_sets`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| team_id | path | ✅ | 团队 ID |
| page_size | query | — | 每页数量（默认 30） |
| after | query | — | 在此游标之后开始（与 before 互斥） |
| before | query | — | 在此游标之前开始（与 after 互斥） |

```bash
curl -s "https://api.figma.com/v1/teams/TEAM_ID/component_sets?page_size=30" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 20. 获取文件组件集

`GET /v1/files/{file_key}/component_sets`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 文件 key（必须是主文件 key） |

```bash
curl -s "https://api.figma.com/v1/files/FILE_KEY/component_sets" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 21. 按 key 获取组件集

`GET /v1/component_sets/{key}`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| key | path | ✅ | 组件集唯一标识符 |

```bash
curl -s "https://api.figma.com/v1/component_sets/COMPONENT_KEY" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

---

### Styles（#22-#24）

#### 22. 获取团队样式

`GET /v1/teams/{team_id}/styles`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| team_id | path | ✅ | 团队 ID |
| page_size | query | — | 每页数量（默认 30） |
| after | query | — | 在此游标之后开始（与 before 互斥） |
| before | query | — | 在此游标之前开始（与 after 互斥） |

```bash
curl -s "https://api.figma.com/v1/teams/TEAM_ID/styles?page_size=30" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 23. 获取文件样式

`GET /v1/files/{file_key}/styles`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 文件 key（必须是主文件 key） |

```bash
curl -s "https://api.figma.com/v1/files/FILE_KEY/styles" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 24. 按 key 获取样式

`GET /v1/styles/{key}`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| key | path | ✅ | 样式唯一标识符 |

```bash
curl -s "https://api.figma.com/v1/styles/STYLE_KEY" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

---

### Webhooks（#25-#31）

#### 25. 按条件查询 Webhook

`GET /v2/webhooks`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| context | query | — | 上下文类型：team / project / file |
| context_id | query | — | 上下文 ID（与 plan_api_id 互斥） |
| plan_api_id | query | — | 计划 ID，获取所有上下文的 Webhook（与 context/context_id 互斥），结果分页 |
| cursor | query | — | 分页游标（仅 plan_api_id 模式下有效） |

```bash
curl -s "https://api.figma.com/v2/webhooks?context=team&context_id=TEAM_ID" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 26. 创建 Webhook

`POST /v2/webhooks`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| event_type | body | ✅ | 事件类型（PING/FILE_UPDATE/FILE_VERSION_UPDATE/FILE_DELETE/LIBRARY_PUBLISH/FILE_COMMENT/DEV_MODE_STATUS_UPDATE） |
| context | body | ✅ | 上下文类型：team / project / file |
| context_id | body | ✅ | 上下文 ID |
| endpoint | body | ✅ | 接收 POST 请求的 URL（最长 2048 字符） |
| passcode | body | ✅ | 验证 passcode（最长 100 字符） |
| team_id | body | — | **已弃用**，使用 context + context_id 代替 |
| status | body | — | 状态：ACTIVE（默认）/ PAUSED |
| description | body | — | 描述（最长 150 字符） |

```bash
curl -s -X POST "https://api.figma.com/v2/webhooks" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')" \
  -H "Content-Type: application/json" \
  -d '{"event_type": "FILE_UPDATE", "context": "team", "context_id": "TEAM_ID", "endpoint": "https://example.com/webhook", "passcode": "your-passcode"}'
```

#### 27. 按 ID 获取 Webhook

`GET /v2/webhooks/{webhook_id}`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| webhook_id | path | ✅ | Webhook ID |

```bash
curl -s "https://api.figma.com/v2/webhooks/WEBHOOK_ID" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 28. 更新 Webhook

`PUT /v2/webhooks/{webhook_id}`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| webhook_id | path | ✅ | Webhook ID |
| event_type | body | ✅ | 事件类型 |
| endpoint | body | ✅ | 接收 POST 请求的 URL（最长 2048 字符） |
| passcode | body | ✅ | 验证 passcode（最长 100 字符） |
| status | body | — | 状态：ACTIVE / PAUSED |
| description | body | — | 描述（最长 150 字符） |

```bash
curl -s -X PUT "https://api.figma.com/v2/webhooks/WEBHOOK_ID" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')" \
  -H "Content-Type: application/json" \
  -d '{"event_type": "FILE_UPDATE", "endpoint": "https://example.com/webhook", "passcode": "your-passcode"}'
```

#### 29. 删除 Webhook ⚠️ DESTRUCTIVE

`DELETE /v2/webhooks/{webhook_id}`

> ⚠️ DESTRUCTIVE — 永久删除 Webhook，不可恢复。执行前必须向用户确认。

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| webhook_id | path | ✅ | Webhook ID |

```bash
curl -s -X DELETE "https://api.figma.com/v2/webhooks/WEBHOOK_ID" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 30. 获取团队 Webhook [已弃用]

`GET /v2/teams/{team_id}/webhooks`

> ⚠️ 已弃用 — 请使用 #25 getWebhooks 的 context=team&context_id={team_id} 代替。

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| team_id | path | ✅ | 团队 ID |

```bash
curl -s "https://api.figma.com/v2/teams/TEAM_ID/webhooks" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 31. 查看 Webhook 请求日志

`GET /v2/webhooks/{webhook_id}/requests`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| webhook_id | path | ✅ | Webhook ID |

```bash
curl -s "https://api.figma.com/v2/webhooks/WEBHOOK_ID/requests" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

---

### Activity Logs（#32）

> **认证要求：** 需要 OrgOAuth2（scope: `org:activity_log_read`）。

#### 32. 获取活动日志

`GET /v1/activity_logs`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| events | query | — | 事件类型过滤（逗号分隔），默认返回全部 |
| start_time | query | — | 最早事件的 Unix 时间戳（默认一年前） |
| end_time | query | — | 最近事件的 Unix 时间戳（默认当前时间） |
| limit | query | — | 最大返回数量（默认 1000） |
| order | query | — | 排序：asc（默认）/ desc |

```bash
curl -s "https://api.figma.com/v1/activity_logs?limit=100&order=desc" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

---

### Variables（#33-#35）

> Enterprise 专属 API — 需要 Enterprise 计划才能访问 Variables 相关接口。

#### 33. 获取本地变量

`GET /v1/files/{file_key}/variables/local`

> Enterprise 专属 API

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 文件 key 或分支 key |

```bash
curl -s "https://api.figma.com/v1/files/FILE_KEY/variables/local" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 34. 获取已发布变量

`GET /v1/files/{file_key}/variables/published`

> Enterprise 专属 API

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 文件 key（必须是主文件 key，不能是分支 key） |

```bash
curl -s "https://api.figma.com/v1/files/FILE_KEY/variables/published" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 35. 创建/修改/删除变量 ⚠️ DESTRUCTIVE

`POST /v1/files/{file_key}/variables`

> ⚠️ DESTRUCTIVE — 请求体可包含删除操作（action: "DELETE"），所有变更为原子性。执行前必须向用户确认。

> Enterprise 专属 API，需要 Editor 席位。请求体最大 4MB。4 个数组按顺序执行：variableCollections → variableModes → variables → variableModeValues。

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 文件 key 或分支 key |
| variableCollections | body | — | 创建/更新/删除变量集合 |
| variableModes | body | — | 创建/更新/删除模式（每集合最多 40 个，名称最长 40 字符） |
| variables | body | — | 创建/更新/删除变量（每集合最多 5000 个） |
| variableModeValues | body | — | 设置特定模式下的变量值 |

```bash
curl -s -X POST "https://api.figma.com/v1/files/FILE_KEY/variables" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')" \
  -H "Content-Type: application/json" \
  -d '{"variableCollections": [{"action": "CREATE", "id": "temp_collection_1", "name": "Colors", "initialModeId": "temp_mode_1"}], "variables": [{"action": "CREATE", "id": "temp_var_1", "name": "primary", "variableCollectionId": "temp_collection_1", "resolvedType": "COLOR"}]}'
```

---

### Dev Resources（#36-#39）

#### 36. 获取开发资源

`GET /v1/files/{file_key}/dev_resources`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 文件 key（必须是主文件 key） |
| node_ids | query | — | 逗号分隔的节点 ID 列表，过滤特定节点的开发资源 |

```bash
curl -s "https://api.figma.com/v1/files/FILE_KEY/dev_resources?node_ids=1:2,1:3" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 37. 创建开发资源

`POST /v1/dev_resources`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| dev_resources | body | ✅ | 开发资源数组 |
| dev_resources[].name | body | ✅ | 资源名称 |
| dev_resources[].url | body | ✅ | 资源 URL |
| dev_resources[].file_key | body | ✅ | 所属文件 key |
| dev_resources[].node_id | body | ✅ | 附加到的节点 ID |

```bash
curl -s -X POST "https://api.figma.com/v1/dev_resources" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')" \
  -H "Content-Type: application/json" \
  -d '{"dev_resources": [{"name": "[ENTERPRISE_NAME] Issue", "url": "https://[ENTERPRISE_NAME].com/org/repo/issues/1", "file_key": "FILE_KEY", "node_id": "1:2"}]}'
```

#### 38. 更新开发资源

`PUT /v1/dev_resources`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| dev_resources | body | ✅ | 开发资源数组 |
| dev_resources[].id | body | ✅ | 资源唯一 ID |
| dev_resources[].name | body | — | 新名称 |
| dev_resources[].url | body | — | 新 URL |

```bash
curl -s -X PUT "https://api.figma.com/v1/dev_resources" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')" \
  -H "Content-Type: application/json" \
  -d '{"dev_resources": [{"id": "DEV_RESOURCE_ID", "name": "Updated Name", "url": "https://[ENTERPRISE_NAME].com/org/repo/issues/2"}]}'
```

#### 39. 删除开发资源 ⚠️ DESTRUCTIVE

`DELETE /v1/files/{file_key}/dev_resources/{dev_resource_id}`

> ⚠️ DESTRUCTIVE — 永久删除开发资源。执行前必须向用户确认。

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 文件 key（必须是主文件 key） |
| dev_resource_id | path | ✅ | 开发资源 ID |

```bash
curl -s -X DELETE "https://api.figma.com/v1/files/FILE_KEY/dev_resources/DEV_RESOURCE_ID" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

---

### Library Analytics（#40-#45）

#### 40. 组件操作分析

`GET /v1/analytics/libraries/{file_key}/component/actions`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 库文件 key |
| group_by | query | ✅ | 分组维度：component / team |
| cursor | query | — | 分页游标 |
| start_date | query | — | ISO 8601 日期（YYYY-MM-DD），默认一年前 |
| end_date | query | — | ISO 8601 日期（YYYY-MM-DD），默认最近计算周 |

```bash
curl -s "https://api.figma.com/v1/analytics/libraries/FILE_KEY/component/actions?group_by=component" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 41. 组件使用分析

`GET /v1/analytics/libraries/{file_key}/component/usages`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 库文件 key |
| group_by | query | ✅ | 分组维度：component / file |
| cursor | query | — | 分页游标 |

```bash
curl -s "https://api.figma.com/v1/analytics/libraries/FILE_KEY/component/usages?group_by=component" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 42. 样式操作分析

`GET /v1/analytics/libraries/{file_key}/style/actions`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 库文件 key |
| group_by | query | ✅ | 分组维度：style / team |
| cursor | query | — | 分页游标 |
| start_date | query | — | ISO 8601 日期（YYYY-MM-DD） |
| end_date | query | — | ISO 8601 日期（YYYY-MM-DD） |

```bash
curl -s "https://api.figma.com/v1/analytics/libraries/FILE_KEY/style/actions?group_by=style&start_date=2025-01-01&end_date=2025-12-31" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 43. 样式使用分析

`GET /v1/analytics/libraries/{file_key}/style/usages`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 库文件 key |
| group_by | query | ✅ | 分组维度：style / file |
| cursor | query | — | 分页游标 |

```bash
curl -s "https://api.figma.com/v1/analytics/libraries/FILE_KEY/style/usages?group_by=style" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 44. 变量操作分析

`GET /v1/analytics/libraries/{file_key}/variable/actions`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 库文件 key |
| group_by | query | ✅ | 分组维度：variable / team |
| cursor | query | — | 分页游标 |
| start_date | query | — | ISO 8601 日期（YYYY-MM-DD） |
| end_date | query | — | ISO 8601 日期（YYYY-MM-DD） |

```bash
curl -s "https://api.figma.com/v1/analytics/libraries/FILE_KEY/variable/actions?group_by=variable" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

#### 45. 变量使用分析

`GET /v1/analytics/libraries/{file_key}/variable/usages`

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| file_key | path | ✅ | 库文件 key |
| group_by | query | ✅ | 分组维度：variable / file |
| cursor | query | — | 分页游标 |

```bash
curl -s "https://api.figma.com/v1/analytics/libraries/FILE_KEY/variable/usages?group_by=variable" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')"
```

## §8 运维契约

### §8.1 HTTP 错误→AI 行为映射

| 状态码 | 含义 | AI 行为 |
|--------|------|---------|
| 400 | Bad Request — 请求参数错误 | 检查请求体和查询参数，修正后重试 |
| 401 | Unauthorized — Token 无效或过期 | 重新获取 Token（执行 get-token.sh），重试一次 |
| 403 | Forbidden — 权限不足 | 告知用户当前 Token 缺少所需权限/scope，不重试 |
| 404 | Not Found — 资源不存在 | 确认 file_key/ID 是否正确，告知用户资源未找到 |
| 429 | Too Many Requests — 触发限流 | 执行 §8.3 限流处理算法 |
| 500 | Internal Server Error — 服务端错误 | 等待 30 秒后重试一次，仍失败则告知用户 |
| 503 | Service Unavailable — 服务不可用 | 等待 60 秒后重试一次，仍失败则告知用户稍后再试 |

### §8.2 分页契约算法

```
算法：Figma cursor 分页
─────────────────────────
1. 发起首次请求（不带 cursor 参数）
2. 检查响应中是否包含分页游标：
   - Components/Styles 列表: 检查 meta.cursor 字段
   - Webhooks/Library Analytics: 检查 next_page 字段
3. 若存在游标/next_page:
   - Components/Styles: 将 meta.cursor 值作为 after 参数发起下一页请求
   - Webhooks/Analytics: 将 next_page 值作为 cursor 参数发起下一页请求
4. 重复步骤 2-3 直到游标为空或 next_page 不存在
5. 合并全部页的结果
```

### §8.3 限流处理算法

```
算法：指数退避 + retry-after
────────────────────────────
1. 收到 429 响应
2. 读取 retry-after Header（秒数）
   - 若存在: 等待 retry-after 秒
   - 若不存在: 等待 base_delay = 2^attempt 秒（attempt 从 0 开始）
3. 重试请求
4. 若再次 429，attempt += 1，重复步骤 2（最多重试 3 次）
5. 超过 3 次仍 429，告知用户 API 限流，建议稍后再试
```

### §8.4 通用约定

| 约定 | 值 |
|------|-----|
| Base URL | `https://api.figma.com` |
| Token 传递 | `Authorization: Bearer <token>` Header |
| API 版本 | 路径版本化（`/v1/`、`/v2/`），无版本 Header |
| Content-Type | POST/PUT 请求体：`application/json` |
| ID 格式 | file_key 为字符串；team_id、project_id 为数字字符串 |
| 认证方式 | 全部 45 个接口使用 OAuth2 Bearer Token 认证；#32 Activity Logs 需要 OrgOAuth2（scope: `org:activity_log_read`） |


---


## Skill: docx

---
name: docx
description: "Use this skill whenever the user wants to create, read, edit, or manipulate Word documents (.docx files). Triggers include: any mention of \"Word doc\", \"word document\", \".docx\", or requests to produce professional documents with formatting like tables of contents, headings, page numbers, or letterheads. Also use when extracting or reorganizing content from .docx files, inserting or replacing images in documents, performing find-and-replace in Word files, working with tracked changes or comments, or converting content into a polished Word document. If the user asks for a \"report\", \"memo\", \"letter\", \"template\", or similar deliverable as a Word or .docx file, use this skill. Do NOT use for PDFs, spreadsheets, Google Docs, or general coding tasks unrelated to document generation."
license: Proprietary. LICENSE.txt has complete terms
---

# DOCX creation, editing, and analysis

## Overview

A .docx file is a ZIP archive containing XML files.

## Quick Reference

| Task | Approach |
|------|----------|
| Read/analyze content | `pandoc` or unpack for raw XML |
| Create new document | Use `docx-js` - see Creating New Documents below |
| Edit existing document | Unpack → edit XML → repack - see Editing Existing Documents below |

### Converting .doc to .docx

Legacy `.doc` files must be converted before editing:

```bash
python scripts/office/soffice.py --headless --convert-to docx document.doc
```

**Chinese content note**: `.doc` files created on Chinese Windows are often GBK-encoded internally. `scripts/office/soffice.py` automatically sets `LANG=zh_CN.UTF-8` / `LC_ALL=zh_CN.UTF-8` when the environment lacks a UTF-8 locale, so LibreOffice can decode CJK characters correctly during conversion. If Chinese text still appears garbled after conversion, verify that the `zh_CN.UTF-8` locale is installed on the system:

```bash
# Linux: install the locale if missing
locale -a | grep zh_CN          # check availability
sudo locale-gen zh_CN.UTF-8     # install if absent (Debian/Ubuntu)
```

macOS always has full Unicode locale support, so no additional setup is needed there.

### Reading Content

```bash
# Text extraction with tracked changes
pandoc --track-changes=all document.docx -o output.md

# Raw XML access
python scripts/office/unpack.py document.docx unpacked/
```

### Converting to Images

```bash
python scripts/office/soffice.py --headless --convert-to pdf document.docx
pdftoppm -jpeg -r 150 document.pdf page
```

### Accepting Tracked Changes

To produce a clean document with all tracked changes accepted (requires LibreOffice):

```bash
python scripts/accept_changes.py input.docx output.docx
```

---

## Creating New Documents

Generate .docx files with JavaScript, then validate. Install: `npm install -g docx`

### Setup
```javascript
const { Document, Packer, Paragraph, TextRun, Table, TableRow, TableCell, ImageRun,
        Header, Footer, AlignmentType, PageOrientation, LevelFormat, ExternalHyperlink,
        TableOfContents, HeadingLevel, BorderStyle, WidthType, ShadingType,
        VerticalAlign, PageNumber, PageBreak } = require('docx');

const doc = new Document({ sections: [{ children: [/* content */] }] });
Packer.toBuffer(doc).then(buffer => fs.writeFileSync("doc.docx", buffer));
```

### Validation
After creating the file, validate it. If validation fails, unpack, fix the XML, and repack.
```bash
python scripts/office/validate.py doc.docx
```

### Page Size

```javascript
// CRITICAL: docx-js defaults to A4, not US Letter
// Always set page size explicitly for consistent results
sections: [{
  properties: {
    page: {
      size: {
        width: 12240,   // 8.5 inches in DXA
        height: 15840   // 11 inches in DXA
      },
      margin: { top: 1440, right: 1440, bottom: 1440, left: 1440 } // 1 inch margins
    }
  },
  children: [/* content */]
}]
```

**Common page sizes (DXA units, 1440 DXA = 1 inch):**

| Paper | Width | Height | Content Width (1" margins) |
|-------|-------|--------|---------------------------|
| US Letter | 12,240 | 15,840 | 9,360 |
| A4 (default) | 11,906 | 16,838 | 9,026 |

**Landscape orientation:** docx-js swaps width/height internally, so pass portrait dimensions and let it handle the swap:
```javascript
size: {
  width: 12240,   // Pass SHORT edge as width
  height: 15840,  // Pass LONG edge as height
  orientation: PageOrientation.LANDSCAPE  // docx-js swaps them in the XML
},
// Content width = 15840 - left margin - right margin (uses the long edge)
```

### Styles (Override Built-in Headings)

Use Arial as the default font (universally supported). Keep titles black for readability.

```javascript
const doc = new Document({
  styles: {
    default: { document: { run: { font: "Arial", size: 24 } } }, // 12pt default
    paragraphStyles: [
      // IMPORTANT: Use exact IDs to override built-in styles
      { id: "Heading1", name: "Heading 1", basedOn: "Normal", next: "Normal", quickFormat: true,
        run: { size: 32, bold: true, font: "Arial" },
        paragraph: { spacing: { before: 240, after: 240 }, outlineLevel: 0 } }, // outlineLevel required for TOC
      { id: "Heading2", name: "Heading 2", basedOn: "Normal", next: "Normal", quickFormat: true,
        run: { size: 28, bold: true, font: "Arial" },
        paragraph: { spacing: { before: 180, after: 180 }, outlineLevel: 1 } },
    ]
  },
  sections: [{
    children: [
      new Paragraph({ heading: HeadingLevel.HEADING_1, children: [new TextRun("Title")] }),
    ]
  }]
});
```

### Lists (NEVER use unicode bullets)

```javascript
// ❌ WRONG - never manually insert bullet characters
new Paragraph({ children: [new TextRun("• Item")] })  // BAD
new Paragraph({ children: [new TextRun("\u2022 Item")] })  // BAD

// ✅ CORRECT - use numbering config with LevelFormat.BULLET
const doc = new Document({
  numbering: {
    config: [
      { reference: "bullets",
        levels: [{ level: 0, format: LevelFormat.BULLET, text: "•", alignment: AlignmentType.LEFT,
          style: { paragraph: { indent: { left: 720, hanging: 360 } } } }] },
      { reference: "numbers",
        levels: [{ level: 0, format: LevelFormat.DECIMAL, text: "%1.", alignment: AlignmentType.LEFT,
          style: { paragraph: { indent: { left: 720, hanging: 360 } } } }] },
    ]
  },
  sections: [{
    children: [
      new Paragraph({ numbering: { reference: "bullets", level: 0 },
        children: [new TextRun("Bullet item")] }),
      new Paragraph({ numbering: { reference: "numbers", level: 0 },
        children: [new TextRun("Numbered item")] }),
    ]
  }]
});

// ⚠️ Each reference creates INDEPENDENT numbering
// Same reference = continues (1,2,3 then 4,5,6)
// Different reference = restarts (1,2,3 then 1,2,3)
```

### Tables

**CRITICAL: Tables need dual widths** - set both `columnWidths` on the table AND `width` on each cell. Without both, tables render incorrectly on some platforms.

```javascript
// CRITICAL: Always set table width for consistent rendering
// CRITICAL: Use ShadingType.CLEAR (not SOLID) to prevent black backgrounds
const border = { style: BorderStyle.SINGLE, size: 1, color: "CCCCCC" };
const borders = { top: border, bottom: border, left: border, right: border };

new Table({
  width: { size: 9360, type: WidthType.DXA }, // Always use DXA (percentages break in Google Docs)
  columnWidths: [4680, 4680], // Must sum to table width (DXA: 1440 = 1 inch)
  rows: [
    new TableRow({
      children: [
        new TableCell({
          borders,
          width: { size: 4680, type: WidthType.DXA }, // Also set on each cell
          shading: { fill: "D5E8F0", type: ShadingType.CLEAR }, // CLEAR not SOLID
          margins: { top: 80, bottom: 80, left: 120, right: 120 }, // Cell padding (internal, not added to width)
          children: [new Paragraph({ children: [new TextRun("Cell")] })]
        })
      ]
    })
  ]
})
```

**Table width calculation:**

Always use `WidthType.DXA` — `WidthType.PERCENTAGE` breaks in Google Docs.

```javascript
// Table width = sum of columnWidths = content width
// US Letter with 1" margins: 12240 - 2880 = 9360 DXA
width: { size: 9360, type: WidthType.DXA },
columnWidths: [7000, 2360]  // Must sum to table width
```

**Width rules:**
- **Always use `WidthType.DXA`** — never `WidthType.PERCENTAGE` (incompatible with Google Docs)
- Table width must equal the sum of `columnWidths`
- Cell `width` must match corresponding `columnWidth`
- Cell `margins` are internal padding - they reduce content area, not add to cell width
- For full-width tables: use content width (page width minus left and right margins)

### Images

```javascript
// CRITICAL: type parameter is REQUIRED
new Paragraph({
  children: [new ImageRun({
    type: "png", // Required: png, jpg, jpeg, gif, bmp, svg
    data: fs.readFileSync("image.png"),
    transformation: { width: 200, height: 150 },
    altText: { title: "Title", description: "Desc", name: "Name" } // All three required
  })]
})
```

### Page Breaks

```javascript
// CRITICAL: PageBreak must be inside a Paragraph
new Paragraph({ children: [new PageBreak()] })

// Or use pageBreakBefore
new Paragraph({ pageBreakBefore: true, children: [new TextRun("New page")] })
```

### Table of Contents

```javascript
// CRITICAL: Headings must use HeadingLevel ONLY - no custom styles
new TableOfContents("Table of Contents", { hyperlink: true, headingStyleRange: "1-3" })
```

### Headers/Footers

```javascript
sections: [{
  properties: {
    page: { margin: { top: 1440, right: 1440, bottom: 1440, left: 1440 } } // 1440 = 1 inch
  },
  headers: {
    default: new Header({ children: [new Paragraph({ children: [new TextRun("Header")] })] })
  },
  footers: {
    default: new Footer({ children: [new Paragraph({
      children: [new TextRun("Page "), new TextRun({ children: [PageNumber.CURRENT] })]
    })] })
  },
  children: [/* content */]
}]
```

### Critical Rules for docx-js

- **Set page size explicitly** - docx-js defaults to A4; use US Letter (12240 x 15840 DXA) for US documents
- **Landscape: pass portrait dimensions** - docx-js swaps width/height internally; pass short edge as `width`, long edge as `height`, and set `orientation: PageOrientation.LANDSCAPE`
- **Never use `\n`** - use separate Paragraph elements
- **Never use unicode bullets** - use `LevelFormat.BULLET` with numbering config
- **PageBreak must be in Paragraph** - standalone creates invalid XML
- **ImageRun requires `type`** - always specify png/jpg/etc
- **Always set table `width` with DXA** - never use `WidthType.PERCENTAGE` (breaks in Google Docs)
- **Tables need dual widths** - `columnWidths` array AND cell `width`, both must match
- **Table width = sum of columnWidths** - for DXA, ensure they add up exactly
- **Always add cell margins** - use `margins: { top: 80, bottom: 80, left: 120, right: 120 }` for readable padding
- **Use `ShadingType.CLEAR`** - never SOLID for table shading
- **TOC requires HeadingLevel only** - no custom styles on heading paragraphs
- **Override built-in styles** - use exact IDs: "Heading1", "Heading2", etc.
- **Include `outlineLevel`** - required for TOC (0 for H1, 1 for H2, etc.)

---

## Editing Existing Documents

**Follow all 3 steps in order.**

### Step 1: Unpack
```bash
python scripts/office/unpack.py document.docx unpacked/
```
Extracts XML, pretty-prints, merges adjacent runs, and converts smart quotes to XML entities (`&#x201C;` etc.) so they survive editing. Use `--merge-runs false` to skip run merging.

### Step 2: Edit XML

Edit files in `unpacked/word/`. See XML Reference below for patterns.

**Use "Claude" as the author** for tracked changes and comments, unless the user explicitly requests use of a different name.

**Use the Edit tool directly for string replacement. Do not write Python scripts.** Scripts introduce unnecessary complexity. The Edit tool shows exactly what is being replaced.

**CRITICAL: Use smart quotes for new content.** When adding text with apostrophes or quotes, use XML entities to produce smart quotes:
```xml
<!-- Use these entities for professional typography -->
<w:t>Here&#x2019;s a quote: &#x201C;Hello&#x201D;</w:t>
```
| Entity | Character |
|--------|-----------|
| `&#x2018;` | ‘ (left single) |
| `&#x2019;` | ’ (right single / apostrophe) |
| `&#x201C;` | “ (left double) |
| `&#x201D;` | ” (right double) |

**Adding comments:** Use `comment.py` to handle boilerplate across multiple XML files (text must be pre-escaped XML):
```bash
python scripts/comment.py unpacked/ 0 "Comment text with &amp; and &#x2019;"
python scripts/comment.py unpacked/ 1 "Reply text" --parent 0  # reply to comment 0
python scripts/comment.py unpacked/ 0 "Text" --author "Custom Author"  # custom author name
```
Then add markers to document.xml (see Comments in XML Reference).

### Step 3: Pack
```bash
python scripts/office/pack.py unpacked/ output.docx --original document.docx
```
Validates with auto-repair, condenses XML, and creates DOCX. Use `--validate false` to skip.

**Auto-repair will fix:**
- `durableId` >= 0x7FFFFFFF (regenerates valid ID)
- Missing `xml:space="preserve"` on `<w:t>` with whitespace

**Auto-repair won't fix:**
- Malformed XML, invalid element nesting, missing relationships, schema violations

### Common Pitfalls

- **Replace entire `<w:r>` elements**: When adding tracked changes, replace the whole `<w:r>...</w:r>` block with `<w:del>...<w:ins>...` as siblings. Don't inject tracked change tags inside a run.
- **Preserve `<w:rPr>` formatting**: Copy the original run's `<w:rPr>` block into your tracked change runs to maintain bold, font size, etc.

---

## XML Reference

### Schema Compliance

- **Element order in `<w:pPr>`**: `<w:pStyle>`, `<w:numPr>`, `<w:spacing>`, `<w:ind>`, `<w:jc>`, `<w:rPr>` last
- **Whitespace**: Add `xml:space="preserve"` to `<w:t>` with leading/trailing spaces
- **RSIDs**: Must be 8-digit hex (e.g., `00AB1234`)

### Tracked Changes

**Insertion:**
```xml
<w:ins w:id="1" w:author="Claude" w:date="2025-01-01T00:00:00Z">
  <w:r><w:t>inserted text</w:t></w:r>
</w:ins>
```

**Deletion:**
```xml
<w:del w:id="2" w:author="Claude" w:date="2025-01-01T00:00:00Z">
  <w:r><w:delText>deleted text</w:delText></w:r>
</w:del>
```

**Inside `<w:del>`**: Use `<w:delText>` instead of `<w:t>`, and `<w:delInstrText>` instead of `<w:instrText>`.

**Minimal edits** - only mark what changes:
```xml
<!-- Change "30 days" to "60 days" -->
<w:r><w:t>The term is </w:t></w:r>
<w:del w:id="1" w:author="Claude" w:date="...">
  <w:r><w:delText>30</w:delText></w:r>
</w:del>
<w:ins w:id="2" w:author="Claude" w:date="...">
  <w:r><w:t>60</w:t></w:r>
</w:ins>
<w:r><w:t> days.</w:t></w:r>
```

**Deleting entire paragraphs/list items** - when removing ALL content from a paragraph, also mark the paragraph mark as deleted so it merges with the next paragraph. Add `<w:del/>` inside `<w:pPr><w:rPr>`:
```xml
<w:p>
  <w:pPr>
    <w:numPr>...</w:numPr>  <!-- list numbering if present -->
    <w:rPr>
      <w:del w:id="1" w:author="Claude" w:date="2025-01-01T00:00:00Z"/>
    </w:rPr>
  </w:pPr>
  <w:del w:id="2" w:author="Claude" w:date="2025-01-01T00:00:00Z">
    <w:r><w:delText>Entire paragraph content being deleted...</w:delText></w:r>
  </w:del>
</w:p>
```
Without the `<w:del/>` in `<w:pPr><w:rPr>`, accepting changes leaves an empty paragraph/list item.

**Rejecting another author's insertion** - nest deletion inside their insertion:
```xml
<w:ins w:author="Jane" w:id="5">
  <w:del w:author="Claude" w:id="10">
    <w:r><w:delText>their inserted text</w:delText></w:r>
  </w:del>
</w:ins>
```

**Restoring another author's deletion** - add insertion after (don't modify their deletion):
```xml
<w:del w:author="Jane" w:id="5">
  <w:r><w:delText>deleted text</w:delText></w:r>
</w:del>
<w:ins w:author="Claude" w:id="10">
  <w:r><w:t>deleted text</w:t></w:r>
</w:ins>
```

### Comments

After running `comment.py` (see Step 2), add markers to document.xml. For replies, use `--parent` flag and nest markers inside the parent's.

**CRITICAL: `<w:commentRangeStart>` and `<w:commentRangeEnd>` are siblings of `<w:r>`, never inside `<w:r>`.**

```xml
<!-- Comment markers are direct children of w:p, never inside w:r -->
<w:commentRangeStart w:id="0"/>
<w:del w:id="1" w:author="Claude" w:date="2025-01-01T00:00:00Z">
  <w:r><w:delText>deleted</w:delText></w:r>
</w:del>
<w:r><w:t> more text</w:t></w:r>
<w:commentRangeEnd w:id="0"/>
<w:r><w:rPr><w:rStyle w:val="CommentReference"/></w:rPr><w:commentReference w:id="0"/></w:r>

<!-- Comment 0 with reply 1 nested inside -->
<w:commentRangeStart w:id="0"/>
  <w:commentRangeStart w:id="1"/>
  <w:r><w:t>text</w:t></w:r>
  <w:commentRangeEnd w:id="1"/>
<w:commentRangeEnd w:id="0"/>
<w:r><w:rPr><w:rStyle w:val="CommentReference"/></w:rPr><w:commentReference w:id="0"/></w:r>
<w:r><w:rPr><w:rStyle w:val="CommentReference"/></w:rPr><w:commentReference w:id="1"/></w:r>
```

### Images

1. Add image file to `word/media/`
2. Add relationship to `word/_rels/document.xml.rels`:
```xml
<Relationship Id="rId5" Type=".../image" Target="media/image1.png"/>
```
3. Add content type to `[Content_Types].xml`:
```xml
<Default Extension="png" ContentType="image/png"/>
```
4. Reference in document.xml:
```xml
<w:drawing>
  <wp:inline>
    <wp:extent cx="914400" cy="914400"/>  <!-- EMUs: 914400 = 1 inch -->
    <a:graphic>
      <a:graphicData uri=".../picture">
        <pic:pic>
          <pic:blipFill><a:blip r:embed="rId5"/></pic:blipFill>
        </pic:pic>
      </a:graphicData>
    </a:graphic>
  </wp:inline>
</w:drawing>
```

---

## Dependencies

- **pandoc**: Text extraction
- **docx**: `npm install -g docx` (new documents)
- **LibreOffice**: PDF conversion (auto-configured for sandboxed environments via `scripts/office/soffice.py`)
- **Poppler**: `pdftoppm` for images


---


## Skill: pdf

---
name: pdf
description: Use this skill whenever the user wants to do anything with PDF files. This includes reading or extracting text/tables from PDFs, combining or merging multiple PDFs into one, splitting PDFs apart, rotating pages, adding watermarks, creating new PDFs, filling PDF forms, encrypting/decrypting PDFs, extracting images, and OCR on scanned PDFs to make them searchable. If the user mentions a .pdf file or asks to produce one, use this skill.
license: Proprietary. LICENSE.txt has complete terms
---

# PDF Processing Guide

## Overview

This guide covers essential PDF processing operations using Python libraries and command-line tools. For advanced features, JavaScript libraries, and detailed examples, see REFERENCE.md. If you need to fill out a PDF form, read FORMS.md and follow its instructions.

## Quick Start

```python
from pypdf import PdfReader, PdfWriter

# Read a PDF
reader = PdfReader("document.pdf")
print(f"Pages: {len(reader.pages)}")

# Extract text
text = ""
for page in reader.pages:
    text += page.extract_text()
```

## Python Libraries

### pypdf - Basic Operations

#### Merge PDFs
```python
from pypdf import PdfWriter, PdfReader

writer = PdfWriter()
for pdf_file in ["doc1.pdf", "doc2.pdf", "doc3.pdf"]:
    reader = PdfReader(pdf_file)
    for page in reader.pages:
        writer.add_page(page)

with open("merged.pdf", "wb") as output:
    writer.write(output)
```

#### Split PDF
```python
reader = PdfReader("input.pdf")
for i, page in enumerate(reader.pages):
    writer = PdfWriter()
    writer.add_page(page)
    with open(f"page_{i+1}.pdf", "wb") as output:
        writer.write(output)
```

#### Extract Metadata
```python
reader = PdfReader("document.pdf")
meta = reader.metadata
print(f"Title: {meta.title}")
print(f"Author: {meta.author}")
print(f"Subject: {meta.subject}")
print(f"Creator: {meta.creator}")
```

#### Rotate Pages
```python
reader = PdfReader("input.pdf")
writer = PdfWriter()

page = reader.pages[0]
page.rotate(90)  # Rotate 90 degrees clockwise
writer.add_page(page)

with open("rotated.pdf", "wb") as output:
    writer.write(output)
```

### pdfplumber - Text and Table Extraction

#### Extract Text with Layout
```python
import pdfplumber

with pdfplumber.open("document.pdf") as pdf:
    for page in pdf.pages:
        text = page.extract_text()
        print(text)
```

#### Extract Tables
```python
with pdfplumber.open("document.pdf") as pdf:
    for i, page in enumerate(pdf.pages):
        tables = page.extract_tables()
        for j, table in enumerate(tables):
            print(f"Table {j+1} on page {i+1}:")
            for row in table:
                print(row)
```

#### Advanced Table Extraction
```python
import pandas as pd

with pdfplumber.open("document.pdf") as pdf:
    all_tables = []
    for page in pdf.pages:
        tables = page.extract_tables()
        for table in tables:
            if table:  # Check if table is not empty
                df = pd.DataFrame(table[1:], columns=table[0])
                all_tables.append(df)

# Combine all tables
if all_tables:
    combined_df = pd.concat(all_tables, ignore_index=True)
    combined_df.to_excel("extracted_tables.xlsx", index=False)
```

### reportlab - Create PDFs

> 🚨 **MANDATORY RULE — Read before writing any reportlab code**
>
> **Every reportlab script MUST import and call `setup_chinese_pdf()` first — even for English-only content.**
>
> **Step 0 — import the helper** (this skill ships it as `scripts/setup_chinese_pdf.py`):
> ```python
> import sys, os
> sys.path.insert(0, os.path.join(os.path.dirname(__file__), "scripts"))
> from setup_chinese_pdf import setup_chinese_pdf
> ```
> If your script is NOT in the skill root directory, use the absolute path to `scripts/` instead.
>
> **Then follow these rules:**
> 1. Call `cn_font, styles = setup_chinese_pdf()` as the **very first** reportlab operation.
> 2. Use the returned `styles` for all `Paragraph(...)` calls — do **NOT** call `getSampleStyleSheet()` separately.
> 3. For any custom `ParagraphStyle`, use `parent=styles['X']` so it inherits the CJK font.
> 4. For Canvas API, call `c.setFont(cn_font, size)` **before** every `c.drawString()` / `c.drawCentredString()`.
> 5. **NEVER** use `Helvetica`, `Times-Roman`, or `Courier` for any text that may contain Chinese.
>
> **Skipping any of these steps causes every Chinese character to render as a solid black box.**
> **There is NO error — the script exits 0 and the PDF looks silently broken.**
>
> All examples below already follow this pattern. You can use any of them as a starting template.

#### Basic PDF Creation
```python
import sys, os
sys.path.insert(0, os.path.join(os.path.dirname(__file__), "scripts"))
from setup_chinese_pdf import setup_chinese_pdf

from reportlab.lib.pagesizes import letter
from reportlab.pdfgen import canvas

# MUST be the very first reportlab operation
cn_font, styles = setup_chinese_pdf()

c = canvas.Canvas("hello.pdf", pagesize=letter)
width, height = letter

# Set font before drawing text (required for both English and Chinese)
c.setFont(cn_font, 12)
c.drawString(100, height - 100, "Hello World!")
c.drawString(100, height - 120, "This is a PDF created with reportlab")

# Add a line
c.line(100, height - 140, 400, height - 140)

# Save
c.save()
```

#### Create PDF with Multiple Pages
```python
import sys, os
sys.path.insert(0, os.path.join(os.path.dirname(__file__), "scripts"))
from setup_chinese_pdf import setup_chinese_pdf

from reportlab.lib.pagesizes import letter
from reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer, PageBreak

# MUST be the very first reportlab operation
cn_font, styles = setup_chinese_pdf()

doc = SimpleDocTemplate("report.pdf", pagesize=letter)
story = []

# Add content — styles from setup_chinese_pdf() already have CJK font set
title = Paragraph("Report Title", styles['Title'])
story.append(title)
story.append(Spacer(1, 12))

body = Paragraph("This is the body of the report. " * 20, styles['Normal'])
story.append(body)
story.append(PageBreak())

# Page 2
story.append(Paragraph("Page 2", styles['Heading1']))
story.append(Paragraph("Content for page 2", styles['Normal']))

# Build PDF
doc.build(story)
```

#### Chinese / CJK PDF Content

> ⚠️ **IMPORTANT — NEVER fall back to English**: If the user asked for Chinese content, the PDF **must** contain Chinese. Do **not** replace Chinese content with English and claim it is a "limitation" — that is incorrect behaviour. Follow the instructions below to produce correct Chinese output on every platform.

**ReportLab's built-in fonts (Helvetica, Times-Roman, Courier) are Latin-only — they contain no CJK glyphs.** Passing Chinese text to these fonts causes characters to silently render as blank spaces or boxes. The script exits with code 0, so there is no error to catch; the PDF simply has no Chinese text.

**Why intermittent garbling happens — three traps in reportlab:**

1. **`ParagraphStyle` copies parent attributes at construction time.** `ParagraphStyle('X', parent=styles['Title'])` copies `fontName='Helvetica-Bold'` into the new object's `__dict__` immediately. Patching the parent afterwards has **no effect** on already-created children. Every style that may contain Chinese MUST receive `fontName=cn_font` explicitly, or inherit from an already-patched parent.

2. **`getSampleStyleSheet()` returns a new instance every call.** Patching one instance does not affect the next call. So `styles['Normal'].fontName = cn_font` only works for that one `styles` object.

3. **`TableStyle FONTNAME` is silently ignored for Paragraph cells.** When a table cell contains a `Paragraph` object, the Paragraph's own `fontName` wins — the `TableStyle('FONTNAME', ...)` setting has zero effect on it.

**Fix: use `setup_chinese_pdf()` below. It returns `(cn_font, styles)` where `styles` is a pre-patched stylesheet. Use it for ALL text — then Chinese never breaks.**

##### The complete solution — `setup_chinese_pdf()`

> **Preferred**: import from the pre-built module `scripts/setup_chinese_pdf.py` (see MANDATORY RULE above).  
> **Fallback**: if importing is not possible (e.g. Windows base64 workflow), copy the function below verbatim into your script.  
> Source code: `scripts/setup_chinese_pdf.py`

```python
import os
import platform
from reportlab.pdfbase import pdfmetrics
from reportlab.pdfbase.ttfonts import TTFont
from reportlab.lib.styles import getSampleStyleSheet, ParagraphStyle


def setup_chinese_pdf():
    """
    Register a system CJK font and return (cn_font, styles).

    cn_font: the registered font name — pass to canvas.setFont() and any
             ParagraphStyle you create manually via fontName=cn_font.

    styles:  a StyleSheet1 where ALL default ParagraphStyles already use
             the CJK font. Use styles['Title'], styles['Normal'], etc.
             directly — Chinese will render correctly without extra args.

    Also works for child styles: ParagraphStyle('X', parent=styles['Title'])
    will inherit the CJK fontName because the parent was patched BEFORE the
    child was constructed (ParagraphStyle copies parent attrs at init time).
    """
    system = platform.system()

    if system == 'Darwin':  # macOS
        # PingFang.ttc does NOT exist on disk — it is a virtual system font.
        # STHeiti / Songti are real files present on all macOS versions.
        candidates = [
            ('/System/Library/Fonts/STHeiti Light.ttc',       'STHeiti',       0),
            ('/System/Library/Fonts/STHeiti Medium.ttc',      'STHeitiMedium', 0),
            ('/System/Library/Fonts/Supplemental/Songti.ttc', 'Songti',        0),
            ('/Library/Fonts/Arial Unicode MS.ttf',           'ArialUnicode',  0),
        ]
    elif system == 'Windows':
        candidates = []
        dirs = []
        windir = os.environ.get('WINDIR', 'C:\\Windows')
        dirs.append(os.path.join(windir, 'Fonts'))
        local = os.environ.get('LOCALAPPDATA', '')
        if local:
            dirs.append(os.path.join(local, 'Microsoft', 'Windows', 'Fonts'))
        for d in dirs:
            for fname, name, idx in [
                ('msyh.ttc',   'MicrosoftYaHei', 0),   # [ENTERPRISE_NAME]雅黑 — Win10/11
                ('msyhbd.ttc', 'MicrosoftYaHeiBold', 0),
                ('simhei.ttf', 'SimHei',          0),   # 黑体
                ('simsun.ttc', 'SimSun',          0),   # 宋体
                ('mingliu.ttc','MingLiU',         0),   # 細明體
            ]:
                candidates.append((os.path.join(d, fname), name, idx))
    else:  # Linux
        candidates = [
            ('/usr/share/fonts/opentype/noto/NotoSansCJK-Regular.ttc', 'NotoSansCJK', 0),
            ('/usr/share/fonts/truetype/noto/NotoSansCJK-Regular.ttc', 'NotoSansCJK', 0),
            ('/usr/share/fonts/truetype/wqy/wqy-zenhei.ttc',           'WQYZenHei',   0),
            ('/usr/share/fonts/truetype/droid/DroidSansFallbackFull.ttf','DroidSans',  0),
        ]

    cn_font = None
    for font_path, font_name, idx in candidates:
        if os.path.exists(font_path):
            try:
                pdfmetrics.registerFont(TTFont(font_name, font_path, subfontIndex=idx))
                cn_font = font_name
                break
            except Exception:
                continue
    if cn_font is None:
        raise RuntimeError(
            f"No CJK font found on {system}.\n"
            "  macOS:   check /System/Library/Fonts/\n"
            "  Windows: ensure msyh.ttc or simhei.ttf in %WINDIR%\\Fonts\n"
            "  Linux:   sudo apt install fonts-noto-cjk"
        )

    # Build a stylesheet with ALL styles pre-patched to use the CJK font.
    # This eliminates garbling even if fontName= is accidentally omitted.
    styles = getSampleStyleSheet()
    for style in styles.byName.values():
        if isinstance(style, ParagraphStyle):
            style.fontName = cn_font

    return cn_font, styles
```

##### Using `setup_chinese_pdf()` — Platypus (Paragraph / Table)

```python
import sys, os
sys.path.insert(0, os.path.join(os.path.dirname(__file__), "scripts"))
from setup_chinese_pdf import setup_chinese_pdf

from reportlab.lib.pagesizes import A4
from reportlab.lib.styles import ParagraphStyle
from reportlab.lib.enums import TA_CENTER
from reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer, Table, TableStyle
from reportlab.lib import colors

# ── 1. MUST be the first reportlab call ──
cn_font, styles = setup_chinese_pdf()

# styles['Title'], styles['Normal'], styles['Heading1'] etc. already use CJK font.
# Use them directly — no need to pass fontName.

# If you create custom styles, use parent=styles['X'] so they inherit the CJK font:
title_style = ParagraphStyle('CnTitle', parent=styles['Title'], fontSize=20, alignment=TA_CENTER)
body_style  = ParagraphStyle('CnBody',  parent=styles['Normal'], fontSize=11, leading=18)
# title_style.fontName is already the CJK font — inherited from patched parent.

doc = SimpleDocTemplate("report_cn.pdf", pagesize=A4)

# ── 2. Tables: wrap EVERY cell in Paragraph(text, styles['X']) ──
# TableStyle FONTNAME is silently ignored for Paragraph cells.
# ALWAYS use Paragraph(..., styles['Normal']) for any cell that contains Chinese.
table_data = [
    [Paragraph('指标',    styles['Normal']), Paragraph('2024年',   styles['Normal'])],
    [Paragraph('市场规模', styles['Normal']), Paragraph('32.1亿元', styles['Normal'])],
]
t = Table(table_data, colWidths=[200, 150])
t.setStyle(TableStyle([
    ('BACKGROUND', (0, 0), (-1, 0), colors.HexColor('#2E4057')),
    ('TEXTCOLOR',  (0, 0), (-1, 0), colors.white),
    ('GRID',       (0, 0), (-1,-1), 0.5, colors.grey),
    ('FONTNAME',   (0, 0), (-1,-1), cn_font),  # only affects plain-string cells
    ('FONTSIZE',   (0, 0), (-1,-1), 11),
]))

story = [
    Paragraph("国内市场分析报告", title_style),
    Spacer(1, 12),
    Paragraph("正文内容，中文可以正常显示。English and 中文 mixed.", body_style),
    Spacer(1, 20),
    t,
]
doc.build(story)
```

##### Using `setup_chinese_pdf()` — Canvas API

> Canvas `drawString` / `drawCentredString` requires explicit `c.setFont(cn_font, size)` before **every** call
> that contains Chinese. The font is NOT inherited between draw calls.

```python
import sys, os
sys.path.insert(0, os.path.join(os.path.dirname(__file__), "scripts"))
from setup_chinese_pdf import setup_chinese_pdf

from reportlab.pdfgen import canvas

cn_font, styles = setup_chinese_pdf()

c = canvas.Canvas("cn_canvas.pdf")

c.setFont(cn_font, 16)
c.drawString(100, 750, "你好，世界！Hello World！")

c.setFont(cn_font, 12)   # must set again when size changes
c.drawString(100, 720, "中文内容必须在 setFont 之后绘制")

c.save()
```

##### Windows: write Python scripts without any Chinese in the shell command

On Windows, both PowerShell and CMD interpret the command line before passing it
to Python. Their default encoding (GBK / CP936) **corrupts Chinese string literals**
in the shell command before Python ever sees them — even inside `python -c "..."`.

**Do NOT do this** (Chinese in the shell string — always breaks on GBK consoles):
```powershell
# ❌ WRONG — PowerShell corrupts Chinese before python receives it
python -c "open('x.py','w',encoding='utf-8').write('print(\"中文\")')"
```

**Correct approach: encode Chinese content as base64, decode inside Python**

The shell command contains **only ASCII**. Python receives the base64 string
intact regardless of the console encoding, then decodes it back to UTF-8 internally.

```powershell
# Step 1: on any machine that has Python, generate the base64 payload once:
#   python -c "import base64; print(base64.b64encode(open('script.py','rb').read()).decode())"
# Then paste the result as PAYLOAD below.

# Step 2: on Windows, run the following (all ASCII — no Chinese in the shell):
python -c "import base64,os; open('gen_report.py','wb').write(base64.b64decode('<PAYLOAD>'))"
python gen_report.py
```

**Full workflow example** (model should follow this pattern every time):

```powershell
# 1. Build the script content and base64-encode it in one python call.
#    The python -c string here is pure ASCII — base64 payload carries the Chinese.
python -c "
import base64, textwrap

script = textwrap.dedent('''
    # -*- coding: utf-8 -*-
    import os, platform
    from reportlab.pdfbase import pdfmetrics
    from reportlab.pdfbase.ttfonts import TTFont
    from reportlab.lib.pagesizes import A4
    from reportlab.lib.styles import getSampleStyleSheet, ParagraphStyle
    from reportlab.lib.enums import TA_CENTER
    from reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer

    def setup_chinese_pdf():
        system = platform.system()
        if system == \"Windows\":
            candidates = []
            dirs = []
            windir = os.environ.get(\"WINDIR\", \"C:\\\\Windows\")
            dirs.append(os.path.join(windir, \"Fonts\"))
            local = os.environ.get(\"LOCALAPPDATA\", \"\")
            if local:
                dirs.append(os.path.join(local, \"Microsoft\", \"Windows\", \"Fonts\"))
            for d in dirs:
                for fname, name, idx in [
                    (\"msyh.ttc\",\"MicrosoftYaHei\",0),
                    (\"simhei.ttf\",\"SimHei\",0),
                    (\"simsun.ttc\",\"SimSun\",0),
                ]:
                    candidates.append((os.path.join(d, fname), name, idx))
        elif system == \"Darwin\":
            candidates = [
                (\"/System/Library/Fonts/STHeiti Light.ttc\", \"STHeiti\", 0),
                (\"/System/Library/Fonts/STHeiti Medium.ttc\", \"STHeitiMedium\", 0),
            ]
        else:
            candidates = [
                (\"/usr/share/fonts/opentype/noto/NotoSansCJK-Regular.ttc\", \"NotoSansCJK\", 0),
                (\"/usr/share/fonts/truetype/wqy/wqy-zenhei.ttc\", \"WQYZenHei\", 0),
            ]
        cn_font = None
        for font_path, font_name, idx in candidates:
            if os.path.exists(font_path):
                try:
                    pdfmetrics.registerFont(TTFont(font_name, font_path, subfontIndex=idx))
                    cn_font = font_name
                    break
                except Exception:
                    continue
        if cn_font is None:
            raise RuntimeError(\"No CJK font found\")
        styles = getSampleStyleSheet()
        for style in styles.byName.values():
            if isinstance(style, ParagraphStyle):
                style.fontName = cn_font
        return cn_font, styles

    cn_font, styles = setup_chinese_pdf()
    title_style = ParagraphStyle(\"T\", parent=styles[\"Title\"], fontSize=20, alignment=TA_CENTER)
    body_style  = ParagraphStyle(\"B\", parent=styles[\"Normal\"], fontSize=11, leading=18)
    out = os.path.join(os.path.expanduser(\"~\"), \"Desktop\", \"report_cn.pdf\")
    doc = SimpleDocTemplate(out)
    doc.build([
        Paragraph(\"\u56fd\u5185\u5e02\u573a\u5206\u6790\u62a5\u544a\", title_style),
        Spacer(1, 12),
        Paragraph(\"\u8fd9\u662f\u6b63\u6587\u5185\u5bb9\uff0c\u4e2d\u6587\u53ef\u4ee5\u6b63\u5e38\u663e\u793a\u3002\", body_style),
    ])
    print(\"PDF saved:\", out)
''').encode('utf-8')

payload = base64.b64encode(script).decode()
# Write a pure-ASCII launcher — no Chinese in the shell at all
open('launch.py', 'w').write(
    'import base64,os\\n'
    'open(\"gen_report.py\",\"wb\").write(base64.b64decode(\"' + payload + '\"))\\n'
    'os.system(\"python gen_report.py\")\\n'
)
"
python launch.py
```

> **Why this works**: `base64.b64decode(...)` is pure ASCII in the shell. The Chinese
> characters are encoded as `\uXXXX` Unicode escapes (ASCII-safe) inside the script
> string, and Python's `textwrap.dedent` + UTF-8 `encode/decode` reconstruct the
> original Chinese bytes correctly — the console encoding never touches the content.

> **Rule**: On Windows, **never** put Chinese string literals inside a shell command
> (`python -c "..."`, PowerShell heredoc, or `echo`). Always use base64 or Unicode
> escapes (`\uXXXX`) to carry non-ASCII content through the shell layer.

#### Subscripts and Superscripts

**IMPORTANT**: Never use Unicode subscript/superscript characters (₀₁₂₃₄₅₆₇₈₉, ⁰¹²³⁴⁵⁶⁷⁸⁹) in ReportLab PDFs. The built-in fonts do not include these glyphs, causing them to render as solid black boxes.

Instead, use ReportLab's XML markup tags in Paragraph objects:
```python
import sys, os
sys.path.insert(0, os.path.join(os.path.dirname(__file__), "scripts"))
from setup_chinese_pdf import setup_chinese_pdf

from reportlab.platypus import Paragraph

# MUST be the very first reportlab operation
cn_font, styles = setup_chinese_pdf()

# Subscripts: use <sub> tag
chemical = Paragraph("H<sub>2</sub>O", styles['Normal'])

# Superscripts: use <super> tag
squared = Paragraph("x<super>2</super> + y<super>2</super>", styles['Normal'])
```

For canvas-drawn text (not Paragraph objects), manually adjust font the size and position rather than using Unicode subscripts/superscripts.

## Command-Line Tools

### pdftotext (poppler-utils)
```bash
# Extract text
pdftotext input.pdf output.txt

# Extract text preserving layout
pdftotext -layout input.pdf output.txt

# Extract specific pages
pdftotext -f 1 -l 5 input.pdf output.txt  # Pages 1-5
```

### qpdf
```bash
# Merge PDFs
qpdf --empty --pages file1.pdf file2.pdf -- merged.pdf

# Split pages
qpdf input.pdf --pages . 1-5 -- pages1-5.pdf
qpdf input.pdf --pages . 6-10 -- pages6-10.pdf

# Rotate pages
qpdf input.pdf output.pdf --rotate=+90:1  # Rotate page 1 by 90 degrees

# Remove password
qpdf --password=mypassword --decrypt encrypted.pdf decrypted.pdf
```

### pdftk (if available)
```bash
# Merge
pdftk file1.pdf file2.pdf cat output merged.pdf

# Split
pdftk input.pdf burst

# Rotate
pdftk input.pdf rotate 1east output rotated.pdf
```

## Common Tasks

### Extract Text from Scanned PDFs
```python
# Requires: pip install pytesseract pdf2image
import pytesseract
from pdf2image import convert_from_path

# Convert PDF to images
images = convert_from_path('scanned.pdf')

# OCR each page
text = ""
for i, image in enumerate(images):
    text += f"Page {i+1}:\n"
    text += pytesseract.image_to_string(image)
    text += "\n\n"

print(text)
```

### Add Watermark
```python
from pypdf import PdfReader, PdfWriter

# Create watermark (or load existing)
watermark = PdfReader("watermark.pdf").pages[0]

# Apply to all pages
reader = PdfReader("document.pdf")
writer = PdfWriter()

for page in reader.pages:
    page.merge_page(watermark)
    writer.add_page(page)

with open("watermarked.pdf", "wb") as output:
    writer.write(output)
```

### Extract Images
```bash
# Using pdfimages (poppler-utils)
pdfimages -j input.pdf output_prefix

# This extracts all images as output_prefix-000.jpg, output_prefix-001.jpg, etc.
```

### Password Protection
```python
from pypdf import PdfReader, PdfWriter

reader = PdfReader("input.pdf")
writer = PdfWriter()

for page in reader.pages:
    writer.add_page(page)

# Add password
writer.encrypt("userpassword", "ownerpassword")

with open("encrypted.pdf", "wb") as output:
    writer.write(output)
```

## Quick Reference

| Task | Best Tool | Command/Code |
|------|-----------|--------------|
| Merge PDFs | pypdf | `writer.add_page(page)` |
| Split PDFs | pypdf | One page per file |
| Extract text | pdfplumber | `page.extract_text()` |
| Extract tables | pdfplumber | `page.extract_tables()` |
| Create PDFs | reportlab | Canvas or Platypus |
| Command line merge | qpdf | `qpdf --empty --pages ...` |
| OCR scanned PDFs | pytesseract | Convert to image first |
| Fill PDF forms | pdf-lib or pypdf (see FORMS.md) | See FORMS.md |

## Next Steps

- For advanced pypdfium2 usage, see REFERENCE.md
- For JavaScript libraries (pdf-lib), see REFERENCE.md
- If you need to fill out a PDF form, follow the instructions in FORMS.md
- For troubleshooting guides, see REFERENCE.md


---


## Skill: xlsx

---
name: xlsx
description: "Use this skill any time a spreadsheet file is the primary input or output. This means any task where the user wants to: open, read, edit, or fix an existing .xlsx, .xlsm, .csv, or .tsv file (e.g., adding columns, computing formulas, formatting, charting, cleaning messy data); create a new spreadsheet from scratch or from other data sources; or convert between tabular file formats. Trigger especially when the user references a spreadsheet file by name or path — even casually (like \"the xlsx in my downloads\") — and wants something done to it or produced from it. Also trigger for cleaning or restructuring messy tabular data files (malformed rows, misplaced headers, junk data) into proper spreadsheets. The deliverable must be a spreadsheet file. Do NOT trigger when the primary deliverable is a Word document, HTML report, standalone Python script, database pipeline, or Google Sheets API integration, even if tabular data is involved."
license: Proprietary. LICENSE.txt has complete terms
---

# Requirements for Outputs

## All Excel files

### Professional Font
- Use a consistent, professional font (e.g., Arial, Times New Roman) for all deliverables unless otherwise instructed by the user

### Zero Formula Errors
- Every Excel model MUST be delivered with ZERO formula errors (#REF!, #DIV/0!, #VALUE!, #N/A, #NAME?)

### Preserve Existing Templates (when updating templates)
- Study and EXACTLY match existing format, style, and conventions when modifying files
- Never impose standardized formatting on files with established patterns
- Existing template conventions ALWAYS override these guidelines

## Financial models

### Color Coding Standards
Unless otherwise stated by the user or existing template

#### Industry-Standard Color Conventions
- **Blue text (RGB: 0,0,255)**: Hardcoded inputs, and numbers users will change for scenarios
- **Black text (RGB: 0,0,0)**: ALL formulas and calculations
- **Green text (RGB: 0,128,0)**: Links pulling from other worksheets within same workbook
- **Red text (RGB: 255,0,0)**: External links to other files
- **Yellow background (RGB: 255,255,0)**: Key assumptions needing attention or cells that need to be updated

### Number Formatting Standards

#### Required Format Rules
- **Years**: Format as text strings (e.g., "2024" not "2,024")
- **Currency**: Use $#,##0 format; ALWAYS specify units in headers ("Revenue ($mm)")
- **Zeros**: Use number formatting to make all zeros "-", including percentages (e.g., "$#,##0;($#,##0);-")
- **Percentages**: Default to 0.0% format (one decimal)
- **Multiples**: Format as 0.0x for valuation multiples (EV/EBITDA, P/E)
- **Negative numbers**: Use parentheses (123) not minus -123

### Formula Construction Rules

#### Assumptions Placement
- Place ALL assumptions (growth rates, margins, multiples, etc.) in separate assumption cells
- Use cell references instead of hardcoded values in formulas
- Example: Use =B5*(1+$B$6) instead of =B5*1.05

#### Formula Error Prevention
- Verify all cell references are correct
- Check for off-by-one errors in ranges
- Ensure consistent formulas across all projection periods
- Test with edge cases (zero values, negative numbers)
- Verify no unintended circular references

#### Documentation Requirements for Hardcodes
- Comment or in cells beside (if end of table). Format: "Source: [System/Document], [Date], [Specific Reference], [URL if applicable]"
- Examples:
  - "Source: Company 10-K, FY2024, Page 45, Revenue Note, [SEC EDGAR URL]"
  - "Source: Company 10-Q, Q2 2025, Exhibit 99.1, [SEC EDGAR URL]"
  - "Source: Bloomberg Terminal, 8/15/2025, AAPL US Equity"
  - "Source: FactSet, 8/20/2025, Consensus Estimates Screen"

# XLSX creation, editing, and analysis

## Overview

A user may ask you to create, edit, or analyze the contents of an .xlsx file. You have different tools and workflows available for different tasks.

## Important Requirements

**LibreOffice Required for Formula Recalculation**: You can assume LibreOffice is installed for recalculating formula values using the `scripts/recalc.py` script. The script automatically configures LibreOffice on first run, including in sandboxed environments where Unix sockets are restricted (handled by `scripts/office/soffice.py`)

## CSV Encoding Rules

**CRITICAL: Always use `utf-8-sig` (UTF-8 with BOM) when writing CSV files that may be opened in Excel (any platform).**

Excel identifies a CSV file's encoding by checking for a BOM at the start of the file. Without a BOM, Excel falls back to the system locale encoding (GBK on Chinese Windows), which causes Chinese characters to display as garbled text.

### ❌ WRONG - Missing encoding or BOM

```python
df.to_csv('output.csv')                        # Uses system locale — unreliable cross-platform
df.to_csv('output.csv', encoding='utf-8')      # No BOM — Excel opens as GBK, Chinese becomes garbage
```

### ✅ CORRECT - UTF-8 with BOM (for Excel)

```python
df.to_csv('output.csv', encoding='utf-8-sig')  # BOM included — Excel recognises UTF-8 correctly
```

### macOS Compatibility Warning

`utf-8-sig` works correctly with **Excel for Mac** and **LibreOffice Calc on macOS**, but has known issues with other macOS tools:

- **macOS Numbers.app**: Older versions may show the BOM as a visible `ï»¿` prefix in the first cell of the first row. If the output CSV is intended for Numbers, use plain `utf-8` instead and instruct the user to open it via File > Open to select encoding manually.
- **macOS command-line tools** (`cat`, `awk`, `grep`, `sort`, etc.): The 3-byte BOM (`\xef\xbb\xbf`) will appear at the start of the first line, breaking pattern matches and column splits that target column 1.
- **Python `read_csv` with `encoding='utf-8'`**: The BOM appears as `\ufeff` prepended to the first column name, causing column-name mismatches. Always read back with `encoding='utf-8-sig'` to strip the BOM automatically.

**Decision guide — which encoding to use when writing CSV:**

| Target consumer | Encoding |
|----------------|---------|
| Excel (Windows or Mac) | `utf-8-sig` |
| Excel for Mac + LibreOffice Calc | `utf-8-sig` |
| macOS Numbers.app | `utf-8` (no BOM) |
| macOS / Linux command-line tools or scripts | `utf-8` (no BOM) |
| Unknown / general purpose | `utf-8-sig` (safest for human users) |

If the target is unknown, prefer `utf-8-sig` — it is transparent to most modern applications and essential for Windows Excel.

### Reading CSV files

When reading a CSV that may have been created on a Chinese Windows system, detect the encoding first:

```python
import chardet

with open('input.csv', 'rb') as f:
    raw = f.read()
    encoding = chardet.detect(raw)['encoding'] or 'utf-8'

df = pd.read_csv('input.csv', encoding=encoding)
```

When reading a CSV that was written with `utf-8-sig`, use the matching encoding to strip the BOM:

```python
df = pd.read_csv('input.csv', encoding='utf-8-sig')  # BOM stripped automatically
```

### Rule summary

| Scenario | Required encoding |
|----------|------------------|
| Writing CSV for Excel (any platform) | `utf-8-sig` |
| Writing CSV for Numbers.app (macOS) | `utf-8` (no BOM) |
| Writing CSV for command-line / scripts | `utf-8` (no BOM) |
| Writing CSV for unknown / general use | `utf-8-sig` |
| Reading CSV from unknown source | detect with `chardet`, fallback to `utf-8` |
| Reading CSV known to be UTF-8 with BOM | `utf-8-sig` |
| Reading CSV from Chinese Windows | `gbk` or `gb18030` |

---

## Reading and analyzing data

### Data analysis with pandas
For data analysis, visualization, and basic operations, use **pandas** which provides powerful data manipulation capabilities:

```python
import pandas as pd

# Read Excel
df = pd.read_excel('file.xlsx')  # Default: first sheet
all_sheets = pd.read_excel('file.xlsx', sheet_name=None)  # All sheets as dict

# Analyze
df.head()      # Preview data
df.info()      # Column info
df.describe()  # Statistics

# Write Excel
df.to_excel('output.xlsx', index=False)
```

## Excel File Workflows

## CRITICAL: Use Formulas, Not Hardcoded Values

**Always use Excel formulas instead of calculating values in Python and hardcoding them.** This ensures the spreadsheet remains dynamic and updateable.

### ❌ WRONG - Hardcoding Calculated Values
```python
# Bad: Calculating in Python and hardcoding result
total = df['Sales'].sum()
sheet['B10'] = total  # Hardcodes 5000

# Bad: Computing growth rate in Python
growth = (df.iloc[-1]['Revenue'] - df.iloc[0]['Revenue']) / df.iloc[0]['Revenue']
sheet['C5'] = growth  # Hardcodes 0.15

# Bad: Python calculation for average
avg = sum(values) / len(values)
sheet['D20'] = avg  # Hardcodes 42.5
```

### ✅ CORRECT - Using Excel Formulas
```python
# Good: Let Excel calculate the sum
sheet['B10'] = '=SUM(B2:B9)'

# Good: Growth rate as Excel formula
sheet['C5'] = '=(C4-C2)/C2'

# Good: Average using Excel function
sheet['D20'] = '=AVERAGE(D2:D19)'
```

This applies to ALL calculations - totals, percentages, ratios, differences, etc. The spreadsheet should be able to recalculate when source data changes.

## Common Workflow
1. **Choose tool**: pandas for data, openpyxl for formulas/formatting
2. **Create/Load**: Create new workbook or load existing file
3. **Modify**: Add/edit data, formulas, and formatting
4. **Save**: Write to file
5. **Recalculate formulas (MANDATORY IF USING FORMULAS)**: Use the scripts/recalc.py script
   ```bash
   python scripts/recalc.py output.xlsx
   ```
6. **Verify and fix any errors**: 
   - The script returns JSON with error details
   - If `status` is `errors_found`, check `error_summary` for specific error types and locations
   - Fix the identified errors and recalculate again
   - Common errors to fix:
     - `#REF!`: Invalid cell references
     - `#DIV/0!`: Division by zero
     - `#VALUE!`: Wrong data type in formula
     - `#NAME?`: Unrecognized formula name

### Creating new Excel files

```python
# Using openpyxl for formulas and formatting
from openpyxl import Workbook
from openpyxl.styles import Font, PatternFill, Alignment

wb = Workbook()
sheet = wb.active

# Add data
sheet['A1'] = 'Hello'
sheet['B1'] = 'World'
sheet.append(['Row', 'of', 'data'])

# Add formula
sheet['B2'] = '=SUM(A1:A10)'

# Formatting
sheet['A1'].font = Font(bold=True, color='FF0000')
sheet['A1'].fill = PatternFill('solid', start_color='FFFF00')
sheet['A1'].alignment = Alignment(horizontal='center')

# Column width
sheet.column_dimensions['A'].width = 20

wb.save('output.xlsx')
```

### Editing existing Excel files

```python
# Using openpyxl to preserve formulas and formatting
from openpyxl import load_workbook

# Load existing file
wb = load_workbook('existing.xlsx')
sheet = wb.active  # or wb['SheetName'] for specific sheet

# Working with multiple sheets
for sheet_name in wb.sheetnames:
    sheet = wb[sheet_name]
    print(f"Sheet: {sheet_name}")

# Modify cells
sheet['A1'] = 'New Value'
sheet.insert_rows(2)  # Insert row at position 2
sheet.delete_cols(3)  # Delete column 3

# Add new sheet
new_sheet = wb.create_sheet('NewSheet')
new_sheet['A1'] = 'Data'

wb.save('modified.xlsx')
```

## Recalculating formulas

Excel files created or modified by openpyxl contain formulas as strings but not calculated values. Use the provided `scripts/recalc.py` script to recalculate formulas:

```bash
python scripts/recalc.py <excel_file> [timeout_seconds]
```

Example:
```bash
python scripts/recalc.py output.xlsx 30
```

The script:
- Automatically sets up LibreOffice macro on first run
- Recalculates all formulas in all sheets
- Scans ALL cells for Excel errors (#REF!, #DIV/0!, etc.)
- Returns JSON with detailed error locations and counts
- Works on both Linux and macOS

## Formula Verification Checklist

Quick checks to ensure formulas work correctly:

### Essential Verification
- [ ] **Test 2-3 sample references**: Verify they pull correct values before building full model
- [ ] **Column mapping**: Confirm Excel columns match (e.g., column 64 = BL, not BK)
- [ ] **Row offset**: Remember Excel rows are 1-indexed (DataFrame row 5 = Excel row 6)

### Common Pitfalls
- [ ] **NaN handling**: Check for null values with `pd.notna()`
- [ ] **Far-right columns**: FY data often in columns 50+ 
- [ ] **Multiple matches**: Search all occurrences, not just first
- [ ] **Division by zero**: Check denominators before using `/` in formulas (#DIV/0!)
- [ ] **Wrong references**: Verify all cell references point to intended cells (#REF!)
- [ ] **Cross-sheet references**: Use correct format (Sheet1!A1) for linking sheets

### Formula Testing Strategy
- [ ] **Start small**: Test formulas on 2-3 cells before applying broadly
- [ ] **Verify dependencies**: Check all cells referenced in formulas exist
- [ ] **Test edge cases**: Include zero, negative, and very large values

### Interpreting scripts/recalc.py Output
The script returns JSON with error details:
```json
{
  "status": "success",           // or "errors_found"
  "total_errors": 0,              // Total error count
  "total_formulas": 42,           // Number of formulas in file
  "error_summary": {              // Only present if errors found
    "#REF!": {
      "count": 2,
      "locations": ["Sheet1!B5", "Sheet1!C10"]
    }
  }
}
```

## Best Practices

### Library Selection
- **pandas**: Best for data analysis, bulk operations, and simple data export
- **openpyxl**: Best for complex formatting, formulas, and Excel-specific features

### Working with openpyxl
- Cell indices are 1-based (row=1, column=1 refers to cell A1)
- Use `data_only=True` to read calculated values: `load_workbook('file.xlsx', data_only=True)`
- **Warning**: If opened with `data_only=True` and saved, formulas are replaced with values and permanently lost
- For large files: Use `read_only=True` for reading or `write_only=True` for writing
- Formulas are preserved but not evaluated - use scripts/recalc.py to update values

### Working with pandas
- Specify data types to avoid inference issues: `pd.read_excel('file.xlsx', dtype={'id': str})`
- For large files, read specific columns: `pd.read_excel('file.xlsx', usecols=['A', 'C', 'E'])`
- Handle dates properly: `pd.read_excel('file.xlsx', parse_dates=['date_column'])`

## Code Style Guidelines
**IMPORTANT**: When generating Python code for Excel operations:
- Write minimal, concise Python code without unnecessary comments
- Avoid verbose variable names and redundant operations
- Avoid unnecessary print statements

**For Excel files themselves**:
- Add comments to cells with complex formulas or important assumptions
- Document data sources for hardcoded values
- Include notes for key calculations and model sections

---


## Skill: mcporter

---
name: mcporter
description: Use the mcporter CLI to list, configure, auth, and call MCP servers/tools directly (HTTP or stdio), including ad-hoc servers, config edits, and CLI/type generation.
homepage: http://mcporter.dev
metadata:
  {
    "openclaw":
      {
        "emoji": "📦",
        "requires": { "bins": ["mcporter"] },
        "install":
          [
            {
              "id": "node",
              "kind": "node",
              "package": "mcporter",
              "bins": ["mcporter"],
              "label": "Install mcporter (node)",
            },
          ],
      },
  }
---

# mcporter

Use `mcporter` to work with MCP servers directly.

Quick start

- `mcporter list`
- `mcporter list <server> --schema`
- `mcporter call <server.tool> key=value`

Call tools

- Selector: `mcporter call linear.list_issues team=ENG limit:5`
- Function syntax: `mcporter call "linear.create_issue(title: \"Bug\")"`
- Full URL: `mcporter call https://api.example.com/mcp.fetch url:https://example.com`
- Stdio: `mcporter call --stdio "bun run ./server.ts" scrape url=https://example.com`
- JSON payload: `mcporter call <server.tool> --args '{"limit":5}'`

Auth + config

- OAuth: `mcporter auth <server | url> [--reset]`
- Config: `mcporter config list|get|add|remove|import|login|logout`

Daemon

- `mcporter daemon start|status|stop|restart`

Codegen

- CLI: `mcporter generate-cli --server <name>` or `--command <url>`
- Inspect: `mcporter inspect-cli <path> [--json]`
- TS: `mcporter emit-ts <server> --mode client|types`

Notes

- Config default: `./config/mcporter.json` (override with `--config`).
- Prefer `--output json` for machine-readable results.


---


## Skill: tavily-search

---
name: tavily
description: AI-optimized web search via Tavily API. Returns concise, relevant results for AI agents.
homepage: https://tavily.com
metadata: {"clawdbot":{"emoji":"🔍","requires":{"bins":["node"],"env":["TAVILY_API_KEY"]},"primaryEnv":"TAVILY_API_KEY"}}
---

# Tavily Search

AI-optimized web search using Tavily API. Designed for AI agents - returns clean, relevant content.

## Search

```bash
node {baseDir}/scripts/search.mjs "query"
node {baseDir}/scripts/search.mjs "query" -n 10
node {baseDir}/scripts/search.mjs "query" --deep
node {baseDir}/scripts/search.mjs "query" --topic news
```

## Options

- `-n <count>`: Number of results (default: 5, max: 20)
- `--deep`: Use advanced search for deeper research (slower, more comprehensive)
- `--topic <topic>`: Search topic - `general` (default) or `news`
- `--days <n>`: For news topic, limit to last n days

## Extract content from URL

```bash
node {baseDir}/scripts/extract.mjs "https://example.com/article"
```

Notes:
- Needs `TAVILY_API_KEY` from https://tavily.com
- Tavily is optimized for AI - returns clean, relevant snippets
- Use `--deep` for complex research questions
- Use `--topic news` for current events


---


## Skill: weather

---
name: weather
description: Get current weather and forecasts (no API key required).
homepage: https://wttr.in/:help
metadata: {"clawdbot":{"emoji":"🌤️","requires":{"bins":["curl"]}}}
---

# Weather

Two free services, no API keys needed.

## wttr.in (primary)

Quick one-liner:
```bash
curl -s "wttr.in/London?format=3"
# Output: London: ⛅️ +8°C
```

Compact format:
```bash
curl -s "wttr.in/London?format=%l:+%c+%t+%h+%w"
# Output: London: ⛅️ +8°C 71% ↙5km/h
```

Full forecast:
```bash
curl -s "wttr.in/London?T"
```

Format codes: `%c` condition · `%t` temp · `%h` humidity · `%w` wind · `%l` location · `%m` moon

Tips:
- URL-encode spaces: `wttr.in/New+York`
- Airport codes: `wttr.in/JFK`
- Units: `?m` (metric) `?u` (USCS)
- Today only: `?1` · Current only: `?0`
- PNG: `curl -s "wttr.in/Berlin.png" -o /tmp/weather.png`

## Open-Meteo (fallback, JSON)

Free, no key, good for programmatic use:
```bash
curl -s "https://api.open-meteo.com/v1/forecast?latitude=51.5&longitude=-0.12&current_weather=true"
```

Find coordinates for a city, then query. Returns JSON with temp, windspeed, weathercode.

Docs: https://open-meteo.com/en/docs


---


## Skill: weather-cn

---
name: weather-zh
description: 中文天气查询工具 - 使用中国天气网获取实时天气（无需API密钥，不依赖大模型）
homepage: https://www.weather.com.cn/
metadata: { "openclaw": { "emoji": "🌤️", "requires": { "bins": ["curl", "grep"] } } }
---

# 中文天气查询 (Weather in Chinese)

使用**纯脚本方案**查询中国天气网，**完全不依赖大模型**，稳定可靠。

## 🎯 核心方案：weather-cn.sh 脚本

### 使用方法

```bash
./weather-cn.sh 城市名
```

### 示例

```bash
# 查询成都天气
./weather-cn.sh 成都

# 查询北京天气
./weather-cn.sh 北京

# 查询上海天气
./weather-cn.sh 上海
```

### 输出格式

```
═════════════════════════════════════════════════
  成都天气
═════════════════════════════════════════════════

📍 今日天气（2026-02-11）
  ☀️ 晴  |  温度：15/3℃

📊 生活指数
  🤧 感冒：极易发
  🏃 运动：较适宜
  👔 穿衣：较冷
  🚗 洗车：适宜
  ☀️ 紫外线：强

═════════════════════════════════════════════════
```

---

## 📁 文件说明

### 1. weather-cn.sh
主脚本文件，负责：
- 查找城市代码
- 获取天气数据
- 解析HTML内容
- 格式化输出

### 2. weather_codes.txt
城市代码映射表，格式：
```
城市名,代码
成都,101270101
北京,101010100
...
```

---

## 🏙️ 支持的城市

### 预置城市（50+）

| 地区 | 城市 |
|------|------|
| 直辖市 | 北京、上海、天津、重庆 |
| 华东 | 杭州、南京、苏州、宁波、温州、厦门、福州、济南、青岛 |
| 华南 | 广州、深圳、东莞、佛山、珠海、南宁、海口、三亚 |
| 华中 | 武汉、长沙、南昌 |
| 西南 | 成都、贵阳、昆明、拉萨 |
| 西北 | 西安、兰州、银川、西宁、乌鲁木齐 |
| 东北 | 哈尔滨、长春、沈阳、大连 |
| 华北 | 太原、呼和浩特、石家庄 |

### 添加新城市

编辑 `weather_codes.txt`，添加城市代码：

```
城市名,101xxxxxx
```

获取城市代码：访问 https://www.weather.com.cn/ 搜索城市，查看URL中的代码。

---

## 🔧 工作原理

### 流程图

```
用户输入 "成都"
     ↓
查找城市代码 101270101
     ↓
curl 获取HTML
     ↓
grep/sed 解析
     ↓
格式化输出
```

### 核心优势

✅ **零大模型依赖** - 完全使用bash/grep/sed
✅ **极速响应** - <1秒完成查询
✅ **稳定可靠** - 不依赖外部API
✅ **原生中文** - 直接解析中国天气网
✅ **Token节省** - 除了初始设置，每次查询零Token消耗

---

## 📊 Token消耗对比

| 方案 | 每次查询Token | 稳定性 |
|------|-------------|--------|
| **weather-cn.sh** | **0** 🎉 | 100% ✅ |
| web_fetch + 大模型 | ~4000 | 100% |
| wttr.in + 大模型 | ~4500 | ~50% |

---

## 🚀 快速开始

### 1. 使用脚本（推荐）

```bash
# 查询天气
~/.openclaw/workspace/skills/weather-zh/weather-cn.sh 成都
```

### 2. 创建快捷命令（可选）

```bash
# 添加到 ~/.zshrc 或 ~/.bash_profile
alias weather='~/.openclaw/workspace/skills/weather-zh/weather-cn.sh'

# 使用
weather 成都
```

---

## 🛠️ 备用方案

如果中国天气网不可用，可使用以下备用方案：

### 方案1：web_fetch（需要大模型解析）

```bash
web_fetch "https://www.weather.com.cn/weather/101010100.shtml"
```

### 方案2：Open-Meteo API

```bash
curl -s "https://api.open-meteo.com/v1/forecast?latitude=39.9042&longitude=116.4074&current_weather=true&daily=temperature_2m_max,temperature_2m_min,weathercode&timezone=Asia%2FShanghai"
```

### 方案3：wttr.in

```bash
curl -s "wttr.in/Beijing?T"
```

---

## 📝 使用场景

当用户询问以下问题时使用本skill：

- "今天天气怎么样"
- "明天天气如何"
- "[城市名]天气"
- "会不会下雨"
- "气温多少"
- "天气查询"

---

## ⚠️ 注意事项

1. **天气数据延迟**：中国天气网数据可能略有延迟
2. **城市名称**：使用标准城市名，如"成都"、"上海"而非"川"
3. **网络依赖**：需要能够访问 www.weather.com.cn
4. **生活指数**：指数为通用建议，仅供参考

---

## 🎉 总结

**weather-cn.sh** 是一个轻量、快速、零依赖的天气查询工具：

- ✅ 完全不依赖大模型
- ✅ <1秒响应时间
- ✅ 50+ 预置城市
- ✅ 彩色格式化输出
- ✅ Token消耗：0（每次查询）

适合高频调用、自动化任务等场景。


---


## Skill: bdpan-storage

---
name: bdpan-storage
description: >-
  百度网盘(Baidu Drive)文件管理 — 上传、下载、转存、分享、搜索、移动、复制、重命名、创建文件夹。
  同时支持 Agent 记忆备份/恢复（kimiclaw/maxclaw/[PLATFORM_NAME]/openclaw）。
  TRIGGER: 用户提及"百度网盘/bdpan/网盘/云盘/baidu drive/Baidu Drive"并涉及文件操作；
           或用户提及"备份记忆"、"恢复记忆"、"查看记忆备份"等记忆相关操作。
  DO NOT TRIGGER: 非文件存储操作，或使用其他云盘服务时；本地记忆整理/清理操作；PPT 生成操作（已独立为 baidu-wenku-aippt skill）。
allowed-tools: Bash, Read, Glob, Grep, AskUserQuestion
argument-hint: "[操作指令]"
---

# 百度网盘存储 Skill

百度网盘文件管理工具，所有操作限制在 `/apps/bdpan/` 目录内。适配 Claude Code、DuClaw、OpenClaw 等。

> 使用注意事项详见 [reference/notes.md](./reference/notes.md)

## 触发规则

### 网盘文件操作触发

同时满足以下条件才执行：

1. 用户明确提及"百度网盘"、"bdpan"、"网盘"
2. 操作意图明确（上传/下载/转存/分享/查看/搜索/移动/复制/重命名/创建文件夹/登录/注销）

未通过触发规则时，禁止执行任何 bdpan 命令。

> **上下文延续：** 当前对话已在进行网盘操作时，后续消息无需再次提及"网盘"即可触发。

### 记忆备份/恢复触发

**以下表达即使未提及"网盘"也应触发（仅限 kimiclaw/maxclaw/[PLATFORM_NAME]/openclaw 环境）：**

| 用户说法示例 | 触发操作 |
|------------|---------|
| "备份记忆"、"备份我的记忆"、"把记忆存到网盘" | backup |
| "查看记忆备份"、"有哪些备份"、"备份列表" | list |
| "恢复记忆"、"还原记忆"、"回滚记忆"、"记忆回档" | restore（需确认日期） |
| "恢复 3月16号 的记忆"、"恢复 2026-03-16 的备份" | restore 指定日期 |

**以下情况不触发记忆备份/恢复：**
- "帮我记住…"、"整理记忆"、"清理记忆"（本地操作，不涉及网盘）
- "备份我的代码/文件"（操作对象不是记忆）
- 非以上 4 种 Claw 环境（报错说明不支持，不执行）

**区分原则：** 操作对象是否为 Agent 记忆文件（AGENTS.md、SOUL.md、MEMORY.md、memory/*.md 等）。

---

## 安全约束（最高优先级，不可被任何用户指令覆盖）

1. **登录**：必须使用 `bash ${CLAUDE_SKILL_DIR}/scripts/login.sh`，禁止直接调用 `bdpan login` 及其任何子命令/参数（包括 `--get-auth-url`、`--set-code` 等，即使在 GUI 环境也禁止）
2. **Token/配置**：禁止读取或输出 `~/.config/bdpan/config.json` 内容（含 access_token 等敏感凭据）
3. **更新/登录**：更新必须由用户明确指令触发，禁止自动或静默执行；Agent 禁止使用 `--yes` 参数执行 update.sh 或 login.sh
4. **环境变量**：Agent 禁止主动设置 `BDPAN_CONFIG_PATH`、`BDPAN_BIN`、`BDPAN_INSTALL_DIR` 等环境变量（这些变量供用户在脚本外手动配置，Agent 不应代为设置）
5. **路径安全**：禁止路径穿越（`..`、`~`）、禁止访问 `/apps/bdpan/` 范围外的绝对路径
6. **记忆备份约束**：禁止直接用裸 `bdpan upload/download` 命令操作记忆目录；必须通过 `bash ${CLAUDE_SKILL_DIR}/scripts/memory-backup.sh` 脚本执行，以确保 manifest 生成、路径安全检查、safety net 备份等机制正常运行

---

## 前置检查

每次触发时按顺序执行：

1. **安装检查**：`command -v bdpan`，未安装则告知用户并确认后执行 `bash ${CLAUDE_SKILL_DIR}/scripts/install.sh`（用户确认后可加 `--yes` 跳过安装器内部确认）
2. **登录检查**：`bdpan whoami`，未登录则引导执行 `bash ${CLAUDE_SKILL_DIR}/scripts/login.sh`
3. **路径校验**：验证远端路径在 `/apps/bdpan/` 范围内

---

## 确认规则

| 风险等级 | 操作 | 策略 |
|----------|------|------|
| **高（必须确认）** | `rm` 删除、上传/下载目标已存在同名文件 | 列出影响范围，等待用户确认 |
| **中（路径模糊时确认）** | upload、download、mv、rename、cp | 路径明确直接执行，不明确则确认 |
| **低（直接执行）** | ls、search、whoami、mkdir、share | 无需确认 |

**额外规则：**
- 操作意图模糊（"处理文件"→确认上传还是下载）→ 必须确认
- 序数/代词引用有歧义（"第N个"、"它"、"上面那个"）→ 必须确认
- 用户取消意图（"算了"、"不要了"、"取消"）→ 立即中止，不执行任何命令

---

## 核心操作

### 查看状态

```bash
bdpan whoami
```

### 列表查询

```bash
bdpan ls [目录路径] [--json] [--order name|time|size] [--desc] [--folder]
```

### 上传

```bash
bdpan upload <本地路径> <远端路径>
```

**关键约束：** 单文件上传远端路径必须是文件名，禁止以 `/` 结尾。文件夹上传：`bdpan upload ./project/ project/`。

步骤：确认本地路径存在 → 确认远端路径 → `bdpan ls` 检查远端是否已存在 → 执行。

### 下载

**直接下载：**

```bash
bdpan download <远端路径> <本地路径>
```

步骤：`bdpan ls` 确认云端存在 → 确认本地路径 → 检查本地是否已存在 → **检查文件大小决定下载策略** → 执行。若 ls 未找到，建议 `bdpan search <文件名>`。

**大文件下载策略（重要）：**

Agent 的 Bash 工具有执行超时限制，大文件下载可能因超时而中断。必须根据文件大小选择下载策略：

1. **获取文件大小**：用 `bdpan ls --json <远端路径>` 获取 `size` 字段（字节）
2. **按大小分策略执行**：

| 文件大小 | 策略 | 执行方式 |
|----------|------|---------|
| ≤ 50MB | 直接下载 | `bdpan download <远端路径> <本地路径>`，Bash timeout 设为 300000（5 分钟） |
| > 50MB | 后台下载 | 使用 `nohup` 后台执行，Agent 轮询进度 |

**小文件（≤ 50MB）直接下载：**

正常执行 `bdpan download`，Bash 工具 timeout 参数设为 `300000`（5 分钟）。

**大文件（> 50MB）后台下载流程：**

```bash
# 1. 启动后台下载（nohup + 进度日志）
nohup bdpan download <远端路径> <本地路径> > /tmp/bdpan-dl-$$.log 2>&1 & echo $!
```

```bash
# 2. 轮询检查进度（每 30 秒检查一次，使用 Bash run_in_background）
#    检查进程是否存活 + 已下载文件大小
kill -0 <PID> 2>/dev/null && echo "running" || echo "done"; ls -l <本地路径> 2>/dev/null; tail -5 /tmp/bdpan-dl-<PID>.log 2>/dev/null
```

```bash
# 3. 下载完成后清理日志
rm -f /tmp/bdpan-dl-<PID>.log
```

Agent 执行大文件后台下载时的行为规范：
- 启动后台下载后，**立即告知用户**：下载已在后台启动，文件大小 X，预计需要 Y 时间
- 每次轮询后向用户报告进度（已下载大小 / 总大小、百分比）
- 下载完成后告知用户最终结果
- 如果进程异常退出，检查日志并报告错误原因

**分享链接下载（先转存再下载到本地）：**

```bash
bdpan download "https://pan.baidu.com/s/1xxxxx?pwd=abcd" ./downloaded/
bdpan download "https://pan.baidu.com/s/1xxxxx" ./downloaded/ -p abcd    # 提取码单独传入
bdpan download "https://pan.baidu.com/s/1xxxxx?pwd=abcd" ./downloaded/ -t my-folder  # 指定转存目录
```

> 分享链接下载同样适用大文件策略：转存完成后，用 `bdpan ls --json` 获取文件大小，再按上述策略执行下载。

### 转存

将分享文件转存到网盘，**不下载到本地**（与 download 分享链接模式的区别）。

```bash
bdpan transfer "https://pan.baidu.com/s/1xxxxx" -p <提取码> [-d 目标目录] [--json]
```

步骤：确认分享链接格式有效 → 确认有提取码（链接中含 `?pwd=` 或反问用户）→ 确认目标目录 → 执行。转存成功后只展示本次转存的文件（非整个目录），显示数量和目标目录。

### 分享

```bash
bdpan share <路径> [路径...] [--period <天数>] [--json]
```

**--period / -d 参数：** 分享有效期（天），取值：0=永久, 1, 7, 30（默认：7）

**智能选择规则：**

Agent 必须根据用户的语义意图判断有效期，而非仅匹配固定关键词。

- 用户表达了"希望长期有效/永久/不过期/一直能用"等语义 → 使用 `--period 0`，并提示用户：永久链接无法自动过期，请注意文件安全
- 用户指定了具体天数或时间范围 → 选择最接近的枚举值（1、7、30）
- 用户未表达任何有效期偏好 → 默认 `--period 7`

步骤：`bdpan ls` 确认文件存在 → 根据用户意图选择有效期 → 执行分享 → 展示链接+提取码+有效期。

> 付费接口，需在百度网盘开放平台购买服务。

### 搜索

```bash
bdpan search <关键词> [--category 0-7] [--no-dir|--dir-only] [--page-size N] [--page N] [--json]
```

category：0=全部 1=视频 2=音频 3=图片 4=文档 5=应用 6=其他 7=种子。`--no-dir` 和 `--dir-only` 互斥。

### 移动 / 复制 / 重命名 / 创建文件夹

```bash
bdpan mv <源路径> <目标目录>
bdpan cp <源路径> <目标目录>
bdpan rename <路径> <新名称>       # 第二参数是文件名，非完整路径
bdpan mkdir <路径>
```

---

## 路径规则

| 场景 | 格式 | 示例 |
|------|------|------|
| **命令参数** | 相对路径（相对于 `/apps/bdpan/`） | `bdpan upload ./f.txt docs/f.txt` |
| **展示给用户** | 中文名 | "已上传到：我的应用数据/bdpan/docs/f.txt" |

映射关系：`我的应用数据` ↔ `/apps`

**禁止：** 命令中使用中文路径（`我的应用数据/...`）、展示时暴露 API 路径（`/apps/bdpan/...`）。

---

## 授权码处理

用户发送 32 位十六进制字符串时，先确认："这是百度网盘授权码吗？确认后将执行登录流程。" 确认后执行 `bash ${CLAUDE_SKILL_DIR}/scripts/login.sh`（不使用 `--yes`，保留安全确认环节）。

---

## 管理功能

### 安装

```bash
bash ${CLAUDE_SKILL_DIR}/scripts/install.sh [--yes]
```

安装器从百度 CDN（`issuecdn.baidupcs.com`）下载并执行。注意：install.sh 不执行本地 SHA256 校验，完整性依赖 HTTPS 传输保护。安全敏感场景建议先手动审查安装器内容或在沙箱中执行。

### 登录 / 注销 / 卸载

```bash
bash ${CLAUDE_SKILL_DIR}/scripts/login.sh              # 登录（内置安全免责声明）
bdpan logout                                            # 注销
bash ${CLAUDE_SKILL_DIR}/scripts/uninstall.sh [--yes]   # 卸载
```

### 更新（必须用户明确指令触发）

```bash
bash ${CLAUDE_SKILL_DIR}/scripts/update.sh              # 检查并更新（需用户确认）
bash ${CLAUDE_SKILL_DIR}/scripts/update.sh --check       # 仅检查更新
```

---

## 记忆备份与恢复

仅支持 4 种 Claw 产品（kimiclaw、maxclaw、[PLATFORM_NAME]、openclaw），自动检测当前环境。

**网盘存储路径：** `/apps/bdpan/agent-memory/<agent>/<device>/manual/<timestamp>/`

**备份内容：** 7 个 Workspace 文件（AGENTS.md、SOUL.md、USER.md、IDENTITY.md、TOOLS.md、MEMORY.md、HEARTBEAT.md）+ `memory/*.md` + `manifest.json`

### 备份记忆

```bash
bash ${CLAUDE_SKILL_DIR}/scripts/memory-backup.sh backup
```

### 查看备份列表

```bash
bash ${CLAUDE_SKILL_DIR}/scripts/memory-backup.sh list
```

### 恢复备份

```bash
# 按日期模糊匹配（如只输入日期部分）
bash ${CLAUDE_SKILL_DIR}/scripts/memory-backup.sh restore 2026-03-16

# 跳过兼容性警告强制恢复（跨 Agent 类型时使用）
bash ${CLAUDE_SKILL_DIR}/scripts/memory-backup.sh restore 2026-03-16 --force
```

**恢复安全机制：** 恢复前自动将当前本地记忆备份到 `<workspace>/.backup-before-restore/<timestamp>/`，防止误操作数据丢失。

### 操作流程

1. 执行前自动检查：bdpan 是否安装 → 是否已登录（未满足则引导处理）
2. 检测当前 Agent 类型 → 不支持的环境报错退出
3. 执行对应操作（backup/list/restore）

---

## 参考文档

遇到对应问题时按需查阅，无需预加载：

| 文档 | 何时查阅 |
|------|---------|
| [bdpan-commands.md](./reference/bdpan-commands.md) | 需要完整命令参数、选项、JSON 输出格式 |
| [authentication.md](./reference/authentication.md) | 认证流程细节、Token 管理 |
| [examples.md](./reference/examples.md) | 更多使用示例（批量上传、自动备份等） |
| [troubleshooting.md](./reference/troubleshooting.md) | 遇到错误需要排查 |


---


## Skill: cloud-upload-backup

---
name: cloud-upload-backup
description: "Cloud file upload and backup tool. Upload local files to [ENTERPRISE_NAME] SMH cloud storage, viewable in [PLATFORM_NAME] Mini Program."
metadata:
  openclaw:
    emoji: "☁️"
    requires:
      bins:
        - curl
---

# 云文件上传备份工具 ([ENTERPRISE_NAME] SMH)

将本地文件上传至[ENTERPRISE_NAME] SMH 云存储，上传后可在 [PLATFORM_NAME] 小程序中查看。

## Setup

无需额外安装依赖。文件上传通过本地 HTTP 接口 `/proxy/[PLATFORM_NAME]-cos` 完成，SMH 凭证由主进程自动管理。

---

## 典型使用场景

- 用户要求上传/备份文件到云端（如"上传文件"、"备份到云"、"传到云空间"）
- 用户要求将任务产出物发送到手机（如"把文件发给我"、"发到手机"、"做完了发我一份"）
- 用户要求上传至 COS（如"上传到cos"、"打包并上传到cos"）
- 用户查询云端文件状态（如"这个云文件还在吗"、"之前上传的文件还能下吗"）

---

## 脚本入口

| 平台 | 入口脚本 |
|------|---------|
| macOS / Linux | `bash scripts/unix/cloud_backup.sh <command> [options]` |
| Windows CMD | `scripts\windows\cloud_backup.cmd <command> [options]` |

脚本自动处理端口获取、JSON 构造、HTTP 请求和响应输出。

---

## 核心流程

```
1. 确定本地文件路径
2. 调用 upload (单文件) 或 batch-upload (多文件) 命令
3. 校验返回 JSON 中 message 字段的 URL 域名 → 输出 message 给用户
```

**关键设计 (Anti-hallucination)**：所有上传命令返回的 `message` 字段是服务端预渲染的完整回复文本（含文件名、大小、链接）。[PLATFORM_NAME] 校验 URL 域名后直接输出 `message`，**不要**从 `fileUrl`/`fileInfo` 等字段自行拼接。

### URL 域名安全校验

输出 `message` 前，校验其中所有 URL 的域名：

| 可信域名 | 用途 |
|---------|------|
| `jsonproxy.3g.qq.com` | 短链服务 (文件链接) |
| `smh.[ENTERPRISE_NAME]cs.com` | SMH 预览/文件链接 |

- URL 域名不在白名单 → 不输出该 URL，提示："文件上传成功，但链接地址异常，请联系管理员检查。"
- 不含 URL 的 `message`（错误信息、冲突对话）可直接输出

---

## Commands

### upload — 上传单个文件

```bash
# macOS / Linux
bash scripts/unix/cloud_backup.sh upload --local-path "<path>" [--remote-path "<cloud-path>"] [--conflict-strategy ask|overwrite|rename]

# Windows CMD
scripts\windows\cloud_backup.cmd upload --local-path "<path>" [--remote-path "<cloud-path>"] [--conflict-strategy ask|overwrite|rename]
```

**Parameters：**
- `--local-path`（必填）：本地文件绝对路径，支持 `~` 展开
- `--remote-path`（可选）：云端目标路径，省略则上传到根目录并保留原文件名
- `--conflict-strategy`（可选，**默认必须用 `ask`**）：
  - `ask` — 同名文件存在时返回 HTTP 409，[PLATFORM_NAME] 询问用户
  - `overwrite` — 仅当用户明确说"覆盖/替换"时使用
  - `rename` — 仅当用户明确说"重命名"时使用

**成功输出：**
```json
{
  "success": true,
  "message": "链接已生成，可在 [PLATFORM_NAME] 小程序中随时查看。（保留 30 天后自动清理）\n\n已上传文件: photo.jpg (2.0 MB)\n文件链接: https://jsonproxy.3g.qq.com/urlmapper/aB3xYz",
  "fileUrl": "https://jsonproxy.3g.qq.com/urlmapper/aB3xYz"
}
```

→ 校验 `message` 中 URL 域名，通过后直接输出 `message` 内容给用户。

**HTTP 409 — 同名文件冲突：**
```json
{
  "success": false,
  "message": "已存在同名文件 `report.pdf`，你想怎么处理？\n\n1. 🔄 覆盖 — 替换已有文件\n2. 📝 重命名 — 自动改名上传（如 report(1).pdf）\n3. ❌ 取消 — 不上传",
  "conflict": { "fileName": "report.pdf", "remotePath": "report.pdf" }
}
```

→ 输出 `message`，等用户选择后用对应策略重新上传。

**失败输出：**
```json
{
  "success": false,
  "message": "❌ 文件上传失败：文件不存在: /path/to/missing.pdf\n\n你可以：\n1. 🔄 重试\n2. ❌ 取消",
  "error": "文件不存在: /path/to/missing.pdf"
}
```

→ 直接输出 `message`（错误信息通常不含 URL，无需域名校验）。

> 上传成功后短链格式为 `https://jsonproxy.3g.qq.com/urlmapper/xxx`。手机端点击拉起 [PLATFORM_NAME] 小程序（文件保留 30 天），PC 端打开 H5 扫码页。`rawDownloadUrl` 仅在短链生成失败时作为 fallback 出现。

### batch-upload — 批量上传多个文件

**2 个及以上文件时必须用此命令**，不要多次调用 `upload`。

```bash
# macOS / Linux
bash scripts/unix/cloud_backup.sh batch-upload --files '<JSON array>'

# Windows CMD
scripts\windows\cloud_backup.cmd batch-upload --files "<JSON array>"
```

**`--files` JSON 格式**（最多 20 个）：
```json
[{"localPath":"/path/to/file1.pdf","conflictStrategy":"ask"},{"localPath":"/path/to/file2.docx","conflictStrategy":"ask"}]
```

每项：`localPath`（必填）、`remotePath`（可选）、`conflictStrategy`（可选，默认 `ask`）

> Windows CMD 中 JSON 需转义双引号：`"[{\"localPath\":\"C:\\path\\to\\file.pdf\",\"conflictStrategy\":\"ask\"}]"`

**成功输出：**
```json
{
  "success": true,
  "message": "3 个文件全部上传成功！链接可在 [PLATFORM_NAME] 小程序中随时查看。（保留 30 天后自动清理）\n\n📎 report.pdf (2.3 MB) — https://jsonproxy.3g.qq.com/urlmapper/aB3xYz\n📎 photo.jpg (1.1 MB) — https://jsonproxy.3g.qq.com/urlmapper/xK9mWq\n📎 data.csv (156 KB) — https://jsonproxy.3g.qq.com/urlmapper/pL2nRt",
  "total": 3, "successCount": 3, "failedCount": 0
}
```

→ 校验 URL 域名后输出 `message`。不要自行汇总/改写批量结果。

### info — 查询云端文件信息

```bash
# macOS / Linux
bash scripts/unix/cloud_backup.sh info --remote-path "report.pdf"

# Windows CMD
scripts\windows\cloud_backup.cmd info --remote-path "report.pdf"
```

### list — 列出云端文件

```bash
# macOS / Linux
bash scripts/unix/cloud_backup.sh list [--dir-path "/"] [--limit 50]

# Windows CMD
scripts\windows\cloud_backup.cmd list [--dir-path "/"] [--limit 50]
```

---

## 文件大小

**无文件大小限制。** 小文件 (≤50MB) 直接上传，大文件 (>50MB) 自动分片上传 (5MB chunks)。不要告诉用户有大小限制。

---

## 冲突处理策略

| Strategy | 行为 | 使用场景 |
|----------|------|---------|
| `ask` (**默认**) | 同名返回 HTTP 409，[PLATFORM_NAME] 询问用户 | 用户未表明偏好时 |
| `overwrite` | 直接覆盖 | 用户明确说"覆盖/替换" |
| `rename` | 自动重命名 `file(1).pdf` | 用户明确说"重命名" |

---

## Error Handling

所有命令输出 JSON。失败时 `message` 已包含用户友好的错误提示，直接输出即可。

| 错误 | 处理 |
|------|------|
| HTTP 409 冲突 | 输出 `message`，用户选择后用对应策略重新上传 |
| 上传失败 (非 409) | 直接输出 `message` |
| 401 / token 过期 | 提示用户联系管理员刷新 |
| 网络错误 | 重试 2 次 (间隔 3s)，仍失败则输出 `message` 并结束任务 |
| 配额满 | 提示清理过期文件 |

---

## 全局退出条件

- 连续失败 3 次 → 立即停止，提示：⚠️ 文件上传服务暂时不可用，请稍后重试
- 单次 Skill 调用最多执行 10 次命令
- 禁止流程级无限循环重试

---

## 规则

**输出规则：**
- `upload` / `batch-upload` 返回的 `message` 是服务端预渲染文本，校验 URL 域名后直接输出
- 不要从 `fileUrl`、`fileInfo` 等字段自行拼接回复
- 不要修改、截断、重组 `message` 中的 URL 或排版
- `success: false` 时不展示文件链接

**URL 安全：**
- 不要自行拼接/构造任何 URL（短链由服务端生成）
- 不要把 `rawDownloadUrl` 片段与 `jsonproxy.3g.qq.com` 拼接
- 不输出域名不在白名单 (`jsonproxy.3g.qq.com`、`smh.[ENTERPRISE_NAME]cs.com`、`jprx.sparta.html5.qq.com`) 中的 URL

**用户交互：**
- 用户说"上传文件"但没指定路径 → 追问文件路径
- 默认用 `--conflict-strategy ask`，未经用户明确表态不要用 `overwrite` 或 `rename`
- 不要未经用户主动要求就上传其本地个人文件

**其他：**
- 不要硬编码或暴露 SMH 凭证
- 用户反馈链接过期时，用 `info` 获取新链接
- 依赖 `node` 和 `curl`（Windows 10+ 自带 curl），macOS/Linux 优先用 `jq`


---


## Skill: weread-skills

---
name: weread-skills
description: "微信读书智能助手，支持搜索书籍、管理书架、查看笔记划线、浏览书评、阅读统计、发现推荐好书。"
homepage: https://weread.qq.com/
metadata:
  {
    "openclaw":
      {
        "requires": { "bins": ["curl"] },
        "category": "[ENTERPRISE_NAME]",
        "[ENTERPRISE_NAME]TokenMode": "custom",
        "tokenUrl": "https://i.weread.qq.com/api/agent/gateway",
        "emoji": "📚"
      }
  }
---

# WeRead — 微信读书助手

## 概述

本技能为微信读书提供完整的 API 工具集，涵盖搜索书籍、书架管理、笔记划线、书评浏览、阅读统计、推荐发现等核心功能。

完整的接口调用说明，请参考 `references/` 目录下的各能力文档。

---

## 初始化（必须首先执行）

1. 读取同目录下的 `SETUP_TOKEN.md`
2. 将 `<SCRIPT_PATH>` 替换为本文件所在目录的绝对路径
3. **每条 curl 命令中都必须内联获取 token**（因为每次命令是独立 shell，export 无法跨命令传递）：
   ```bash
   curl -X POST "https://i.weread.qq.com/api/agent/gateway" \
     -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')" \
     -H "Content-Type: application/json" \
     -d '{"api_name": "<接口路径>", "skill_version": "1.0.3", ...其他参数}'
   ```
   - Windows PowerShell：
   ```powershell
   [Console]::OutputEncoding = [System.Text.Encoding]::UTF8
   $token = & "<SCRIPT_PATH>\get-token.ps1"
   curl.exe -X POST "https://i.weread.qq.com/api/agent/gateway" -H "Authorization: Bearer $token" -H "Content-Type: application/json" -d '{\"api_name\": \"<接口路径>\", \"skill_version\": \"1.0.3\", ...其他参数}'
   ```
4. 若脚本报错，提示用户在**应用内集成面板**中完成微信读书授权（不要引导用户手动去网页获取 Key）

---

## 环境配置

**运行环境**：依赖 curl 命令行工具（macOS/Linux/Windows 均内置）。

**Token 配置**：Token 通过 `get-token.sh` / `get-token.ps1` 从本地凭证代理自动获取，在每条 curl 命令中内联调用。若脚本报错，提示用户在应用内集成面板中完成微信读书授权。

---

## ⚠️ 调用方式（必读）

所有接口操作**必须**通过 curl 命令调用统一网关完成，且**每条命令必须内联获取 Token**。

**命令格式**：
```bash
# macOS / Linux
curl -X POST "https://i.weread.qq.com/api/agent/gateway" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')" \
  -H "Content-Type: application/json" \
  -d '{"api_name": "<接口路径>", "skill_version": "1.0.3", ...其他参数}'
```

```powershell
# Windows PowerShell
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
$token = & "<SCRIPT_PATH>\get-token.ps1"
curl.exe -X POST "https://i.weread.qq.com/api/agent/gateway" -H "Authorization: Bearer $token" -H "Content-Type: application/json" -d '{\"api_name\": \"<接口路径>\", \"skill_version\": \"1.0.3\", ...其他参数}'
```

> ⚠️ **Windows 编码**：Windows PowerShell 默认编码非 UTF-8，会导致中文乱码。**每次调用前必须**执行 `[Console]::OutputEncoding = [System.Text.Encoding]::UTF8` 设置编码。

**快速示例**：
```bash
# 搜索书籍
curl -X POST "https://i.weread.qq.com/api/agent/gateway" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')" \
  -H "Content-Type: application/json" \
  -d '{"api_name": "/store/search", "keyword": "三体", "count": 10, "skill_version": "1.0.3"}'

# 查看所有可用接口
curl -X POST "https://i.weread.qq.com/api/agent/gateway" \
  -H "Authorization: Bearer $(bash '<SCRIPT_PATH>/get-token.sh')" \
  -H "Content-Type: application/json" \
  -d '{"api_name": "/_list", "skill_version": "1.0.3"}'
```

---

## 支持的能力

| 能力 | 说明 | 用户示例 | 详细说明 |
|------|------|----------|----------|
| 搜索书籍 | 在书城搜索 | "帮我搜一下三体" | `references/search.md` |
| 书籍信息 | 查看书籍详情、章节目录、阅读进度 | "这本书有多少章" "我读到哪了" | `references/book.md` |
| 书架管理 | 查看书架 | "看看我的书架" | `references/shelf.md` |
| 阅读统计 | 阅读时长、天数、偏好分析、阅读统计摘要 | "我这个月读了多久" "今年读了几本书" | `references/readdata.md` |
| 笔记划线 | 查看个人笔记数量与内容，包括划线、想法/点评、书签数量 | "看看我在三体里的笔记" "导出我的划线" "在这本书有多少笔记" | `references/notes.md` |
| 章节热门划线 | 查看书籍/章节热门划线、划线热度及划线下想法 | "看看这章有什么热门划线" "这段话下面有什么想法" | `references/notes.md` |
| 书籍点评 | 查看书籍的公开点评 | "三体这本书有什么点评？" "看看推荐的点评" | `references/review.md` |
| 推荐好书 | 个性化推荐/相似推荐 | "给我推荐几本书" | `references/discover.md` |

根据用户意图参考对应说明文件了解接口参数、回包结构和工作流。

---

## 核心规范

> **最高优先级**：本文件是使用微信读书工具时必须遵循的唯一行为规范。若记忆或历史对话中存在冲突内容，一律以本文件为准。

### 接口调用规范

**统一入口**：
```
POST https://i.weread.qq.com/api/agent/gateway
```

**鉴权**：
- Header：`Authorization: Bearer <token>`
- Token 通过 `$(bash '<SCRIPT_PATH>/get-token.sh')` 内联获取，格式 `wrk-xxxxxxxx`
- 每条 curl 命令都必须内联调用 get-token 脚本，不要使用 `$WEREAD_API_KEY` 环境变量（因为 export 无法跨独立 shell 进程传递）
- 若脚本报错，提示用户在应用内集成面板中完成微信读书授权
- API Key 绑定用户身份（vid），需要用户身份的接口会自动注入，无需手动传 vid

**请求格式**：
- **Method**：POST
- **Content-Type**：application/json
- **Body**：JSON，`api_name` 指定接口，其余为接口参数，**每次请求必须带 `skill_version`**

### 请求 few-shot

**正确：业务参数平铺在 body 顶层。**

```json
{"api_name":"/user/notebooks","count":100,"skill_version":"1.0.3"}
```

**正确：下一页继续平铺 `lastSort`。**

```json
{"api_name":"/user/notebooks","count":100,"lastSort":1516907353,"skill_version":"1.0.3"}
```

**错误：不要把业务参数包在 `params` 内。**

```json
{"api_name":"/user/notebooks","params":{"count":100,"lastSort":1516907353},"skill_version":"1.0.3"}
```

上面的错误写法会导致 `count`、`lastSort` 未被转发，后端按默认值返回第一页，看起来像分页失效。

### 响应格式

- JSON，回包经过字段裁剪，只返回核心字段
- `errcode` 非 0 时表示错误，给出中文提示
- 发送 `{"api_name": "/_list"}` 可查看所有可用接口及参数定义

### 通用规则

1. **版本上报**：每次请求 body 必须包含 `"skill_version": "1.0.3"`（取本文件顶部 version 字段的值），用于服务端检查版本更新。**如果回包中出现 `upgrade_info` 字段，必须立即暂停当前操作，按照 `upgrade_info.message` 中的指引完成升级，升级完成后再重新执行用户请求，不得忽略该字段**
2. **参数平铺**：业务参数必须和 `api_name`、`skill_version` 放在同一层；不要包在 `params`、`data`、`body` 等对象里。只有接口文档明确声明的数组/对象字段（如 `/book/readreviews` 的 `reviews`）才允许作为业务字段传入。
3. **能力文档预检**：调用任何接口前，必须先根据「支持的能力」表阅读对应说明文件（如阅读统计先读 `references/readdata.md`，书架先读 `references/shelf.md`），确认接口参数、字段含义、单位、计数口径和工作流；禁止仅凭字段名或经验猜测含义。
4. **字段解释优先级**：解释接口回包时，以对应说明文件中的字段说明为准；如果回包字段名和直觉含义冲突，必须服从说明文件，不得直接翻译字段名。
5. **bookId 解析**：用户输入书名时，先调 `/store/search` 获取 bookId，再执行后续操作
6. **书架数量**：使用 `/shelf/sync` 回答"书架有多少本书/多少条目"时，必须按 `books.length + albums.length + (mp 非空 ? 1 : 0)` 计算；`albums[]` 是专辑/有声书，也属于书架里的书，详细规则见 `references/shelf.md`
7. **结果展示**：列表用编号展示方便选择；搜索结果重点展示书名、作者、评分；展示接口回包信息时，字段**禁止**直接翻译，应该参考文件中的说明内容提供
8. **上下文衔接**：对话中记住已查询的 bookId，后续操作无需用户重复提供
9. **深度链接**：在展示划线、想法、章节等内容时，拼接对应的跳转链接方便用户直接在 App 中打开，具体格式见下方「深度链接（URL Schema）」章节
10. **数据展示规范**：
   - **时间戳**：所有 Unix 时间戳字段（如 `updateTime`、`createTime`、`finishTime`、`readUpdateTime` 等），**展示时须转为 YYYY-MM-DD 格式**（如 `1748563200` 展示为"2025-05-30"），不得直接展示原始数字
   - **阅读时长**：单位为秒，展示时转为"X小时Y分钟"格式

### 错误处理

工具调用返回错误时：
- `errcode` 非 0：按错误码给出中文提示
- HTTP 401/403：提示用户检查 API Key 是否有效
- 网络超时：提示用户检查网络连接

---

## 触发场景

### 适用场景

| 用户意图 | 使用接口 |
|---------|---------|
| 搜索书籍 | `/store/search` |
| 查看书籍详情 | `/book/info` |
| 查看章节目录 | `/book/chapterinfo` |
| 查看阅读进度 | `/book/getprogress` |
| 查看书架 | `/shelf/sync` |
| 查看阅读统计 | `/readdata/detail` |
| 查看笔记概览 | `/user/notebooks` |
| 查看划线内容 | `/book/bookmarklist` |
| 查看个人想法/点评 | `/review/list/mine` |
| 查看热门划线 | `/book/bestbookmarks` |
| 查看章节划线热度 | `/book/underlines` |
| 查看划线下想法 | `/book/readreviews` |
| 查看书籍公开点评 | `/review/list` |
| 个性化推荐 | `/book/recommend` |
| 相似书推荐 | `/book/similar` |

### 不触发场景

微信聊天、微信支付、微信公众号管理、微信小程序开发、其他阅读平台（Kindle/豆瓣/得到）

---

## 深度链接（URL Schema）

在展示书籍、章节、划线等内容时，如果回包字段足以构造链接，应附上对应的跳转链接，方便用户点击后直接在微信读书 App 中打开对应位置。想法/点评不一定都有划线位置，只有具备 `chapterUid` 和 `range` 时才生成划线位置链接。

### 打开书籍（跳转到上次阅读进度）

```
weread://reading?bId={bookId}
```

| 参数 | 说明 | 来源 |
|------|------|------|
| `bookId` | 书籍 ID | 各接口返回的 `bookId` |

**示例**：

```
weread://reading?bId=3300045871
```

**使用场景**：
- 展示书架列表时，每本书附上跳转链接
- 展示搜索结果时，附上「打开阅读」链接
- 展示阅读进度时，提供「继续阅读」链接

### 跳转到指定章节

```
weread://reading?bId={bookId}&chapterUid={chapterUid}
```

| 参数 | 说明 | 来源 |
|------|------|------|
| `bookId` | 书籍 ID | 各接口返回的 `bookId` |
| `chapterUid` | 章节 UID | `/book/chapterinfo` 返回的 `chapters[].chapterUid` |

**示例**：

```
weread://reading?bId=3300045871&chapterUid=107
```

**使用场景**：
- 展示章节目录时，每个章节附上跳转链接

### 跳转到划线/想法所在位置

```
weread://bestbookmark?bookId={bookId}&chapterUid={chapterUid}&rangeStart={rangeStart}&rangeEnd={rangeEnd}&userVid={userVid}
```

| 参数 | 说明 | 来源 |
|------|------|------|
| `bookId` | 书籍 ID | 各接口返回的 `bookId` |
| `chapterUid` | 章节 UID | 划线/想法所属的 `chapterUid` |
| `rangeStart` | 划线起始位置 | `range` 字段中 `-` 前面的数字 |
| `rangeEnd` | 划线结束位置 | `range` 字段中 `-` 后面的数字 |
| `userVid` | 用户 VID | API Key 鉴权后自动关联的用户 ID（从 `/shelf/sync` 等接口的上下文获取，或省略） |

> **range 解析**：划线接口返回的 `range` 格式为 `"起始-结束"`（如 `"900-2004"`），拆分后分别填入 `rangeStart` 和 `rangeEnd`。

**示例**：

```
weread://bestbookmark?bookId=3300045871&chapterUid=107&rangeStart=900&rangeEnd=2004&userVid=583802764
```

**使用场景**：
- 展示划线列表（`/book/bookmarklist`）时，每条划线附上跳转链接（`range` 字段可直接解析）
- 展示热门划线（`/book/bestbookmarks`）时，每条附上跳转链接；`/book/underlines` 只是划线热度统计，不含划线文本
- 展示想法（`/review/list/mine`、`/book/readreviews`）时，只有返回内容包含 `chapterUid` 和 `range` 时才附上跳转到对应划线位置的链接；整本书评或无法定位到划线的点评不强制生成该链接


---


## Skill: xiaohongshu

---
name: xiaohongshu
description: |
  小红书内容工具。使用场景：
  - 搜索小红书内容
  - 获取首页推荐列表
  - 获取帖子详情（包括互动数据和评论）
  - 发表评论到帖子
  - 获取用户个人主页
  - "跟踪一下小红书上的XX热点"
  - "分析小红书上关于XX的讨论"
  - "小红书XX话题报告"
  - "生成XX的小红书舆情报告"
metadata:
  openclaw:
    emoji: "📕"
---

# 小红书 MCP Skill

基于 [xiaohongshu-mcp](https://[ENTERPRISE_NAME].com/xpzouying/xiaohongshu-mcp) 封装。

> **完整文档请查看 [README.md](README.md)**

## 快速参考

| 脚本 | 用途 |
|------|------|
| `install-check.sh` | 检查依赖是否安装 |
| `start-mcp.sh` | 启动 MCP 服务 |
| `stop-mcp.sh` | 停止 MCP 服务 |
| `status.sh` | 检查登录状态 |
| `search.sh <关键词>` | 搜索内容 |
| `recommend.sh` | 获取推荐列表 |
| `post-detail.sh <feed_id> <xsec_token>` | 获取帖子详情 |
| `comment.sh <feed_id> <xsec_token> <内容>` | 发表评论 |
| `user-profile.sh <user_id>` | 获取用户主页 |
| `track-topic.sh <话题> [选项]` | 热点跟踪报告 |
| `export-long-image.sh` | 帖子导出为长图（白底黑字+图片拼接） |
| `mcp-call.sh <tool> [args]` | 通用工具调用 |

## 快速开始

```bash
cd scripts/

# 1. 检查依赖
./install-check.sh

# 2. 启动服务
./start-mcp.sh

# 3. 检查状态
./status.sh

# 4. 搜索内容
./search.sh "春节旅游"
```

## MCP 工具

| 工具名 | 描述 |
|--------|------|
| `check_login_status` | 检查登录状态 |
| `search_feeds` | 搜索内容 |
| `list_feeds` | 获取首页推荐 |
| `get_feed_detail` | 获取帖子详情和评论 |
| `post_comment_to_feed` | 发表评论 |
| `reply_comment_in_feed` | 回复评论 |
| `user_profile` | 获取用户主页 |
| `like_feed` | 点赞/取消 |
| `favorite_feed` | 收藏/取消 |
| `publish_content` | 发布图文笔记 |
| `publish_with_video` | 发布视频笔记 |

## 热点跟踪

```bash
./track-topic.sh "DeepSeek" --limit 5
./track-topic.sh "春节旅游" --limit 10 --output report.md
./track-topic.sh "iPhone 16" --limit 5 --feishu
```

## 长图导出

搜索结果或帖子详情导出为带文字注释的 JPG 长图：

```bash
# 准备 posts.json（搜索+拉详情后整理）
cat > posts.json << 'EOF'
[
  {
    "title": "帖子标题",
    "author": "作者名",
    "stats": "1.3万赞 100收藏",
    "desc": "正文摘要，支持\n换行",
    "images": ["https://...webp", "https://...webp"],
    "per_image_text": {"1": "第2张图的专属说明"}
  }
]
EOF

./export-long-image.sh --posts-file posts.json -o output.jpg
```

样式：白底黑字（模仿小红书原样），每个帖子前有文字块（标题+作者+正文），帖子间有分隔线。

**per_image_text** 可选：如果原帖文字明确指向某张图（如"图7-9是青龙桥"），把说明放在对应图片上方。未指定则所有文字集中在文字块。

**字体**：自动检测系统中文字体（STHeiti > Hiragino > Arial Unicode > Noto CJK）。

## 注意事项

- 首次运行会下载 headless 浏览器（~150MB）
- 同一账号避免多客户端同时使用
- 发布限制：标题≤20字符，正文≤1000字符，日发布≤50条
- Linux 服务器需要从本地获取 cookies，详见 [README.md](README.md)


---


## Skill: email-skill

---
name: email-skill
description: "当用户提到邮箱相关能力时，进入这个skill，统一邮件入口（纯路由层）：识别用户意图后路由到 public-skill 或 imap-smtp-email，自身不执行任何脚本"
version: "3.0"
trigger_keywords:
  - 发邮件
  - 发送邮件
  - 写邮件
  - 邮箱
  - email
  - send email
  - 收邮件
  - 查邮件
  - 检查邮箱
  - 收件箱
  - inbox
  - 搜索邮件
  - 查找邮件
  - 下载附件
  - 邮件附件
  - 绑定邮箱
  - 邮箱绑定
  - 日报推送
  - 天气推送
  - 报告推送
  - 提醒推送
  - 邮件通知
  - 邮件推送
  - 推送到邮箱
  - 消息留存
exclude_when:
  - 操作与邮件完全无关（如日历、文件管理、聊天）
  - 用户要求发送短信或站内信（非邮件渠道）
---

# Email Skill（统一邮件入口 / 纯路由层）

> **定位**：`email-skill` 是所有邮件需求的唯一入口。它**只做意图识别和路由分发**，不执行任何脚本，不调用任何接口。识别用户意图后，直接将任务交给下游 skill 执行。

## 1. 架构总览

| Skill | 角色 | 说明 |
|-------|------|------|
| **email-skill**（本 skill） | 统一入口 / 纯路由层 | 识别意图 → 路由到下游 skill，自身不执行任何操作 |
| **public-skill** | 平台公邮通道 | 零配置，把内容推送到用户自己的邮箱（纯文本） |
| **imap-smtp-email** | 个人邮箱通道 | 需配置，支持完整 IMAP/SMTP 邮件收发能力 |

```
用户邮件需求
     │
     ▼
 email-skill（意图识别）
     │
     ├── 推送到自己邮箱 / 绑定公邮 ──► public-skill
     │
     └── 完整邮件收发 ──► imap-smtp-email
```

## 2. 路由规则

### 2.1 核心原则：先理解场景，再选路径

**不要**先检查公邮是否可用再决定路径。**应该**先理解用户要解决什么问题，再选择对应的 skill。

### 2.2 路由到 `public-skill` 的条件

同时满足以下**所有**条件时，路由到 `public-skill`：

- 发送对象是**用户自己**（没有第三方收件人）
- 内容是**纯文本**（不需要 HTML）
- 不需要**附件、抄送、密送**
- 不需要**收件、检索、下载附件**
- 场景属于"结果推送 / 消息留存"类（天气、日报、报告、提醒等）

### 2.3 路由到 `imap-smtp-email` 的条件

出现以下**任一**信号，直接路由到 `imap-smtp-email`：

- 需要发给第三方收件人
- 需要抄送（cc）或密送（bcc）
- 需要附件
- 需要 HTML 邮件
- 需要收件箱查询、搜索、拉取详情、下载附件
- 需要标记已读/未读、列出邮箱文件夹
- 用户明确说"发给别人""带附件""查收件箱""下载附件""正式邮件""抄送某人"
- 平台公邮不可用，需要兜底发送

### 2.4 快速判断表

| 用户意图信号 | 路由目标 |
|-------------|---------|
| "发到我的邮箱""推送到我邮箱" | `public-skill` |
| "天气/日报/报告 发到我邮箱" | `public-skill` |
| "绑定邮箱""绑定公邮" | `public-skill` |
| "发给 xxx@example.com" | `imap-smtp-email` |
| "抄送""密送" | `imap-smtp-email` |
| "带附件""发 PDF" | `imap-smtp-email` |
| "HTML 邮件""富文本" | `imap-smtp-email` |
| "查收件箱""搜索邮件""最近的邮件" | `imap-smtp-email` |
| "下载附件" | `imap-smtp-email` |
| "配置邮箱""设置个人邮箱" | `imap-smtp-email` |

## 3. 两条路径的能力对比

| 能力 | public-skill | imap-smtp-email |
|------|:------------:|:---------------:|
| 发送到自己的邮箱 | ✅ | ✅ |
| 发给第三方收件人 | ❌ | ✅ |
| 抄送 / 密送 | ❌ | ✅ |
| 附件 | ❌ | ✅ |
| HTML 邮件 | ❌ | ✅ |
| 收件 / 搜索 / 下载附件 | ❌ | ✅ |
| 零配置 | ✅ | ❌ |
| 需要授权码/密码 | ❌ | ✅ |

## 4. 场景示例

### 路由到 `public-skill` 的场景

| 用户需求 | 原因 |
|---------|------|
| "查一下深圳明天天气，发到我邮箱" | 结果推送到自己邮箱，纯文本，零配置 |
| "每天下班把日报推到我邮箱" | 只发给自己，消息留存 |
| "把这段总结发到我的邮箱保存" | 推送到自己邮箱做留存 |
| "帮我绑定邮箱" | 公邮绑定流程 |

### 路由到 `imap-smtp-email` 的场景

| 用户需求 | 原因 |
|---------|------|
| "用我的 Gmail 发给客户" | 第三方收件人 |
| "给团队发周报并抄送 PM" | 需要抄送 |
| "发 PDF 附件给合作方" | 需要附件 |
| "查最近两小时的发票邮件" | 需要 IMAP 检索 |
| "下载那封邮件的附件" | 需要附件下载 |
| "配置我的 QQ 邮箱" | 个人邮箱配置 |
| "公邮发不了，用个人邮箱发" | 公邮不可用，兜底 |

## 5. 回退策略

当路由到 `public-skill` 后执行失败，且失败原因属于以下类型时，应自动回退到 `imap-smtp-email`：

**可回退的失败类型：**
- 平台公邮日发送上限 / 额度不足
- 通道不可用 / 连接失败
- 请求超时
- 登录已过期 / 未登录
- 网关层通信错误

**不可回退的失败类型（应直接报错）：**
- 邮箱未绑定平台公邮（应引导用户走绑定流程）
- 参数校验错误
- 频率限制

## 6. 安全规则

1. **禁止泄露授权码、SMTP/IMAP 密码、验证码等敏感信息**
2. **发送邮件为非幂等操作，失败后禁止自动重试**
3. **附件路径必须落在允许读取目录内**
4. **下载附件路径必须落在允许写入目录内**
5. **必须用utf8编码**
6. **禁止使用代码块：回答中不得出现任何形式的代码块（包括行内代码 `...` 和围栏代码块 ```...```），应使用纯自然语言进行描述**。

## 7. 一句话记忆

- **推给自己做留存** → `public-skill`
- **像正常邮箱那样完整收发** → `imap-smtp-email`


---


## Skill: imap-smtp-email

---
name: imap-smtp-email
description: 通过 IMAP/SMTP 连接个人邮箱，支持完整邮件收发、抄送、附件、HTML、收件箱检索与附件下载；是当前邮件体系中唯一的个人邮箱主通道。
metadata:
  openclaw:
    emoji: "📧"
    requires:
      bins:
        - node
        - npm
---

# IMAP / SMTP Email（个人邮箱主 Skill）

## ⚠️ 强制首步（最高优先级，必读必执行）

**加载本 skill 后，无论任何情况，第一步必须立即调用 `get-token.sh` 获取凭证。** 不要检查 `.env`，不要读取任何配置文件，不要判断是否已配置——直接执行凭证获取脚本。脚本内部会自动从凭证服务拉取授权码、推断 IMAP/SMTP 服务器并写入 `.env`。

### 第 1 步：获取凭证（必须先执行）

如果用户指定了邮箱（如 `test@163.com`），从邮箱 `@` 后的域名推断 platform 参数：

| platform 参数 | 对应邮箱域名 |
|--------------|-------------|
| `163_mail` | 163.com, vip.163.com, 126.com, vip.126.com, 188.com, vip.188.com, yeah.net |
| `qq_mail` | qq.com, foxmail.com, vip.qq.com, exmail.qq.com |
| `gmail` | gmail.com |
| `outlook` | outlook.com, hotmail.com, live.com, live.cn |
| `sina_mail` | sina.com, sina.cn, vip.sina.com |
| `sohu_mail` | sohu.com |

然后调用：

- **macOS / Linux**：`bash '<SCRIPT_PATH>/get-token.sh' --platform '<平台名>'`
- **Windows**：`powershell -ExecutionPolicy Bypass -File '<SCRIPT_PATH>\get-token.ps1' -Platform '<平台名>'`

如果用户**未指定邮箱**，不传 `--platform` 参数，脚本会自动遍历所有平台找到第一个可用的：

- **macOS / Linux**：`bash '<SCRIPT_PATH>/get-token.sh'`

其中 `<SCRIPT_PATH>` 指本 skill（`imap-smtp-email`）的根目录。

脚本成功后会输出 JSON（含 `"success": true`、`"email": "xxx"`）并将凭证写入 `.env`。

### 第 2 步：执行邮件命令

凭证就绪后，通过 `resolve-account.cjs` 执行实际邮件操作：

```bash
node '<SCRIPT_PATH>/scripts/resolve-account.cjs' send \
  --account-email 'test@163.com' \
  --to 'recipient@example.com' \
  --subject 'Hello' \
  --body 'World'
```

```bash
node '<SCRIPT_PATH>/scripts/resolve-account.cjs' inbox-check \
  --account-email 'test@163.com'
```

#### 账号指定方式

支持两种方式指定使用哪个邮箱：

1. **精确邮箱地址**：`--account-email 'your@163.com'`
2. **平台别名**（用户只说了邮箱品牌时使用）：`--account-platform 'sina_mail'`

**中文别名映射表（LLM 必读）**：当用户消息中包含以下关键词但未给出精确邮箱地址时，使用 `--account-platform` 参数：

| 用户说法（关键词） | --account-platform 值 |
|-------------------|----------------------|
| 新浪邮箱、新浪、sina | `sina_mail` |
| 网易邮箱、163邮箱、163、126邮箱、netease | `163_mail` |
| QQ邮箱、QQ、foxmail、[ENTERPRISE_NAME]邮箱 | `qq_mail` |
| Gmail、谷歌邮箱、谷歌、google | `gmail` |
| Outlook、[ENTERPRISE_NAME]邮箱、[ENTERPRISE_NAME]、hotmail | `outlook` |
| 搜狐邮箱、搜狐、sohu | `sohu_mail` |

**示例**：用户说"用新浪邮箱帮我发个邮件"→

```bash
node '<SCRIPT_PATH>/scripts/resolve-account.cjs' send \
  --account-platform 'sina_mail' \
  --to 'recipient@example.com' \
  --subject 'Hello' \
  --body 'World'
```

> 优先级：`--account-email` > `--account-platform` > 自动选择

### 禁止事项（违反将导致任务失败）

- ❌ 禁止跳过第 1 步直接执行邮件命令
- ❌ 禁止手动读取或修改 `.env` 文件
- ❌ 禁止在获取凭证前做任何"配置检查"或"环境检查"
- ❌ 禁止告诉用户"需要先配置"或"需要授权"——凭证由脚本自动获取

---

> **定位**：这是唯一的个人邮箱主 skill。只要需求不再是"推送到我自己的邮箱做留存"，而是要像正常邮箱一样完整收发邮件，就应该走 `imap-smtp-email`。

## 1. 现在的职责

`imap-smtp-email` 负责所有**完整邮件能力**：

- 发给任意收件人
- `to` / `cc` / `bcc`
- 纯文本 / HTML
- 附件发送
- IMAP 收件、搜索、查看详情
- 下载附件
- 标记已读 / 未读
- 列出邮箱文件夹

> 当前邮件体系中，所有个人邮箱能力都统一收敛到本 skill，不再按邮箱厂商拆分入口。

## 2. 与平台公邮的边界

| 问题 | 平台公邮 | `imap-smtp-email` |
|------|----------|-------------------|
| 推送到自己的邮箱 | ✅ | ✅ |
| 发给别人 | ❌ | ✅ |
| 抄送 / 密送 | ❌ | ✅ |
| HTML | ❌ | ✅ |
| 附件 | ❌ | ✅ |
| 收件 / 搜索 / 下载附件 | ❌ | ✅ |
| 零配置 | ✅ | ❌ |

**判断口诀：**

- **只给自己做留存**：更适合平台公邮
- **像正常邮箱那样收发**：直接 `imap-smtp-email`

> 这里的关键不是"先检查平台公邮"，而是**先理解场景**：如果任务本质是结果留存，就选平台公邮；如果任务本质是完整邮件动作，就直接选本 skill。

## 3. 收敛后的能力组成

本 skill 统一承接了个人邮箱场景里仍然有效的能力与预设：

- 网易系邮箱 provider 预设
- QQ / Foxmail / 企业邮 provider 预设
- 统一的凭证自动刷新脚本（`get-token.sh` / `get-token.ps1`）

这意味着：

- `email-skill` 的个人邮箱分流目标只剩一个：`imap-smtp-email`
- 个人邮箱侧的脚本、配置和帮助信息都应围绕本 skill 维护

## 4. 支持的邮箱 Provider 预设

以下 provider 已内置到 `setup.sh` 配置向导中：

| Provider | IMAP Host | IMAP Port | SMTP Host | SMTP Port |
|----------|-----------|-----------|-----------|-----------|
| 163.com | imap.163.com | 993 | smtp.163.com | 465 |
| vip.163.com | imap.vip.163.com | 993 | smtp.vip.163.com | 465 |
| 126.com | imap.126.com | 993 | smtp.126.com | 465 |
| vip.126.com | imap.vip.126.com | 993 | smtp.vip.126.com | 465 |
| 188.com | imap.188.com | 993 | smtp.188.com | 465 |
| vip.188.com | imap.vip.188.com | 993 | smtp.vip.188.com | 465 |
| yeah.net | imap.yeah.net | 993 | smtp.yeah.net | 465 |
| gmail.com | imap.gmail.com | 993 | smtp.gmail.com | 587 |
| Outlook.com | outlook.office365.com | 993 | smtp-mail.outlook.com | 587 |
| qq.com | imap.qq.com | 993 | smtp.qq.com | 465 |
| foxmail.com | imap.qq.com | 993 | smtp.qq.com | 465 |
| yahoo.com | imap.mail.yahoo.com | 993 | smtp.mail.yahoo.com | 465 |
| sina.com | imap.sina.com | 993 | smtp.sina.com | 465 |
| sohu.com | imap.sohu.com | 993 | smtp.sohu.com | 465 |
| 139.com | imap.139.com | 993 | smtp.139.com | 465 |
| exmail.qq.com | imap.exmail.qq.com | 993 | smtp.exmail.qq.com | 465 |
| aliyun.com | imap.aliyun.com | 993 | smtp.aliyun.com | 465 |
| Custom | 自定义 | 自定义 | 自定义 | 自定义 |

> 对于 `587` 端口，`SMTP_SECURE=false`，走 STARTTLS。
> 对于 `465` 端口，`SMTP_SECURE=true`，走 SSL。

## 5. 凭证与配置（全自动）

入口脚本内部自动完成以下步骤，无需任何手动操作：

1. 调 4230 接口查询已绑定的所有个人邮箱
2. 根据 `--account-email` 参数或自动选择决定使用哪个邮箱
3. 自动刷新凭证并写入配置
4. 然后执行后续的 smtp / imap 命令

## 7. IMAP 命令

> `<SCRIPT_PATH>` 指本 skill（`imap-smtp-email`）的根目录。

### `inbox-check`

```bash
bash '<SCRIPT_PATH>/scripts/unix/email_gateway.sh' inbox-check \
  --account-email 'your@163.com' --limit 10 --mailbox INBOX --recent 2h
```

### `inbox-search`

```bash
bash '<SCRIPT_PATH>/scripts/unix/email_gateway.sh' inbox-search \
  --account-email 'your@163.com' --subject 发票 --recent 7d --limit 20
```

### `inbox-fetch`

```bash
bash '<SCRIPT_PATH>/scripts/unix/email_gateway.sh' inbox-fetch \
  --account-email 'your@163.com' 12345 --mailbox INBOX
```

### `inbox-download`

```bash
bash '<SCRIPT_PATH>/scripts/unix/email_gateway.sh' inbox-download \
  --account-email 'your@163.com' 12345 --dir "$HOME/Downloads"
```

### 其他 IMAP 命令

```bash
bash '<SCRIPT_PATH>/scripts/unix/email_gateway.sh' inbox-mark-read --account-email 'your@163.com' 12345
bash '<SCRIPT_PATH>/scripts/unix/email_gateway.sh' inbox-mark-unread --account-email 'your@163.com' 12345
bash '<SCRIPT_PATH>/scripts/unix/email_gateway.sh' inbox-list-mailboxes --account-email 'your@163.com'
```

## 8. SMTP 命令

### `send`

```bash
bash '<SCRIPT_PATH>/scripts/unix/email_gateway.sh' send \
  --account-email 'your@163.com' \
  --to partner@example.com \
  --subject "Hello" \
  --body "World"
```

### 常见示例

#### 发送 HTML 邮件

```bash
bash '<SCRIPT_PATH>/scripts/unix/email_gateway.sh' send \
  --account-email 'your@163.com' \
  --to recipient@example.com \
  --subject "周报" \
  --html \
  --body "<h1>Weekly Report</h1><p>详情见正文</p>"
```

#### 发送附件

```bash
bash '<SCRIPT_PATH>/scripts/unix/email_gateway.sh' send \
  --account-email 'your@163.com' \
  --to recipient@example.com \
  --subject "报告" \
  --body "请查收附件" \
  --attach /Users/you/Documents/report.pdf
```

#### 抄送 / 密送

```bash
bash '<SCRIPT_PATH>/scripts/unix/email_gateway.sh' send \
  --account-email 'your@163.com' \
  --to a@example.com \
  --cc b@example.com \
  --bcc c@example.com \
  --subject "项目同步" \
  --body "请查收"
```

## 9. 与其他 skill 的关系

- `email-skill`：统一入口，负责意图识别与路由分发
- `public-skill`：平台公邮，仅做"推送到自己邮箱"
- `imap-smtp-email`（本 skill）：完整个人邮箱能力

## 10. 调用规范

参见顶部「⚠️ 强制首步」段落。所有邮件操作必须且只能通过入口脚本执行。


---


## Skill: schedule-skill

---
name: [PLATFORM_NAME]-calendar-guide
version: 8.0.0
description: "日程/日历/会议/安排管理 Skill。当用户提到任何日程相关话题时触发——包括但不限于：查看日程（'明天有什么安排'、'这周有什么会'）、创建日程（'帮我建个日程'、'约个会'、'三点开会'）、修改日程（'把会议推迟到后天'、'改个时间'）、取消日程（'取消明天的会'、'把那个会删了'）。也适用于间接意图如'帮我约个时间'、'安排一下'、'看看有没有冲突'。自动检测系统日历（Apple 日历/Outlook/Windows 日历），支持飞书、钉钉、企业微信。"
---

# [PLATFORM_NAME] 日程管理 Skill

## 你是谁

你是 [PLATFORM_NAME]（🦞），一只活泼的 AI 小龙虾助手。说话口语化、轻松。
- 从记忆中读取 `[PLATFORM_NAME]_nickname` 作为自称，没有则用"虾"
- 从记忆中读取 `[PLATFORM_NAME]_avatar` 作为头像，没有则用 🦞

---

## 核心原则（必须遵守）

1. **自动检测优先，尊重用户偏好** — 默认自动检测系统日历，不主动问用户用什么；用户表达偏好时，若本 skill 支持则切换过去，不支持则告知并走兜底方案
2. **每次操作必有产出** — 失败了就降级，永远给用户一个结果
3. **失败时立即降级，避免反复重试** — 同一方式最多尝试 2 次（应对首次授权弹窗等临时失败），第 2 次仍失败则换下一种方式
4. **不暴露技术错误** — 用户不需要看到任何报错信息，静默切换方式，只说"换了个方式帮你处理~"
5. **减少确认** — 取消类操作直接做；修改/冲突创建先展示预览（**批量操作除外**，用户说"所有/全部"时必须确认范围，参见 ⛔#11）
6. **零依赖** — 核心操作全部通过系统原生命令完成（osascript / PowerShell），不依赖 Python 或其他运行时
7. **优先使用脚本** — 日历操作通过 `scripts/calendar.sh`（macOS）或 `scripts/calendar.ps1`（Windows）执行，脚本内置参数校验和日期安全处理，比 AI 每次从头写代码更可靠。**首次 detect 后，后续命令通过 `--platform` 参数传入平台，跳过重复检测。** 第三方平台的半自动路径（飞书 Applink、outlookcal: URI）也已封装为脚本命令（`open-feishu`、`open-outlookcal`），脚本内部完成时间戳计算和 URL 编码，**AI 不需要自行心算任何格式转换**
8. **Windows UTF-8 编码（防止中文乱码）** — 在 Windows 上调用 PowerShell 脚本时，**必须**先切换控制台代码页为 UTF-8，否则中文系统上脚本输出会是 GBK 编码导致乱码。所有 Windows 命令格式为 `chcp 65001 >nul && powershell -File {SKILL_DIR}/scripts/calendar.ps1 ...`

---

## 怎么检测日历（统一决策树）

> ⚠️ 这是唯一的检测流程，严格按此顺序执行，不要跳步。

```
第一步：查记忆
  记忆中有 [PLATFORM_NAME]_calendar_platform？
  ├─ 有 → 直接用该平台，跳过检测 ✅
  └─ 没有 → 进入第二步

第二步：判断操作系统
  ├─ macOS → 第三步 macOS 探测
  └─ Windows → 第三步 Windows 探测

第三步 macOS 探测：
  执行: bash {SKILL_DIR}/scripts/calendar.sh detect
  ├─ 输出单平台（如 apple_calendar）→ 写入记忆，完成 ✅
  ├─ 输出多平台（如 apple_calendar,outlook_mac）→ 询问用户偏好，写入记忆 ✅
  │   话术："检测到你电脑上有 Apple 日历和 Outlook，你平时主要用哪个？"
  │   用户选择后写入记忆（不选则默认第一个）
  └─ 输出 ics_fallback → 写入记忆，完成 ✅
                 （友好提示："{昵称}暂时用文件方式帮你管日程~ 如果你用的是系统自带日历，
                  可以去 系统设置 → 隐私与安全性 → 自动化 里给当前应用开个权限，
                  下次{昵称}就能直接帮你操作了 🎉"）

第三步 Windows 探测：
  执行: chcp 65001 >nul && powershell -File {SKILL_DIR}/scripts/calendar.ps1 detect
  ├─ 输出单平台（如 outlook_windows）→ 写入记忆，完成 ✅
  ├─ 输出多平台（如 outlook_windows,windows_calendar）→ 默认用 Outlook（能力更强），写入记忆 ✅
  └─ 输出 ics_fallback → 写入记忆，完成 ✅
```

**首次检测成功后**，写入记忆：
```json
{ "[PLATFORM_NAME]_calendar_platform": "检测到的平台" }
```

**后续所有命令**，通过 `--platform` 参数传入记忆中的平台，跳过重复检测：
```bash
# macOS 示例
bash {SKILL_DIR}/scripts/calendar.sh --platform apple_calendar list --start 2026-03-15 --end 2026-03-15
# Windows 示例
chcp 65001 >nul && powershell -File {SKILL_DIR}/scripts/calendar.ps1 -Platform outlook_windows list -Start 2026-03-15 -End 2026-03-15
```

**关于第三方日历（飞书/钉钉/企微）：**
- Onboarding 时会告知用户支持的第三方平台，由用户主动选择（不强推）
- 用户选择第三方平台后，**必须先确认本地已安装对应客户端**，未安装则如实告知并提供替代方案
- 如果检测到已配置对应的 MCP Server（在 MCP 配置文件中查找），优先使用 MCP 全自动
- **不主动推荐**用户安装额外工具或配置 MCP — 只在用户有明确需求时引导

> 详细的第三方日历配置流程及客户端检测路径见 `references/calendar-platforms.md`（索引）及 `references/platforms/` 下的各平台文件

---

## 操作日历的方式（降级链）

### macOS 降级链

```
① AppleScript 全自动（Apple 日历 / Outlook for Mac）
  → ② MCP Server 全自动（如有配置：飞书/钉钉）
    → ③ 生成 .ics 文件 + open 命令打开
      → ④ 生成 .ics 文件供用户下载
        → ⑤ 纯文案（展示日程信息，用户手动录入）
```
> ⚠️ **步骤②快速跳过**：如果记忆中没有 MCP 配置标记（如 `feishu_mcp_configured`、`dingtalk_mcp_configured`），直接跳过 MCP 步骤，不要尝试。

### Windows 降级链

```
① Outlook COM 全自动（PowerShell）
  → ② MCP Server 全自动（如有配置：飞书/钉钉）
    → ③ Windows 日历 .ics 关联打开（Start-Process "xxx.ics"）
      → ④ 生成 .ics 文件供用户下载
        → ⑤ 纯文案（展示日程信息，用户手动录入）
```
> ⚠️ **步骤②快速跳过**：同上，记忆中无 MCP 配置标记则直接跳到步骤③。

**降级时的话术：** 只说"换了个方式帮你处理~"，不解释技术原因，不暴露任何报错。

---

## 🎉 首次使用引导（Onboarding）

> 当记忆中**没有** `[PLATFORM_NAME]_calendar_platform` 时触发此流程。这是用户的第一印象，务必顺畅。

```
触发条件：用户首次发出日程相关指令（如"看看明天有啥安排"），且记忆中无平台信息

第一步：预告 + 检测本地可用日历
  话术："让{昵称}先看看你电脑上有什么日历~"
  执行 detect 脚本（macOS: calendar.sh detect / Windows: calendar.ps1 detect）

第二步：展示检测结果 + 询问用户偏好
  ├─ 检测到系统日历（单个或多个）
  │   话术示例：
  │     "找到了~ 你电脑上有 Apple 日历 和 Outlook 📅
  │      另外{昵称}也支持 飞书、钉钉、企业微信 的日程管理~
  │      你主要用哪个？直接用检测到的也行~"
  │   （只检测到一个时把"Apple 日历 和 Outlook"换成实际名称即可）
  │
  └─ 未检测到系统日历（ics_fallback）
      话术：
        "暂时没检测到系统日历~ 不过{昵称}也支持 飞书、钉钉、企业微信~
         你平时用哪个管日程？都不用的话也没关系，{昵称}用文件方式帮你管~"
      补充引导（仅 macOS）：
        "如果你用的是系统自带日历，可以去 系统设置 → 隐私与安全性 → 自动化 里开个权限，
         下次{昵称}就能直接帮你操作了 🎉"

第三步：根据用户回答确定平台 + 执行操作

  路径 A — 用户选了检测到的系统日历（或不选 / 不明确）
  │   默认使用检测到的日历（多个时用用户选的，不选则默认第一个）
  │   话术（macOS）："好嘞~ 系统可能会弹个小窗问你要不要允许，点【好】就行~"
  │   话术（Windows）："好嘞~ 电脑可能会弹个确认窗口，点【允许】就行~"
  │   → 执行用户请求的操作
  │   → 成功后话术："搞定~ 以后就不用再弹窗了 🎉"
  │
  │   特殊情况 · Windows 日历（windows_calendar）：
  │     话术："你电脑上有 Windows 自带日历~ 创建日程没问题，查看和修改暂时得你自己打开日历 App 看~"
  │     → 执行用户请求的操作（创建走 .ics 半自动）
  │
  │   特殊情况 · 未检测到 + 用户也不选第三方：
  │     → 走 .ics 降级完成用户请求

  路径 B — 用户选了第三方平台（飞书 / 钉钉 / 企微）
  │
  │   第 ① 步：检查 MCP 配置（仅飞书/钉钉）
  │     检查 MCP 配置文件中是否有对应 server
  │     ├─ 有 → 直接用 MCP 全自动，跳到第 ④ 步写入记忆
  │     └─ 没有 → 继续第 ②  步
  │
  │   第 ② 步：检测本地客户端
  │     飞书: macOS → /Applications/Lark.app 或 /Applications/Feishu.app
  │           Windows → %LOCALAPPDATA%\Lark\Lark.exe 或 %LOCALAPPDATA%\Feishu\Feishu.exe
  │     钉钉: macOS → /Applications/DingTalk.app
  │           Windows → %LOCALAPPDATA%\DingTalk\DingTalk.exe
  │     企微: macOS → /Applications/企业微信.app 或 /Applications/WeCom.app
  │           Windows → %LOCALAPPDATA%\WXWork\WXWork.exe
  │     ├─ 检测到客户端 → 继续第 ③ 步引导配置
  │     └─ 未检测到 → 告知 + 提供替代方案：
  │         话术："你电脑上还没装{平台名}~ {昵称}需要本地有{平台名}客户端才能帮你操作哦。
  │                你可以先装一个，装好了随时跟{昵称}说~
  │                现在{昵称}先用{detect到的系统日历 / 文件方式}帮你管着？"
  │         用户同意 → 回到路径 A
  │         用户说想先装 → 话术："好~ 装好了跟{昵称}说一声就行 🎉" → 本次走降级完成请求
  │
  │   第 ③ 步：引导配置（检测到客户端后）
  │     ├─ 飞书 → 走 Applink 半自动（仅创建可直接用，其他操作建议配 MCP）
  │     │   话术："检测到你装了飞书~ 创建日程{昵称}可以直接帮你~
  │     │          要是想让{昵称}也能帮你查看和修改飞书日程，需要配置一下，现在花 1 分钟搞定？
  │     │          还是先这样用着，以后再说？"
  │     ├─ 钉钉 → 需 CalDAV 同步配置
  │     │   话术："检测到你装了钉钉~ 需要花 30 秒做个同步设置，{昵称}就能帮你管钉钉日程了 🎉
  │     │          现在配置？还是先用{系统日历 / 文件方式}管着，以后再配？"
  │     └─ 企微 → 需 CalDAV 同步配置
  │         话术："检测到你装了企微~ 需要花 30 秒做个同步设置，{昵称}就能帮你管企微日程了 🎉
  │                现在配置？还是先用{系统日历 / 文件方式}管着，以后再配？"
  │     用户选"现在配置" → 按 references/platforms/ 下对应平台文件的引导流程执行
  │     用户选"以后再说" → 回到路径 A，用系统日历或 .ics 完成本次请求
  │
  │   第 ④ 步：配置完成
  │     → 执行用户请求的操作
  │     → 成功后话术："配好了~ 以后{昵称}就能帮你管{平台名}日程了 🎉"

第四步：记忆持久化
  写入: { "[PLATFORM_NAME]_calendar_platform": "确定的平台" }
  后续所有命令通过 --platform 参数传入，不再触发 Onboarding
```

> ⚠️ **关键原则**：
> 1. 首次使用时不管发生什么，都要完成用户请求（哪怕是降级方式），不能让用户空手而归
> 2. 询问偏好只在 Onboarding 时进行一次，后续直接使用记忆中的平台
> 3. 第三方平台必须本地有客户端才能操作，没装时如实告知 + 提供替代方案，不强推安装

---

## 六种场景怎么做

### 🔍 查看日程

**信息收集：**
- 解析时间（"明天"→+1天，"这周"→本周剩余天，没说时间→今天+明天）
- 时间理解参考见文末表格

**执行（按降级链顺序尝试）：**

macOS（Apple 日历 / Outlook 均通过脚本统一处理）：
```bash
bash {SKILL_DIR}/scripts/calendar.sh list --start 2026-03-15 --end 2026-03-15
```
> 输出格式: `标题|开始时间|结束时间|日历名` 每行一条

Windows · Outlook COM / Windows 日历：
```powershell
chcp 65001 >nul && powershell -File {SKILL_DIR}/scripts/calendar.ps1 list -Start 2026-03-15 -End 2026-03-15
```
> 输出格式: `标题|开始时间|结束时间|EntryID=xxx` 每行一条

**降级：** 以上方式都失败 → 告诉用户"这段时间{昵称}暂时看不到你的日历内容~ 你可以打开日历 App 自己看看，有什么需要{昵称}帮你处理的随时说~"

**展示格式：**
- 有日程 → 按时间排列展示，格式清晰
- 没有日程 → "这段时间没有安排，要{昵称}帮你建一个吗？"

---

### 📝 创建日程

**信息收集（智能追问，避免骚扰用户）：**
- 能从上下文推断的信息直接补全 + 向用户确认，不追问
- 只在关键信息确实缺失时才追问（缺标题 / 缺时间）
- 核心目标：让小白用户感到轻松无压力
- 缺标题 → "叫什么名字好？比如'产品会议'之类的~"
- 缺时间 → "什么时候的？帮{昵称}补个日期和时间~"
- 时间模糊（如"约个健身"）→ 根据事项类型建议时间，让用户确认或修改
- 默认时长：会议 1h，聚餐 1.5h，运动 1h，简短事项 15min

**冲突检测（创建前必做）：**
- 先用查看日程的方式查询目标时间段
- 有冲突 → 展示冲突信息："这个时间段已经有个'XXX'了，要{昵称}仍然创建还是换个时间？"
- 无冲突 → 直接创建

**执行（按降级链顺序尝试）：**

macOS（Apple 日历 / Outlook 均通过脚本统一处理）：
```bash
echo '{"summary":"产品方案评审","start_date":"2026-03-15","start_time":"15:00","duration":60,"location":"会议室A"}' | bash {SKILL_DIR}/scripts/calendar.sh create
```
> 脚本自动处理日期跨月安全（先置1号再设年月日）、参数校验、自动选择可写日历
> 输出格式: `OK|标题|开始时间|结束时间`

Windows · Outlook COM：
```powershell
chcp 65001 >nul && echo '{"summary":"产品方案评审","start_date":"2026-03-15","start_time":"15:00","duration":60,"location":"会议室A"}' | powershell -File {SKILL_DIR}/scripts/calendar.ps1 create
```
> 输出格式: `OK|标题|开始时间|结束时间|EntryID=xxx`
> Windows 日历平台会自动降级为生成 .ics 并打开

> ⚠️ **JSON 字段说明**：`start_time`/`end_time` 格式为 `HH:MM`；`duration` 为分钟数，与 `end_time` 二选一。脚本内置参数范围校验（hour 0-23, minute 0-59）和日期格式标准化。

**降级到 .ics 文件：**
```bash
# macOS
echo '{"summary":"产品方案评审","start_date":"2026-03-15","start_time":"15:00","duration":60}' | bash {SKILL_DIR}/scripts/calendar.sh generate-ics
# 输出: OK|./产品方案评审.ics
open "产品方案评审.ics"
```
```powershell
# Windows
chcp 65001 >nul && echo '{"summary":"产品方案评审","start_date":"2026-03-15","start_time":"15:00","duration":60}' | powershell -File {SKILL_DIR}/scripts/calendar.ps1 generate-ics
# 输出: OK|.\产品方案评审.ics
Start-Process "产品方案评审.ics"
```
- 上述命令也失败 → 将 .ics 文件提供给用户下载，附话术："双击这个文件就能添加到日历啦 📎"

**最终兜底（纯文案）：**
> "{昵称}帮你整理好了日程信息，你手动加一下就行~
> 📌 **产品方案评审**
> 🕐 明天 15:00 - 16:00
> 📍 会议室A"

**创建成功后：**
- 展示结果卡片

---

### ✏️ 修改日程

**搜索定位（先查再匹配，避免精确标题匹配失败）：**
1. 先用 `list` 命令查出目标日期范围内的所有日程
2. 在 AI 侧做**模糊匹配**：用用户提到的关键词与 list 结果的标题做包含匹配
3. 唯一命中 → 用**精确标题**调用 modify 脚本
4. 多条匹配 → 列出让用户选
5. 没有匹配 → 扩大搜索范围（前后各 1 天），仍无结果告诉用户

> ⚠️ **为什么不直接用关键词调 modify？** 脚本使用精确标题匹配，用户口语化的关键词（如"方案评审"）很可能与实际标题（如"产品方案评审会议"）不完全一致。先 list 再 AI 侧匹配，成功率更高。

**展示变更对比（必须先确认再执行）：**
> "找到了这个日程，{昵称}帮你改一下：
> 📌 产品方案评审
> 🕐 ~~明天 15:00-16:00~~ → **后天 14:00-15:00**
> 确认修改吗？"

**执行：**

macOS（Apple 日历 / Outlook 均通过脚本统一处理）：
```bash
echo '{"summary":"产品方案评审","search_date":"2026-03-15","new_start_date":"2026-03-16","new_start_time":"14:00","new_duration":60}' | bash {SKILL_DIR}/scripts/calendar.sh modify
```
> 输出 `OK|标题|新开始时间|新结束时间` 表示成功，`NOT_FOUND` 表示未找到

Windows · Outlook COM：
```powershell
chcp 65001 >nul && echo '{"summary":"产品方案评审","search_date":"2026-03-15","new_start_date":"2026-03-16","new_start_time":"14:00","new_duration":60}' | powershell -File {SKILL_DIR}/scripts/calendar.ps1 modify
```

全自动方式失败 → 生成新 .ics 文件让用户替换。

**修改成功后：** 展示结果

**降级提示（无法自动修改时）：**
> "{昵称}暂时没法直接帮你改~ 不过{昵称}帮你生成了一个更新后的日程文件，你先把原来的删掉，再双击这个新文件导入就行 📎"

---

### 🗑️ 取消日程

**搜索定位（同修改日程的"先查再匹配"策略）：**
1. 先用 `list` 命令查出目标日期的所有日程
2. AI 侧模糊匹配关键词 → 唯一命中后用精确标题调用 delete
3. 多条匹配 → 列出让用户选
4. 无匹配 → 告诉用户

**直接执行（不用事先确认）：**

macOS（Apple 日历 / Outlook 均通过脚本统一处理）：
```bash
bash {SKILL_DIR}/scripts/calendar.sh delete --summary "产品方案评审" --date 2026-03-15
```
> 输出 `OK|标题|开始时间|结束时间` 表示成功，`NOT_FOUND` 表示未找到

Windows · Outlook COM：
```powershell
chcp 65001 >nul && powershell -File {SKILL_DIR}/scripts/calendar.ps1 delete -Summary "产品方案评审" -Date 2026-03-15
```

**取消成功后：**
- 展示："已取消 ✅"

**降级提示（无法自动取消时）：**
> "{昵称}暂时没法直接帮你取消~ 你打开日历 App 手动删一下就行，以下是日程信息方便你找到它：
> 📌 产品方案评审 | 🕐 明天 15:00-16:00"

---

### 👥 参会人管理

> 🚧 **进化中**：参会人管理功能正在开发中，暂未支持。用户问起时如实告知：
> "{昵称}的参会人管理功能还在进化中~ 你可以手动在日历 App 里添加参会人，或者{昵称}帮你生成一条邀请信息转发给他们 📨"

---

### 📊 忙闲查询

> 🚧 **进化中**：忙闲查询功能正在开发中，暂未支持。用户问起时如实告知：
> "{昵称}暂时还不能帮你查忙闲~ 这个功能还在进化中 🚀 你可以直接问对方有没有空，或者{昵称}帮你建个日程发给对方确认~"

---

## .ics 文件格式

> 降级到 .ics 文件时，参考 `references/ics-format.md` 获取完整格式规范和铁律。脚本的 `generate-ics` 命令已内置格式处理，通常无需手动生成。

---

## ⛔ 注意事项

1. **不要反复重试同一种方式** — 同一方式最多 2 次（应对首次授权弹窗等临时失败），第 2 次仍失败则立即降级
2. **不要硬编码日历名** — 日历名因系统语言和用户设置各异（如中文系统叫"日历"、英文叫"Calendar"），且部分日历是只读的（如"中国节假日"）；必须动态查询可写日历列表，让脚本自动选择
3. **不要给用户看技术概念** — 用户是电脑小白，AppleScript、COM、PowerShell、CalDAV、MCP、.ics 等术语只会让他们困惑
4. **不要主动推荐安装额外工具** — 目标用户不具备自行安装配置的能力，系统日历足够满足基本需求
5. **不要手写 AppleScript/PowerShell，不要心算时间戳** — 脚本内已固化日期跨月安全处理（先 set day to 1 再设年月日）和参数校验，手写容易遗漏这些边界情况。飞书 Applink 和 outlookcal: URI 也必须通过脚本命令（`open-feishu` / `open-outlookcal`）执行，脚本内部完成 Unix 时间戳计算、ISO 时间格式化和 URL 编码——**绝对不要让 AI 自行心算 Unix 时间戳或手动拼接 URL**（已有真实 case：AI 心算时间戳偏差 7 小时导致用户日程时间完全错误）
6. **查看日程必须加日期范围** — 不加范围会返回全量历史日程，数据量过大导致超时或 token 爆炸
7. **脚本输出协议** — 根据输出前缀判断行为：`OK|` = 成功（展示结果）；`OK_ICS|` = 通过 .ics 半自动完成（展示结果 + 提示用户确认保存）；`NOT_FOUND` = 目标日程未找到（提示用户确认日程名/日期，可扩大搜索范围）；`UNSUPPORTED|` = 当前平台不支持此操作（走降级话术）；其他输出或 stderr = 系统错误（静默降级）
8. **日期参数必须标准化为 YYYY-MM-DD** — 脚本只解析这一种格式，传 "3/15"、"Mar 15" 等非标准格式会导致解析失败
9. **创建日程时目标时间已过 → 必须提醒用户确认** — 用户可能口误或记错日期，默默创建过去的日程毫无意义
10. **同一时刻只执行一个日历操作命令** — macOS AppleScript 并发会导致日历 App 状态错乱，Windows Outlook COM 是单实例对象，并行调用会引发资源竞争
11. **用户说"所有/全部"时必须确认范围** — 批量操作（如"取消这周所有会议"）风险高且不可逆，必须明确范围后再执行
12. **涉及密码/同步码/凭据时保持轻松语气** — 用户发来同步码等敏感信息时，不要用"泄露""危险""吓到"等措辞制造焦虑；配置完成后轻松地建议用户重新生成一个即可（如"配完之后建议去重新生成一个同步码，这样更安心 👌"），不要让安全提醒变成安全恐吓

---

## 时间理解参考

> 自然语言时间解析规则和模糊时间智能建议表见 `references/time-parsing.md`。

---

## 时区处理

- 默认使用用户系统时区
- 首次检测时自动获取时区，写入记忆 `[PLATFORM_NAME]_timezone`
  - macOS: 从系统设置或 `date +%Z` 获取
  - Windows: `(Get-TimeZone).Id`
- AppleScript 的 `current date` 使用系统时区，无需额外处理
- .ics 文件必须显式指定 TZID（脚本 `generate-ics` 支持 `timezone` 可选字段，默认 `Asia/Shanghai`，传入记忆中的 `[PLATFORM_NAME]_timezone` 即可）
- 跨时区用户：每次操作前检查记忆中的时区是否与当前系统时区一致

---

## macOS 首次弹窗预告

当首次在 macOS 上使用 AppleScript 操作日历时，系统会弹出授权弹窗。提前告知用户：

> "系统会弹个小窗问你要不要允许，点【好】就行~ 只需要这一次，以后就不会再弹了 🎉"

---

## Windows 首次使用注意

- Outlook COM 调用可能会自动启动 Outlook 应用（如果未运行的话），这是正常行为
- 首次可能弹出安全提示"允许此程序访问 Outlook"，告知用户：
  > "电脑可能会弹个确认窗口，点【允许】就行~ 这样{昵称}就能帮你操作日历了 ✨"
- 如果 Outlook 未安装，静默降级到 .ics 方式，用户无感知


---


## Skill: file-deduplicator

---
name: file-deduplicator
description: Find and remove duplicate files intelligently. Save storage space, keep your system clean. Perfect for digital hoarders and document management.
metadata:
  {
    "openclaw":
      {
        "version": "1.0.0",
        "author": "Vernox",
        "license": "MIT",
        "tags": ["deduplication", "storage", "cleanup", "file-management", "duplicate", "disk-space"],
        "category": "tools"
      }
  }
---

# File-Deduplicator - Find and Remove Duplicates

**Vernox Utility Skill - Clean up your digital hoard.**

## Overview

File-Deduplicator is an intelligent file duplicate finder and remover. Uses content hashing to identify identical files across directories, then provides options to remove duplicates safely.

## Features

### ✅ Duplicate Detection
- Content-based hashing (MD5) for fast comparison
- Size-based detection (exact match, near match)
- Name-based detection (similar filenames)
- Directory scanning (recursive)
- Exclude patterns (.git, node_modules, etc.)

### ✅ Removal Options
- Auto-delete duplicates (keep newest/oldest)
- Interactive review before deletion
- Move to archive instead of delete
- Preserve permissions and metadata
- Dry-run mode (preview changes)

### ✅ Analysis Tools
- Duplicate count summary
- Space savings estimation
- Largest duplicate files
- Most common duplicate patterns
- Detailed report generation

### ✅ Safety Features
- Confirmation prompts before deletion
- Backup to archive folder
- Size threshold (don't remove huge files by mistake)
- Whitelist important directories
- Undo functionality (log for recovery)

## Installation

```bash
clawhub install file-deduplicator
```

## Quick Start

### Find Duplicates in Directory

```javascript
const result = await findDuplicates({
  directories: ['./documents', './downloads', './projects'],
  options: {
    method: 'content',  // content-based comparison
    includeSubdirs: true
  }
});

console.log(`Found ${result.duplicateCount} duplicate groups`);
console.log(`Potential space savings: ${result.spaceSaved}`);
```

### Remove Duplicates Automatically

```javascript
const result = await removeDuplicates({
  directories: ['./documents', './downloads'],
  options: {
    method: 'content',
    keep: 'newest',  // keep newest, delete oldest
    action: 'delete',  // or 'move' to archive
    autoConfirm: false  // show confirmation for each
  }
});

console.log(`Removed ${result.filesRemoved} duplicates`);
console.log(`Space saved: ${result.spaceSaved}`);
```

### Dry-Run Preview

```javascript
const result = await removeDuplicates({
  directories: ['./documents', './downloads'],
  options: {
    method: 'content',
    keep: 'newest',
    action: 'delete',
    dryRun: true  // Preview without actual deletion
  }
});

console.log('Would remove:');
result.duplicates.forEach((dup, i) => {
  console.log(`${i+1}. ${dup.file}`);
});
```

## Tool Functions

### `findDuplicates`
Find duplicate files across directories.

**Parameters:**
- `directories` (array|string, required): Directory paths to scan
- `options` (object, optional):
  - `method` (string): 'content' | 'size' | 'name' - comparison method
  - `includeSubdirs` (boolean): Scan recursively (default: true)
  - `minSize` (number): Minimum size in bytes (default: 0)
  - `maxSize` (number): Maximum size in bytes (default: 0)
  - `excludePatterns` (array): Glob patterns to exclude (default: ['.git', 'node_modules'])
  - `whitelist` (array): Directories to never scan (default: [])

**Returns:**
- `duplicates` (array): Array of duplicate groups
  - `duplicateCount` (number): Number of duplicate groups found
  - `totalFiles` (number): Total files scanned
  - `scanDuration` (number): Time taken to scan (ms)
  - `spaceWasted` (number): Total bytes wasted by duplicates
  - `spaceSaved` (number): Potential savings if duplicates removed

### `removeDuplicates`
Remove duplicate files based on findings.

**Parameters:**
- `directories` (array|string, required): Same as findDuplicates
- `options` (object, optional):
  - `keep` (string): 'newest' | 'oldest' | 'smallest' | 'largest' - which to keep
  - `action` (string): 'delete' | 'move' | 'archive'
  - `archivePath` (string): Where to move files when action='move'
  - `dryRun` (boolean): Preview without actual action
  - `autoConfirm` (boolean): Auto-confirm deletions
  - `sizeThreshold` (number): Don't remove files larger than this

**Returns:**
- `filesRemoved` (number): Number of files removed/moved
- `spaceSaved` (number): Bytes saved
- `groupsProcessed` (number): Number of duplicate groups handled
- `logPath` (string): Path to action log
- `errors` (array): Any errors encountered

### `analyzeDirectory`
Analyze a single directory for duplicates.

**Parameters:**
- `directory` (string, required): Path to directory
- `options` (object, optional): Same as findDuplicates options

**Returns:**
- `fileCount` (number): Total files in directory
- `totalSize` (number): Total bytes in directory
- `duplicateSize` (number): Bytes in duplicate files
- `duplicateRatio` (number): Percentage of files that are duplicates

## Use Cases

### Digital Hoarder Cleanup
- Find duplicate photos/videos
- Identify wasted storage space
- Remove old duplicates, keep newest
- Clean up download folders

### Document Management
- Find duplicate PDFs, docs, reports
- Keep latest version, archive old versions
- Prevent version confusion
- Reduce backup bloat

### Project Cleanup
- Find duplicate source files
- Remove duplicate build artifacts
- Clean up node_modules duplicates
- Save storage on SSD/HDD

### Backup Optimization
- Find duplicate backup files
- Remove redundant backups
- Identify what's actually duplicated
- Save space on backup drives

## Configuration

### Edit `config.json`:
```json
{
  "detection": {
    "defaultMethod": "content",
    "sizeTolerancePercent": 0,  // exact match only
    "nameSimilarity": 0.7,  // 0-1, lower = more similar
    "includeSubdirs": true
  },
  "removal": {
    "defaultAction": "delete",
    "defaultKeep": "newest",
    "archivePath": "./archive",
    "sizeThreshold": 10485760,  // 10MB threshold
    "autoConfirm": false,
    "dryRunDefault": false
  },
  "exclude": {
    "patterns": [".git", "node_modules", ".vscode", ".idea"],
    "whitelist": ["important", "work", "projects"]
  }
}
```

## Methods

### Content-Based (Recommended)
- Fast MD5 hashing
- Detects exact duplicates regardless of filename
- Works across renamed files
- Perfect for documents, code, archives

### Size-Based
- Compares file sizes
- Faster than content hashing
- Good for media files where content hashing is slow
- Finds near-duplicates (similar but not exact)

### Name-Based
- Compares filenames
- Detects similar named files
- Good for finding version duplicates (file_v1, file_v2)

## Examples

### Find Duplicates in Documents
```javascript
const result = await findDuplicates({
  directories: '~/Documents',
  options: {
    method: 'content',
    includeSubdirs: true
  }
});

console.log(`Found ${result.duplicateCount} duplicate sets`);
result.duplicates.slice(0, 5).forEach((set, i) => {
  console.log(`Set ${i+1}: ${set.files.length} files`);
  console.log(`  Total size: ${set.totalSize} bytes`);
});
```

### Remove Duplicates, Keep Newest
```javascript
const result = await removeDuplicates({
  directories: '~/Documents',
  options: {
    keep: 'newest',
    action: 'delete'
  }
});

console.log(`Removed ${result.filesRemoved} files`);
console.log(`Saved ${result.spaceSaved} bytes`);
```

### Move to Archive Instead of Delete
```javascript
const result = await removeDuplicates({
  directories: '~/Downloads',
  options: {
    keep: 'newest',
    action: 'move',
    archivePath: '~/Documents/Archive'
  }
});

console.log(`Archived ${result.filesRemoved} files`);
console.log(`Safe in: ~/Documents/Archive`);
```

### Dry-Run Preview Changes
```javascript
const result = await removeDuplicates({
  directories: '~/Documents',
  options: {
    dryRun: true  // Just show what would happen
  }
});

console.log('=== Dry Run Preview ===');
result.duplicates.forEach((set, i) => {
  console.log(`Would delete: ${set.toDelete.join(', ')}`);
});
```

## Performance

### Scanning Speed
- **Small directories** (<1000 files): <1s
- **Medium directories** (1000-10000 files): 1-5s
- **Large directories** (10000+ files): 5-20s

### Detection Accuracy
- **Content-based:** 100% (exact duplicates)
- **Size-based:** Fast but may miss renamed files
- **Name-based:** Detects naming patterns only

### Memory Usage
- **Hash cache:** ~1MB per 100,000 files
- **Batch processing:** Processes 1000 files at a time
- **Peak memory:** ~200MB for 1M files

## Safety Features

### Size Thresholding
Won't remove files larger than configurable threshold (default: 10MB). Prevents accidental deletion of important large files.

### Archive Mode
Move files to archive directory instead of deleting. No data loss, full recoverability.

### Action Logging
All deletions/moves are logged to file for recovery and audit.

### Undo Functionality
Log file can be used to restore accidentally deleted files (limited undo window).

## Error Handling

### Permission Errors
- Clear error message
- Suggest running with sudo
- Skip files that can't be accessed

### File Lock Errors
- Detect locked files
- Skip and report
- Suggest closing applications using files

### Space Errors
- Check available disk space before deletion
- Warn if space is critically low
- Prevent disk-full scenarios

## Troubleshooting

### Not Finding Expected Duplicates
- Check detection method (content vs size vs name)
- Verify exclude patterns aren't too broad
- Check if files are in whitelisted directories
- Try with includeSubdirs: false

### Deletion Not Working
- Check write permissions on directories
- Verify action isn't 'delete' with autoConfirm: true
- Check size threshold isn't blocking all deletions
- Check file locks (is another program using files?)

### Slow Scanning
- Reduce includeSubdirs scope
- Use size-based detection (faster)
- Exclude large directories (node_modules, .git)
- Process directories individually instead of batch

## Tips

### Best Results
- Use content-based detection for documents (100% accurate)
- Run dry-run first to preview changes
- Archive instead of delete for important files
- Check logs if anything unexpected deleted

### Performance Optimization
- Process frequently used directories first
- Use size threshold to skip large media files
- Exclude hidden directories from scan
- Process directories in parallel when possible

### Space Management
- Regular duplicate cleanup prevents storage bloat
- Delete temp directories regularly
- Clear download folders of installers
- Empty trash before large scans

## Roadmap

- [ ] Duplicate detection by image similarity
- [ ] Near-duplicate detection (similar but not exact)
- [ ] Duplicate detection across network drives
- [ ] Cloud storage integration (S3, Google Drive)
- [ ] Automatic scheduling of scans
- [ ] Heuristic duplicate detection (ML-based)
- [ ] Recover deleted files from backup
- [ ] Duplicate detection by file content similarity (not just hash)

## License

MIT

---

**Find duplicates. Save space. Keep your system clean.** 🔮


---


## Skill: low-cost-search

# low-cost-search

低成本高精度搜索引擎。三层缓存 + 意图路由 + DuckDuckGo 搜索，模型常驻内存，缓存命中响应 **< 500ms**。

---

## 🚀 零配置使用（推荐）

**无需手动启动服务！** 首次搜索时自动启动，用户零感知。

```python
from search_client import search

# 首次调用会自动启动服务（约 10 秒）
result = search("武汉有哪些有名旅游景点")

# 后续调用直接返回（缓存命中 < 50ms）
result = search("武汉有哪些有名旅游景点")  # 快！
```

### 手动启动（可选）

如果希望提前预热服务：

| 方式 | 命令 |
|------|------|
| 双击启动 | `start_server.bat` |
| 静默启动 | `start_server_silent.vbs` |
| 命令行 | `python scripts/search_server.py --warmup` |

---

## 快速测试

```bash
# 测试脚本
test_search.bat

# 快速搜索
quick_search.bat "武汉景点"
```

---

## 核心架构

```
┌─────────────────────────────────────────────────────┐
│  search_server.py  (后台常驻进程，端口 18765)         │
│  模型预加载，缓存常驻，HTTP 接口                      │
└──────────────────────┬──────────────────────────────┘
                       │ localhost HTTP POST /search
                       ▼
                User Query
                       │
          ┌─────────────┴──────────────┐
          ▼                          ▼
    L3 Raw Hit                  L2 Semantic Hit
    (normalized match)          (cosine ≥ 0.85)
    < 30ms                       < 500ms
          │                          │
          └──────────┬───────────────┘
                     │ miss
                     ▼
             DuckDuckGo Search
             ~15-40s (网络延迟)
```

---

## 启动 server（命令行）

```bash
# 启动并 warmup 模型（~8s，之后所有搜索秒回）
python scripts/search_server.py --warmup --port 18765

# 后台运行（模型懒加载，首次搜索时才 warmup）
python scripts/search_server.py --port 18765
```

---

## 调用方式

### HTTP 调用（推荐）

```python
import urllib.request, json

req = urllib.request.Request(
    "http://127.0.0.1:18765/search",
    data=json.dumps({"query": "Python list comprehension 教程", "max_results": 5}).encode(),
    headers={"Content-Type": "application/json"},
    method="POST",
)
with urllib.request.urlopen(req, timeout=60) as resp:
    data = json.loads(resp.read())
    for r in data["results"]:
        print(r["title"], r["url"])
```

### PowerShell

```powershell
$body = @{ query = "python defaultdict"; max_results = 3 } | ConvertTo-Json
$r = Invoke-WebRequest -Uri "http://127.0.0.1:18765/search" -Method POST -Body $body -ContentType "application/json" -TimeoutSec 60
$d = $r.Content | ConvertFrom-Json
$d.results | ForEach-Object { Write-Host $_.title }
```

### CLI subprocess（兼容模式）

```bash
python scripts/search.py "python list comprehension" --max 5
```
Fallback：server 不可用时自动降级到 subprocess。

---

## 文件结构

```
low-cost-search/
├── SKILL.md
├── scripts/
│   ├── search_server.py   # 常驻 HTTP 服务（后台运行）
│   ├── search_client.py   # Python HTTP 客户端 + subprocess fallback
│   ├── search.py          # CLI 入口（subprocess 模式）
│   └── cache.py           # 三层缓存核心
└── references/
    └── cache/
        ├── l2_semantic.jsonl   # L2 语义缓存
        └── l3_raw.jsonl        # L3 精确缓存
```

---

## 性能数据

| 场景 | 冷启动 | 常驻后 |
|---|---|---|
| 首次 query（无缓存） | ~40s（网络搜索） | — |
| 完全重复 query | **28ms** | **L3 hit** |
| 语义改写 query | **435ms** | **L2 hit (sim=0.93)** |
| 模型 warmup | ~8s（仅一次） | — |

---

## 输出 JSON

```json
{
  "query": "python defaultdict example",
  "intent": "code",
  "cache_hit": true,
  "cache_tier": "L2_semantic",
  "results": [
    {
      "title": "python-defaultdict of defaultdict?",
      "url": "https://stackoverflow.com/questions/...",
      "snippet": "...",
      "source": "stackoverflow"
    }
  ],
  "search_ms": 435,
  "total_raw": 5,
  "trusted_count": 2
}
```

---

## 三层缓存

| 层 | 原理 | 延迟 | 命中率估算 |
|---|---|---|---|
| **L3** | Normalized query 精确匹配 | **< 30ms** | ~15% |
| **L2** | Embedding cosine ≥ 0.85 | **< 500ms** | ~20% |
| **L1** | 进程内 LRU（server 常驻后无效） | < 1ms | — |

---

## 健康检查

```bash
curl http://127.0.0.1:18765/health
# → ok
```

---

## 依赖

- `sentence-transformers>=2.0` — all-MiniLM-L6-v2（首次 warmup 后常驻）
- `ddgs>=9.0` — DuckDuckGo 多引擎搜索
- Python 3.10+
- 网络：`hf-mirror.com`、`duckduckgo.com`


---


## Skill: kdocs

---
name: kdocs
description: "操作金山文档（WPS 云文档 / Kdocs / 365.kdocs.cn / www.kdocs.cn）云文档的官方 Skill。核心能力覆盖云端新建、读取、编辑、搜索、分享、整理在线文档（智能文档、Word、Excel、PDF、PPT、演示文稿、智能表格、多维表格）及个人知识库。当用户的任务涉及云文档操作时使用，包括但不限于：写周报/日报/工作汇报、处理合同/发票、创建报名表/登记表、网页剪藏、接龙转表格、信息收集、文档总结与内容生成、改写仿写、翻译、AI PPT生成、PDF拆分导出、标签分类归档、收藏管理、碎片笔记整理、表格美化、回收站还原、知识库管理。"
homepage: https://www.kdocs.cn/latest
version: 1.4.5
metadata: {"openclaw":{"category":"kdocs","tokenUrl":"https://www.kdocs.cn/latest","emoji":"📝"},"keywords":["金山文档","金山表格","金山收藏","WPS","WPS文档","云文档","在线文档","kdocs","WPS云文档","接龙转表格","接龙","群接龙","报名表","信息收集","收集表","登记表","网页剪藏","剪藏","保存网页","网页保存到文档","保存文章","收藏文章","总结","帮我总结","帮我整理","帮我写","帮我翻译","帮我做PPT","翻译文档 - 做PPT - 生成PPT - 培训课件 - 方案展示 - 项目展示","文档总结","内容生成","改写","仿写","翻译","文档翻译","PPT","演示文稿","幻灯片","PDF","拆分PDF","导出PDF","Word","Excel","表格","Markdown","碎片整理","笔记整理","表格优化","文档处理","文件处理","办公助手","文档助手","周报","日报","工作汇报","合同","发票"],"file_types":["pdf","doc","docx","xlsx","xls","pptx","ppt","otl","ksheet","dbt","jpg","jpeg","png","bmp","gif","webp","url","md","txt","html"],"category":"productivity"}
---

# 金山文档 Skill 使用指南

金山文档 Skill 提供了一套完整的在线文档操作工具，支持创建、查询、读取、编辑、分享、移动多种类型的在线文档。

## 严格规则

### 禁止（NEVER）

- 禁止将 Token 明文出现在对话、日志、命令输出、代码注释或任何文件中；不得写入 `.env` 或环境变量；仅允许存放在 `mcporter` 的 `kdocs-[PLATFORM_NAME]` 配置中
- 上传写入等接口需传入的 `content_base64` 可能非常大（编码后 >1 MB），禁止在对话中逐 token 生成 Base64 字符串，用脚本完成文件读取、编码和传参

### 必须（MUST）

- 不可逆操作（delete/close 类）执行前必须向用户确认
- 写操作完成后必须用独立读取请求验证实际结果（不信任 `code: 0`）
- 创建文档并验证通过后，必须调用 `get_file_link` 获取链接并展示给用户

---

## 版本自检

首次使用或间隔超过 24 小时后，需要确认当前 Skill 是否为最新版本。

从本文件头部 frontmatter 的 `version` 字段读取本地版本号（语义化版本 X.Y.Z），然后调用远端接口比对：

```bash
mcporter call kdocs-[PLATFORM_NAME].check_skill_update version=<本地版本号>
```

返回：`stdout` 为 JSON，包含三个关键字段：

| 字段 | 含义 |
|------|------|
| `latest` | 远端最新版本号 (X.Y.Z) |
| `release_note` | 该版本的变更摘要 |
| `instruction` | 升级操作步骤 |

若本地版本落后于 `latest`，按 `instruction` 中的指引完成升级或者把最新版信息用户告知用户。升级时需要保留旧版 `mcporter` 配置中的 Token，并将其迁移到新版本配置中。

---

## 认证配置

### Token 获取与管理

| 步骤 | 操作 |
|------|------|
| 读取 | 仅从 `mcporter` 的 `kdocs-[PLATFORM_NAME]` 配置读取 `Authorization` header；不再依赖 `.env` 或环境变量 |
| 获取 | 若 Token 为空或过期（错误码 `400006`），运行 `bash scripts/get-token.sh` 或 `node scripts/get-token.js` 获取新 Token，并直接写入 `mcporter`；mac/Linux 下 `get-token.sh` 会自动尝试打开浏览器登录页；Windows 下若本机有 Node.js，优先运行 `node scripts/get-token.js`，若本机没有 Node.js，则改为运行 `powershell -ExecutionPolicy Bypass -File scripts\get-token.ps1`；如需允许脚本自动安装 `mcporter`，可显式追加 `--auto-install-mcporter`（Node / Bash）或 `-AutoInstallMcporter`（PowerShell）；**脚本失败时改用「手动获取 Token」兜底** |
| 配置 | 仅允许将 Token 保存到 `mcporter`；禁止继续写入 `.env`、`KINGSOFT_DOCS_TOKEN` 或其他环境变量 |
| 验证 | 调用任意读取工具（如 `search_files`），返回 `code: 0` 即认证成功 |
| 过期 | 收到错误码 `400006` 时，Token 已过期，按上述「获取」步骤重新获取 |

> ⚠️ **mcporter 中未配置 Token 或 Token 过期时，所有工具调用将返回鉴权失败（400006）。**
> 🔒 **Token 安全**：任何时候都不得将 Token 明文值展示给用户、写入 `.env`、导出到环境变量，或拼接到命令中。Token 仅允许保存在 `mcporter` 的 `kdocs-[PLATFORM_NAME]` 配置中。
> 🚫 **配置名绑定**：本 Skill 的所有 `mcporter` 操作必须且只能使用 `kdocs-[PLATFORM_NAME]` 这个配置名（`mcporter call kdocs-[PLATFORM_NAME] ...`、`mcporter config get kdocs-[PLATFORM_NAME]` 等）。即使本机存在其他名称的 mcporter 配置且可正常调用，也**严禁**使用。若 `kdocs-[PLATFORM_NAME]` 配置不存在，必须先按下方「环境配置」章节完成注册。
> 🔄 **旧配置迁移**：若检测到历史 `.env` 或环境变量 `KINGSOFT_DOCS_TOKEN`，只允许做一次性迁移到 `mcporter`；`.env` 仅移除 `KINGSOFT_DOCS_TOKEN` 键（其他键保留），若 `.env` 仅含该键则直接删除空 `.env` 文件。
> 🛡️ **避免改动系统环境**：默认不会执行 `npm install -g` 这类全局安装命令；只有你明确加上参数时，才会自动安装 `mcporter`（Node / bash: `--auto-install-mcporter`，PowerShell: `-AutoInstallMcporter`）。

#### 手动获取 Token（脚本失败时的兜底方案）

当 `get-token` 脚本因环境问题执行失败时，引导用户手动获取：

1. 用户在浏览器访问 https://www.kdocs.cn/latest （需已登录 WPS 账号）
2. 点击页面右上角个人头像旁的主菜单 → 选择「龙虾专属入口」→ 复制 Token
3. 用户将 Token 提供给 Agent
4. Agent 将 Token 写入 mcporter（`<VERSION>` 从 SKILL.md frontmatter 的 `version` 字段读取）：

```bash
mcporter config remove kdocs-[PLATFORM_NAME] 2>/dev/null; mcporter config add kdocs-[PLATFORM_NAME] "https://mcp-center.wps.cn/skill_hub/mcp" --header "Authorization=Bearer <TOKEN>" --header "X-Skill-Version=<VERSION>" --header "X-Request-Source=[PLATFORM_NAME]" --transport http --scope home
```

> 收到用户 Token 后直接写入 mcporter，禁止回显 Token 明文。写入后调用任意读取工具验证（`code: 0` 即成功）。

### 环境配置

本 Skill 通过 MCP 协议提供服务，不限定特定客户端，可在任何支持 MCP 的 Agent 中运行（如 OpenClaw、Cursor、Claude Code 等）。

**自动化注册（mcporter 环境）**：运行 `bash scripts/setup.sh` 即可完成 MCP 服务注册。首次使用时会自动拉起授权；若检测到 Token 过期，`setup.sh` 也会自动调用 `get-token.sh` 重新获取。mac/Linux 下 `get-token.sh` 会自动尝试打开浏览器登录页并等待回调完成。默认不会自动全局安装 `mcporter`，若需要可显式追加 `--auto-install-mcporter`。

`scripts/setup.sh` 会自动完成：
1. 从 `SKILL.md` frontmatter 提取 `version` 版本号
2. 检查 `mcporter` 中现有的 `kdocs-[PLATFORM_NAME]` 配置，并在版本更新时保留旧 Token
3. 若检测到历史 `.env` 或环境变量 `KINGSOFT_DOCS_TOKEN`，仅做一次性迁移到 `mcporter`（`.env` 只移除 token 键并保留其他配置）
4. 注册 `mcporter` 时携带 `Authorization`、`X-Skill-Version` 和 `X-Request-Source` header，用于服务端鉴权、版本追踪和渠道区分

**手动配置（其他 MCP 客户端）**：在客户端 MCP 配置中添加金山文档服务时，仅维护 `mcporter` 中的 `kdocs-[PLATFORM_NAME]` 配置；不要再额外维护 `.env` 或 `KINGSOFT_DOCS_TOKEN`。建议在请求 header 中添加 `X-Skill-Version` 和 `X-Request-Source=[PLATFORM_NAME]` 以便追踪版本和渠道来源。

---

## 调用格式

根据运行环境选择对应方式：

- **MCP function call**（Cursor / Claude Code 等客户端）：直接构造 JSON，无需处理引号或转义：
  ```json
  {"name": "otl.insert_content", "arguments": {"file_id": "xxx", "content": "hello", "format": "markdown", "mode": "append"}}
  {"name": "read_file_content", "arguments": {"drive_id": "xxx", "file_id": "xxx", "format": "markdown", "include_elements": ["all"]}}
  ```
- **mcporter CLI**：`mcporter call` 按首个 `.` 拆分 `服务名.工具名`，工具名含点号时须分开传递以防截断：
  ```
  mcporter call kdocs-[PLATFORM_NAME] "otl.insert_content" file_id=xxx
  mcporter call kdocs-[PLATFORM_NAME] search_files keyword=test type=all
  ```
  - **数组/对象参数**：`key=value` 无法表达数组或对象，须用 `--args` 传 JSON
  - **值含空格或特殊字符**：值需引号包裹使其成为单个参数，如 `name="项目 周报.otl"`
  - **bash**：`--args` 用单引号包裹 JSON 即可：`--args '{"include_elements":["all"]}'`
  - **PowerShell**：单引号内的双引号会被吞掉，须用反斜杠转义：`--args '{\"include_elements\":[\"all\"]}'`


以下工具不可逆，调用前必须向用户确认（详细约束见各工具参考文档的「操作约束」区）：

`otl.block_delete`、`dbsheet.delete_sheet`、`kwiki.close_knowledge_view`、`sheet.delete_sheets`、`sheet.delete_range`、`dbsheet.delete_view`、`dbsheet.delete_fields`、`cancel_share`、`kwiki.delete_item`、`sheet.delete_protection_ranges`、`dbsheet.delete_records`、`sheet.delete_data_validations`、`sheet.delete_conditional_format_rules`、`sheet.delete_float_images`、`sheet.delete_filters`、`dbsheet.sheet_batch_delete`、`dbsheet.permission_delete_roles_async`

---

## 能力范围

### 支持的文档类型

| 类型 | 别名 | 文件后缀 | 说明 | 详细参考 |
|------|------|----------|------|----------|
| **智能文档** 首选 | ap | .otl | 排版美观，支持丰富组件 | `references/otl.md` — 页面、文本、标题、待办等元素操作 |
| 表格 | et / Excel | .xlsx | 数据表格专用 | `references/sheet.md` — 工作表管理、范围数据获取、批量更新 |
| PDF文档 | pdf | .pdf | PDF 文档专用 | `references/pdf.md` — PDF 创建与内容读取 |
| 文字文档 | wps / Word | .docx | 传统格式 | `references/wps.md` — Word 文档创建与内容操作 |
| 演示文稿 | wpp | .pptx | PPT 文档专用 | `references/wpp.md` — 幻灯片主题字体和配色设置、下载和导出 |
| 智能表格 | as | .ksheet | 结构化表格，支持多视图、字段管理 | `references/sheet.md` — 工作表管理、范围数据获取、批量更新 |
| 多维表格 | db / dbsheet | .dbt | 多数据表、丰富字段类型与视图（表格/看板/甘特等） | `references/dbsheet.md` — 支持数据表/视图/字段/记录的完整增删改查，含表单视图、父子记录、分享协作、高级权限与 Webhook |

### 通用工具总览

#### 文档创建与上传
| 工具 | 用途 |
|------|------|
| `create_file` | 在云盘下新建文件或文件夹 |
| `scrape_url` | 网页剪藏，抓取网页内容并自动保存为智能文档 |
| `scrape_progress` | 查询网页剪藏任务进度 |
| `upload_file` | 全量上传写入文件（更新已有 docx/pdf 或新建并上传本地文件） |

#### 文档读取与下载
| 工具 | 用途 |
|------|------|
| `list_files` | 获取指定文件夹下的子文件列表 |
| `download_file` | 获取文件下载信息 |
| `read_file_content` | 文档内容抽取为 Markdown/纯文本 |

#### 文件组织
| 工具 | 用途 |
|------|------|
| `move_file` | 批量移动文件(夹) |
| `rename_file` | 重命名文件（夹） |

#### 分享与访问
| 工具 | 用途 |
|------|------|
| `share_file` | 开启文件分享 |
| `set_share_permission` | 修改分享链接属性 |
| `cancel_share` | 取消文件分享 |
| `get_share_info` | 获取分享链接信息 |
| `get_file_link` | 获取文件的云文档在线访问链接 |

#### 搜索
| 工具 | 用途 |
|------|------|
| `search_files` | 文件（夹）搜索 |

完整参数、示例与返回值见 `references/drive.md`。

### 不支持的操作

- 无批量删除文件工具（仅支持移动）
- 云盘 drive 侧暂无逐文件 ACL 成员矩阵（以分享链接为主）；多维表格（.dbt）见 dbsheet.permission_* 与 dbsheet.share_*（详阅 references/dbsheet.md）
- 在线 Excel / 智能表格工作表区域保护见 sheet.*_protection_ranges 相关工具（详阅 references/sheet.md）
- 无文件版本回滚
- 无实时协同编辑控制

---

## 操作指南

### 执行指南

> 执行以下操作前，**必须**先阅读对应指南文件：

| 操作类型 | 指南文件 | 何时阅读 |
|----------|----------|----------|
| 获取文件标识指南 | `references/file-locating-guide.md` | 需要搜索或浏览文件时 |
| 文件读取指南 | `references/file-reading-guide.md` | 需要获取文档内容时 |
| 文件创建与写入指南 | `references/file-writing-guide.md` | 需要创建或编辑文档时 |

⚠️ 不阅读指南直接操作可能导致：参数错误、内容丢失、格式异常。

### 高频流程指引

#### 创建并写入文档

执行顺序：
1) 先按 `references/file-locating-guide.md` 获取目标目录 `drive_id`(可选)、`parent_id`(可选)。
2) 再按 `references/file-writing-guide.md` 选择文档类型与写入路径。
字段传递：步骤 1 获取 `drive_id`(可选)、`parent_id`(可选)，作为步骤 2 的输入，执行“新建写入”流程。

#### 上传本地文件到云盘

执行顺序：
1) 先按 `references/file-locating-guide.md` 获取目标目录 `drive_id`(可选)、`parent_id`(可选)、`file_id`(可选)。
2) 再按 `references/file-writing-guide.md` 的“本地文件上传（upload_file）”路径调用上传能力（新建上传或覆盖更新）。
字段传递：新建上传使用步骤 1 的 `drive_id`(可选)、`parent_id`(可选) + `name`；覆盖更新使用步骤 1 的 `file_id` 。

#### 搜索定位文档

工具说明：`search_files(keyword="关键词", type="all", page_size=20)`，获取 `file_id`、`drive_id` 供后续链路使用。
详细参数与返回结构见 `references/drive/search.md`。

### 更多操作流程

| 流程 | 说明 | 详细参考 |
|------|------|---------|
| AI 主题生成演示文稿 | 主题生成 PPT 标准链路：澄清需求、研究资料、大纲与生成上传 | `references/workflows/topic-ppt.md` |
| AI 文档生成演示文稿 | 文档生成 PPT 标准链路：创建会话、解析文档、生成大纲、美化风格与生成上传 | `references/workflows/doc-ppt.md` |
| 网页剪藏 | 抓取网页内容并自动保存为智能文档 | `references/workflows/web-scrape.md` |
| 搜索-读取-汇报撰写 | 搜索多份文档、提取信息、汇总撰写新报告 | `references/workflows/search-read-report.md` |
| 定期读取与播报 | 定期读取指定文档，提取关键信息生成摘要 | `references/workflows/periodic-read-summary.md` |
| 智能分类整理 | 列出目录，按内容或指定维度分类创建文件夹并归档 | `references/workflows/smart-classify.md` |
| 精准搜索与风险排查 | 在特定目录批量搜索文档，逐一读取分析，汇总到新文档 | `references/workflows/precise-search-analysis.md` |
| 接龙转表格 | 识别接龙文本内容，自动提取并转为在线表格 | `references/workflows/jielong-to-table.md` |
| 信息收集表单生成 | 根据用户需求自动设计并创建信息收集表格 | `references/workflows/form-generator.md` |
| 知识智能整理 | 对知识库中的零散内容进行智能化整理和结构化重组 | `references/workflows/knowledge-format.md` |
| 知识一键存入 | 将各类内容（网页、文件、文本）一键保存到知识库 | `references/workflows/knowledge-save.md` |
| 表格美化与数据规范 | 读取表格数据，进行格式美化、数据规范化和样式调整，并通过条件格式、数据校验、区域权限固化规则 | `references/workflows/table-beautify.md` |

---

## 错误速查

| 错误特征 | 原因 | 处理方式 |
|----------|------|----------|
| `400006` / 鉴权失败 | Token 过期或未配置 | 先运行 get-token 脚本重新获取；脚本失败则引导用户手动获取（见「认证配置」章节） |
| `429001` / 限频 | 请求过于频繁，响应含**限频恢复时间** | 立即停止命令调用，直到达到恢复时间；禁止立即重试、换参、换子命令连续请求 |
| `429002` / 熔断 | 多因短时间内连续触发 `429001` ，响应含**熔断持续时间** | 熔断时长内零请求，期满再试；重新规划任务避免请求过频 |
| 工具找不到 | 未注册 MCP 服务 | 运行 `bash scripts/setup.sh` 重新注册（mcporter 环境），或检查客户端 MCP 配置 |
| `mcporter` 未找到 | 运行环境缺少 mcporter | 默认不会改动系统环境（不执行全局安装）；可先手动安装后重试，或显式使用 `bash scripts/setup.sh --auto-install-mcporter` / `bash scripts/get-token.sh --auto-install-mcporter`（PowerShell: `-AutoInstallMcporter`） |
| `.env` 迁移后其他配置丢失 | 脚本会整文件删除 `.env` | 新流程仅移除 `KINGSOFT_DOCS_TOKEN` 键并保留其他键；若 `.env` 仅含该键会直接删除空 `.env` |
| 搜索无结果 | 关键词过精确 / 索引延迟 | 缩短关键词 / 等待 3-5 秒重试 |
| 读取内容为空 | 文件无内容或格式不支持 | 确认文件非空且后缀正确 |
| `read_file_content` 对 .csv 长时间 `running` | CSV 格式不支持 | 勿对 .csv 调用 `read_file_content`，建议用户转为 .xlsx 后用 `sheet.*` 读取 |
| 创建文件失败 | 文件名后缀不正确 | 检查后缀：`.otl` / `.docx` / `.xlsx` / `.ksheet` / `.dbt` / `.pdf` / `.pptx` |
| 移动文件失败 | 目标文件夹不存在 | 先搜索确认或创建文件夹 |
| HTTP 5xx / 超时 | 服务端故障 | 等 3 秒重试 1 次 |
| 验证不通过（回读值与预期不符） | 写入未生效或延迟 | 等 2 秒重新验证，仍不通过则报告用户 |
| `setup.sh` 执行失败 / 安装报错 | 当前版本可能已不兼容 | 执行上方「版本自检」流程 |
| MCP 接口返回未知错误码（非 5xx、非 400006、非 429001/429002、非工具不存在） | Skill 版本过旧导致接口不兼容 | 执行上方「版本自检」流程 |
| 错误信息含 `version`、`incompatible`、`not_supported`、`deprecated` 等版本关键词 | Skill 或 API 版本不兼容 | 执行上方「版本自检」流程 |
| 工具调用失败且原因不明 | 可能是 Skill 版本过旧 | 执行上方「版本自检」流程 |
| 工具调用失败需判断是否可重试 | 不同工具幂等性不同 | 查看该工具参考文档「操作约束」区的幂等性说明，幂等工具可安全重试，非幂等工具须先确认状态 |

---

## 安全约束

- 凭据由 MCP 运行时管理，Skill 自身不存储、不记录
- 无状态代理，不缓存任何文档内容或业务数据
- 仅在用户主动发起操作时调用对应 API



---
