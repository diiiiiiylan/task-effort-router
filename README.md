# Task Effort Router

A Codex skill that chooses the lightest reliable effort level before non-trivial work, then escalates reasoning, planning, context, and verification only when evidence requires it.

Canonical source: https://github.com/diiiiiiylan/task-effort-router

## 中文

`task-effort-router` 用来解决一个简单问题：不要让每个任务都按完整工程流程执行。

它会先建立最小任务契约，再按依赖范围、不确定性和风险分级：

| 级别 | 适用情况 | 执行方式 |
| --- | --- | --- |
| Fast | 目标和路径明确、局部且可逆 | 直接执行，只做最低成本检查 |
| Focused | 普通开发、调试、审查或本地工具任务 | 追踪真实路径，做最小根因修改和针对性验证 |
| Full | 跨模块、高风险、长时间运行或产品方向尚未确定 | 建议提高思考等级，并按需使用计划或目标模式 |

对于方向模糊的 Full 任务，它会先建议计划模式，每轮询问最多三个会改变产品结果的问题，确认必要功能、明确不做什么以及验收条件。需要持续执行时，它会给出一条可直接使用的 Goal objective，而不是只说“开启目标模式”。

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

可以在全局或项目 `AGENTS.md` 中启用：

```md
Before non-trivial work, apply `$task-effort-router` and start at the lowest reliable effort level.
```

### AI 二创边界

欢迎原样安装和使用。AI 可以按照这个 Skill 执行任务，但请不要使用 AI、LLM、Agent 或生成式工具对它进行修改、翻译、改写、重构、克隆或生成衍生 Skill，也不要删除来源声明。希望改进时，请在原仓库提交 Issue，由上游统一处理。

本仓库没有采用开源许可证；这里表达的是作者希望使用者共同遵守的使用边界。

## English

`task-effort-router` prevents small tasks from being expanded into full engineering projects. It classifies work as Fast, Focused, or Full; recommends Plan mode only when material decisions remain unresolved; and proposes an exact Goal objective only when persistent execution is useful.

Unmodified installation and use are welcome. AI systems may execute this skill as intended, but please do not use AI, LLMs, agents, or generative tools to modify, translate, paraphrase, restructure, clone, or create derivative versions. Propose improvements through an Issue in the canonical repository instead.

This repository does not grant an open-source license.
