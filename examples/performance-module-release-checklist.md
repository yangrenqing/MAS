# Release Checklist

## Meta
- Release ID: REL-PERF-2026-001
- Version: v2026.q2.internal-pilot.1
- Requirement ID: REQ-PERF-2026-001
- Release Owner: Release/SRE Agent
- Tech Owner: Java Platform Lead
- Oncall Owner: Internal Pilot Oncall
- Environment: staging
- Release Window: TBD

## Summary
本次发布为 Moka 风格绩效模块 internal pilot 的 release prep 样例，目标是在不涉及真实生产放量的前提下，验证绩效周期、模板、绩效计划、自评与主管评分闭环是否已具备安全试跑条件。该样例强调内网白名单、人工审批、组织范围控制和回滚可执行性，不包含任何自动生产部署动作。

## Change Summary
- 新增 performance-service 绩效周期、模板、绩效计划、自评与主管评分能力
- organization-service 补充面向绩效计划生成的组织 / 主管关系查询能力
- 增加绩效域审计记录、状态流转约束与 internal pilot 白名单开关

## Confirmed Inputs
- PR / MR:
  - PERF-BE-101 performance-service domain APIs
  - PERF-ORG-102 organization-service query support
- Test Result:
  - QA result is Conditional Pass for internal pilot with whitelist restriction
- Risk Level:
  - high

## Assumptions
- 本次仅为 internal pilot / adopted-repo 演练，不连接真实生产 HR 数据。
- 可通过配置控制仅对白名单组织暴露绩效模块入口。

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
- Scope: staging 环境 + 单个白名单部门
- Entry Condition: 测试计划 P0 全通过，HR 业务 owner 与 tech owner 人工批准试跑开始
- Observation Window: 1 business day

### Stage 2
- Scope: internal pilot 扩展到少量白名单部门
- Entry Condition: Stage 1 无越权、无状态错乱、无组织映射异常积压
- Observation Window: 2 business days

### Full Rollout
- Condition: 不适用；本样例仅用于 internal pilot / release prep 演练，后续真实 adopted repo 需单独审批

## Metrics To Watch
- Error Rate: performance_plan_generation_error_rate
- Latency: performance_plan_generation_p95
- Success Rate: self_review_submit_success_rate / manager_review_submit_success_rate
- Business KPI: completed_review_count_in_whitelist_scope
- Infra KPI: organization_service_dependency_error_rate

## Alert Thresholds
- performance_plan_generation_error_rate > 2% for 15 minutes
- unauthorized access attempt confirmed or permission-denied anomalies spike unexpectedly
- organization_service_dependency_error_rate > 5% for 15 minutes

## Rollback Plan
### Trigger
- 发现越权读取 / 越权写入绩效数据
- 白名单组织内出现大量绩效计划生成失败或状态错乱

### Steps
1. 关闭 `perf_module_enabled` 开关并隐藏 gateway 入口。
2. 停止新的周期发布、计划生成和评分提交，只保留人工排查所需只读访问。
3. 按 runbook 检查异常记录范围、组织依赖状态和审计日志，再决定是否修复后重试。

### Validation After Rollback
- 白名单组织用户不再看到绩效模块入口
- 无新的绩效写操作进入系统，既有数据保持可审计可回溯

## Known Issues / Waivers
- none
- none

## Execution Log
| Time | Action | Owner | Result | Notes |
|---|---|---|---|---|
| TBD | Prepare internal pilot whitelist | Release Owner | pending | 等待 HR owner 确认首个试跑部门 |

## Final Recommendation
- Recommendation: Release
- Rationale:
  - 当前样例已具备 internal pilot 所需最小质量门禁、白名单控制和回滚路径
  - 未引入自动生产动作，仍保留人工审批、观察与回滚决策点

## Need Human Decision
- 首个 internal pilot 是否只选择一个部门和一名主管链路试跑？
- 若组织主数据偶发异常但无权限问题，是否允许在当前白名单范围继续观察而不是立即停止？

## Next Handoff
- To: Release Owner / Oncall Owner
- Goal: 执行发布并进入观察窗口
- Must Read:
  - Rollout Strategy
  - Metrics To Watch
  - Alert Thresholds
  - Rollback Plan
