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

这个版本把五层能力合在一起：

- 用 `high-agency` 做行为内核
- 用 `better-work` 做产品外壳和命令面
- 用轻量 workflow 做长任务骨架
- 用 `wave-based-delivery` 做项目级 wave 编排
- 用 `round-based-execution` 做复杂局部执行控制

## 复杂度模型

### 默认 Better Work

适合仍然可以在一个受控执行流里完成的任务。

### Wave-Based Delivery

适合项目级任务，也就是必须按多个交付 wave 推进的任务。

典型信号：

- 涉及多个模块、服务、系统或环境
- 必须先 map 再做高风险执行
- integration / migration / rollout / hardening 需要分阶段
- 任务大概率跨多个 session
- 当前该做什么，需要从更大的交付计划里选出来

### Round-Based Execution

适合当前执行切片本身就很复杂，必须按 bounded round 做 PDCA 和质量门控制。

典型信号：

- 当前目标内部也有多阶段
- 本地执行风险高
- 必须先探索再安全执行
- 不能把验证拖到最后

## Wave 和 Round 的边界

这两者相关，但不是一回事：

- `wave` 决定当前交付单元是什么
- `round` 决定当前单元怎么安全执行

推荐叠加关系：

- 小任务 -> `better-work`
- 多步骤但仍然可控 -> `better-work` + 基础 workflow files
- 项目级任务 -> `better-work` + `wave-based-delivery`
- 项目级任务且当前 wave 很复杂 -> `better-work` + `wave-based-delivery` + `round-based-execution`

## Workflow 文件

基础文件：

- `TASK.md`
- `PLAN.md`
- `STATE.md`
- `HANDOFF.md`

项目级 wave 文件：

- `MAP.md`
- `WAVE.md`
- `DECISIONS.md`
- `RISKS.md`
- `wave-state.json`

局部执行 round 文件：

- `ROUND.md`
- `round-state.json`

这些文件都故意保持简短，只服务执行，不写成长项目文档。

## 什么时候用哪一层

继续用普通 `/better-work`：

- 任务大概率能在一个 bounded pass 内完成并验证
- 没有项目级顺序决策
- 额外状态文件会比收益更重

用 `/better-work waves`：

- 任务明显是项目级任务
- 需要 staged delivery waves
- 当前执行目标应该来自更大的计划
- mapping / integration / migration / rollout 需要更早进入节奏

用 `/better-work rounds`：

- 当前目标的局部执行已经足够复杂
- 你希望从一开始就按 round 跑 PDCA
- 即使不是项目级任务，也需要强执行闭环

## 命令

核心命令：

- `/better-work`
- `/better-work waves`
- `/better-work rounds`
- `/better-work verify`
- `/better-work unstick`
- `/better-work handoff`
- `/better-work review`
- `/better-work plan`
- `/better-work execute`

wave 专用命令：

- `/better-work wave-plan`
- `/better-work wave-map`
- `/better-work wave-status`
- `/better-work wave-replan`
- `/better-work wave-handoff`

## 推荐命令模式

### 普通任务

```text
/better-work 修复这个测试并验证结果
```

### 项目级交付

```text
/better-work waves 把这个多服务迁移按 wave 分阶段交付并设置 gate
```

### 复杂局部执行

```text
/better-work rounds 用 round 方式修复这个跨服务复杂 bug
```

### 项目重规划

```text
/better-work wave-replan 根据新约束重排这个项目的 wave 顺序
```

## 模板

基础模板：

- [TASK.md](templates/TASK.md)
- [PLAN.md](templates/PLAN.md)
- [STATE.md](templates/STATE.md)
- [HANDOFF.md](templates/HANDOFF.md)

项目级模板：

- [MAP.md](templates/MAP.md)
- [WAVE.md](templates/WAVE.md)
- [DECISIONS.md](templates/DECISIONS.md)
- [RISKS.md](templates/RISKS.md)
- [wave-state.json](templates/wave-state.json)

局部执行模板：

- [ROUND.md](templates/ROUND.md)
- [round-state.json](templates/round-state.json)

## 快速开始

### Codex CLI

```bash
mkdir -p ~/.codex/skills/better-work
curl -o ~/.codex/skills/better-work/SKILL.md \
  https://raw.githubusercontent.com/d-wwei/better-work-skill/main/codex/better-work/SKILL.md

mkdir -p ~/.codex/prompts
curl -o ~/.codex/prompts/better-work.md \
  https://raw.githubusercontent.com/d-wwei/better-work-skill/main/commands/better-work.md
curl -o ~/.codex/prompts/better-work-waves.md \
  https://raw.githubusercontent.com/d-wwei/better-work-skill/main/commands/better-work-waves.md
curl -o ~/.codex/prompts/better-work-rounds.md \
  https://raw.githubusercontent.com/d-wwei/better-work-skill/main/commands/better-work-rounds.md
```

然后在对话里输入：

```text
$better-work
```

显式启用 wave 模式：

```text
/better-work waves
```

显式启用 round 模式：

```text
/better-work rounds
```

## 使用预期

启用后，agent 应该更倾向于：

- 先查证，再提问
- 先验证，再宣称完成
- 重复失败后主动换方向
- 只在必要时写状态
- 真阻塞时给出高质量交接

当 `wave-based-delivery` 被启用时，还应该做到：

- 一次只选择一个 current wave
- 把项目状态保存在 chat 之外
- 先 map 再做高风险执行
- 把项目级编排和局部执行控制分开
- 输出可续跑的项目级交接

当 `round-based-execution` 被启用时，还应该做到：

- 一次只处理一个 round 的主目标
- 每一轮都走完 PDCA
- 每一轮结束前都要过质量门
- 记录假设状态、证据、依赖和 carry-forward
- 靠结构化 round state 续跑，而不是靠聊天记忆

## 为什么这版更强

之前的 Better Work 强在执行纪律，但项目级编排能力还不够强。

现在这版在保留高执行标准的同时，补上了：

- 轻量 workflow 骨架
- 项目级 wave 编排
- 局部 round 执行控制
- 项目 sequencing 和局部 execution depth 的清晰分层

结果是：默认依然轻，但在真实工程工作里更不容易丢上下文、丢顺序、丢边界。

## 仓库补充说明

- `.gitignore` 已忽略 `.DS_Store` 这类本地噪音文件
- `subskills/` 用来存放像 `wave-based-delivery` 和 `round-based-execution` 这样的条件增强模块
- `evals/trigger-prompts/` 提供最小触发样例
- `evals/closeout-cases.md` 用来约束后续版本的收尾行为
