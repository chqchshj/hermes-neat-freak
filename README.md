# hermes-neat-freak

一个面向 **Hermes Agent** 的“收尾同步”技能：在一个开发阶段结束后，系统性检查并同步 **项目文档、Hermes 记忆、可复用技能、跨会话上下文**，避免代码已经变了，但 `AGENTS.md`、README、网站文档、记忆和技能还停留在旧状态。

## 这是什么

这个技能来自对 `neat-freak` 思路的 Hermes 化改造。

原始 `neat-freak` 很强，核心是：
- 把 AI 从“记录员”变成“编辑”
- 收尾时不只是追加，而是要审查、合并、修正、删除
- 保证项目知识层和真实代码保持同步

但 Hermes Agent 有自己非常鲜明的知识机制：
- `memory`：长期稳定事实
- `skill_manage`：可复用流程
- `session_search`：跨会话回顾
- `AGENTS.md`：项目级 agent 指令

所以我把它改造成了一个 **Hermes-native** 版本。

## 适用场景

当你刚做完一轮开发、调试、重构、文档整理、平台接入，想让 Hermes 做一次真正的“收尾”时使用。

典型触发词：
- `收尾一下`
- `同步一下`
- `整理一下`
- `sync up`
- `/neat`

## 它会做什么

- 枚举当前项目文档（`AGENTS.md`、`README.md`、`docs/`、散落的 markdown）
- 回顾当前对话和相关历史会话
- 判断哪些变化应该写进文档，哪些应该写进记忆，哪些应该沉淀成 skill
- 修正文档漂移、记忆漂移、技能漂移
- 输出一份简洁的同步摘要

## 相比原版 neat-freak 的改动

### 1. 明确适配 Hermes 的三套知识机制
原版更偏通用 agent。这个版本把 Hermes 的三层机制拆清楚了：

- **稳定事实** → `memory`
- **可复用流程** → `skill_manage`
- **历史任务过程** → `session_search`

避免把：
- 临时过程塞进 memory
- 稳定事实写进 skill
- 本该 session_search 回顾的内容长期堆在记忆里

### 2. 收尾流程里加入 `session_search`
在 Hermes 里，跨会话背景本来就可以搜索回放，所以这个版本把 **历史 session 回顾** 明确纳入收尾步骤，减少用户重复解释背景。

### 3. 补充 Hermes 的技能目录与 external dirs 约定
增加了：
- `~/.hermes/skills/` 是主技能目录
- `skills.external_dirs` 更适合做只读发现源
- agent-managed skill 默认应优先写入本地主技能目录

### 4. 增加“事实 vs 流程”分层原则
明确要求：
- 事实写进 memory
- 流程写成 skill
- 文档归文档
- 不混写

### 5. 增加高风险外部写入前确认的提醒
对于数据库批量写入、线上配置变更等高风险动作，这个版本特别强调：
- 文档里可以记录方案与约束
- 但没有确认时，不能写成“已执行”

### 6. 更贴近 Hermes Agent 仓库的文档结构
这个版本更适合检查：
- `AGENTS.md`
- `README.md`
- `website/docs/`
- skills / memory / docs 之间的漂移

## 安装方式

### 方式一：让支持 Skill 的 Agent 直接安装

在 Claude Code、Codex、OpenCode、OpenClaw、Hermes 这类支持 skills 的 agent 里，直接说：

```text
帮我安装这个 skill：https://github.com/chqchshj/hermes-neat-freak
```

如果 agent 支持从仓库根目录直接安装，它会自动读取 `SKILL.md` 和 `references/`。

### 方式二：手动安装到 Hermes

把仓库 clone 到本地后，复制到 Hermes 的技能目录：

```bash
git clone https://github.com/chqchshj/hermes-neat-freak.git
mkdir -p ~/.hermes/skills/hermes-agent/
cp -r hermes-neat-freak ~/.hermes/skills/hermes-agent/
```

安装后，Hermes 中可按技能名加载：

```text
/hermes-neat-freak
```

或者在自然语言里直接触发，例如：
- `收尾一下`
- `同步一下文档和记忆`
- `整理一下`
- `sync up`

### 方式三：作为外部技能目录挂载

如果你已经有统一的 skills 仓库，也可以把它放进外部技能目录，然后在 `~/.hermes/config.yaml` 里配置：

```yaml
skills:
  external_dirs:
    - /path/to/your/skills-repo
```

> 注意：对 Hermes 来说，`external_dirs` 更适合作为**只读发现源**。如果后续 agent 要修改/增强技能，建议沉淀到 `~/.hermes/skills/` 本地主目录。

## 仓库结构

```text
.
├── SKILL.md
├── README.md
└── references/
    ├── agent-paths.md
    └── sync-matrix.md
```

## 文件说明

- `SKILL.md`：主技能定义
- `references/agent-paths.md`：Hermes / Claude Code / Codex / OpenCode / OpenClaw 的知识与技能路径速查
- `references/sync-matrix.md`：常见代码变更 → 应同步哪些文档/记忆/技能

## 适合谁

适合这些人：
- 长期用 Hermes 跑开发与维护
- 很在意文档和记忆别“脑腐”
- 经常跨 session 持续迭代项目
- 希望 AI 不只是改代码，还能把知识体系一起维护干净

## 致谢

灵感来自 `neat-freak` 的原始思路，这个仓库是在其基础上面向 Hermes Agent 的定向改造版。
