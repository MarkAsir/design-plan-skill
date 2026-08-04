# Design Plan Convergence

[English](README.md)

## 为什么需要这个 skill

模型生成的设计和计划文档未必完善。即使使用 OpenSpec、Superpowers 等成熟 skill 生成了结构化的设计或计划，也不能替代针对当前项目事实、门禁、风险、兼容性、回滚和验证责任的独立审核。如果缺少这一环节，重要的边界和依赖可能仍然是隐含的，或者没有得到证据验证。

`design-plan-convergence` 正是用来补上这个审核环节：通过证据驱动的门禁审核、风险适配的问题分流、授权整改、支持断点恢复的多轮收敛，以及独立最终复审，帮助设计和计划文档达到可执行的就绪状态。

`design-plan-remediation` 针对 `design-plan-convergence` 发现的问题进行再次确认，并在授权的设计或计划文档范围内实施最小修改。`design-plan-convergence-loop` 则将审核与整改两个环节串联起来，循环执行审核、问题确认、修改和复审，直到问题收敛、需要决策，或被正确路由到后续阶段。

面向设计文档与实施计划的证据驱动、多轮审核与整改 skill 集合。

它会先冻结当前门禁、范围、决策和证据边界，再按风险审查文档；当发现当前门禁的 `BLOCKER` 或 `REQUIRED` 问题时，只在获得授权的规划文档范围内整改，并通过后续复审验证是否收敛。

支持以下文档形态：

- OpenSpec change 文档：proposal、spec、design、tasks 等角色化产物。
- Superpowers implementation plan。
- 不遵循固定框架的通用设计文档和实施计划。
- 冻结范围明确包含用户可见 UI/UX 行为的设计文档和实施计划。

## 核心特性

- 多轮闭环：初始审核 → 授权整改 → 影响审计 → 趋势判断 → 最终盲审。
- 断点恢复：在中断、超时或跨会话场景保存最小运行状态；恢复前重新校验运行身份、门禁、边界和快照，不匹配时重新开始对应审核代次。
- 门禁明确：支持 `DESIGN READY` 和 `PLAN READY`，不会自动越过实现或发布门禁。
- 风险分层：默认 `AUTO`，也支持 `LEAN` 和 `ASSURANCE`；只对安全、数据、兼容性、并发、回滚等高风险切片加深审核。
- 证据隔离：基于当前快照、路径状态、文件哈希、规则快照和外部事实版本，避免把旧报告或整改声明当成当前事实。
- 发现分流：区分 `BLOCKER`、`REQUIRED`、`CONCERN`、`WATCH`、`NEXT-STAGE NOTE`、`OUT OF SCOPE`、`REJECTED` 等状态。
- 范围控制：不修改实现、测试、外部系统或冻结的上游决策；整改工具只交付 `CANDIDATE CLOSED`，最终关闭由独立审核确认。
- 阶段正确的验证：设计阶段保留行为、验收场景和验证责任；计划阶段检查路径、依赖、顺序、命令和稳定 oracle，不把未来执行结果伪装成已完成。
- 有界 UI/UX 规划审核：仅在 UI/UX 行为明确进入范围时，检查关键旅程、相关用户可见状态、适用约束、责任、追溯和稳定 oracle；不生成原型、不评价审美，也不规定前端实现细节。

## 三个 skill

| Skill                                                                   | 用途                        | 是否修改文档         |
| ----------------------------------------------------------------------- | ------------------------- | -------------- |
| [`design-plan-convergence`](design-plan-convergence/SKILL.md)           | 对当前设计或计划执行一次只读、风险适配的门禁审核  | 否              |
| [`design-plan-convergence-loop`](design-plan-convergence-loop/SKILL.md) | 在授权范围内执行审核、整改、复审的多轮收敛闭环   | 仅修改获授权的设计/计划文档 |
| [`design-plan-remediation`](design-plan-remediation/SKILL.md)           | 根据已确认的当前门禁发现，对规划产物做最小根因修订 | 仅修改获授权的设计/计划文档 |

典型协作关系：`convergence` 负责发现问题，`remediation` 负责授权整改，`convergence-loop` 负责编排多轮流程并由独立审核给出最终门禁结论。

## 用法

### 只读审核

适合先确认设计是否足以进入计划阶段，或确认现有计划是否具备执行条件：

```text
使用 $design-plan-convergence 审核 openspec/changes/<change-name>，目标门禁为 DESIGN READY。
只读检查，不修改任何文件；按 AUTO 风险配置输出问题、证据和门禁结论。
```

审核实施计划时：

```text
使用 $design-plan-convergence 审核 <plan-file>，目标门禁为 PLAN READY。
将上游设计视为冻结基线，检查依赖、粒度、风险、覆盖和可执行性。
```

审核明确包含 UI/UX 范围的设计或计划时：

```text
使用 $design-plan-convergence 审核 <design-or-plan-path>，选择对应的文档门禁。
仅对冻结范围内的用户可见行为应用有界 UI/UX 规划 profile；不生成视觉产物，也不扩展到前端实现。
```

### 多轮审核与整改

当你明确允许修改设计或计划文档时：

```text
使用 $design-plan-convergence-loop 对 <design-or-plan-path> 执行多轮审核、整改和最终复审。
目标门禁为 DESIGN READY；只允许修改指定规划文档，不修改代码、测试或外部系统。
```

闭环会在以下情况停止：

- 当前门禁已经满足，且最终快照未发生变化。
- 需要用户确认产品、范围、风险接受或写入权限。
- 问题属于实现、集成、发布等后续阶段。
- 发现整改出现停滞、发散或未经批准的新机制。

### 单独整改

如果已经有独立审核结果，并且只想修订获授权的规划文件：

```text
使用 $design-plan-remediation 根据当前快照中的 BLOCKER 和 REQUIRED 修订 <plan-file>。
保留冻结设计和既有结构，只做最小根因修复，并输出供独立复审的候选关闭证据。
```

`design-plan-remediation` 不会自行授予 `CLOSED` 或 `READY`。如果计划问题暴露了上游设计缺陷，会返回 `UPSTREAM DESIGN REOPEN REQUIRED`，而不是通过修改计划掩盖设计问题。

## 审核维度

审核会沿着“证据 → 契约 → 决策 → 工作项 → 验证 → 交付与恢复”的完整链路展开，不会停留在格式或清单是否齐全。

### 设计审核的角度

| 角度    | 重点检查                                           |
| ----- | ---------------------------------------------- |
| 前提与价值 | 问题是否有证据，价值是否清晰，可观察结果是否定义，以及完成标准是否可判断。          |
| 范围与边界 | 范围、Non-Goals、不得改变的行为、验收边界是否明确，是否存在隐性范围扩张。      |
| 架构与契约 | 行为是否有明确归属，决策和权衡是否清楚，兼容性和中间状态是否覆盖，受影响消费者是否识别。   |
| 交付与运维 | 依赖、实施责任、发布、风险、回滚或 fail-closed 行为，以及残余风险责任是否明确。 |

### 计划审核的角度

| 角度  | 重点检查                                                 |
| --- | ---------------------------------------------------- |
| 依赖  | 是否先产出后使用，环境、权限、所有者和阶段顺序是否可信。                         |
| 粒度  | 每个任务是否有一个行为结果、稳定中间状态和可用的验证 oracle；是否按行为、兼容性或失败边界拆分。  |
| 风险  | 破坏性操作、兼容性边界、信任边界、生产激活、回滚和 fail-closed 行为是否有安全顺序与责任人。 |
| 覆盖  | 每个已批准契约是否映射到实施工作和验证责任，每个任务是否都能追溯回已批准契约。              |
| 可行性 | 路径、符号、接口、工具、工作目录、前置条件、命令、预期结果和失败解释是否基于当前事实成立。        |

当冻结范围有权威依据地包含 UI/UX 行为时，审核还会检查关键旅程、仅影响范围内行为或稳定 oracle 的用户可见状态、适用约束、责任和设计到计划的追溯。它不评价审美质量，也不强制组件结构、视口矩阵或测试类型。

## 门禁范围

### DESIGN READY

审核设计是否已经具备进入实施计划阶段的条件：

- 问题、价值、范围、Non-Goals 和可观察结果。
- 行为契约、验收边界、拒绝类和关键场景。
- 架构归属、兼容性、权衡、受影响消费者和中间状态。
- 依赖、实施责任、风险、回滚或 fail-closed 原则。
- 每条关键决策链的验证责任和后续阶段归属。

### PLAN READY

审核实施计划是否能够被可靠执行：

- 依赖和阶段顺序是否满足“先产出、后使用”。
- 任务是否按行为结果、兼容性阶段或失败边界拆分。
- 路径、符号、接口、所有者、权限、工作目录和命令是否可信。
- 每个已批准契约是否映射到工作项和验证责任，且每个工作项都能追溯回契约。
- 破坏性操作、兼容性边界、回滚和 fail-closed 责任是否明确。

实现、集成、部署、发布和完整 E2E 结果属于后续门禁，不会因为文档审核通过而自动执行。

## 安装

本项目是纯 skill 文档包，不引入额外运行时依赖。将三个目录复制到 skill 运行器识别的技能目录即可；不要只安装 `convergence-loop`，因为闭环依赖审核和整改两个角色。

### Codex（Windows PowerShell）

在仓库根目录执行：

```powershell
$codexSkills = Join-Path $env:USERPROFILE ".codex\skills"
New-Item -ItemType Directory -Force $codexSkills | Out-Null
Copy-Item -Recurse -Force .\design-plan-convergence, .\design-plan-convergence-loop, .\design-plan-remediation $codexSkills
```

### Codex（Linux/macOS）

```bash
mkdir -p ~/.codex/skills
cp -R design-plan-convergence design-plan-convergence-loop design-plan-remediation ~/.codex/skills/
```

安装后重启或刷新 skill 运行器，然后使用上面的 `$design-plan-*` 名称调用。

## 目录结构

```text
design-plan-convergence/
├── SKILL.md
├── agents/openai.yaml
└── references/
design-plan-convergence-loop/
├── SKILL.md
├── agents/openai.yaml
└── references/
design-plan-remediation/
├── SKILL.md
├── agents/openai.yaml
└── references/
```

`references/` 包含通用设计、有界 UI/UX 规划、实施计划、风险门禁、证据快照、状态编排和整改准入等规则；skill 会按权威产物范围和目标文档类型加载对应规则，不要求通用文档强行改成 OpenSpec 目录结构。

## 许可证

本项目采用 [MIT License](https://opensource.org/license/mit)。
