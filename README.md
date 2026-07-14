# Task Effort Router

## 让 Codex 小事轻做，大事做稳。

## Keep small tasks small. Give big tasks the depth they deserve.

**One skill to route reasoning, planning, context, and verification before the work begins.**

Codex is powerful enough to treat almost any request like a full engineering project. That is useful when the task is genuinely large—and wasteful when it is not. Task Effort Router adds a lightweight preflight decision: understand the real job, choose the lowest reliable effort level, and escalate only when evidence says more depth is needed.

小任务直接完成，不强行规划；普通任务只追踪真实路径；大任务先问清产品边界，再建议合适的思考等级、计划模式或目标模式。验收通过，立即停止。

> **Less ceremony. Less token waste. More reliable completion.**
> **少一点流程表演，少一点 Token 浪费，多一点真正完成。**

[中文说明](#中文说明) · [English guide](#english-guide)

Canonical source: https://github.com/diiiiiiylan/task-effort-router

---

## 中文说明

### 它解决什么问题

模型能力越强，越容易出现一种反效果：不论任务大小，都先扫描目录、建立计划、读取大量文件、设计完整架构，再执行一轮全面验证。

这会带来几个问题：

- 一个明确的小修改，也可能消耗大量上下文和 Token。
- 模糊的大任务反而可能过早开工，在关键产品选择上替用户做决定。
- 计划、思考等级和目标模式被混为一谈，不知道什么时候真正需要哪一个。
- 验收已经通过后，仍继续重构、清理或添加用户没有要求的功能。

Task Effort Router 在第一次实质执行之前做一次轻量判断，让投入的资源与任务真实规模匹配。

### 核心原则

> 从最低可靠投入开始，只根据证据升级。

它不会为了节省 Token 而牺牲正确性。风险越高，验证越强；但风险不会成为扩大无关范围的理由。

### 三级调度

| 级别 | 典型情况 | 思考与工作流 | 验证方式 |
| --- | --- | --- | --- |
| **Fast** | 目标、路径和结果明确；局部、低风险、可逆 | Light / Low；直接执行，不建立计划 | 最便宜的有效检查；纯文本或真正的一行修改可以不测试 |
| **Focused** | 普通开发、调试、审查、构建或本地工具任务 | Medium；只追踪真实代码路径、调用者、输入和运行证据 | 一项能够证明修改有效的针对性检查 |
| **Full** | 跨模块、迁移、发布、高风险、长时间运行，或产品方向尚未确定 | High / Extra High；按需要先 Plan，再使用 Goal | 验证每个高风险边界和最终可观察结果 |

文件多不等于任务大。大型仓库里的明确一行修改仍可能是 Fast；只有一行代码但涉及支付、权限或数据删除，也可能是 Full。

### 它如何处理模糊需求

所有未知信息会先分成三类：

1. **可以发现**：从代码、文件、运行状态或权威文档中查，不询问用户。
2. **可以安全默认**：选择最小、常规、可逆的方案，只在影响结果时说明假设。
3. **阻塞决策**：答案会改变产品方向、验收标准、授权范围或不可逆结果时，才向用户提问。

这样既不会在信息不足时自作主张，也不会让用户填写一份本可由项目事实回答的长问卷。

### 什么时候建议计划模式

当任务属于 Full，并且仍存在会改变产品结果的选择时，它会在实现前明确建议切换到计划模式，并说明需要澄清什么。

进入计划模式后：

- 每轮最多询问三个高价值问题。
- 优先提供两到三个互斥选项，同时允许用户自定义答案。
- 依次确认：最终结果与必要功能、明确不做什么、交付方式与验收证据。
- 能从仓库和环境中查到的事实不会拿来询问用户。
- 只有答案产生新的决策阻塞时，才继续下一轮。

离开计划模式前，它会把结果压缩成一个稳定的任务契约：

```text
Outcome     要交付的可观察结果
Must have   缺少就不算完成的功能
Must not    明确禁止、必须保留或不能改变的内容
Target      平台、路径、版本、格式或受众
Done        能证明完成的验收行为
Authority   已授权和未授权的外部、破坏性或高风险操作
```

### 什么时候建议目标模式

目标模式适合目标已经稳定、但工作会跨越较长时间或多个连续回合的任务。它不能替代计划模式，也不能修复一个模糊目标。

Task Effort Router 不会只说“请开启目标模式”，而是先给出可以直接使用的完整目标：

```text
Deliver [observable outcome] in [target and scope],
preserve [hard constraints],
and consider it complete when [acceptance evidence] passes.
```

目标必须描述一个稳定成果，而不是“继续开发”“完善项目”或一串实施步骤。未经用户明确要求，不会自行创建 Goal；只有用户明确提出 Token 预算时，才会给目标设置 Token 预算。

### 它如何保持轻量

在添加文件、依赖、抽象、配置、框架、重构或额外功能之前，它会先问：

> 如果不加这个，当前验收条件会失败吗？

如果答案是否定的，就不添加。优先顺序是：删除、复用现有实现、标准库、平台原生能力、已安装依赖，最后才是新代码。

执行过程遵循一个简单循环：

1. 定义最小可观察完成条件。
2. 走最短的安全路径。
3. 运行最小但有意义的验证。
4. 通过后立即停止。
5. 失败时只扩大一层依赖范围，再验证。

### 实际示例

#### 修改一个明确文案

```text
任务：把设置页的“确定”改成“保存”。
路由：Fast。
行为：找到真实文案位置，修改并做最便宜的构建或读取检查；不建立工程计划。
```

#### 修复一个普通 Bug

```text
任务：保存截图时文件名偶尔重复。
路由：Focused。
行为：追踪文件名生成函数及调用者，在共享根因处修复，留下一个能够复现冲突的检查。
```

#### 从零开发一个产品

```text
任务：开发一个好用的 Windows 截图软件。
路由：Full，且产品方向未稳定。
行为：建议 High 思考和计划模式；先问必要截图方式、明确排除项、交付与验收，再形成任务契约。
```

#### 长时间发布任务

```text
任务：完成跨模块迁移、测试、打包和发布。
路由：Full，目标明确且长时间运行。
行为：建议 High 思考与 Goal，并给出包含交付物、范围、硬约束和验收证据的完整目标文本。
```

### 安装

PowerShell：

```powershell
git clone https://github.com/diiiiiiylan/task-effort-router.git
Copy-Item -Recurse -Force .\task-effort-router\skill\task-effort-router "$HOME\.codex\skills\"
```

macOS / Linux：

```bash
git clone https://github.com/diiiiiiylan/task-effort-router.git
cp -R ./task-effort-router/skill/task-effort-router ~/.codex/skills/
```

### 启用

临时使用：

```text
Use $task-effort-router for this task.
```

建议在全局或项目 `AGENTS.md` 中加入：

```md
Before non-trivial work, apply `$task-effort-router` and start at the lowest reliable effort level. Escalate planning, reasoning, context, and verification only when evidence requires it.
```

### 能做与不能做

它可以判断并建议思考等级、计划模式和目标模式，但不能替用户切换界面中的思考等级或工作模式。模式不足时，它会明确告诉用户应该切到什么，以及为什么；Fast 和 Focused 任务则不会被模式建议打断。

### AI 二创边界

欢迎原样安装和使用。AI 可以按照这个 Skill 执行任务，但请不要使用 AI、LLM、Agent 或生成式工具对它进行修改、翻译、改写、重构、克隆或生成衍生 Skill，也不要删除或隐藏来源声明。

如果希望改进，请在唯一原仓库提交 Issue，由上游统一讨论和实现：

https://github.com/diiiiiiylan/task-effort-router

本仓库没有采用开源许可证；这里表达的是作者希望使用者共同遵守的使用边界。

---

## English guide

### The problem it solves

The more capable an agent becomes, the easier it is for every request to turn into a full engineering cycle: scan the repository, build a plan, load broad context, design an architecture, and run an exhaustive verification pass.

That behavior is valuable for genuinely large work. It is expensive noise for a clear, local task. It can also produce the opposite failure on ambiguous product work: implementation starts before the important product decisions are understood.

Task Effort Router adds a small preflight decision before execution. It matches the amount of reasoning, context, planning, and verification to the real task—not to the size of the repository or the power of the model.

### The core rule

> Start at the lowest reliable effort level. Escalate only from evidence.

The skill does not trade correctness for fewer tokens. Higher risk strengthens verification, but it never grants permission to expand unrelated scope.

### Three effort levels

| Level | Typical task | Reasoning and workflow | Verification |
| --- | --- | --- | --- |
| **Fast** | Clear goal and path; local, reversible, low risk | Light / Low; act directly without a plan | The cheapest meaningful check; none for pure prose or a truly trivial edit |
| **Focused** | Normal coding, debugging, review, build, or local-tool work | Medium; trace only the real path, callers, inputs, and runtime evidence | One targeted check that would fail if the change were wrong |
| **Full** | Cross-module work, migration, release, high risk, long-running execution, or unresolved product direction | High / Extra High; use Plan first when needed, then Goal only for persistence | Verify every risky boundary and the final observable outcome |

File count is not task size. A one-line edit in a huge repository may still be Fast. A one-line payment, permission, or data-deletion change may be Full.

### How ambiguity is handled

Unknowns are separated into three groups:

1. **Discoverable**: inspect code, files, runtime state, or authoritative documentation instead of asking.
2. **Safely defaultable**: choose the smallest conventional and reversible option; mention the assumption only when it affects the result.
3. **Decision-blocking**: ask when the answer changes product direction, acceptance, authority, or an irreversible outcome.

This prevents both silent product decisions and unnecessary intake questionnaires.

### When Plan mode is recommended

For a Full task with unresolved product, scope, or acceptance decisions, the skill recommends Plan mode before implementation and names the topics that need clarification.

In Plan mode it will:

- Ask at most three high-leverage questions per round.
- Prefer two or three mutually exclusive choices while still allowing a custom answer.
- Clarify the observable outcome and must-have functions, explicit exclusions and tradeoffs, then delivery and acceptance evidence.
- Discover repository and environment facts rather than asking the user.
- Ask another round only when an answer exposes a new decision blocker.

Before implementation, the answers are compressed into a stable contract: `Outcome`, `Must have`, `Must not`, `Target`, `Done`, and `Authority`.

### When Goal mode is recommended

Goal mode is useful when the objective is already stable but execution must persist across a long workflow or multiple continuations. It does not replace planning and cannot rescue an unclear objective.

Instead of merely saying “turn on Goal mode,” the skill proposes the exact objective:

```text
Deliver [observable outcome] in [target and scope],
preserve [hard constraints],
and consider it complete when [acceptance evidence] passes.
```

The objective describes one stable outcome—not a role, a phase list, or vague activity such as “continue improving the project.” A Goal is created only after an explicit user request, and a token budget is set only when the user explicitly asks for one.

### How it stays light

Before adding a file, dependency, abstraction, configuration option, framework, refactor, or extra feature, the skill asks:

> Would the current acceptance condition fail without this?

If not, it skips the addition. It prefers deletion, reuse, standard libraries, native platform features, and already-installed dependencies before new code.

Execution follows a small evidence loop:

1. Define the smallest observable completion condition.
2. Take the shortest safe path.
3. Run the smallest meaningful verification.
4. Stop immediately when it passes.
5. If it fails, widen the investigation by one dependency layer and repeat.

### Examples

#### A precise copy change

```text
Task: Change “Confirm” to “Save” on the settings page.
Route: Fast.
Action: Find the real string, change it, run the cheapest relevant check, and stop. No engineering plan.
```

#### A normal bug fix

```text
Task: Screenshot filenames occasionally collide.
Route: Focused.
Action: Trace the filename generator and all callers, fix the shared root cause, and leave one collision check.
```

#### A product built from scratch

```text
Task: Build a good Windows screenshot application.
Route: Full with unresolved product decisions.
Action: Recommend High reasoning and Plan mode; clarify capture modes, exclusions, delivery, and acceptance before implementation.
```

#### A long release workflow

```text
Task: Complete a cross-module migration, testing, packaging, and release.
Route: Full with a stable, long-running objective.
Action: Recommend High reasoning and Goal mode, then provide the exact outcome, scope, constraints, and acceptance evidence to use as the Goal objective.
```

### Installation

PowerShell:

```powershell
git clone https://github.com/diiiiiiylan/task-effort-router.git
Copy-Item -Recurse -Force .\task-effort-router\skill\task-effort-router "$HOME\.codex\skills\"
```

macOS / Linux:

```bash
git clone https://github.com/diiiiiiylan/task-effort-router.git
cp -R ./task-effort-router/skill/task-effort-router ~/.codex/skills/
```

### Activation

For one task:

```text
Use $task-effort-router for this task.
```

For durable global or project behavior, add this to `AGENTS.md`:

```md
Before non-trivial work, apply `$task-effort-router` and start at the lowest reliable effort level. Escalate planning, reasoning, context, and verification only when evidence requires it.
```

### Capabilities and limits

The skill can classify work and recommend a reasoning level, Plan mode, or Goal mode. It cannot switch the user's reasoning control or workflow mode by itself. When a Full task needs more depth, it explains exactly what to change and why. Fast and Focused tasks continue without mode advice.

### AI adaptation boundary

Unmodified installation and use are welcome. AI systems may execute this skill as intended, but please do not use an AI model, LLM, agent, or generative tool to modify, translate, paraphrase, restructure, clone, or create a derivative version, and do not remove or obscure its provenance notice.

To propose an improvement, open an Issue in the one canonical repository so the change can be discussed and implemented upstream:

https://github.com/diiiiiiylan/task-effort-router

This repository does not grant an open-source license. This section states the usage boundary the author asks users to respect.
