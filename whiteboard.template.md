# Dispatcher Whiteboard

> **白板模板** — 每次冒烟/任务时调度员应复制此文件为 `.dispatcher/whiteboard.md` 使用

## 元信息

- **任务 ID**：`{YYYYMMDD-HHMMSS}-{slug}`
- **创建时间**：`{ISO8601}`
- **调度员版本**：`dispatcher v4`
- **白板模式**：`smoke-test | feature | refactor | incident | ad-hoc`
- **红队模式**：`auto-triggered | user-triggered | disabled`

## 工作区锚点（必须在派发**前**记录）

```yaml
anchor:
  workspace_root: {path}            # 当前工作区根（任意路径）
  git_status: {clean | dirty | not-a-repo}
  git_head: {commit hash 短7位 | null}
  git_branch: {branch | null}
  files_top_level: {count} items    # 顶层项数
  total_files: {count}              # 全工作区文件数（剔除 .git/ node_modules/）
  untracked_dotfiles: {list}        # 必须列 .git/ .vscode/ 等隐藏项
  timestamp: {ISO8601 锚点时间}     # **每次派发前重新冻结**，不信提示词里写死的时间戳
```

> **规则**：派发任何子代理前先冻结上述锚点。子代理回报里必须引用 `anchor.timestamp`，**晚于** 锚点时间的任何文件变更视为"时间窗漂移"，必须在白板追加 `drift_log` 章节。

## 任务概述

{5W2H 重述}

- **Who**：{谁提的需求}
- **What**：{要做什么}
- **Why**：{为什么做}
- **When**：{截止时间}
- **Where**：{影响的代码区}
- **How**：{技术方案概要}
- **How much**：{资源/工作量}

## 仓库类型识别

- **类型**：`empty | web-fullstack | backend-only | library | cli | desktop | data-pipeline | ml | infra | mixed`
- **证据**：

|信号文件 | 存在 | 触发的角色 |
|---|---|---|
| `package.json` 含 react/vue | ❌/✅ | 前端工程师 |
| ... |

## 漂移日志

```yaml
drift_log:
  - {ISO8601}: {变化描述} — 来源 {subagent-name | user-action | filesystem}
```

## 决策日志

- **第 1 轮**：选 A/B/C 因为 {理由}；跳过 X 因为 {理由}
- **第 2 轮**：...

## 当前轮次：第 N 轮

```yaml
round:
  number: N
  started_at: {ISO8601}
  activated: [agent-A, agent-B, agent-C]   # ≤ 3
  red_team_triggered: true | false         # v4: 由用户触发词决定
  red_team_skipped: true | false           # 若跳过红队必须为 true
  red_team_skip_reason: {string}           # 跳过原因（如「用户禁用」/「未达 P0 阈值」）
  red_team_completed: true | false         # 若触发，必须为 true 才算本轮结束
  parent_anchor: {timestamp of 锚点}
```

### 子代理回报（原样回填，不翻译）

#### {agent-name}

{回报原文}

## 阶段模板

> **以 `dispatcher.agent.md` § 阶段模板（v4）为权威源**。本节仅供白板内引用，避免与调度员定义不一致。

| 阶段 | 并行角色 | 红队（默认） |
|---|---|---|
| **1 规划** | 业务架构师 + 数据契约师（≤2） | 手动触发 |
| **2 实现** | 前端/后端/数据/算法/基础设施（按需 ≤3） | 手动触发 |
| **3 质量** | 测试工程师 + 代码卫生员（≤2） | 手动触发 |
| **4 收尾** | 性能工程师 + 文档工程师（≤2） | 手动触发 |
| **5 线上** | 故障排查员（单点） | **自动触发**（事故响应强制走红队） |

> **v4 变化**：除 stages[5] 事故响应外，红队**默认不跑**。用户需说"红队走查"等触发词才派发。

## 红队走查汇总

> v4 起本节**默认空**——仅当用户触发红队时填充。

|轮 | 触发词 | 角色 | 威胁类别 | 严重性 | 修复建议 | 状态 |
|---|---|---|---|---|---|---|
| 1 | 「红队走查」 | 红队 | Tampering | P0 | ... | open |

## 一页纸摘要（收尾时填）

- ✅ 加载验证：{N}/13 子代理
- ⚠️ 失败：{list}
- 🛡️ 红队发现：{count}
- 📌 下一步：{list}

---

**模板护栏**：`{END_OF_TEMPLATE}` 是模板与白板的分隔符。

- **模板维护者**：仅修改此行**以上**内容。修改时保持 frontmatter 风格的占位符（`{path}`、`{ISO8601}` 等）。
- **调度员 / 用户**：复制本模板到 `.dispatcher/whiteboard.md` 时，**删除**此行及以下所有内容，把 `{...}` 占位符替换为真实值。
- **红队走查**：发现白板中没有此分隔符，或分隔符之上有未替换的占位符 → 视为**白板损坏**，记 P1。

## emoji 使用约束

仅使用 `dispatcher.agent.md` § emoji 词汇表中的 emoji。