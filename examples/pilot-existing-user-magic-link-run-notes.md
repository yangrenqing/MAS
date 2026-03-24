# Pilot Run Notes

## Meta
- Pilot ID: PILOT-2026-002
- Project / Repo: ai-rd-team starter
- Requirement ID: REQ-PILOT-2026-001
- Owner: Knowledge/Ops Agent
- Date: 2026-03-23
- Scope: 用 `Existing User Magic Link Login` 场景真实走一遍 starter 的 requirement → PRD → design → test plan → release prep 链路

## Summary
本次试跑没有复用现成 sample 文件作为“最终答案”，而是基于现有模板和样例重新产出一整套 pilot 文档链路，用来验证 starter 是否已经可以支持一次真实的内部演练。整体结果为“Needs Small Fixes”：主链路已经顺畅，质量门禁与人工决策点也能自然落位，但首轮采用时仍暴露出 requirement 起点模板、真实集成映射和 repo-specific CI 命令三个缺口。

## Confirmed Facts
- 已新建 requirement、PRD、design、test plan、release checklist、pilot run notes 六个 pilot 文档。
- 本次试跑完整走到 Release Preparation Quality，但没有执行真实 deploy。
- 所有文档都保留了 `Assumptions`、`Need Human Decision` 和 `Next Handoff` 结构。

## Assumptions
- 本次为 starter 仓库内的文档试跑，不代表真实业务仓库已经完成接入。
- GitHub / GitLab / Jira / 通知系统集成在本次试跑中未做真实联通。

## Pilot Goal
- Primary goal: 验证 starter 是否已经具备可复用的最小文档闭环，并记录首次采用前仍需补强的点。
- Success signal:
  - 一个中等复杂度需求可以自然走到 release prep
  - 试跑后可以明确列出需要回填到状态文档和模板的事项

## Requirement and Context
- Requirement summary:
  - 为现有注册用户增加邮箱 magic link 登录能力，作为密码登录的补充入口，并保留 feature flag、canary、rollback 等人工决策点。
- Workflow used:
  - Requirement Card → PRD
  - PRD → Technical Design → Test Plan
  - Test Plan → Release Checklist → Pilot Run Notes
- Artifacts referenced:
  - `docs/templates/prd.ai.md`
  - `docs/templates/design.ai.md`
  - `docs/templates/test-plan.ai.md`
  - `docs/templates/release-checklist.ai.md`
  - `docs/templates/pilot-run-notes.ai.md`

## What Worked Well
- 模板之间的 `Need Human Decision` 与 `Next Handoff` 结构一致，下游承接清晰。
- `docs/quality-gates.md` 能自然约束 release prep 之前必须补齐的关键治理信息。
- release checklist 能清楚表达 feature flag、canary、metrics、alerts 和 rollback，而不诱导真实生产动作。

## Friction Points
| Area | What slowed us down | Impact | Severity |
|---|---|---|---|
| requirement start | starter 目前没有独立的 requirement intake 模板，只能从 sample requirement 倒推结构 | 首次从真实 issue 开始时需要额外整理格式 | medium |
| release realism | starter 内部 pilot 无法验证真实仪表盘、告警和 CI 命令 | release prep 只能验证文档完备性，不能替代真实项目演练 | medium |
| external mapping | 当前没有带真实 issue / PR / MR / ticket 编号的完整映射示例 | adoption 到真实协作平台时仍需二次解释 | medium |

## Gaps Found in the Starter
- Missing template / doc:
  - 可考虑补一个 requirement intake 模板，或在 adoption 文档中明确“如何从原始 issue 归一化成 requirement card”
- Confusing step / unclear ownership:
  - 最小 pilot 主链路与可选 artifacts（change summary / staging checklist / canary checklist）的边界还可以再写清楚
- External system integration gap:
  - 缺少一次带真实 GitHub / GitLab / Jira 编号映射的演示样例
- Automation gap:
  - CI 仍是 placeholder，进入真实仓库时必须替换成 repo-specific lint / typecheck / test / security 命令

## Quality Gate Review
- Gate reached: Release Preparation Quality (documentation pilot only)
- Gates that felt clear:
  - Requirement Quality
  - Design Quality
  - QA / Security Quality
  - Release Preparation Quality
- Gates that felt unclear or heavy:
  - Staging / Canary gate 在没有真实指标、告警和环境的 starter 仓库里只能做演练
- Did any human decision point get skipped? no
- Notes:
  - 当前 starter 的治理设计是成立的，但真实项目采用后仍需补上平台化数据和组织角色映射

## Follow-up Actions
- Template update needed:
  - 评估是否新增 requirement intake 模板，或至少在 adoption/checklist 中补明确入口说明
- README / handoff update needed:
  - 记录 starter 内部 pilot 已完成，并把下一步切换为“真实项目 pilot”
- Integration update needed:
  - 第一次真实项目试跑后补一份 issue / PR / MR / Jira 编号映射示例
- Hook / guardrail update needed:
  - 暂无必须修改项；等真实仓库采用后再决定是否收紧高风险路径规则

## Recommendation
- Result: Needs Small Fixes
- Notes:
  - 这套 starter 已经可以支撑一次完整的内部文档试跑
  - 在真实仓库采用前，仍应先完成 repo-specific CI 命令替换，并记录一次真实项目 pilot

## Need Human Decision
- 下一次真实项目 pilot 应优先选 feature、bugfix 还是 migration 场景？
- 是否要把 requirement intake 模板补进 starter，而不是继续仅靠 sample requirement 起步？

## Next Handoff
- To: Knowledge/Ops Agent / Repo Maintainer
- Goal: 把试跑结论回填到状态文档、入口文档和后续 adoption 指引
- Must Read:
  - Friction Points
  - Gaps Found in the Starter
  - Follow-up Actions
  - Recommendation
