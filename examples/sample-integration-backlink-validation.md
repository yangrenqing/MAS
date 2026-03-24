# Sample Integration / Backlink Guidance Validation

## Goal
给第一次采用 starter 的团队一份“当前 integration / backlink guidance 是否已经足够支撑首轮真实 adoption”的验证样例。

## Meta
- Validation ID: `INTEGRATION-BACKLINK-VALIDATION-REAL-2026-001`
- Guidance Under Review:
  - `docs/integrations/github.md`
  - `docs/integrations/gitlab.md`
  - `docs/integrations/jira.md`
- Supporting References:
  - `examples/sample-external-system-mapping.md`
  - `examples/sample-real-adoption-validation.md`
  - `docs/starter-adoption-checklist.md`
- Example Project / Repo: `customer-portal-web`
- Requirement ID: `REQ-AUTH-2026-014`
- Primary Workflow Platform: `Jira`
- Primary Code Review Platform: `GitHub`
- Secondary Infra / Config Platform: `GitLab`
- Date: `2026-03-23`

## Summary
本示例验证 starter 当前的 integration / backlink guidance，是否已经足够支撑 adopted repo 第一次把 Jira、GitHub、GitLab 与 repo docs 串成同一条可追踪链路。结果为 `Reusable As-Is`：平台集成文档已经对齐 `Status Source of Truth`、`Primary Tracker ID` 和 backlink baseline；`examples/sample-external-system-mapping.md` 已提供具体编号映射；`examples/sample-real-adoption-validation.md` 已演示这条链路如何在真实项目风格 adoption 中落地。剩余要不要用 Jira 还是 GitHub 作为状态主来源、是否保留 GitLab companion change、以及 reviewer policy / security-style checks 的细则，属于 adopted repo 自身的人类决策，而不是 starter 集成文档缺口。

## Confirmed Facts
- `docs/integrations/github.md`、`docs/integrations/gitlab.md` 和 `docs/integrations/jira.md` 已显式对齐同一套 tracking baseline：先固定 `Status Source of Truth`，再保持稳定的 `Primary Tracker ID` 与 backlinks。
- `examples/sample-external-system-mapping.md` 已给出 Jira story、GitHub issue / PR、GitLab MR、repo docs、change ticket 之间的 canonical identifier set 与 stage-by-stage source-of-truth 映射。
- `examples/sample-real-adoption-validation.md` 已在真实项目风格 adoption 场景中演示完整 tracking chain，而不只是停留在平台说明层。
- `docs/starter-adoption-checklist.md` 已把平台工作流、人工审批点、高风险改动额外 review 等 adopted-repo 最小动作列成清单。

## Assumptions
- 本文件验证的是 starter 当前 integration / backlink guidance 是否足够清晰，不代表某个真实 adopted repo 已经完成所有外部系统接入。
- 首次 adoption 团队会把平台集成文档、mapping 示例和整体 adoption validation 一起阅读，而不是只读单个平台说明。

## Validation Scope
- 三份平台集成文档是否已经共享同一套 source-of-truth / tracker / backlink 语言
- starter 是否已经给出足够具体的跨系统编号映射与回链样例
- adopted repo 第一次接入时，是否还需要新增 starter 级 integration 模板或 validation 母版
- 当前 guidance 是否会错误暗示“所有系统都应同时承担状态主来源”
- 当前 guidance 是否已经足够暴露需要 adopted repo 自己决定的人类事项

## Guidance Chain Reviewed
| Layer | Artifact | Why it matters |
|---|---|---|
| Platform rule | `docs/integrations/jira.md` | 说明 Jira 作为流程主系统时，如何承接状态、摘要、Need Human Decision 和外部回链 |
| Platform rule | `docs/integrations/github.md` | 说明 GitHub issue / PR 如何承接代码协作、评审与 release prep backlinks |
| Platform rule | `docs/integrations/gitlab.md` | 说明 GitLab issue / MR / environment 记录在 adopted repo 中的边界 |
| Concrete mapping | `examples/sample-external-system-mapping.md` | 给出 canonical identifier set 与跨系统回填样例 |
| Adoption usage | `examples/sample-real-adoption-validation.md` | 证明这套映射已经能在真实项目风格 adoption 中落地 |
| Operational checklist | `docs/starter-adoption-checklist.md` | 约束 adopted repo 仍要保留人工审批、protected branch 和高风险额外 review |

## Validation Results
| Check | Result | Notes |
|---|---|---|
| 三个平台文档是否共享同一套 source-of-truth / primary-tracker / backlink baseline | pass | GitHub / GitLab / Jira 文档已不再各说各话 |
| 是否已经有一份足够具体的 canonical identifier mapping | pass | `sample-external-system-mapping.md` 已明确 story / issue / PR / MR / change ticket / repo doc 的编号关系 |
| 是否已经说明不同阶段应由哪个系统承担主状态入口 | pass | mapping 示例已给出按 stage 划分的 source-of-truth 建议 |
| 是否已经说明 repo docs 与外部系统各自承载什么 | pass | 集成文档已明确长文档留在 repo / wiki，外部系统承接摘要、状态、审批协作和 backlinks |
| adopted repo 首次接入时是否还需要新增 starter 级 integration 模板 | pass | 当前平台文档 + mapping 示例 + adoption validation 组合已经足够 |
| 当前 guidance 是否会鼓励多个系统同时做状态主来源 | pass | 三份平台文档和 mapping 示例都明确要求先固定一个主来源 |
| 当前 guidance 是否已把 remaining human decisions 暴露出来 | pass | Jira vs GitHub 主入口、GitLab companion change、reviewer policy 仍被保留为 adopted-repo 决策 |
| 当前 guidance 是否完全不依赖任何 companion reference 就能单独用一份文档讲清全部 adoption 细节 | partial | 首次 adoption 仍建议把平台文档与 mapping 示例、adoption validation 一起读 |

## What Worked Well
- 三份平台集成文档现在都围绕同一套 tracking vocabulary 组织，降低了 adopted repo 第一次接入时的术语漂移。
- `sample-external-system-mapping.md` 把“编号如何串起来”从抽象原则变成了可直接抄的样例，避免团队只知道要回链，却不知道具体字段怎么填。
- `sample-real-adoption-validation.md` 证明了这套 guidance 不只是理论说明，而是可以真正支撑文档落位、owner mapping、CI cutover 与 tracking chain 一起验证。

## Friction Points
| Area | What may still confuse a first-time team | Impact | Severity |
|---|---|---|---|
| companion references | 如果团队只读某一个平台文档，可能仍不清楚跨系统编号链路该如何整体落位 | 首次 adoption 执行效率下降 | medium |
| source-of-truth choice | starter 会要求先定主来源，但不会替团队决定 Jira / GitHub / GitLab 谁是主入口 | 仍需 adopted repo 先做人类约定 | medium |
| GitLab scope | 有些团队会误以为 GitLab MR 是默认必需对象，而不是“存在真实 infra/config companion change 时才出现” | 可能引入不必要复杂度 | low |
| reviewer policy depth | 集成 guidance 会提醒高风险改动要额外 review，但不会替 adopted repo 固化具体 approver 规则 | 仍需团队补足本地治理细节 | low |

## Gaps Found in the Starter
- Missing template / doc:
  - 未发现新的 starter 级 integration/backlink 模板缺口；当前更需要的是把现有平台文档、mapping 示例和 adoption validation 作为一组 reference 使用。
- Confusing step / unclear ownership:
  - 更需要持续强调“先固定状态主来源，再做跨系统同步”，而不是继续新增抽象说明文档。
- External system integration gap:
  - 剩余差异主要来自 adopted repo 自己是否同时使用 Jira、GitHub、GitLab，以及 GitLab 是否只承载 companion change，而不是 starter 缺少映射规则。
- Automation gap:
  - reviewer policy、security-style checks、release/change ticket 时机仍应由 adopted repo 自行决定，不应由 starter 自动替团队定死。

## Recommendation
- Result: Reusable As-Is
- Notes:
  - 当前 integration / backlink guidance 已足够支撑 adopted repo 的第一次真实 adoption 准备。
  - 后续优先级应转向 adopted-repo guide、reviewer-policy 和 security-style-check guidance 的真实反馈迭代，而不是继续新增 integration 母版。

## Need Human Decision
- adopted repo 最终要把 Jira、GitHub Issue、GitLab Issue 还是 repo doc 设为 `Status Source of Truth`？
- adopted repo 是否真的存在需要 GitLab 承载的 infra / config companion change，还是只保留 Jira + GitHub + repo docs 即可？
- 高风险 auth / billing / migration 相关改动的额外 reviewer policy，是否要在 adopted repo 中显式固化？

## Next Handoff
- To: Repo Maintainer / Knowledge-Ops Agent
- Goal: 将本验证结果沉淀到 continuity 文档，并把后续工作重点转移到 adopted-repo feedback 驱动的 reviewer-policy / security-style-check refinement
- Must Read:
  - Validation Results
  - Friction Points
  - Gaps Found in the Starter
  - Recommendation
