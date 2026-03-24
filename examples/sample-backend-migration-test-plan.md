# Sample Backend Migration Test Plan

## Meta
- Test Plan ID: TP-2026-003
- Requirement ID: REQ-2026-003
- Design ID: DES-2026-003
- Owner: QA/Security Agent
- Target Release: 2026-05 Wave 1
- Environment: staging + controlled production rollout

## Summary
本测试计划聚焦订单账单字段迁移的正确性、可恢复性和发布安全性，重点验证 additive migration、双写一致性、历史 backfill 的幂等与节流、结构化字段切读后的口径一致性，以及异常情况下的快速回退能力。

## Confirmed Inputs
- Acceptance Criteria:
  - 新订单双写成功
  - 历史回填可恢复且输出失败明细
  - 切读后关键结果与旧口径一致
- Affected Modules:
  - order-service
  - billing-reporting-job
  - db migration runner
- Known Risks:
  - 数据不一致风险
  - backfill 带来的数据库稳定性风险

## Assumptions
- staging 可提供一批覆盖正常、缺字段、异常 JSON 的历史样本订单。
- 切读路径可通过 flag 在无需重新部署的情况下切回。

## Test Objectives
- 验证结构化列 schema 变更不会破坏现有订单读写。
- 验证新订单双写和历史回填结果正确、可恢复、可观测。
- 验证读路径切换与回退不会造成持续口径漂移。

## In Scope
- additive migration 执行验证
- 双写路径与差异指标
- backfill worker、失败重试与节流策略
- 报表 / 查询口径比对与读路径切换

## Out of Scope
- contract 阶段删除旧字段
- 财务业务规则本身的重新定义

## Risk Focus
- High-Risk Path:
  - order write dual-write path
  - historical backfill update path
  - structured-field read switch path
- High-Risk Dependency:
  - primary database / replicas
  - reporting validation jobs
- Security Focus:
  - 日志与告警不得泄露原始账单敏感数据

## Test Strategy
### Unit
- Target:
  - JSON → structured mapping parser
  - dual-write diff detector
  - backfill checkpoint / retry logic
- Pass Condition:
  - 成功、缺字段、异常 JSON、重复执行等分支均被覆盖

### Integration
- Target:
  - order-service 双写 + 数据库存储
  - backfill worker + checkpoint resume
  - read flag toggle + fallback
- Pass Condition:
  - 新订单双写一致，历史回填可从中断位置继续，切读失败时可快速回退

### E2E / Critical Path
- Target:
  - 创建新订单后在查询与报表侧读到一致的结构化账单字段
- Pass Condition:
  - 新订单在新旧字段、报表输出和下游读取中的口径一致

### Regression
- Must Recheck:
  - 订单创建主链路
  - 现有账单报表导出
  - 依赖旧 JSON 的兼容查询

### Smoke
- Pre-Release Smoke:
  - additive migration 后核心订单读写正常
  - 双写打开后新订单字段齐全
- Post-Release Smoke:
  - backfill 节流稳定
  - 切读小流量后关键查询与报表差异在阈值内

## Test Cases
| ID | Title | Level | Preconditions | Steps | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-301 | New order writes both legacy and structured billing fields | Integration | 双写已开启 | 创建订单并读取数据库记录 | JSON 与结构化列字段一致 | P0 |
| TC-302 | Backfill resumes safely after interruption | Integration | 存在未回填历史订单 | 执行 backfill，中途停止后恢复 | 已成功记录不被破坏，进度从 checkpoint 继续 | P0 |
| TC-303 | Read path falls back to legacy JSON after flag rollback | E2E | 已开启结构化列切读 | 关闭 read flag 并重查订单/报表 | 系统恢复旧口径，无持续错误 | P0 |

## Edge / Failure Testing
- Boundary Case:
  - 接近批次边界的订单不会被重复遗漏处理
- Invalid Input:
  - JSON 缺字段、字段类型异常、未知 currency 值
- Timeout / Retry:
  - backfill 更新超时后记录失败并可重试
- Idempotency / Concurrency:
  - 同一批次重复执行不会破坏已成功记录；多 worker 并发时不会重复抢写同一范围

## Security Checks
- AuthZ Check: yes - 新结构化列不应绕过现有订单访问权限
- Sensitive Data Check: yes - 日志、告警、失败样本中不输出完整账单原文
- Input Validation Check: yes - backfill 仅接受合法批次范围和受控参数
- Dependency Scan Needed: no

## Test Data / Setup
- Test Orders:
  - 正常 JSON 样本
  - 缺失 `billing_currency` 样本
  - 异常 `tax_region` 样本
- Seed Data: 一批已知历史订单 ID 范围与预期结构化映射结果
- Feature Flag: `orders_structured_billing_read`
- Mock / Stub: reporting validation job 可输出新旧口径 diff 报告

## Release Gate
### Must Pass
- [ ] P0 cases pass
- [ ] 新订单双写一致
- [ ] backfill 可恢复且失败明细可审阅
- [ ] 关键查询 / 报表 diff 在阈值内
- [ ] Rollback plan exists
- [ ] 数据库稳定性指标在可接受范围内

### Block Release If
- 切读后关键报表或订单查询 diff 持续超阈值
- backfill 导致数据库 CPU、锁等待或复制延迟明显异常
- 失败样本中存在无法解释的大面积数据丢失风险

## Open Defects
| Bug ID | Severity | Summary | Status | Blocks Release |
|---|---|---|---|---|
| none | none | none | none | No |

## Recommendation
- Result: Conditional Pass
- Notes:
  - 允许进入受控发布，但必须先完成 backfill 验证与小流量切读观察
  - contract 阶段删除旧字段不应并入本次窗口

## Need Human Decision
- 关键查询 / 报表 diff 的阻断阈值按 0.1% 还是 0.5% 执行？
- 回填失败样本是否要求平台、数据、财务三方联合签字后才能切读？

## Next Handoff
- To: Release/SRE Agent
- Goal: 准备迁移执行窗口、观察指标与回退步骤
- Must Read:
  - Risk Focus
  - Release Gate
  - Recommendation
  - Edge / Failure Testing
