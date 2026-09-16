# AI 编程团队 — Agent & Prompt 套件

> 这是**用户级 VS Code 自定义代理目录**的发布镜像。VS Code 实际加载的是用户级源（Windows 路径为 `%APPDATA%\Code\User\prompts\`），本仓库供版本管理、跨设备同步、团队分发使用。

## 📦 内容清单

本仓库共 **16 个文件**：

| 类型 | 文件 | 说明 |
|---|---|---|
| 🟢 **调度员** | `dispatcher.agent.md` | 唯一用户可见的代理；强中心调度，并发硬上限 3 |
| 🔴 **红队** | `red-team.agent.md` | v4 起手动触发；威胁建模 + 假设证伪 |
| ⚙️ **13 个工程师** | `business-architect.agent.md`<br>`data-contractor.agent.md`<br>`frontend-engineer.agent.md`<br>`backend-engineer.agent.md`<br>`data-engineer.agent.md`<br>`algo-engineer.agent.md`<br>`infra-engineer.agent.md`<br>`qa-engineer.agent.md`<br>`code-hygienist.agent.md`<br>`perf-engineer.agent.md`<br>`sre-incident.agent.md`<br>`doc-engineer.agent.md` | 各司其职，单一职责 |
| 📋 **模板** | `whiteboard.template.md` | 白板初始结构（v4） |
| 🗑️ **占位** | `ai.agent.md` | VS Code 默认占位，可忽略 |

## 🚀 使用

### 1. 冒烟（验证团队加载）

1. 重启 VS Code 让 Copilot 扫描 `.agent.md`
2. 打开 Copilot Chat，选"调度员"
3. 在 Chat 中描述任务：例如「验证所有 13 个子代理能否被加载」

> 💡 没有预制冒烟剧本。调度员会按 v4 协议**按需派发**——不需要时不会空跑。

### 2. 真实任务（stages[1] → 5）

1. 调度员识别仓库类型（自动）
2. 空仓库 → 问用户选 6 个最小启动选项
3. 非空仓库 → stages[1] 派发业务架构师 + 数据契约师
4. 若实体冲突 → 调度员会主动询问（A/B/C/D 选项）
5. stages[2..5] 按需推进
6. stages[5] 事故响应 → 红队**自动**触发
7. 其他阶段需要对抗性审视 → 说「红队走查」

### 3. 同步流程

每次修改源文件后，**重新运行同步**：

```powershell
# Windows：把仓库内容复制到用户级 prompts 目录
Copy-Item -Path ".\*.agent.md",".\whiteboard.template.md" -Destination "<USER_PROMPTS_DIR>" -Force

# macOS / Linux：复制到对应用户 prompts 目录
# cp ./*.agent.md ./whiteboard.template.md "$HOME/Library/Application Support/Code/User/prompts/"
```

> 💡 把 `<USER_PROMPTS_DIR>` 替换为你的实际用户级 prompts 目录路径。

## 🔄 当前版本（v4）

| 项 | 当前 |
|---|---|
| 调度员 | `dispatcher v4` |
| 模板 | `whiteboard.template v4` |
| 红队 | **手动触发**（仅 stages[5] 自动） |
| 指纹机制 | ❌ 已移除 |
| 工作区硬编码 | ❌ 已移除 |

## 🔴 红队触发词（v4）

**触发**（任一即触发）：
- 中文：「红队走查」「让红队看下」「对抗性审视」「安全审查」「假设证伪」「威胁建模」
- 英文：`red team`、`adversarial review`、`threat model`、`attack surface review`、`red team this`

**取消**：
- 「跳过红队」「不用红队」「disable red team」「skip adversarial」

**自动触发**（无需用户触发）：
1. stages[5] 事故响应
2. 子代理产出含 P0
3. 漂移检测 ≥3 个文件异常变更
4. 提示词显式声明"红队必走"

## ⚖️ 团队宪法（摘要）

- 同时激活子代理 ≤ 3（**红队不计入**并行上限）
- 每个子代理必须引用 `anchor.timestamp`
- 调度员必须冻结锚点 + 漂移检测
- 提示词不硬编码工作区路径、不写死时间戳
- 只用 emoji 词汇表（见 `dispatcher.agent.md`）
- 调度员与用户**始终中文**，子代理之间不限制

## 📝 变更日志

- **v4.0**（2026-09-16）：红队手动触发；移除指纹机制；移除工作区硬编码；删除 INDEX 与 smoke-test 提示词（最小化原则）
- **v3.x**：emoji 词汇表、预计耗时模板、用户消息捕获协议

---

> 💡 **仓库 vs 用户级源**：本仓库是分发与版本控制用的镜像；VS Code 实际加载的是用户级 prompts 目录。
> 修改后请运行"使用 § 3"中的同步命令。