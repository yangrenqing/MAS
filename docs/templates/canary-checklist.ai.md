# Canary Checklist

## Meta
- Canary Check ID: {{canary_check_id}}
- Release ID: {{release_id}}
- Requirement ID: {{requirement_id}}
- Owner: {{owner}}
- Environment: canary
- Canary Scope: {{traffic_scope_or_segment}}
- Observation Window: {{window}}

## Summary
{{one_paragraph_summary}}

## Confirmed Inputs
- Staging Result:
  - {{staging_result_summary}}
- Release Checklist:
  - {{release_checklist_path_or_id}}
- Metrics Dashboard:
  - {{dashboard_or_TBD}}
- Alerts:
  - {{alert_set_or_TBD}}

## Assumptions
- {{assumption_1_or_none}}
- {{assumption_2_or_none}}

## Entry Criteria
- [ ] Staging sign-off completed
- [ ] Rollback owner identified
- [ ] Monitoring dashboard available
- [ ] Alert thresholds agreed
- [ ] Oncall aware of canary window

## Metrics To Watch
- Error Rate: {{metric_1}}
- Latency: {{metric_2}}
- Success Rate: {{metric_3}}
- Business KPI: {{metric_4}}
- Infra KPI: {{metric_5_or_none}}

## Alert Thresholds
- {{threshold_1}}
- {{threshold_2}}
- {{threshold_3}}

## Observation Log
| Time | Signal | Owner | Status | Notes |
|---|---|---|---|---|
| {{time_1}} | {{signal_1}} | {{owner_1}} | {{normal|warning|critical}} | {{notes_1}} |
| {{time_2}} | {{signal_2}} | {{owner_2}} | {{normal|warning|critical}} | {{notes_2}} |

## Decision
- Recommendation: {{Continue Rollout|Hold|Rollback}}
- Reason:
  - {{reason_1}}
  - {{reason_2}}

## Rollback Trigger Check
- [ ] No rollback trigger hit
- [ ] If trigger hit, rollback path is executable
- [ ] Recovery validation steps are known

## Need Human Decision
- {{decision_1_or_none}}
- {{decision_2_or_none}}

## Next Handoff
- To: Release Owner / Oncall Owner
- Goal: 决定继续放量、保持 canary，或执行回滚
- Must Read:
  - Metrics To Watch
  - Alert Thresholds
  - Observation Log
  - Decision
