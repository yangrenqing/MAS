# Sample Backend Migration Requirement Card

## Meta
- Requirement ID: REQ-2026-003
- Title: Migrate Legacy Order Billing JSON to Structured Columns
- Type: migration
- Priority: P1
- Source: Data quality issues + reporting gaps
- Requested By: Platform Team
- Intake Owner: Intake Agent

## Summary
将订单表中历史遗留的 `legacy_billing_payload` JSON 账单字段迁移为结构化列，采用“扩容字段 → 回填历史数据 → 双写验证 → 受控切读”的方式，降低报表解析错误、下游集成歧义和后续变更成本，同时避免直接中断现有订单链路。

## Confirmed Facts
- 当前部分账单信息仍存放在 `orders.legacy_billing_payload` JSON 字段中。
- 数据分析和对账流程已经出现字段命名不一致、解析失败和空值补丁逻辑。
- 新的报表与下游集成需要稳定的结构化账单字段，而不是继续依赖运行时 JSON 解析。

## Business Context
- Problem: 历史账单数据依赖 JSON 解析，导致查询复杂、报表口径不稳定、问题排查成本高。
- Why Now: 新一轮账单报表与对账治理需要统一字段口径，继续拖延会放大后续改造成本。
- Expected Value: 降低解析失败率、提升查询与治理效率、为后续 billing / finance 需求提供稳定数据基础。

## Initial Scope
### In Scope
- 在 `orders` 表新增结构化账单列，如 `billing_country_code`、`billing_currency`、`tax_region`。
- 为历史订单执行可恢复、可分批的回填任务。
- 在迁移过渡期支持新老字段双写与读路径切换。
- 为回填进度、解析失败和切读结果补充监控与验证。

### Out of Scope
- 重算历史订单价格或税额。
- 重做结账 UI 或订单创建流程。
- 在首个发布窗口内删除 `legacy_billing_payload`。
- 处理所有脏数据的人工修复，只处理可自动映射范围。

## Risks Seen At Intake
- 订单 / billing / migration 路径属于高风险区域，必须保留人工 review 与回滚方案。
- 大批量回填若执行不当，可能引发锁等待、复制延迟或查询抖动。
- 若新旧字段映射不一致，可能导致报表口径漂移或对账异常。

## Missing Information
- 历史数据中无法自动解析的比例阈值仍需业务和平台共同确认。
- 切换读路径时是否允许保留少量人工豁免订单仍需确认。
- 旧 JSON 字段的最终清理窗口是否与本次迁移解耦仍需确认。

## Need Human Decision
- 历史回填阶段是否必须安排独立低峰窗口执行？
- 若存在少量无法自动映射的订单，是否允许先带豁免切读，再单独补修？

## Next Handoff
- To: PRD Agent
- Goal: 将该迁移需求转换为可验收的迁移范围、成功标准与发布要求
- Must Read:
  - Initial Scope
  - Risks Seen At Intake
  - Missing Information
