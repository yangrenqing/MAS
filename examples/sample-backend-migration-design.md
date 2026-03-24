# Sample Backend Migration Design

## Meta
- Design ID: DES-2026-003
- Requirement ID: REQ-2026-003
- Title: Migrate Legacy Order Billing JSON to Structured Columns
- Author: Architect Agent
- Reviewers: Tech Lead, DBA, Security Reviewer
- Target Release: 2026-05 Wave 1

## Summary
采用 expand-migrate-switch 的分阶段方案：先对 `orders` 表做可回滚的加列 migration，再在订单写入路径开启双写，随后用分批、可恢复的 backfill 任务补齐历史订单，验证新旧字段一致性后再通过 feature flag 切换读路径。旧 JSON 字段保留到后续 contract 阶段单独清理。

## Confirmed Inputs
- PRD Version: sample-backend-migration-prd.md v1
- Related Modules:
  - order-service
  - billing-reporting-job
  - db migration runner
- Constraints:
  - 不允许一次性硬切读写
  - 首期不删除旧 JSON 字段

## Assumptions
- `orders` 表支持在线加 nullable 列和必要索引。
- 现有订单写入服务允许在过渡期双写新旧字段。

## Technical Goal
- 在不影响订单主链路的前提下提供结构化账单字段。
- 让历史回填、读路径切换和异常回退都具备显式控制面。

## Non-Goals
- 重写订单领域模型。
- 在本阶段移除所有旧字段兼容逻辑。

## Current State
- Current Flow: 订单账单字段部分存于 `legacy_billing_payload` JSON，报表与下游读取时再做解析。
- Current Limitation:
  - JSON 结构不稳定，查询与聚合成本高。
  - 历史兼容逻辑分散在多个读取方，容易口径不一致。

## Proposed Approach
### Overview
通过数据库扩容列承接稳定字段，写路径在过渡期双写 JSON 与结构化列；历史回填任务按照主键范围分批处理，每批输出成功/失败/跳过统计；验证期对比新旧字段结果与报表口径；最终在 feature flag 控制下将读路径切换到结构化列，并保留快速回退到 JSON 读取的能力。

### Affected Modules
- order-service
- billing-reporting-job
- analytics export job
- db migration runner

### Step-by-Step Design
1. 执行 additive migration：为 `orders` 增加 `billing_country_code`、`billing_currency`、`tax_region` 等 nullable 列，并补充必要索引。
2. 订单写入路径开启双写：新订单创建或更新时，同时写入 JSON 与结构化列，并记录双写差异指标。
3. backfill worker 按主键分片扫描历史订单，解析 JSON 后写入结构化列；对无法映射的记录仅记录失败原因，不写入猜测值。
4. reporting / downstream job 在验证阶段支持双读比对，将新旧口径差异打点到仪表盘。
5. 通过 `orders_structured_billing_read` flag 控制读路径切换；先对内部查询和少量消费者开启，再逐步扩大。
6. 等稳定后再单独安排 contract 阶段，移除旧 JSON 读取和字段。

## API / Contract Changes
### New
- Name: internal backfill progress endpoint / metrics
- Caller: platform operators
- Input: batch range, dry_run flag
- Output: processed_count, success_count, failed_count
- Errors: invalid_range, worker_unavailable

### Modified
- Name: order write path
- Change: 内部写入逻辑新增结构化列双写
- Compatibility: backward compatible

### Unchanged but Relevant
- existing order read APIs
- reporting export contracts

## Data Changes
### Schema / Model
- Add:
  - `orders.billing_country_code`
  - `orders.billing_currency`
  - `orders.tax_region`
- Modify:
  - order persistence layer writes both legacy JSON and structured columns during migration window
- Remove: none in this phase

### Migration
- Needed: yes
- Plan:
  1. additive schema change
  2. dual-write enablement
  3. historical backfill
  4. read switch by flag
  5. later contract cleanup
- Reversible: partial

## Security / Permission Impact
- Auth Impact: no direct auth change, but order / billing data remains sensitive and must preserve existing access controls.
- Permission Impact: 读取与导出权限不变，不能因新列暴露更宽范围的数据。
- Sensitive Data Impact: backfill 日志和告警仅记录 order_id、失败原因和统计信息，不输出完整账单原文。

## Reliability / Performance Impact
- Latency Risk: 双写会增加订单写路径数据库写入量。
- Throughput Risk: 大批量 backfill 可能影响数据库 CPU、IO 和复制延迟。
- Dependency Risk: reporting job 若未同步支持新字段验证，切读判断会失真。

## Compatibility
- Backward Compatibility: yes
- Forward Compatibility: partial
- Feature Flag Needed: yes
- Rollout Guard: `orders_structured_billing_read` + backfill throttle control

## Rollback Plan
- Rollback Trigger:
  - 切读后查询结果或报表差异持续超阈值
  - 数据库压力异常影响订单主链路
- Rollback Steps:
  1. 关闭 `orders_structured_billing_read`，恢复从旧 JSON 读取。
  2. 暂停 backfill worker 或将并发降到 0。
  3. 保持双写，待问题确认后重新验证再切读。
- Non-Reversible Risk: additive schema change 保留在库内，但不影响回退到旧读路径。

## Alternatives Considered
### Option A
- Description: 一次性执行 SQL，将所有历史 JSON 直接转换后立刻切读
- Pros:
  - 总体周期短
- Cons:
  - 风险集中、回退困难、难以观察数据差异
- Rejected Because: 订单与 billing 路径风险高，不适合一次性硬切。

### Option B
- Description: 继续保持 JSON 存储，只在查询层封装统一解析
- Pros:
  - 无需 migration 和 backfill
- Cons:
  - 无法解决查询性能、字段治理和长期维护问题
- Rejected Because: 只能延后问题，不能为后续需求提供稳定数据基础。

## Tasking Guidance
- Backend Tasks:
  - additive migration、双写改造、backfill worker、差异指标
- Data Tasks:
  - 映射规则确认、验证查询、失败样本分类
- QA Tasks:
  - 双写一致性、回填恢复、切读回退、性能观察
- Release Tasks:
  - 低峰窗口执行计划、阈值观察、人工批准切读

## Risks
- 部分历史订单 JSON 结构异常，导致无法完全自动回填。
- backfill throttle 失控可能带来数据库压力抖动。
- contract 阶段若拖延过久，会放大双写维护成本。

## Need Human Decision
- 切读前要求的结构化字段覆盖率阈值是多少？
- 无法自动映射的订单是否允许保留旧读路径豁免名单？

## Next Handoff
- To: Planning Agent
- Goal: 拆解成 migration、backfill、validation、release 四类可执行任务
- Must Read:
  - Step-by-Step Design
  - Data Changes
  - Rollback Plan
  - Risks
