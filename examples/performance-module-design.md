# Technical Design

## Meta
- Design ID: DES-PERF-2026-001
- Requirement ID: REQ-PERF-2026-001
- Title: Moka-Style Performance Management Module
- Author: Architect Agent
- Reviewers: Java Tech Lead, Security Reviewer, HR Domain Reviewer
- Target Release: 2026-Q2 Internal Pilot

## Summary
在 Spring Boot / Spring Cloud 微服务体系中增加一套 Moka 风格绩效管理模块。首轮采用 `performance-service` 承载绩效周期、模板、绩效计划与评分流程，`organization-service` 提供组织与员工主数据，`gateway` 提供统一入口，配置通过 Nacos 管理，业务数据落 MySQL。交付重点是保证状态流转清晰、权限边界明确、批量生成可控，并为后续测试计划和发布准备提供稳定边界。

## Confirmed Inputs
- PRD Version: performance-module-prd.md v1
- Related Modules:
  - gateway
  - organization-service
  - performance-service
- Constraints:
  - 首轮只做绩效管理最小闭环，不扩展到薪酬与校准场景
  - 敏感人事数据必须保留人工 review 与审计要求

## Assumptions
- `organization-service` 已能提供部门、员工、直属主管与在职状态等主数据查询接口。
- 首轮以内网 Web 管理台为主要调用方，不额外设计移动端专属接口。

## Technical Goal
- 在不引入复杂流程引擎的前提下，完成绩效周期、模板、绩效计划、自评与主管评分的可执行服务设计。
- 通过清晰的状态机、权限校验和审计记录控制绩效数据变更风险。

## Non-Goals
- 实现复杂的多级审批、校准会和强制分布算法。
- 建设完整 BI 报表、消息通知中心或外部薪酬联动能力。

## Current State
- Current Flow: 当前仅有练手项目构想，没有现成的绩效域实现；starter 侧已有文档模板、质量门禁与 pilot 参考链路。
- Current Limitation:
  - 缺少贴近 Java 微服务企业内部系统的真实设计样例。
  - 绩效流程涉及多角色、多状态与敏感数据，若没有明确边界，后续 handoff 容易失真。

## Proposed Approach
### Overview
采用领域边界清晰的微服务设计：`performance-service` 负责绩效域核心对象与状态机；`organization-service` 提供只读组织主数据；`gateway` 统一鉴权与路由。绩效域以 MySQL 持久化周期、模板、计划与评分记录，通过同步接口完成主路径，批量生成可预留异步任务扩展点。所有关键状态变更写入审计字段和 review record，首轮发布通过环境隔离与人工检查控制风险。

### Affected Modules
- gateway
- organization-service
- performance-service

### Step-by-Step Design
1. HR 管理员经 gateway 调用 `performance-service` 创建绩效周期与模板；模板在发布前执行结构校验，如权重、指标完整性和评分方式合法性。
2. `performance-service` 在生成绩效计划时调用 `organization-service` 拉取目标员工、部门与直属主管信息，并写入 `perf_plan` 与 `perf_plan_item`。
3. 员工自评与主管评分均通过 `performance-service` 状态机推进；服务在每次提交时校验当前状态、操作者角色与目标数据归属，并将结果写入 `perf_review_record` 供审计与回溯。

## API / Contract Changes
### New
- Name: POST /api/performance/cycles
- Caller: HR admin portal via gateway
- Input: cycle_name, review_window, template_id, target_scope
- Output: cycle_id, status=draft
- Errors: validation_failed, organization_scope_invalid

### Modified
- Name: GET /api/org/employees
- Change: 增加按部门范围与主管关系查询的筛选能力，供绩效计划生成使用
- Compatibility: backward compatible

### Unchanged but Relevant
- employee / department query APIs from organization-service

## Data Changes
### Schema / Model
- Add: `perf_cycle`, `perf_template`, `perf_template_item`, `perf_plan`, `perf_plan_item`, `perf_review_record`
- Modify: `employee`、`department` 仅作为已有主数据读取模型，不在本需求内改表
- Remove: none

### Migration
- Needed: yes
- Plan: 在 `performance-service` 独立 schema 中新增绩效域业务表，首轮 migration 仅包含建表与必要索引
- Reversible: yes

## Security / Permission Impact
- Auth Impact: 需根据登录用户角色区分 HR 管理员、员工和直属主管的读写范围。
- Permission Impact: 同一绩效计划仅允许对应员工编辑自评，仅允许指定主管提交评分，HR 管理员拥有配置与运营权限。
- Sensitive Data Impact: 绩效结果、评语与评分记录属于敏感人事数据，日志中不得输出完整业务内容，查询接口需避免越权透出。

## Reliability / Performance Impact
- Latency Risk: 绩效计划生成依赖组织服务查询，若一次性拉取范围过大可能导致同步请求变慢。
- Throughput Risk: 周期发布或集中提交自评时，`performance-service` 写请求会短时间升高。
- Dependency Risk: `organization-service` 主数据不一致会直接影响主管分配与计划生成准确性。

## Compatibility
- Backward Compatibility: yes
- Forward Compatibility: partial
- Feature Flag Needed: yes
- Rollout Guard: `perf_module_enabled` + internal pilot organization whitelist

## Rollback Plan
- Rollback Trigger:
  - 绩效计划生成错误率持续高于阈值
  - 权限校验异常导致越权读写或关键状态错乱
- Rollback Steps:
  1. 关闭 `perf_module_enabled` 开关并停止新的周期发布。
  2. 将入口从 gateway 隐藏，仅保留已有非绩效业务路径。
  3. 保留数据表和审计记录，人工确认问题范围后再决定是否修复后重试。
- Non-Reversible Risk: 已产生的业务记录不应直接删除，需要保留审计痕迹。

## Alternatives Considered
### Option A
- Description: 将绩效周期、模板、计划与组织主数据全部做成单体服务
- Pros:
  - 首轮实现路径更短，跨服务调用更少
- Cons:
  - 不利于体现 Spring Cloud + Nacos 场景下的 adopted-repo 演练价值
- Rejected Because: 本次练手项目明确希望覆盖微服务边界与上游依赖交互。

### Option B
- Description: 引入通用工作流引擎统一处理自评、评分与审批状态
- Pros:
  - 更适合后续扩展复杂审批链路
- Cons:
  - 首轮范围明显过大，并会模糊 starter 对最小闭环的验证重点
- Rejected Because: 首轮只需要清晰、可验证的有限状态流转，不需要额外引入流程引擎复杂度。

## Tasking Guidance
- Backend Tasks:
  - 实现周期、模板、绩效计划、自评与评分核心接口及状态校验
- Frontend Tasks:
  - 提供 HR 配置页、员工自评页与主管评分页的最小交互原型
- QA Tasks:
  - 覆盖状态流转、越权访问、组织数据异常与批量生成失败场景
- Release Tasks:
  - 配置 Nacos 开关、验证 organization-service 依赖、检查审计与回滚入口

## Risks
- 主管关系、组织范围或人员状态数据不准确会导致计划生成错误指派。
- 状态机设计不完整会让自评、评分或关闭周期出现不可恢复的中间态。
- 绩效敏感数据若日志脱敏或权限校验不足，会带来明显合规风险。

## Need Human Decision
- 首轮是否要求批量生成绩效计划采用异步任务模式，还是允许先用同步接口实现？
- 是否将 HRBP 视为独立角色纳入首轮权限模型，还是先只保留 HR 管理员 / 员工 / 主管三类角色？

## Next Handoff
- To: Planning Agent
- Goal: 拆解成可并行执行任务
- Must Read:
  - Affected Modules
  - API / Contract Changes
  - Data Changes
  - Rollback Plan
