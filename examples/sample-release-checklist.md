# Release Checklist

## Meta
- Release ID: REL-2026-001
- Version: v2026.04.0
- Requirement ID: REQ-2026-001
- Release Owner: Carol Liu
- Tech Owner: Bob Wang
- Oncall Owner: David Lin
- Environment: canary
- Release Window: 2026-04-15 14:00-16:00 local time

## Summary
本次发布为 Web 端 existing user magic link login 首发，采用 feature flag 控制并从小流量 canary 开始。重点观察 verify 错误率、登录成功率、邮件送达延迟和异常重放行为，若异常持续则立即停止放量并关闭功能入口。

## Change Summary
- 登录页新增 magic link 入口与请求表单
- auth-service 新增 request / verify 接口与一次性令牌逻辑
- 监控新增 magic link request/verify 成功率、错误率与邮件延迟指标

## Confirmed Inputs
- PR / MR:
  - PR-101 web-login-ui magic link entry
  - PR-102 auth-service magic link backend
- Test Result:
  - Staging critical path passed; QA result is Conditional Pass with canary requirement
- Risk Level:
  - high

## Assumptions
- 事务邮件服务在发布窗口内稳定。
- feature flag 支持按百分比放量并可即时关闭。

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

### Operational Readiness
- [ ] Monitoring dashboard ready
- [ ] Alerts configured
- [ ] Rollback steps validated
- [ ] Oncall aware
- [ ] Release note ready

## Rollout Strategy
### Stage 1
- Scope: internal users + 5% eligible external web traffic
- Entry Condition: staging sign-off complete and release owner approves canary start
- Observation Window: 30 minutes

### Stage 2
- Scope: 10% eligible external web traffic
- Entry Condition: Stage 1 no sustained alert and login success rate within agreed threshold
- Observation Window: 45 minutes

### Full Rollout
- Condition: human approval after canary windows complete with no rollback trigger

## Metrics To Watch
- Error Rate: magic_link_verify_error_rate
- Latency: magic_link_request_p95
- Success Rate: magic_link_login_success_rate
- Business KPI: successful return-user login conversion
- Infra KPI: email delivery delay p95

## Alert Thresholds
- magic_link_verify_error_rate > 2% for 10 minutes
- magic_link_login_success_rate drops > 5% below password-login baseline for 15 minutes
- email delivery delay p95 > 120 seconds for 15 minutes

## Rollback Plan
### Trigger
- verify error rate exceeds threshold and does not recover in observation window
- security anomaly detected such as token replay spike or invalid redirect acceptance

### Steps
1. Set `auth_magic_link_login` feature flag to 0% immediately.
2. Confirm login page falls back to password-only entry for canary users.
3. Verify key login metrics recover and notify oncall + release owner.

### Validation After Rollback
- password login success rate returns to baseline
- no new magic link verify traffic is accepted after flag shutdown

## Known Issues / Waivers
- none
- none

## Execution Log
| Time | Action | Owner | Result | Notes |
|---|---|---|---|---|
| 14:00 | Start Stage 1 canary | Release Owner | pending | Waiting for final human approval |

## Final Recommendation
- Recommendation: Release
- Rationale:
  - Staging and QA gates are satisfied with controlled canary requirement
  - Feature flag and rollback path make the first release operationally manageable

## Need Human Decision
- 是否批准从 internal + 5% eligible external traffic 开始 canary？
- 若邮件延迟超阈值但安全指标正常，是否继续维持当前 canary 而不是立即回滚？

## Next Handoff
- To: Release Owner / Oncall Owner
- Goal: 执行发布并进入观察窗口
- Must Read:
  - Rollout Strategy
  - Metrics To Watch
  - Alert Thresholds
  - Rollback Plan
