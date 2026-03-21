# better-work-skill

### 面向编码代理的验证优先执行协议

**[🇺🇸 English](README.md)** | **🇨🇳 中文**

`better-work-skill` 是一个轻量的 coding agent 工作协议。它不提供新工具，也不提供新模型能力，而是优化 agent 使用现有工具的方式：

- 更少猜测
- 更少过早放弃
- 更少过早要求用户介入
- 更少“没验证就说完成”
- 更多主动排查
- 更多基于证据的收尾
- 更好的阻塞交接质量
- 更好的多步骤任务连续性

这个版本把四层能力合在一起：

- 用 `high-agency` 做行为内核
- 用 `better-work` 做产品外壳和命令面
- 用轻量版 GSD 思路做长任务骨架
- 用 `round-based-execution` 做复杂任务的轮式执行增强模块

## 现在它多了什么

除了原来的高执行标准之外，这个版本还新增了：

1. 轻量阶段流
2. `plan / execute` 两个模式
3. 面向长任务的最小状态文件
4. 面向复杂任务的轮式执行协议

它的目标不是把简单任务变重，而是让中等复杂度任务更稳。

## 工作流模型

Better Work 现在采用四阶段模型：

1. `intake`：澄清目标、约束、成功标准
2. `plan`：拆成可验证的小步
3. `execute`：逐步推进并保留最新证据
4. `closeout`：以验证完成或结构化交接结束

对于小任务，直接执行，不强制写文件。

对于多步骤或跨会话任务，可以使用 `templates/` 里的四个模板：

- `TASK.md`
- `PLAN.md`
- `STATE.md`
- `HANDOFF.md`

对于复杂任务，还可以启用轮式协议，并额外使用：

- `ROUND.md`
- `round-state.json`

这些文件都应该保持简短，只服务执行，不写成长文档。

## 什么时候该用状态文件

以下场景建议启用这些模板：

- 任务涉及多个文件或多个系统
- 任务大概率会跨会话继续
- 任务已经开始循环或丢上下文
- 任务很可能需要交接

如果任务足够小，能马上完成并验证，就不要强行写文件。

## 复杂任务增强模式

`round-based-execution` 不是一个独立平级 skill，而是 `better-work` 下面的 subskill / execution protocol module。

它适合这些场景：

- 任务天然是多阶段推进
- 任务涉及多个文件、模块、系统或 agent
- 任务里有明显未知，必须先探索再执行
- 任务很可能跨多个 session
- 任务风险较高，不能把验证拖到最后
- 任务已经出现 scope creep 或上下文开始变脏

它解决的问题是：

- 任务越做越长，后半程越来越浅
- 前期弱假设被后期当真复用
- 多 agent 工作碎片化
- “做过了”被误认为“做好了”
- 没有轮次级别的闭环、质量门和交接物

轮式模式下，agent 会：

- 每次只推进一个受控 round
- 每个 round 都经过 `PDCA`
- 每个 round 都必须给出质量门结果
- 没有 `PASS` / 合规的 `PARTIAL_PASS` 就不能进入下一轮
- 把关键假设、证据等级、依赖、遗留项写进结构化状态

参考：

- [activation-policy.md](activation-policy.md)
- [subskills/round-based-execution.md](subskills/round-based-execution.md)
- [templates/ROUND.md](templates/ROUND.md)
- [templates/round-state.json](templates/round-state.json)

## 自动激活和显式激活

现在有两种进入轮式模式的方式：

- 自动激活：使用 `/better-work`，由 Better Work 根据任务复杂度决定是否切入轮式模式
- 显式激活：使用 `/better-work rounds`，直接强制进入轮式工作法

什么时候用普通 `/better-work`：

- 任务也许还能在一个受控 pass 内完成
- 你希望协议自己判断是否需要更重的结构

什么时候直接用 `/better-work rounds`：

- 你已经明确知道这是复杂任务
- 你希望从第一步开始就按 round 推进
- 你希望每一轮都有 PDCA、质量门和结构化交接
- 你不希望任务演变成一条无限拉长的线性线程

## 命令

- `/better-work`
- `/better-work rounds`
- `/better-work verify`
- `/better-work unstick`
- `/better-work handoff`
- `/better-work review`
- `/better-work plan`
- `/better-work execute`

其中：

- `rounds`：显式启用轮式工作法，强制进入 `round-based-execution`
- `verify`：强化验证和收尾质量
- `unstick`：强化诊断、换路和排障恢复
- `handoff`：输出高信息量交接
- `review`：检查同类问题、边界和上下游影响
- `plan`：澄清任务并拆解步骤
- `execute`：从现有计划或状态继续推进

## 推荐命令用法

### 普通任务

```text
/better-work 修复这个测试并验证结果
```

### 复杂 bug

```text
/better-work rounds 用轮式工作法修复这个跨服务复杂 bug
```

### 陌生代码库研究

```text
/better-work rounds 研究这个陌生代码库，并留下可续跑 round 状态
```

### 中型功能开发

```text
/better-work rounds 把这个功能拆成多轮最小切片实现，并每轮验证
```

## 轻量模板

这些模板故意保持很短，方便真实使用：

- [TASK.md](templates/TASK.md)
- [PLAN.md](templates/PLAN.md)
- [STATE.md](templates/STATE.md)
- [HANDOFF.md](templates/HANDOFF.md)
- [ROUND.md](templates/ROUND.md)
- [round-state.json](templates/round-state.json)

复杂任务推荐显式使用：

- `/better-work rounds`

显式 rounds 模式下，最常用的状态文件组合是：

- `TASK.md`：整体目标、约束、成功标准
- `PLAN.md`：更大的执行路径
- `STATE.md`：当前总体进展
- `ROUND.md`：当前这一轮的边界、动作、验证、质量门
- `round-state.json`：机器友好的轮次持久化状态

## 快速开始

### Codex CLI

```bash
mkdir -p ~/.codex/skills/better-work
curl -o ~/.codex/skills/better-work/SKILL.md \
  https://raw.githubusercontent.com/d-wwei/better-work-skill/main/codex/better-work/SKILL.md

mkdir -p ~/.codex/prompts
curl -o ~/.codex/prompts/better-work.md \
  https://raw.githubusercontent.com/d-wwei/better-work-skill/main/commands/better-work.md
curl -o ~/.codex/prompts/better-work-rounds.md \
  https://raw.githubusercontent.com/d-wwei/better-work-skill/main/commands/better-work-rounds.md
```

然后在对话里输入：

```text
$better-work
```

如果你想显式启用轮式模式：

```text
/better-work rounds
```

## 使用预期

启用后，agent 应该更倾向于：

- 先查证，再提问
- 先验证，再宣称完成
- 重复失败后主动换方向
- 长任务时只在必要时写轻量状态
- 真阻塞时给出有边界、有证据的交接

当 `round-based-execution` 被启用时，还应该做到：

- 一次只处理一个 round 的主目标
- 每一轮都走完 `PDCA`
- 每一轮结束前都要过质量门
- 记录假设状态、证据等级、依赖门和 carry-forward 分类
- 依赖结构化 round state 续跑，而不是靠聊天记忆

## 为什么这版更强

旧版 Better Work 强在执行纪律，但对长任务的上下文保持支持不够。

这版保留了原来的“高执行标准”，同时补上了：

- 轻量计划模式
- 连续执行模式
- 最小状态模板
- 小任务和长任务的分流策略

再加上 `round-based-execution` 之后，它还能在复杂任务里做到：

- 把线性长任务改造成轮次闭环
- 防止弱证据和未验证假设跨轮放大
- 让多 agent 协作时主线更清晰
- 让跨 session 恢复更稳定

结果是：依然轻，但更不容易在真实工程任务里散架。

## 仓库补充说明

- `.gitignore` 已忽略 `.DS_Store` 这类本地噪音文件
- `evals/trigger-prompts/` 提供最小触发样例
- `evals/closeout-cases.md` 用来约束后续版本的收尾行为
- `subskills/` 目录用于存放像 `round-based-execution` 这样的条件增强模块
