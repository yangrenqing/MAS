# GitLab Integration

## Goal
把这套 AI R&D starter 接到 GitLab，用 GitLab 承担需求流转、Merge Request 协作、环境发布记录和发布门禁。

## Recommended Scope
适合以 GitLab 为主平台的团队：
- Issues：需求、缺陷、任务
- Epics：跨需求主题
- Merge Requests：代码评审
- Milestones：发布窗口
- Environments / Deployments：staging、canary、production 记录
- Wiki 或仓库文档：设计、测试、复盘沉淀

## Minimal Object Mapping
| SOP Stage | GitLab Object | Recommended Owner |
|---|---|---|
| Intake | Issue | Intake Agent / 产品 |
| PRD | Issue description update + 文档链接 | PRD Agent |
| Technical Design | Wiki page 或 repo 文档 | Architect Agent |
| Planning | 子 issue / task list | Planning Agent / Dev Lead |
| Development | Merge Request | Developer Agent |
| QA / Security | MR discussion / pipeline summary | QA/Security Agent |
| Release Preparation | Milestone + release checklist 文档 | Release/SRE Agent |
| Release / Observe | Environment deployment record | Release Owner / Oncall |
| Postmortem | Issue / Wiki page | Knowledge/Ops Agent |

## Recommended Labels / Fields
- Type: `feature`, `bug`, `incident`
- Risk: `risk::low`, `risk::medium`, `risk::high`
- Stage: `stage::prd`, `stage::design`, `stage::qa`, `stage::release`
- Approval: `need-human-decision`
- Ops: `rollback-ready`, `canary-required`

如团队已启用 GitLab custom fields，可固定这些字段：
- Requirement ID
- Acceptance Criteria
- Risk Level
- Rollback Ref
- Release Window
- Owner

## Tracking and Backlink Baseline
- 每个需求先确定一个 `Status Source of Truth`；如果 GitLab issue 承担主入口，就把对应 issue 编号作为 `Primary Tracker ID`。
- Requirement ID 应在 issue、MR、release checklist 和外部系统记录中保持稳定。
- 长文档继续放在仓库或 wiki，GitLab 主要承载状态、审批协作和 backlinks，不与 repo 文档重复维护长状态说明。

## Agent Input / Output Landing
### Intake / PRD
- 将需求标准化后回写到 issue description。
- 长文档放仓库中，issue 中只保留摘要与链接。
- intake 阶段建议显式写出 `Status Source of Truth`、`Primary Tracker ID` 与外部 references。

### Architect / Planning
- 使用仓库文档或 wiki 保存设计与任务拆解。
- Planning 输出应包含 DoD、测试要求、发布要求。

### Developer
- MR 描述至少包含：
  - linked issue
  - change summary
  - self-check
  - tests added/updated
  - rollback notes

### QA / Security
- 在 MR discussion 或 pipeline summary 中记录：
  - critical path result
  - regression focus
  - security check result
  - release recommendation

### Release / SRE
- 使用 milestone / environment 页面记录：
  - rollout strategy
  - canary scope
  - metrics to watch
  - rollback trigger

## Automation Boundary
### Allowed automation
- 自动创建 / 更新 issue、MR 草稿、评论
- 自动补 labels、关联 milestone、附文档链接
- 自动汇总 pipeline 结果到 MR summary
- 自动生成 release checklist / staging checklist 草稿

### Must keep manual confirmation
- 合并到 protected branch
- 执行 production deploy job
- 修改 protected variables / secret
- 批准高风险 MR
- 执行 rollback
- 决定是否继续放量

## Recommended GitLab Guardrails
- 使用 protected branches
- 使用 approval rules，至少一名人工 reviewer
- production deploy job 设为 manual
- 高风险路径变更要求额外 approver
- 发布前必须有 rollback 文档链接

## Minimal Example
1. 需求进入 GitLab Issue。
2. Intake Agent 标准化描述，并补风险标签。
3. PRD/Architect Agent 生成文档并链接到 issue。
4. Planning Agent 把工作拆到 task list。
5. Developer Agent 提交 MR，附 change summary。
6. QA/Security Agent 根据 pipeline 与测试结果给出建议。
7. Release/SRE Agent 准备 milestone 与 release checklist。
8. 人类负责人手动执行 production job，并在观察窗口记录结论。

## Suggested First Integration
先做最小集成：
1. issue 模板
2. MR 模板
3. risk / stage labels
4. protected branch + approval rule
5. manual production job skeleton
