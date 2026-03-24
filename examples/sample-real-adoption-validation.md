# Sample Real Adoption Validation

## Goal
给第一次把 starter 落到真实项目仓库的团队一份“整体 adoption 是否已经 ready”的真实项目风格验证样例：不仅检查 placeholder CI 是否被安全替换，还一起验证文档落位、owner 映射、外部回链、高风险路径调优，以及人工审批点是否仍然完整保留。

## Meta
- Validation ID: `ADOPTION-VALIDATION-REAL-2026-001`
- Project / Repo: `customer-portal-web`
- Runtime: `Node.js + TypeScript`
- Package Manager: `pnpm`
- Primary Workflow Platform: `Jira`
- Primary Code Review Platform: `GitHub`
- Secondary Infra / Config Platform: `GitLab`
- Requirement ID: `REQ-AUTH-2026-014`
- Date: `2026-03-23`

## Summary
本示例演示一个 adopted repo 在首次接入 starter 后，如何检查“文档、流程、质量门禁、人工决策点”是否已经形成最小闭环。结果为 `Needs Small Fixes`：主链路已经足够支撑第一次 adoption，但团队仍需在真实仓库里固定状态主来源、补齐 repo-specific 高风险路径，并决定 security-style checks 的默认接入位置。

## Confirmed Facts
- 该 adopted repo 已将 starter 的核心资产接入到项目内：`.claude/agents/`、`.claude/settings.json`、`docs/templates/*.ai.md`、协作模板与 runbooks。
- 项目内长文档已经收敛到稳定目录：`docs/ai/requirements/`、`docs/ai/prd/`、`docs/ai/design/`、`docs/ai/test-plans/`、`docs/ai/release/`、`docs/ai/pilot-runs/`。
- 默认 PR 质量门禁已经替换为 repo-local 命令：`pnpm lint`、`pnpm typecheck`、`pnpm test -- password-reset`，且不包含 deploy、rollback 或流量放量动作。

## Assumptions
- 本文件是“真实项目风格 adoption 验证”样例，用于验证 starter 的可落地性，不代表真实生产变更已经执行。
- 本样例沿用 Jira 作为需求状态主来源、GitHub 作为代码协作主入口、GitLab 只在存在配套 infra/config 改动时出现。

## Validation Scope
- 文档目录是否固定且可导航
- Requirement ID、tracker ID 和 backlinks 是否形成稳定链路
- release approver / rollback owner / on-call 是否有最小 owner 映射
- placeholder CI 是否已替换为 repo-local 质量命令
- repo-specific 高风险路径是否比 starter 默认值更贴近真实代码结构
- release / canary / rollback 是否仍保留人工节点

## Document Placement Applied
| Artifact type | Path chosen in adopted repo | Why this is good enough for first adoption |
|---|---|---|
| Requirement intake | `docs/ai/intake/REQ-AUTH-2026-014-intake.md` | 让 intake 与后续 requirement 文档分离，便于回看原始输入 |
| Normalized requirement | `docs/ai/requirements/REQ-AUTH-2026-014.md` | 作为长文档主入口，稳定承接 Requirement ID |
| PRD | `docs/ai/prd/PRD-AUTH-2026-014.md` | 与 requirement 并列，方便 handoff 和对照 |
| Design | `docs/ai/design/DESIGN-AUTH-2026-014.md` | 明确技术决策、风险和 rollback 方案 |
| Test plan | `docs/ai/test-plans/TEST-AUTH-2026-014.md` | 让 QA/security gate 有固定落点 |
| Release prep | `docs/ai/release/REL-AUTH-2026-014.md` | 把 rollout、metrics、alerts、rollback trigger 集中管理 |
| Pilot notes | `docs/ai/pilot-runs/PILOT-AUTH-2026-014.md` | 便于 adoption 后回填摩擦点和改进建议 |

## Minimum Owner Mapping Applied
| Responsibility | Human owner in adopted repo | Why this mapping is acceptable for first adoption |
|---|---|---|
| Requirement intake | Product manager | 负责把支持反馈或业务输入归一化 |
| PRD acceptance | Product owner | 负责 scope / non-scope / acceptance 最终口径 |
| Technical design | Tech lead | 负责影响范围、风险、rollback 思路 |
| Implementation coordination | Engineering lead | 负责任务拆分、依赖和 reviewer 安排 |
| QA / security review | Senior engineer acting as QA owner | 小团队下可合并角色，但责任仍明确 |
| Release approval | Release manager | 保留人工批准，不因 CI 通过自动放行 |
| Rollback decision | On-call / incident commander | 保留最终人类判断 |

## Tracking Chain Applied
| Stage | Source of truth used | Linked artifacts kept in sync |
|---|---|---|
| Intake / requirement | Jira `AUTH-142` | GitHub issue `customer-portal-web#128` + requirement doc |
| PRD / design / test plan | Repo docs | Jira story + GitHub issue |
| Development | GitHub PR `customer-portal-web#412` | Jira story + Requirement ID + release checklist |
| Release prep | Jira change ticket `CHG-221` | GitHub PR + GitLab MR `platform/runtime-config!88` + release checklist |
| Infra / companion change | GitLab MR `!88` | Jira change ticket + GitHub PR |

## Repo-Specific High-Risk Path Tuning Applied
| Rule area | Starter default posture | Repo-specific tuning added in adopted repo | Why it matters |
|---|---|---|---|
| Auth-sensitive code | `auth/` | `identity/`, `session/`, `login/` | 这些目录同样会影响认证、会话和恢复链路 |
| Data / migration | `db/migrations/`, `migrations/` | `backfill/`, `ledger/` | 避免只盯 schema 目录，漏掉高风险数据修复脚本 |
| Privileged internal surfaces | not always explicit | `admin/`, `internal-api/` | 首次 adoption 时常被忽略，但需要更强 review |
| Runtime / config | keyword-based only | `runtime-config/` | 配置 companion change 也可能影响 rollout 与 rollback |

## Selected Minimum Command Set
| Check type | Command selected | Why it stayed in first-cut PR gate |
|---|---|---|
| Lint | `pnpm lint` | 已是团队本地稳定主入口 |
| Typecheck | `pnpm typecheck` | 可快速发现 build/type regressions |
| Tests | `pnpm test -- password-reset` | 先接最小关键回归，不把全量 e2e 一次塞进来 |
| Security-style checks | not yet in default PR gate | 团队尚未统一主命令和噪音阈值 |

## Validation Results
| Check | Result | Notes |
|---|---|---|
| 文档目录固定且 README 可导航 | pass | artifact placement 已有一致路径，不再散落 |
| Requirement ID / tracker / backlinks 保持稳定 | pass | Jira、GitHub、GitLab 与 repo docs 形成单链路 |
| release approver / rollback owner / on-call 已明确 | pass | 首轮 adoption 已满足最低责任兜底 |
| placeholder CI 已替换为 repo-local 质量命令 | pass | gate 名称与 README / test plan 对齐 |
| 默认 pipeline 不暗示 staging / production deploy | pass | 仍只有 local-quality checks |
| repo-specific 高风险路径已完成首轮调优 | partial | `identity/`、`session/` 已补，但仍需在真实事故后复核 |
| security-style checks 的默认接入策略已固定 | partial | 仍需团队决定放入 PR gate 还是 nightly/manual |

## What Worked Well
- `docs/adopted-repo-guide.md` 的目录建议让 adopted repo 更容易一次性固定 artifact 放置位置。
- `docs/templates/requirement-intake.ai.md` 的 `Status Source of Truth`、`Primary Tracker ID` 和 backlink 字段，足以支撑跨系统回链。
- 把质量门禁限制在 repo-local lint / typecheck / focused test，能让首次 adoption 保持轻量且不误触 deploy 范围。

## Friction Points
| Area | What slowed us down | Impact | Severity |
|---|---|---|---|
| status ownership | Jira 与 GitHub Issue 都能承载状态，第一次 adoption 需要先定主入口 | 若不先固定，状态会双写漂移 | medium |
| reviewer policy | 团队知道 auth 改动高风险，但不一定一开始就写清“谁必须额外 review” | 高风险变更流程仍可能依赖口头约定 | medium |
| security checks | 团队有若干零散扫描命令，但没有统一低噪音主入口 | 不适合直接塞进默认 PR gate | medium |
| path tuning depth | 首轮多靠目录名称判断风险，尚未经过真实 incident / release 回看 | 规则仍可能存在漏报或误报 | medium |

## Gaps Found in the Starter
- Missing template / doc:
  - 在本样例补充之前，starter 缺少一份把“文档落位 + owner 映射 + CI 替换 + backlinks + 风险路径”放在同一处验证的整体 adoption 样例。
- Confusing step / unclear ownership:
  - 小团队 adoption 时，release approver 与 rollback decider 的最小拆分仍容易被忽略，需要持续强调。
- External system integration gap:
  - 虽然 mapping 示例已存在，但团队仍需要在首次 adoption 时明确 Jira 或 GitHub 谁是状态主来源。
- Automation gap:
  - starter 只能给出安全边界和示例，repo-specific reviewer policy 与 security-style checks 仍需 adopted repo 自行固定。

## Recommendation
- Result: Needs Small Fixes
- Notes:
  - 这套 starter 已足够支撑 adopted repo 的第一次整体落地验证，而不只是提供文档母版。
  - 真正进入长期使用前，团队仍应补齐 repo-specific reviewer 规则、security-style check 策略和第二轮高风险路径复核。

## Need Human Decision
- adopted repo 最终要把 Jira 还是 GitHub Issue 设为需求状态主来源？
- dependency / static security checks 应该进入默认 PR gate，还是先保留为 nightly / manual 入口？

## Next Handoff
- To: Repo Maintainer / Knowledge-Ops Agent
- Goal: 把这份 adoption 验证样例中的有效做法回填到 adopted repo 的 README、PR/MR 模板、release checklist 和风险路径规则
- Must Read:
  - Document Placement Applied
  - Minimum Owner Mapping Applied
  - Validation Results
  - Friction Points
