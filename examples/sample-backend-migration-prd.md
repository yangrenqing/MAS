# Sample Backend Migration PRD

## Meta
- Requirement ID: REQ-2026-003
- Title: Migrate Legacy Order Billing JSON to Structured Columns
- Type: migration
- Priority: P1
- Requested By: Platform Team
- Product Owner: Alice Chen
- Tech Owner: Bob Wang
- Target Release: 2026-05 Wave 1
- Due Date: 2026-05-20

## Summary
将订单账单相关字段从历史 JSON 结构迁移到结构化列，首期目标是完成加列、历史回填、双写验证和受控切读，使新报表与下游系统可基于稳定列读取，同时保留可回滚路径，不在首期删除旧字段。

## Confirmed Facts
- 当前部分账单字段仅存在于 `legacy_billing_payload` JSON 中。
- 现有报表与对账逻辑已经包含补丁式 JSON 解析和字段兼容逻辑。
- 新的下游消费者需要稳定的结构化列作为长期接口。

## Assumptions
- 新增结构化列均可为空，允许分阶段回填。
- 迁移首发采用双写 + feature flag 切读，而不是一次性硬切。

## Business Context
- Problem: JSON 解析导致账单字段口径不统一、排障困难、查询效率低。
- Why Now: 后续 billing / reporting 工作已经依赖结构化字段，若不先迁移会阻塞后续项目。
- If Not Done: 继续维护多套解析逻辑，报表错误和数据治理成本会持续增加。

## Goals
- Business Goal:
  - 为报表、对账和下游系统提供稳定字段口径。
  - 降低历史账单数据解析失败和人工补丁成本。
- User Goal:
  - 内部运营、财务和数据团队可以稳定读取账单字段，不再依赖临时 JSON 解析。

## Non-Goals
- 删除旧 JSON 字段。
- 修改订单结算规则。
- 回填所有历史脏数据到完全无人工介入。

## Target Users
- Primary User: 平台、数据、财务和对账相关内部团队
- Secondary User: 依赖订单账单字段的下游服务

## User Stories
1. As a data consumer, I want structured billing fields on orders, so that I can query and aggregate reliably.
2. As a platform engineer, I want the migration to be staged and reversible, so that historical data changes do not break the live order flow.

## Scope
### In Scope
- 为订单表新增结构化账单列。
- 新订单路径写入新旧两套字段。
- 历史订单分批回填并记录失败原因。
- 通过 flag 控制读路径从旧 JSON 切换到新结构化列。

### Out of Scope
- 首期删除旧字段或清理所有兼容代码。
- 改造订单创建 API 对外契约。
- 变更财务计算逻辑。

## Main Flow
1. 发布加列 migration，不影响现有读写。
2. 新写入路径开启双写，确保新增订单同时写入 JSON 与结构化列。
3. 历史回填任务按批次执行，并输出成功率、失败原因和剩余量。
4. 验证通过后，将读路径按 flag 切换到结构化列。

## Edge Cases
- 历史 JSON 缺失关键字段时，回填任务记录失败并跳过，不写入错误值。
- 回填任务中断后可从上次进度继续，不重复破坏已成功数据。
- 切读后如发现结构化字段异常，可快速回退到旧 JSON 读路径。

## Acceptance Criteria
- [ ] 目标结构化列已添加且新订单写入同时覆盖旧 JSON 与新列。
- [ ] 历史订单回填任务可分批执行、可恢复，并输出失败明细。
- [ ] 结构化列切读后，关键查询、报表和下游读取结果与旧口径一致。
- [ ] 迁移过程具备监控、回滚方案和人工决策点。

## Failure Conditions
- 切读后订单查询或报表结果与旧口径持续不一致。
- 回填过程导致明显数据库压力异常或订单主链路受损。

## Dependencies
- Upstream Dependency: 数据库 migration 执行能力与回填 worker 资源
- External System: 报表 / 对账任务需要支持双读验证
- Team Dependency: 平台、数据和财务团队共同确认字段映射规则

## Constraints
- Time Constraint: 需在 2026-05 Wave 1 前提供可切读的结构化字段能力
- Compliance Constraint: 迁移日志中不得暴露敏感账单原文
- Security Constraint: 不允许因为迁移导致订单访问权限或账单展示越权
- Performance Constraint: 回填和切读不得显著影响订单主链路延迟和数据库稳定性

## Risks
- 历史 JSON 数据质量不一致可能导致部分记录无法自动映射。
- 双写窗口过长会增加维护成本和数据漂移风险。

## Unknowns
- 无法自动映射订单的占比阈值最终定为多少仍待确认。
- 旧字段清理是否放到单独一期 contract 阶段仍待确认。

## Need Human Decision
- 回填失败率达到多少时必须阻止切读？
- 旧 JSON 字段的清理是否必须单独走下一次发布窗口？

## Next Handoff
- To: Architect Agent
- Goal: 输出最小可执行的分阶段迁移方案与回滚策略
- Must Read:
  - Acceptance Criteria
  - Edge Cases
  - Risks
  - Dependencies
