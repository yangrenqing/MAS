# PRD

## Meta
- Requirement ID: REQ-PERF-2026-001
- Title: Moka-Style Performance Management Module
- Type: feature
- Priority: P1
- Requested By: HR Systems Team
- Product Owner: HR Product Manager
- Tech Owner: Java Platform Lead
- Target Release: 2026-Q2 Internal Pilot
- Due Date: TBD

## Summary
为内部 HR 系统增加一套 Moka 风格绩效管理模块，覆盖绩效周期管理、绩效模板管理、绩效计划生成、员工自评与主管评分等最小闭环能力。首轮交付以学习与演练为目标，要求结构足够接近真实企业内部系统，同时保持范围可控、发布可灰度、权限边界清晰、文档链路完整。

## Confirmed Facts
- 项目技术栈已明确为 JDK 17、Spring Boot、Spring Cloud、Nacos 与 MySQL。
- 首轮样例聚焦绩效管理最小闭环，不追求完整 HR 套件能力。
- starter 需要通过该项目继续验证真实项目风格需求链路是否足够可复用。

## Assumptions
- 组织、员工与汇报关系由独立组织域服务提供，绩效域以读取这些主数据为主。
- 首发通过环境隔离、人工审批点与最小流量验证方式控制风险，不做任何自动生产发布。

## Business Context
- Problem: 仅靠通用示例不足以验证 starter 在 Java 微服务和内部业务系统场景下的 handoff、边界和质量门禁是否清晰。
- Why Now: 当前需要一个比通用 feature demo 更真实、又比生产项目更可控的练手题目来继续演练 adoption。
- If Not Done: 团队仍然缺少一条贴近真实内部系统的端到端参考链路，后续 adopted repo 落地时会增加试错成本。

## Goals
- Business Goal:
  - 为内部绩效流程提供可演示的最小业务闭环。
  - 为 starter 提供一套贴近 Java 微服务场景的真实项目风格参考。
- User Goal:
  - HR 管理员能够配置绩效周期与模板并生成绩效计划。
  - 员工与主管能够在明确状态下完成自评和评分。

## Non-Goals
- 打通薪酬、晋升、人才盘点或 360 评估等扩展模块。
- 实现复杂审批流引擎、BI 报表平台或移动端能力。

## Target Users
- Primary User: HR 管理员、员工、直属主管
- Secondary User: HRBP、平台运维与测试人员

## User Stories
1. As an HR admin, I want to configure a performance cycle and template, so that I can launch a controlled review round for a target organization.
2. As an employee, I want to complete my self-review against assigned indicators, so that my manager can review the same performance plan in a consistent workflow.

## Scope
### In Scope
- 绩效周期创建、发布、关闭与状态查看。
- 绩效模板与模板指标项配置，包括名称、权重、评分方式与说明。
- 基于周期与组织范围批量生成绩效计划，并支持员工自评、主管评分与状态流转。
- 基础权限校验、审计字段、操作日志与关键状态变更记录。

### Out of Scope
- 校准会、强制分布、多人会签与跨级审批。
- 薪酬结果联动、绩效等级分布分析与报表中心。
- 外部 SSO、移动端 App 与 IM 通知集成。

## Main Flow
1. HR 管理员创建绩效周期并配置模板与指标项。
2. 系统根据周期、组织范围与人员信息生成绩效计划。
3. 员工在开放窗口内提交自评内容。
4. 直属主管完成评分并提交结果，周期进入后续汇总或关闭阶段。

## Edge Cases
- 模板权重合计不为 100% 或指标项缺失时，不允许模板发布。
- 周期已关闭或绩效计划已提交后，不允许继续编辑对应自评或主管评分。
- 员工汇报关系缺失、组织数据不完整或人员状态异常时，批量生成绩效计划需要给出明确失败结果。

## Acceptance Criteria
- [ ] HR 管理员可创建绩效周期，并在未发布前编辑模板、组织范围与时间窗口。
- [ ] 模板必须包含有效指标项和合法权重，才能用于生成绩效计划。
- [ ] 系统可为指定周期和目标员工批量生成绩效计划，并记录生成结果。
- [ ] 员工可在规定状态下提交自评，直属主管可在后续状态下完成评分。
- [ ] 关键状态流转、权限限制与审计记录在测试计划中可验证，并支持发布前检查。

## Failure Conditions
- 未授权用户可查看或修改非本人/非本团队的绩效计划。
- 周期、模板或绩效计划状态流转不受约束，导致重复提交、越权修改或数据错乱。

## Dependencies
- Upstream Dependency: 组织与员工主数据服务可提供部门、员工与汇报关系。
- External System: Nacos 提供配置管理与服务注册发现；MySQL 提供业务存储。
- Team Dependency: HR 业务方确认评分模型、周期规则与首轮角色边界。

## Constraints
- Time Constraint: 首轮仅做 internal pilot 级别样例，不扩展到完整绩效体系。
- Compliance Constraint: 绩效记录属于敏感人事数据，访问与审计要求必须明确。
- Security Constraint: 必须保证角色访问边界、状态校验与审计可追溯。
- Performance Constraint: 批量生成绩效计划时应支持中等规模组织的可接受执行时间，避免单次请求无限阻塞。

## Risks
- 组织主数据不稳定会直接影响绩效计划生成与主管分配准确性。
- 评分模型和审批链路若过早做复杂，会显著扩大样例范围并削弱首轮可落地性。

## Unknowns
- 首轮是否要求支持草稿保存与多次编辑，还是仅支持一次提交，仍待确认。
- 批量生成绩效计划是否需要异步任务化处理，仍待根据目标规模确认。

## Need Human Decision
- 首轮评分模型采用五分制、百分制，还是允许模板级自定义评分规则？
- 首轮是否仅支持一级主管评分，暂不支持二级复核与校准？

## Next Handoff
- To: Architect Agent
- Goal: 将 PRD 转换为最小可执行技术方案
- Must Read:
  - Acceptance Criteria
  - Edge Cases
  - Risks
  - Dependencies
