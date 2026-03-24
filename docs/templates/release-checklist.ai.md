# Release Checklist

## Meta
- Release ID: {{release_id}}
- Version: {{version}}
- Requirement ID: {{requirement_id}}
- Release Owner: {{release_owner}}
- Tech Owner: {{tech_owner}}
- Oncall Owner: {{oncall_owner}}
- Environment: {{staging|canary|production}}
- Release Window: {{release_window}}

## Summary
{{one_paragraph_release_summary}}

## Change Summary
- {{change_1}}
- {{change_2}}
- {{change_3}}

## Confirmed Inputs
- PR / MR:
  - {{pr_1}}
  - {{pr_2}}
- Requirement / PRD / Design:
  - {{requirement_or_prd_or_design_ref_1}}
  - {{requirement_or_prd_or_design_ref_2_or_none}}
- Test Plan / Change Summary:
  - {{test_plan_ref}}
  - {{change_summary_ref_or_none}}
- Test Result:
  - {{test_result_summary}}
- Risk Level:
  - {{low|medium|high}}

## Assumptions
- {{assumption_1}}
- {{assumption_2}}

## Pre-Release Checks
### Code / Config
- [ ] Correct branch confirmed
- [ ] Expected changes only
- [ ] Feature flag confirmed
- [ ] Config diff reviewed
- [ ] No unexpected migration

### Quality Gates
- [ ] Lint passed
- [ ] Type check passed
- [ ] Unit tests passed
- [ ] Integration tests passed
- [ ] Critical path verified
- [ ] Security checks passed
- [ ] Requirement / PRD / Design / Test references recoverable
- [ ] Behavior-changing scope has matching spec-update or change-control note

### Operational Readiness
- [ ] Monitoring dashboard ready
- [ ] Alerts configured
- [ ] Rollback steps validated
- [ ] Oncall aware
- [ ] Release note ready

## Rollout Strategy
### Stage 1
- Scope: {{scope_1}}
- Entry Condition: {{entry_condition_1}}
- Observation Window: {{window_1}}

### Stage 2
- Scope: {{scope_2}}
- Entry Condition: {{entry_condition_2}}
- Observation Window: {{window_2}}

### Full Rollout
- Condition: {{full_rollout_condition}}

## Metrics To Watch
- Error Rate: {{metric_1}}
- Latency: {{metric_2}}
- Success Rate: {{metric_3}}
- Business KPI: {{metric_4}}
- Infra KPI: {{metric_5}}

## Alert Thresholds
- {{threshold_1}}
- {{threshold_2}}
- {{threshold_3}}

## Rollback Plan
### Trigger
- {{rollback_trigger_1}}
- {{rollback_trigger_2}}

### Steps
1. {{rollback_step_1}}
2. {{rollback_step_2}}
3. {{rollback_step_3}}

### Validation After Rollback
- {{rollback_validation_1}}
- {{rollback_validation_2}}

## Known Issues / Waivers
- {{known_issue_1_or_none}}
- {{waiver_1_or_none}}

## Execution Log
| Time | Action | Owner | Result | Notes |
|---|---|---|---|---|
| {{time}} | {{action}} | {{owner}} | {{result}} | {{notes}} |

## Final Recommendation
- Recommendation: {{Release|Hold|Rollback}}
- Rationale:
  - {{reason_1}}
  - {{reason_2}}

## Need Human Decision
- {{decision_1_or_none}}
- {{decision_2_or_none}}

## Next Handoff
- To: Release Owner / Oncall Owner
- Goal: 执行发布并进入观察窗口
- Must Read:
  - Rollout Strategy
  - Metrics To Watch
  - Alert Thresholds
  - Rollback Plan
