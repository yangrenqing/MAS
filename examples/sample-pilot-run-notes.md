# Sample Pilot Run Notes

## Meta
- Pilot ID: PILOT-2026-001
- Project / Repo: ai-rd-team starter
- Requirement ID: REQ-2026-001
- Owner: Knowledge/Ops Agent
- Date: 2026-03-23
- Scope: 用 `Existing User Magic Link Login` 样例试跑 starter 的需求→PRD→设计→测试计划→发布准备链路

## Summary
本次试跑使用现有 magic link 样例串联 starter 中的核心模板与 SOP，验证这套仓库是否已经具备“可直接演练一次完整流程”的最小闭环。整体结果为“Needs Small Fixes”：主链路文档已经能顺畅衔接，但在首轮试跑记录示例、非 feature 类型样例，以及 repo 采用后的 CI 命令落地上仍需要补充。

## Confirmed Facts
- 已使用 `sample-requirement.md`、`sample-prd.md`、`sample-design.md`、`sample-test-plan.md`、`sample-release-checklist.md` 串起主流程。
- `README.md`、`docs/quality-gates.md` 和 `docs/sop/requirement-to-release.md` 可以作为试跑入口与阶段校验参考。
- `.claude/settings.json` 中的 hooks 以提醒和 ask 为主，没有引入自动生产动作。

## Assumptions
- 本次为 starter 仓库内的模拟试跑，不代表真实业务仓库已落地。
- GitHub / GitLab / Jira / 通知系统集成在本次试跑中未做真实联通。

## Pilot Goal
- Primary goal: 验证 starter 是否具备可复用的最小文档闭环，并找出首轮采用前最值得补的缺口。
- Success signal:
  - 一个中等复杂度需求可顺着模板走到 release prep
  - 试跑后能明确列出需要回填到 README、模板和治理文件的事项

## Requirement and Context
- Requirement summary:
  - 为现有注册用户增加邮箱 magic link 登录能力，作为密码登录的补充入口，并保留 feature flag、canary、rollback 等人工决策点。
- Workflow used:
  - Intake / Requirement Card → PRD
  - PRD → Technical Design → Test Plan
  - Test Plan → Release Checklist → Pilot Review
- Artifacts referenced:
  - `examples/sample-prd.md`
  - `examples/sample-release-checklist.md`

## What Worked Well
- 模板之间的 `Need Human Decision` 与 `Next Handoff` 结构一致，下游承接清晰。
- 质量门禁与 release checklist 能较自然地约束 feature flag、canary、rollback 等高风险动作。
- Claude Code settings 的提醒型 hooks 与 starter 的“人类保留最终决策”原则一致。

## Friction Points
| Area | What slowed us down | Impact | Severity |
|---|---|---|---|
| examples | 之前缺少首轮试跑记录示例，使用者不容易知道 pilot-run-notes 应该如何回填 | 首次 adoption 时需要额外解释 | medium |
| sample coverage | 之前样例主要偏 feature 流程，缺少 bugfix 类型入口参考 | 会让团队误以为 starter 只适合新功能需求 | medium |
| CI adoption | CI 仍是 placeholder，进入真实仓库时必须再手动替换命令 | starter 可读但还不能直接代表项目质量命令 | medium |

## Gaps Found in the Starter
- Missing template / doc:
  - 缺少具体的 pilot-run-notes 示例和 bugfix 样例入口（本轮已补齐）
- Confusing step / unclear ownership:
  - 从“样例演练”到“真实项目首次采用”的切换步骤需要继续依赖 adoption checklist
- External system integration gap:
  - 缺少一次带真实 issue / PR / MR 编号的映射示例
- Automation gap:
  - CI 尚未替换成真实 lint / typecheck / test / security 命令

## Quality Gate Review
- Gate reached: Release Preparation Quality
- Gates that felt clear:
  - Requirement Quality
  - Design Quality
  - Development Quality
  - QA / Security Quality
  - Release Preparation Quality
- Gates that felt unclear or heavy:
  - Staging / Canary gate 在没有真实项目指标和告警阈值前只能作为模板演练
- Did any human decision point get skipped? no
- Notes:
  - 当前 gates 对 starter 仓库是合适的，但在真实仓库中仍需要结合平台、监控和组织职责细化。

## Follow-up Actions
- Template update needed:
  - 后续可继续补 incident 或 backend migration 类型样例
- README / handoff update needed:
  - 将新增的 bugfix 样例和 pilot 示例加入入口文档与状态文档
- Integration update needed:
  - 第一次真实 pilot 后补一份 GitHub / GitLab / Jira 编号映射示例
- Hook / guardrail update needed:
  - 暂无必须修改项；等真实项目采用后再决定是否收紧高风险路径规则

## Recommendation
- Result: Needs Small Fixes
- Notes:
  - 这套 starter 已经适合做内部演练和模板母版
  - 在真实仓库采用前，仍应先完成 repo-specific CI 命令替换并记录一次真实 pilot

## Need Human Decision
- 首次真实 pilot 选择哪个中等复杂度需求最合适？
- starter 被采用到真实仓库时，CI 最少要接入哪些质量命令？

## Next Handoff
- To: Knowledge/Ops Agent / Repo Maintainer
- Goal: 把试跑结论回填到模板、文档和治理配置
- Must Read:
  - Friction Points
  - Gaps Found in the Starter
  - Follow-up Actions
  - Recommendation
