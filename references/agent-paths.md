# Agent 记忆与配置路径速查

## Hermes Agent

| 用途 | 路径 / 机制 |
|---|---|
| 长期事实记忆 | Hermes `memory` 工具（持久化到 Hermes 自身存储，不直接手改 markdown） |
| 跨会话回顾 | `session_search` 工具 |
| 项目级指令 | 项目根 `AGENTS.md` |
| 技能主目录 | `~/.hermes/skills/` |
| 外部技能目录 | `config.yaml` → `skills.external_dirs`（默认只读发现，不作为 agent-managed 默认写入位置） |

Hermes 的一个关键差异是：**记忆、技能、会话检索是三套独立机制**。稳定事实写入 `memory`，可复用流程沉淀为 skill，历史任务过程通过 `session_search` 回看。不要把三者混用。

## 其他常见 Agent（简表）

| 平台 | 项目级指令 | 记忆/技能特征 |
|---|---|---|
| Claude Code | `CLAUDE.md` | 有独立记忆目录与 skills 目录 |
| OpenAI Codex | `AGENTS.md` | 跨会话信息多落在 AGENTS.md / skills |
| OpenCode | `.opencode/` + `AGENTS.md` | 会扫描多种 skills 目录 |
| OpenClaw | 项目根 markdown + `.openclaw/skills/` | 无独立 memory 索引机制 |
