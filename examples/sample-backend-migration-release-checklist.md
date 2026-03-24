# Sample Backend Migration Release Checklist

## Meta
- Release ID: REL-2026-003
- Version: v2026.05.0
- Requirement ID: REQ-2026-003
- Release Owner: Carol Liu
- Tech Owner: Bob Wang
- Oncall Owner: David Lin
- Environment: controlled production rollout
- Release Window: 2026-05-20 20:00-23:00 local time

## Summary
本次发布执行订单账单字段迁移的 expand-migrate-switch 阶段：先确认 additive migration 与双写稳定，再在受控低峰窗口执行历史 backfill，验证关键查询和报表 diff 后，按人工批准逐步切换读路径到结构化列。若出现 diff、数据库压力或订单主链路异常，则立即停止 backfill 并回退到旧 JSON 读取。

## Change Summary
- `orders` 表新增结构化账单列
- order-service 新增账单字段双写逻辑与差异指标
- backfill worker 新增历史订单回填与 checkpoint 恢复能力
- reporting validation job 新增新旧口径 diff 报告

## Confirmed Inputs
- PR / MR:
  - PR-201 additive order billing columns
  - PR-202 order-service dual write
  - PR-203 backfill worker and validation metrics
- Test Result:
  - Staging migration rehearsal passed; QA result is Conditional Pass with controlled read switch requirement
- Risk Level:
  - high

## Assumptions
- 数据库 DBA 在发布窗口内可观察主库与副本状态。
- 切读 flag 支持快速回退到旧 JSON 口径。

## Pre-Release Checks
### Code / Config
- [ ] Correct branch confirmed
- [ ] Expected changes only
- [ ] Read flag confirmed off by default
- [ ] Backfill throttle config reviewed
- [ ] No destructive migration in this release

### Quality Gates
- [ ] Lint passed
- [ ] Type check passed
- [ ] Unit tests passed
- [ ] Integration tests passed
- [ ] Migration rehearsal verified
- [ ] Security checks passed

### Operational Readiness
- [ ] Monitoring dashboard ready
- [ ] Alerts configured
- [ ] Rollback steps validated
- [ ] Oncall aware
- [ ] Diff report ready for review

## Rollout Strategy
### Stage 1
- Scope: apply additive schema change and enable dual-write only
- Entry Condition: release owner + DBA approve start
- Observation Window: 30 minutes

### Stage 2
- Scope: run throttled historical backfill on a limited ID range
- Entry Condition: Stage 1 stable, no order write regression
- Observation Window: 45 minutes

### Stage 3
- Scope: expand backfill to full target range and enable structured read for internal / validation consumers only
- Entry Condition: Stage 2 diff report and DB metrics within threshold
- Observation Window: 45 minutes

### Full Rollout
- Condition: human approval after internal read validation succeeds and no rollback trigger fires

## Metrics To Watch
- Error Rate: order_billing_structured_read_error_rate
- Data Quality: billing_field_diff_rate
- DB Health: primary_cpu, lock_wait_time, replica_lag_seconds
- Backfill KPI: backfill_success_rate, backfill_remaining_rows
- Business KPI: order query success rate / reporting freshness

## Alert Thresholds
- billing_field_diff_rate > 0.5% for 15 minutes
- replica_lag_seconds > 120 for 10 minutes
- order query error rate > 1% above baseline for 10 minutes
- primary_cpu sustained above agreed threshold during backfill window

## Rollback Plan
### Trigger
- read diff rate exceeds threshold and does not recover in observation window
- order-service write latency or error rate degrades materially
- DB health indicates unsafe backfill pressure

### Steps
1. Set `orders_structured_billing_read` flag to off immediately.
2. Pause backfill worker by setting throttle / concurrency to 0.
3. Confirm order reads and reporting fall back to legacy JSON path.
4. Notify release owner, DBA, and dependent data teams.

### Validation After Rollback
- order query success rate returns to baseline
- no further structured-read traffic is served
- backfill worker remains paused and checkpoints preserved

## Known Issues / Waivers
- none
- none

## Execution Log
| Time | Action | Owner | Result | Notes |
|---|---|---|---|---|
| 20:00 | Start Stage 1 additive migration + dual-write verification | Release Owner | pending | Waiting for final human approval |

## Final Recommendation
- Recommendation: Release with Controls
- Rationale:
  - 迁移设计具备双写、回填、切读和回退分层控制
  - 高风险部分保留了人工观察与批准点，适合受控发布

## Need Human Decision
- Stage 3 内部 / 验证消费者范围是否足以作为切读前观察样本？
- 若 backfill 完成但 diff 略高于目标阈值，是否允许维持 dual-write 而暂不切读？

## Next Handoff
- To: Release Owner / Oncall Owner / DBA
- Goal: 执行迁移窗口并完成受控观察
- Must Read:
  - Rollout Strategy
  - Metrics To Watch
  - Alert Thresholds
  - Rollback Plan
