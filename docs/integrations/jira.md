# Jira Integration

## Goal
把这套 AI R&D starter 接到 Jira，用 Jira 承担需求流转、状态管理、责任分配和发布追踪；代码与长文档仍建议落在代码仓库里。

## Recommended Scope
Jira 更适合作为流程系统，而不是长文档主存储：
- Epic：业务主题 / 大需求
- Story / Task：标准需求与交付项
- Bug：缺陷
- Sub-task：开发、测试、发布准备分工
- Comments / Links / Attachments：链接 AI 产物与仓库文档

## Minimal Field Mapping
建议固定这些字段：
- Summary
- Description
- Acceptance Criteria
- Scope / Out of Scope
- Risk Level
- Owner
- Release Target
- Rollback Reference
- Need Human Decision
- Linked Repo / PR / MR

## Recommended Workflow Mapping
| SOP Stage | Jira Status | Main Artifact |
|---|---|---|
| Intake | Intake | 标准需求卡 |
| PRD | PRD Ready | `prd.ai.md` |
| Technical Design | Design Ready | `design.ai.md` |
| Planning | Planned | 任务拆解 / DoD |
| Development | In Dev | PR / MR |
| QA / Security | In QA | `test-plan.ai.md` + 测试结果 |
| Release Preparation | Release Ready | `release-checklist.ai.md` |
| Release / Observe | Released / Observing | staging/canary 记录 |
| Postmortem | Closed / Learning Captured | `postmortem.ai.md` |

## Tracking and Backlink Baseline
- 如果 Jira 是流程主系统，就把 Jira issue key 作为 `Primary Tracker ID`，并明确 Jira 是 `Status Source of Truth`。
- Requirement ID 应在 repo 文档、GitHub PR / Issue、GitLab MR、change ticket 中保持稳定，不要每个系统各起一套主编号。
- 长文档继续放在仓库或知识库；Jira 重点保存摘要、状态、`Need Human Decision` 和对外部产物的 backlinks。

## Agent Input / Output Landing
### Intake Agent
- 从 Jira issue 描述、评论、附件中提炼需求。
- 产出标准需求卡并回写 issue 描述或评论。

### PRD / Architect / Planning Agents
- 建议把结构化文档保存在代码仓库或知识库中。
- Jira 中保留：
  - 摘要
  - 文档链接
  - 状态
  - 需要人决策的事项

### Developer Agent
- 在 Jira 中更新：
  - linked branch / PR / MR
  - self-check 摘要
  - 风险等级变化
- 不建议把完整技术细节全部塞进 Jira 描述。

### QA / Security Agent
- 更新测试结论、blocker、风险变化。
- 若未过 gate，应明确阻塞原因和解除阻塞责任人。

### Release / SRE Agent
- 在 Jira release ticket 或 change ticket 中记录：
  - rollout strategy
  - metrics to watch
  - alert thresholds
  - rollback plan

## Automation Boundary
### Allowed automation
- 自动创建 story / sub-task 草稿
- 自动补充 acceptance criteria、risk 字段、文档链接
- 自动把 PR / MR 链接同步回 Jira
- 自动生成测试/发布准备摘要评论

### Must keep manual confirmation
- 状态改到可生产发布的最终批准
- 生产变更审批
- 事故等级确认
- 生产回滚决策
- 对外承诺发布日期或客户通知

## Recommended Jira Guardrails
- 高风险需求必须有 Risk Level 字段
- Release ticket 必须有 rollback reference
- 进入 QA 前必须有 acceptance criteria
- 进入 Release Ready 前必须附 change summary 和 test summary
- 生产相关状态流转保留人工审批节点

## Minimal Example
1. 产品在 Jira 创建 Story。
2. Intake Agent 规范化描述并补缺失信息。
3. PRD Agent 生成 PRD 文档，Jira comment 中回填链接。
4. Architect Agent 输出设计文档并链接。
5. Planning Agent 创建开发/测试/发布准备 sub-task。
6. Developer Agent 在 Jira 绑定 PR。
7. QA/Security Agent 更新 QA 结果与风险状态。
8. Release/SRE Agent 创建 release/change ticket，并准备 rollout/rollback 信息。
9. 人类负责人批准后再进入生产执行。

## Suggested First Integration
先做最小集成：
1. 一个标准 issue type scheme
2. 一套风险/发布相关字段
3. 一个从 Intake 到 Release Ready 的 workflow
4. 仓库链接规范
5. release/change ticket 模板
