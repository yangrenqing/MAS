# Test Plan

## Meta
- Test Plan ID: {{test_plan_id}}
- Requirement ID: {{requirement_id}}
- Design ID: {{design_id}}
- Owner: {{qa_owner}}
- Target Release: {{target_release}}
- Environment: {{test_env}}

## Summary
{{one_paragraph_test_summary}}

## Confirmed Inputs
- Acceptance Criteria:
  - {{ac_1}}
  - {{ac_2}}
- Affected Modules:
  - {{module_1}}
  - {{module_2}}
- Known Risks:
  - {{risk_1}}
  - {{risk_2}}

## Assumptions
- {{assumption_1}}
- {{assumption_2}}

## Test Objectives
- {{objective_1}}
- {{objective_2}}
- {{objective_3}}

## In Scope
- {{scope_1}}
- {{scope_2}}
- {{scope_3}}

## Out of Scope
- {{out_scope_1}}
- {{out_scope_2}}

## Risk Focus
- High-Risk Path:
  - {{path_1}}
  - {{path_2}}
- High-Risk Dependency:
  - {{dependency_1}}
- Security Focus:
  - {{security_focus_1}}

## Test Strategy
### Unit
- Target:
  - {{unit_target_1}}
- Pass Condition:
  - {{unit_pass_condition}}

### Integration
- Target:
  - {{integration_target_1}}
- Pass Condition:
  - {{integration_pass_condition}}

### E2E / Critical Path
- Target:
  - {{e2e_target_1}}
- Pass Condition:
  - {{e2e_pass_condition}}

### Regression
- Must Recheck:
  - {{regression_item_1}}
  - {{regression_item_2}}

### Smoke
- Pre-Release Smoke:
  - {{pre_smoke_1}}
- Post-Release Smoke:
  - {{post_smoke_1}}

## Test Cases
| ID | Title | Level | Preconditions | Steps | Expected Result | Priority |
|---|---|---|---|---|---|---|
| {{tc_id_1}} | {{title}} | Unit/Integration/E2E/Regression | {{preconditions}} | {{steps}} | {{expected}} | P0 |
| {{tc_id_2}} | {{title}} | Unit/Integration/E2E/Regression | {{preconditions}} | {{steps}} | {{expected}} | P1 |
| {{tc_id_3}} | {{title}} | Unit/Integration/E2E/Regression | {{preconditions}} | {{steps}} | {{expected}} | P1 |

## Edge / Failure Testing
- Boundary Case:
  - {{boundary_1}}
- Invalid Input:
  - {{invalid_input_1}}
- Timeout / Retry:
  - {{timeout_case_1}}
- Idempotency / Concurrency:
  - {{concurrency_case_1}}

## Security Checks
- AuthZ Check: {{yes|no}} - {{notes}}
- Sensitive Data Check: {{yes|no}} - {{notes}}
- Input Validation Check: {{yes|no}} - {{notes}}
- Dependency Scan Needed: {{yes|no}}

## Test Data / Setup
- Test Account: {{account}}
- Seed Data: {{seed_data}}
- Feature Flag: {{flag_or_none}}
- Mock / Stub: {{mock_or_none}}

## Release Gate
### Must Pass
- [ ] P0 cases pass
- [ ] Critical path pass
- [ ] No blocker defect
- [ ] No high-risk security issue
- [ ] Rollback plan exists
- [ ] Monitoring items defined

### Block Release If
- {{block_condition_1}}
- {{block_condition_2}}

## Open Defects
| Bug ID | Severity | Summary | Status | Blocks Release |
|---|---|---|---|---|
| {{bug_id_1_or_none}} | {{severity}} | {{summary}} | {{status}} | Yes/No |

## Recommendation
- Result: {{Pass|Conditional Pass|Block}}
- Notes:
  - {{recommendation_note_1}}
  - {{recommendation_note_2}}

## Need Human Decision
- {{decision_1_or_none}}
- {{decision_2_or_none}}

## Next Handoff
- To: Release/SRE Agent
- Goal: 准备发布与灰度策略
- Must Read:
  - Risk Focus
  - Release Gate
  - Open Defects
  - Recommendation
