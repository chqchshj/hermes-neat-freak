# 变更影响矩阵

遇到不确定“这次改动要同步哪些文件”时查这张表。

## Hermes 常见代码层变更 → 文档层变更

| 本次发生的事 | 要改的文件 |
|---|---|
| 新 slash command / alias | `AGENTS.md` · `website/docs/reference/slash-commands.md` · 对应 user-guide |
| 新 provider / auth 行为 / picker 变化 | `AGENTS.md` · `website/docs/integrations/providers.md` · `website/docs/user-guide/configuration.md` |
| 新 config 字段 / timeout / quick command 行为 | `AGENTS.md` · `website/docs/user-guide/configuration.md` · `website/docs/user-guide/cli.md`（若用户可见） |
| 新工具 / toolset / skills 机制 | `AGENTS.md` · `website/docs/user-guide/features/*.md` · `website/docs/reference/*.md` |
| 新 gateway 平台能力 | `AGENTS.md` · 对应 messaging/platform docs |
| 新开发流程 / contributor 约定 | `AGENTS.md` · README · developer-guide |

## Hermes 记忆 / 技能层变更

| 情况 | 处理方式 |
|---|---|
| 稳定事实新增或变更 | 用 `memory` 更新 |
| 发现可复用流程 | 用 `skill_manage` 新建或修补 skill |
| 临时任务过程 | 不写 memory；如需回顾用 `session_search` |
| skill 过时 / 缺步骤 | 立刻 patch skill |
| 记忆与文档冲突 | 先确认代码真实状态，再同步修正 |

## 高频漏改点

- 代码支持了新命令，但 slash-commands/reference 没更新
- quick_commands / timeout / config 行为改了，但 configuration 文档没更新
- `AGENTS.md` 写了新规则，README 或 docs 还停留在旧行为
- 应该沉淀成 skill 的流程，还只存在于聊天记录里
