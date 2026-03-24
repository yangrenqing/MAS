# Sample Real-Project Pilot Run Notes

## Meta
- Pilot ID: PILOT-REAL-2026-001
- Project / Repo: customer-portal-web
- Requirement ID: REQ-AUTH-2026-014
- Owner: Knowledge/Ops Agent
- Date: 2026-03-23
- Scope: 在一个已采用 starter 的真实项目风格仓库中，用 `Expired Password Reset Link Shows 500 Error` bugfix 需求试跑 requirement → PRD → design → test plan → release prep 链路，并把 repo-specific CI、协作系统链接和人工审批点一起走通

## Summary
本示例展示 starter 被采用到一个真实项目风格仓库后，第一次 pilot run notes 应该如何记录。整体结果为“Needs Small Fixes”：主链路已经能在采用仓库中落地，repo-specific 质量命令与人工审批点也能自然接入，但 labels / 状态同步、发布 owner 映射和高风险路径定制仍需要在首个真实观察周期后继续收敛。

## Confirmed Facts
- 该采用仓库已把 `docs/templates/requirement-intake.ai.md`、`prd.ai.md`、`design.ai.md`、`test-plan.ai.md`、`release-checklist.ai.md` 复制到项目内文档目录。
- 该项目已将 placeholder CI 替换为 repo-specific 检查命令：`pnpm lint`、`pnpm typecheck`、`pnpm test -- password-reset`。
- 发布相关动作仍保留人工批准，未把 canary、放量或 rollback 交给自动化直接执行。

## Assumptions
- 本文件是“真实项目首轮采用”示例，仓库名、编号和命令用于演示 starter 在 adopted repo 中的落地方式。
- 本次 pilot 只走到 release prep，不代表已经执行真实 production deploy。

## Pilot Goal
- Primary goal: 验证 starter 在 adopted repo 中是否仍能保持 handoff 清晰、质量门禁可判断、人工决策点不丢失。
- Success signal:
  - 需求可以从 intake 一路走到 release prep，且每一步都能直接交给下游角色
  - repo-specific CI、风险路径和外部协作链接都能自然接入，而不是停留在 placeholder

## Requirement and Context
- Requirement summary:
  - 修复密码重置流程中“过期或已使用 reset link 偶发返回 500 错误”的问题，确保失败路径可恢复、可解释且不泄露 token 状态。
- Workflow used:
  - Requirement Intake Card → PRD
  - PRD → Technical Design → Test Plan
  - Test Plan → Release Checklist → Pilot Run Notes
- Artifacts referenced:
  - `docs/ai/requirements/REQ-AUTH-2026-014.md`
  - `docs/ai/release/REL-AUTH-2026-014.md`

## What Worked Well
- requirement intake 模板让 issue 描述到标准需求卡的收敛明显更快，减少了第一次采用时的格式犹豫。
- adopted repo 中一旦填入真实 `lint / typecheck / test` 命令，测试计划和 release gate 的阻断条件会变得更具体。
- 高风险 auth 路径、rollback、Need Human Decision 在 adopted repo 中依旧表达自然，没有因为接入真实项目而被弱化。

## Friction Points
| Area | What slowed us down | Impact | Severity |
|---|---|---|---|
| label mapping | GitHub labels、Jira 状态和内部 Requirement ID 的命名没有一次性约定好 | 需要额外人工对齐状态口径 | medium |
| release ownership | Release owner、值班 owner、产品批准人最初分散在不同文档中 | release prep 结论可读，但审批路径不够一眼清楚 | medium |
| risk-path tuning | starter 默认高风险路径规则覆盖了 auth，但没有覆盖该项目自定义的 `identity/` 和 `session/` 路径 | hooks 仍有价值，但需要项目化补强 | medium |

## Gaps Found in the Starter
- Missing template / doc:
  - starter 现在已有 requirement intake 模板与 pilot 样例，但 adopted repo 仍需要一份“项目内文档目录约定”说明
- Confusing step / unclear ownership:
  - Release owner、approver、oncall 的最小角色映射建议仍可再明确
- External system integration gap:
  - 如果团队同时使用 Jira + GitHub + GitLab，仍需要固定 backlink 规则与 source-of-truth 顺序
- Automation gap:
  - hooks 仍只提供提醒；真正的 repo-specific CI 与 release rule 需要采用仓库自行接入

## Quality Gate Review
- Gate reached: Release Preparation Quality (adopted repo pilot)
- Gates that felt clear:
  - Requirement Quality
  - Design Quality
  - QA / Security Quality
  - Release Preparation Quality
- Gates that felt unclear or heavy:
  - Staging / Canary gate 在没有真实观察窗口时仍偏模板化，需要项目团队补阈值
- Did any human decision point get skipped? no
- Notes:
  - 与 starter 仓库内的内部 pilot 相比，真实项目风格采用后最大的价值来自 repo-specific CI、真实风险路径和真实 owner 映射。

## Follow-up Actions
- Template update needed:
  - 可以考虑补一个 adopted repo 文档目录约定示例，说明 requirement / PRD / design / release artifact 放在哪
- README / handoff update needed:
  - 在 starter README 中加入“真实项目风格 pilot 示例”入口，帮助第一次 adoption 快速对照
- Integration update needed:
  - 补一份 issue / PR / MR / Jira 编号映射示例，固定 backlink 与字段同步规则
- Hook / guardrail update needed:
  - 在 adopted repo 中把默认高风险路径从 `auth/billing/orders/migrations` 扩展到项目自定义关键目录

## Recommendation
- Result: Needs Small Fixes
- Notes:
  - starter 已经足够支撑 adopted repo 的首次 pilot，不再只是文档母版
  - 真正进入长期使用前，还应补齐项目内 owner 映射、状态同步和高风险路径定制

## Need Human Decision
- adopted repo 是否要求所有高风险 auth 变更都必须带额外 reviewer，而不仅是 release 前复核？
- adopted repo 是否要统一规定 Jira 还是 GitHub Issue 为需求状态主来源？

## Next Handoff
- To: Knowledge/Ops Agent / Repo Maintainer
- Goal: 把 adopted repo pilot 里的有效做法回填到 starter 的示例、integration guide 和 adoption checklist
- Must Read:
  - Friction Points
  - Gaps Found in the Starter
  - Follow-up Actions
  - Recommendation
