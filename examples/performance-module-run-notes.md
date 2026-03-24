# Pilot Run Notes

## Meta
- Pilot ID: PILOT-PERF-2026-001
- Project / Repo: ai-rd-team starter
- Requirement ID: REQ-PERF-2026-001
- Owner: Knowledge/Ops Agent
- Date: 2026-03-24
- Scope: 用 Moka 风格绩效模块样例试跑 starter 在 Java 微服务 / 内部业务系统场景下的需求→PRD→设计→测试计划→发布准备链路

## Summary
本次试跑围绕一个更接近真实企业内部系统的 Moka 风格绩效模块样例，补齐了 requirement intake、PRD、technical design、test plan、release checklist 与 run notes 全链路。整体结果为 `Reusable As-Is`：现有 starter 模板已经能稳定承载该类项目的 handoff、风险表达和人工决策点；本轮新增样例的主要价值在于拓宽参考覆盖面，而不是暴露新的结构性缺口。

## Confirmed Facts
- 已新增 `examples/performance-module-requirement-intake.md`、`examples/performance-module-prd.md`、`examples/performance-module-design.md`、`examples/performance-module-test-plan.md`、`examples/performance-module-release-checklist.md`、`examples/performance-module-run-notes.md`。
- 新样例覆盖了 JDK 17、Spring Boot、Spring Cloud、Nacos、MySQL 语境下的真实项目风格业务链路。
- 全链路继续保留 `Assumptions`、`Need Human Decision` 与 `Next Handoff`，且未引入自动生产动作。

## Assumptions
- 本次仍为 starter 仓库中的文档级演练，而不是一个已实现代码的 adopted repo。
- 组织服务、网关、Nacos 和 MySQL 的引用主要用于体现真实边界，而非验证具体中间件接入细节。

## Pilot Goal
- Primary goal: 验证 starter 是否已经足够支持一个 Java 微服务 + 内部业务系统风格的真实项目样例链路。
- Success signal:
  - 绩效模块场景下，每个阶段都能自然 handoff 到下游角色
  - 不需要修改模板结构，也能清晰表达权限、状态流转、敏感数据和 internal pilot guardrails

## Requirement and Context
- Requirement summary:
  - 为学习和 adopted-repo 演练构建一套 Moka 风格绩效模块样例，覆盖周期、模板、绩效计划、自评和主管评分最小闭环。
- Workflow used:
  - Requirement Intake → PRD
  - PRD → Technical Design → Test Plan
  - Test Plan → Release Checklist → Pilot Review
- Artifacts referenced:
  - `docs/templates/requirement-intake.ai.md`
  - `docs/templates/prd.ai.md`
  - `docs/templates/design.ai.md`
  - `docs/templates/test-plan.ai.md`
  - `docs/templates/release-checklist.ai.md`

## What Worked Well
- 现有模板对“微服务边界 + 敏感数据 + 多角色状态流转”的表达足够自然，无需额外模板拆分。
- `Need Human Decision` 和 `Next Handoff` 在绩效模块这种业务规则较多的场景下依然清晰，说明 starter 的 handoff 结构具有通用性。
- release checklist 能自然约束 internal pilot 白名单、人工审批、观察窗口与 rollback，而不会暗示直接生产放量。

## Friction Points
| Area | What slowed us down | Impact | Severity |
|---|---|---|---|
| example discovery | README、project-status、session-handoff 需要同步更新，否则新样例不容易被后续会话发现 | 会降低新增样例的可见性 | medium |
| scope control | 绩效模块很容易膨胀到薪酬、校准、360 等复杂子域，需要持续压缩到最小闭环 | 若不收敛范围，会削弱 starter 样例的可复用性 | medium |

## Gaps Found in the Starter
- Missing template / doc:
  - none
- Confusing step / unclear ownership:
  - none
- External system integration gap:
  - none
- Automation gap:
  - none beyond existing adopted-repo-specific CI replacement expectation

## Quality Gate Review
- Gate reached: Release Preparation Quality
- Gates that felt clear:
  - Requirement Quality
  - Design Quality
  - QA / Security Quality
  - Release Preparation Quality
- Gates that felt unclear or heavy:
  - none for starter-level文档演练；真实 adopted repo 仍需自行补平台指标和实际命令
- Did any human decision point get skipped? no
- Notes:
  - 样例中所有高风险点都仍以人工审批、白名单和回滚触发条件约束，没有削弱既有 guardrails。

## Follow-up Actions
- Template update needed:
  - none
- README / handoff update needed:
  - 将新的 performance-module 样例链路加入 README、project-status 与 session-handoff
- Integration update needed:
  - none
- Hook / guardrail update needed:
  - none

## Recommendation
- Result: Reusable As-Is
- Notes:
  - starter 现已同时覆盖通用 feature、bugfix、migration 与 Java 微服务内部业务系统风格样例
  - 下一步更有价值的工作仍然是等待真实 adopted repo 反馈，而不是继续扩模板结构

## Need Human Decision
- 后续若要继续扩展示例，优先补 incident / ops 场景，还是直接等真实 adopted repo 反馈再补？
- 这套绩效模块样例未来是否需要再补一份 external system mapping 示例，还是保持当前抽象层级即可？

## Next Handoff
- To: Knowledge/Ops Agent / Repo Maintainer
- Goal: 把试跑结论回填到模板、文档和治理配置
- Must Read:
  - Friction Points
  - Gaps Found in the Starter
  - Follow-up Actions
  - Recommendation
