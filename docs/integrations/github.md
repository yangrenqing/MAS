# GitHub Integration

## Goal
把这套 AI R&D starter 接到 GitHub，用 GitHub 承担需求追踪、代码协作、评审、发布记录和事故后复盘的落点。

## Recommended Scope
适合把 GitHub 作为主协作系统的团队：
- GitHub Issues：需求、缺陷、任务
- Pull Requests：代码评审与变更说明
- Projects：需求到发布的状态看板
- Releases：版本说明与发布记录
- Discussions 或 issue：复盘沉淀

## Minimal Object Mapping
| SOP Stage | GitHub Object | Recommended Owner |
|---|---|---|
| Intake | Issue | Intake Agent / 产品 |
| PRD | Issue comment + `docs/templates/prd.ai.md` 产物链接 | PRD Agent |
| Technical Design | PR 或设计文档链接 | Architect Agent |
| Planning | 子 issue / task list | Planning Agent / Dev Lead |
| Development | Pull Request | Developer Agent |
| QA / Security | PR review / check result comment | QA/Security Agent |
| Release Preparation | Release checklist 文档 + draft release | Release/SRE Agent |
| Release / Observe | Release note + deployment record issue comment | Release Owner / Oncall |
| Postmortem | Discussion 或 issue | Knowledge/Ops Agent |

## Recommended Labels
- Type: `feature`, `bug`, `incident`, `tech-debt`
- Risk: `risk:low`, `risk:medium`, `risk:high`
- Stage: `stage:intake`, `stage:prd`, `stage:design`, `stage:dev`, `stage:qa`, `stage:release`
- Decision: `need-human-decision`
- Status: `blocked`, `ready-for-qa`, `ready-for-release`

## Cross-System Tracking Baseline
- 每个需求先确定一个 `Status Source of Truth`：GitHub Issue、Jira、GitLab Issue 或 repo 文档，避免多个系统同时承担状态主入口。
- 保留一个稳定的 `Primary Tracker ID`，并在 issue、PR、release prep 产物中持续回填。
- 把 requirement doc、issue、PR、release/change 记录之间的 backlinks 固定下来，避免信息散落。

## Agent Input / Output Landing
### Intake Agent
- Input:
  - issue 标题
  - issue 描述
  - 客户/业务补充评论
- Output:
  - 标准化需求卡
  - 缺失信息清单
  - 风险标签建议
  - `Status Source of Truth`、`Primary Tracker ID` 与相关 backlinks 建议

### PRD / Architect / Planning Agents
- 推荐把结构化产物保存在仓库文档中，再把链接回填到 issue / PR：
  - `docs/templates/prd.ai.md`
  - `docs/templates/design.ai.md`
  - `docs/templates/change-summary.ai.md`
- PR / issue comment 中只保留摘要，避免信息散落。

### Developer Agent
- 输出到 PR：
  - 变更摘要
  - 自测结论
  - 测试补充说明
  - 影响范围
- PR 描述中必须包含：
  - 对应需求 issue
  - 风险等级
  - 回滚说明

### QA / Security Agent
- 输出到 PR comment 或 check summary：
  - 测试结果
  - 未覆盖风险
  - 是否建议进入 release prep

### Release / SRE Agent
- 输出到 release draft / release issue：
  - rollout strategy
  - metrics to watch
  - alert thresholds
  - rollback plan

## Automation Boundary
### Allowed automation
- 创建或更新 issue / PR 的草稿内容
- 自动补全标签、checklist、链接关系
- 将 AI 文档路径回填到 issue / PR
- 自动汇总 change summary、test summary、release summary
- 在 PR 打开时提醒补充 `Need Human Decision`

### Must keep manual confirmation
- 合并到受保护分支
- 修改生产环境 secret / environment config
- 创建 production release
- 执行 rollback
- 关闭 blocker / severity-high 问题
- 最终是否继续 canary / 全量放量

## Recommended GitHub Guardrails
- 开启 protected branches
- 要求至少一名人工 reviewer
- production 环境使用 environment protection rules
- release PR 必须附带 rollback link
- 高风险标签自动提醒额外 review，但不自动放行

## Minimal Example
### Example flow
1. 产品在 GitHub Issue 中提交需求。
2. Intake Agent 规范化 issue，并补 `risk:medium`、`stage:intake`。
3. PRD Agent 生成 `prd.ai.md` 产物，并把链接写回 issue。
4. Architect Agent 生成设计文档并链接到 issue。
5. Planning Agent 拆成 3 个子任务 issue。
6. Developer Agent 提交 PR，PR 描述包含 change summary、test notes、rollback summary。
7. QA/Security Agent 在 PR 中补测试结果与 release 建议。
8. Release/SRE Agent 准备 draft release，并附 staging/canary checklist。
9. 人类负责人确认后再合并、发布和观察。

## Minimum Fields To Standardize
- Requirement ID
- Risk Level
- Acceptance Criteria
- Linked PR / Issue
- Rollback Reference
- Release Owner
- Need Human Decision

## Suggested First Integration
先做最小集成：
1. issue template
2. pull request template
3. labels taxonomy
4. protected branch rules
5. release checklist 链接规范
