# Sample External System Mapping

## Goal
给第一次采用 starter 的团队一个可直接套用的“编号映射样例”，说明一个需求如何在 Jira、GitHub Issue、GitHub PR、GitLab MR 之间保持同一条可追踪链路。

## Example Scope
- Requirement Type: auth bugfix
- Canonical Requirement ID: `REQ-AUTH-2026-014`
- Primary workflow system: Jira
- Primary code review system: GitHub
- Secondary delivery / infra change system: GitLab

## Canonical Identifier Set
| Object | System | Example Identifier | Purpose |
|---|---|---|---|
| Requirement / Story | Jira | `AUTH-142` | 需求状态主入口 |
| Change / release ticket | Jira | `CHG-221` | 发布窗口与审批记录 |
| Requirement issue | GitHub | `customer-portal-web#128` | 代码仓库内的协作入口 |
| Application PR | GitHub | `customer-portal-web#412` | 应用代码变更评审 |
| Infra / config MR | GitLab | `platform/runtime-config!88` | 环境或配置配套变更 |
| Starter artifact path | Repo docs | `docs/ai/requirements/REQ-AUTH-2026-014.md` | AI 文档主存储 |

## Recommended Source of Truth by Stage
| Stage | Source of truth | Must backlink to |
|---|---|---|
| Intake / requirement | Jira `AUTH-142` | GitHub issue + requirement doc |
| PRD / design / test plan | Repo docs | Jira story + GitHub issue |
| Development | GitHub PR `#412` | Jira story + GitHub issue + requirement ID |
| Release prep | Jira `CHG-221` | GitHub PR + GitLab MR + release checklist |
| Infra / env change | GitLab MR `!88` | Jira change ticket + GitHub PR |

## Backlink Rules
1. **Requirement ID stays stable** across all systems and appears in every long-form document.
2. **Jira key goes into GitHub / GitLab titles or descriptions** when the team uses Jira as workflow source.
3. **GitHub issue is the code-facing collaboration anchor** for developers and reviewers.
4. **GitLab MR is only added when there is a real infra / env / config companion change**, not by default.
5. **Release or change ticket links every implementation artifact back together** before any human release approval.

## Minimal Field Mapping
| Field | Jira | GitHub Issue / PR | GitLab MR |
|---|---|---|---|
| Requirement ID | custom field or description | issue body + PR body | MR description |
| Risk Level | custom field | label such as `risk:high` | label such as `risk::high` |
| Need Human Decision | description / comment | issue comment / PR checklist | MR checklist |
| Rollback Reference | change ticket | PR body / release checklist link | MR description |
| Linked Work | linked issues | linked issue / PR | related MR / change ticket |

## Example Link Chain
- Jira Story: `AUTH-142` — Expired Password Reset Link Shows 500 Error
- GitHub Issue: `customer-portal-web#128` — `[AUTH-142] Normalize password reset failure path`
- Requirement doc: `docs/ai/requirements/REQ-AUTH-2026-014.md`
- GitHub PR: `customer-portal-web#412` — `[AUTH-142] Fix expired reset token 500 path`
- GitLab MR: `platform/runtime-config!88` — `[CHG-221] tighten reset-flow alert threshold`
- Jira Change Ticket: `CHG-221` — Password reset bugfix rollout approval

## Example Backfill Snippets
### GitHub Issue body
```md
Requirement ID: REQ-AUTH-2026-014
Jira: AUTH-142
Risk: high
Linked PR: TBD
Release / Change Ticket: CHG-221
```

### GitHub PR description
```md
Linked Issue: customer-portal-web#128
Requirement ID: REQ-AUTH-2026-014
Jira Story: AUTH-142
Release / Change Ticket: CHG-221
Companion GitLab MR: platform/runtime-config!88
Rollback Ref: docs/ai/release/REL-AUTH-2026-014.md
```

### GitLab MR description
```md
Change Ticket: CHG-221
Related App PR: customer-portal-web#412
Requirement ID: REQ-AUTH-2026-014
Reason: align runtime alert thresholds with password reset bugfix rollout
```

### Jira comment
```md
Requirement doc: docs/ai/requirements/REQ-AUTH-2026-014.md
GitHub Issue: customer-portal-web#128
GitHub PR: customer-portal-web#412
GitLab MR: platform/runtime-config!88
Release checklist: docs/ai/release/REL-AUTH-2026-014.md
```

## Guardrails
- 不要让多个系统同时做“状态主来源”；先定一个主系统。
- 只有真的存在 infra / env companion change 时才创建 GitLab MR。
- 发布审批、rollback、是否继续放量仍必须由人类决定。
- 若没有 Jira，就保留 Requirement ID + GitHub Issue / PR 链路，不要为了凑系统而增加复杂度。

## Recommended First Use
1. 先固定一个稳定的 Requirement ID 格式。
2. 决定 Jira 或 GitHub Issue 谁是状态主来源。
3. 在 issue / PR / MR 模板里加入 backlink 字段。
4. 在 release checklist 中要求回填所有外部编号。
5. 第一次真实发布后，再回头删掉不必要的系统映射。
