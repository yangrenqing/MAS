# Staging Checklist

## Meta
- Staging Check ID: {{staging_check_id}}
- Requirement ID: {{requirement_id}}
- Release ID: {{release_id_or_TBD}}
- Owner: {{owner}}
- Environment: staging
- Date: {{date}}

## Summary
{{one_paragraph_summary}}

## Confirmed Inputs
- PR / MR:
  - {{pr_1}}
  - {{pr_2_or_none}}
- Build / Artifact:
  - {{artifact_1}}
- Test Plan:
  - {{test_plan_id_or_path}}
- Rollback Reference:
  - {{rollback_doc_or_release_checklist}}

## Assumptions
- {{assumption_1_or_none}}
- {{assumption_2_or_none}}

## Deployment Readiness
- [ ] Correct branch / commit confirmed
- [ ] Required config available in staging
- [ ] Feature flags aligned with test scope
- [ ] No unexpected schema or migration risk
- [ ] Rollback path understood

## Validation Scope
### Critical Path
- [ ] {{critical_path_1}}
- [ ] {{critical_path_2}}
- [ ] {{critical_path_3}}

### Regression Focus
- [ ] {{regression_focus_1}}
- [ ] {{regression_focus_2}}

### Security / Risk Focus
- [ ] {{risk_focus_1}}
- [ ] {{risk_focus_2}}

## Results
- Build Result: {{pass|fail|partial}}
- Smoke Result: {{pass|fail|partial}}
- Critical Path Result: {{pass|fail|partial}}
- Regression Result: {{pass|fail|partial}}
- Security Check Result: {{pass|fail|partial|not_run}}

## Defects / Gaps
- {{defect_1_or_none}}
- {{defect_2_or_none}}
- {{gap_1_or_none}}

## Recommendation
- Recommendation: {{Ready for Canary|Stay in Staging|Fix Before Proceeding}}
- Rationale:
  - {{reason_1}}
  - {{reason_2}}

## Need Human Decision
- {{decision_1_or_none}}
- {{decision_2_or_none}}

## Next Handoff
- To: Release/SRE Agent / Release Owner
- Goal: 判断是否进入 canary 或继续修复
- Must Read:
  - Validation Scope
  - Results
  - Defects / Gaps
  - Recommendation
