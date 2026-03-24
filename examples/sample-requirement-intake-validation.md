# Sample Requirement Intake Template Validation

## Goal
给第一次在真实项目风格仓库中使用 `docs/templates/requirement-intake.ai.md` 的团队一份“这张 intake 卡是否已经足够支撑后续 PRD handoff”的验证样例。

## Meta
- Validation ID: `INTAKE-VALIDATION-REAL-2026-001`
- Template Under Review: `docs/templates/requirement-intake.ai.md`
- Project / Repo: `customer-portal-web`
- Requirement ID: `REQ-AUTH-2026-014`
- Primary Tracker ID: `AUTH-142`
- Primary Workflow Platform: `Jira`
- Secondary Collaboration Platform: `GitHub`
- Date: `2026-03-23`

## Summary
本示例验证 requirement intake 模板在真实项目风格场景下，是否已经足够承接原始输入、固定状态主来源、保留外部回链、暴露关键风险，并把信息稳定交给 PRD 阶段。结果为 `Reusable As-Is`：模板主结构已经足够支撑第一次 adopted-repo 使用；当前模板已显式提醒先固定 `Status Source of Truth`，并在已知时记录 planned artifact path，剩余决策属于 adopted repo 自身的人类约定，不再是 starter 模板缺口。

## Confirmed Facts
- `docs/templates/requirement-intake.ai.md` 已包含 `Status Source of Truth`、`Primary Tracker ID` 和 `Related References` 字段，并显式提醒 first-time adopted repo 先固定状态主来源、在已知时记录 planned artifact path。
- 仓库中已有真实项目风格示例：`examples/sample-real-project-requirement-intake.md`。
- 当前 starter 还提供了 adopted-repo pilot、external mapping 和整体 adoption validation 样例，可与 intake 卡互相对照。

## Assumptions
- 本文件是“真实项目风格 intake 验证”样例，用于验证 starter 可落地性，不代表真实生产变更已经执行。
- 本样例沿用 Jira 作为需求状态主来源、GitHub Issue 作为代码协作入口，这只是 first-cut 演示，不代表所有 adopted repo 都必须这样选。

## Validation Scope
- intake 模板是否足够承接原始需求并归一化为标准卡片
- source-of-truth、primary tracker 和 backlinks 是否能稳定表达
- 风险、缺失信息和人工决策点是否在 intake 阶段被显式暴露
- PRD handoff 所需信息是否已经在 intake 阶段形成最小闭环
- adopted repo 第一次使用时，模板是否还需要额外配套说明

## Example Intake Card Used
- Artifact reviewed: `examples/sample-real-project-requirement-intake.md`
- Scenario used: `Expired Password Reset Link Shows 500 Error`
- Why this scenario is useful:
  - 属于 auth 高风险链路，能测试模板是否会过早丢失风险信息
  - 同时涉及 Jira、GitHub Issue 和 change ticket，能测试 tracking 字段是否足够
  - 是 bugfix 场景，比大 feature 更适合验证 intake 卡是否足够聚焦

## Validation Results
| Check | Result | Notes |
|---|---|---|
| 原始输入可被归一化为稳定 requirement card | pass | 问题、范围、风险和缺失信息都能在 intake 阶段落位 |
| `Status Source of Truth` 和 `Primary Tracker ID` 足够表达主状态入口 | pass | 能避免 Jira / GitHub 双写时主编号漂移 |
| `Related References` 足够承接外部回链 | pass | GitHub issue、change ticket、planned artifact path 都能在 intake 阶段固定 |
| 风险与缺失信息在 PRD 前已显式暴露 | pass | auth 风险、失败页文案、hotfix 决策都在 intake 阶段被拉出来 |
| `Next Handoff` 足够指导 PRD 阶段 | pass | `Must Read` 聚焦 tracking、scope、risk、missing info，handoff 较清晰 |
| adopted repo 第一次使用时是否还需要额外补模板字段 | pass | planned artifact path 和 source-of-truth 提醒已进模板，剩余目录约定可由 adoption guide 统一 |
| adopted repo 第一次使用时是否天然解决状态主来源争议 | pass | 模板已明确要求先固定主来源；最终选 Jira / GitHub / GitLab 仍是 adopted repo 的人类决策 |

## What Worked Well
- `Tracking and References` 区块让 intake 卡第一次真正具备跨系统可追踪性，而不只是写一段需求摘要。
- `Risks Seen At Intake`、`Missing Information` 和 `Need Human Decision` 让高风险 auth bug 不会在过早阶段被“当成纯实现问题”。
- `Next Handoff` 中的 `Must Read` 已足够把 PRD 阶段的注意力聚焦到最关键的信息，而不是让下游重新通读整张卡。

## Friction Points
| Area | What slowed us down | Impact | Severity |
|---|---|---|---|
| status ownership | 模板提供了 source-of-truth 字段，但团队仍需要先决定 Jira 或 GitHub 谁是主入口 | 若不先定主入口，状态仍会双写漂移 | medium |
| artifact placement | intake 卡可写 planned artifact path，但第一次 adoption 时团队不一定先知道文档应该放在哪 | 容易出现路径写法不一致 | medium |
| scope wording | bugfix 与小 feature 的 scope 粒度差异较大，第一次填写时容易把“修复范围”写成“设计方案” | 可能让 PRD 阶段重新收敛范围 | low |
| change-ticket timing | intake 阶段是否立刻写入 change ticket 取决于组织流程，不是每个团队都会在最前面就有编号 | `Related References` 有时会出现 TBD，需要接受这一点 | low |

## Gaps Found in the Starter
- Missing template / doc:
  - 模板本身够用，但第一次 adoption 时仍需要结合 `docs/adopted-repo-guide.md` 才更容易把 planned artifact path 写一致。
- Confusing step / unclear ownership:
  - `Status Source of Truth` 字段已经存在，但 starter 仍需要持续强调“先定主入口，再开始同步状态”。
- External system integration gap:
  - 当团队同时使用 Jira / GitHub / GitLab 时，intake 卡能承接回链，但仍要靠 mapping 示例帮助团队理解不同系统各自承载什么。
- Automation gap:
  - intake 模板只能固定信息结构，不能自动替团队决定 reviewer policy、change ticket 时机或 docs 目录规则。

## Recommendation
- Result: Reusable As-Is
- Notes:
  - `docs/templates/requirement-intake.ai.md` 已足够支撑 adopted repo 的第一次真实项目风格 intake。
  - 剩余需要团队自行固定的目录落位、状态主来源和跨系统同步口径，属于 adopted repo 的人类约定，不再是 starter 模板缺口。

## Need Human Decision
- adopted repo 是否要求每张 intake 卡都必须先固定 `Status Source of Truth`，否则不得进入 PRD？
- adopted repo 是否要统一规定 intake 阶段就写 `Planned Artifact Path`，还是允许在 PRD 阶段再补？

## Next Handoff
- To: Repo Maintainer / Knowledge-Ops Agent
- Goal: 根据这份验证结果决定是否还要微调 requirement-intake 模板，或只需加强 adoption guidance 与 mapping 说明
- Must Read:
  - Validation Results
  - Friction Points
  - Gaps Found in the Starter
  - Recommendation
