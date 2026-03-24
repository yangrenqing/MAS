# Test Plan

## Meta
- Test Plan ID: TP-PERF-2026-001
- Requirement ID: REQ-PERF-2026-001
- Design ID: DES-PERF-2026-001
- Owner: QA/Security Agent
- Target Release: 2026-Q2 Internal Pilot
- Environment: dev + staging

## Summary
本测试计划聚焦 Moka 风格绩效模块首轮闭环的正确性、权限边界、状态流转和发布前可验证性。重点覆盖绩效周期、模板、绩效计划、自评、主管评分，以及组织主数据依赖异常下的失败路径，确保该样例既能支撑真实项目风格演练，也不会把高风险人事数据与复杂审批场景无约束地放大。

## Confirmed Inputs
- Acceptance Criteria:
  - HR 管理员可创建周期、模板并生成绩效计划
  - 员工与直属主管可在正确状态下完成自评与评分
- Affected Modules:
  - performance-service
  - organization-service
  - gateway
- Known Risks:
  - 敏感绩效数据越权访问风险
  - 状态流转或组织主数据异常导致的绩效计划错乱

## Assumptions
- staging 环境中可准备测试组织、员工、主管与模板数据。
- `perf_module_enabled` 可控制入口暴露范围并支持仅内部试跑组织访问。

## Test Objectives
- 验证绩效周期、模板、绩效计划、自评和主管评分主流程的正确性。
- 验证角色权限、状态流转与审计记录在核心节点上可追溯且不可绕过。
- 验证组织主数据缺失、模板非法和重复提交等失败路径是否可恢复、可解释。

## In Scope
- 周期、模板与绩效计划核心接口
- 员工自评与主管评分状态流转
- 角色权限、审计字段、组织数据依赖异常处理

## Out of Scope
- 薪酬联动、绩效校准会与 360 评估
- 移动端与外部 IM 通知集成

## Risk Focus
- High-Risk Path:
  - performance plan generation with organization dependency
  - self-review / manager-review state transition and permission checks
- High-Risk Dependency:
  - organization-service employee / manager relationship data
- Security Focus:
  - role-based access, cross-employee data isolation, audit completeness

## Test Strategy
### Unit
- Target:
  - 模板权重校验、周期状态机、绩效计划状态机、权限判断与审计字段填充
- Pass Condition:
  - 核心状态机与权限相关逻辑覆盖到成功、非法状态、越权与重复提交场景

### Integration
- Target:
  - performance-service 与 organization-service 的主数据读取、绩效计划生成、自评与主管评分接口
- Pass Condition:
  - 在组织数据完整时可稳定生成绩效计划；在主管关系缺失、员工状态异常等场景下返回可解释失败结果

### E2E / Critical Path
- Target:
  - HR 创建周期和模板 → 生成绩效计划 → 员工提交自评 → 主管提交评分
- Pass Condition:
  - 同一绩效单在完整链路中按预期推进状态，且各角色仅能操作各自允许步骤

### Regression
- Must Recheck:
  - 周期关闭后只读限制
  - 模板发布前后的编辑约束

### Smoke
- Pre-Release Smoke:
  - HR 管理员可创建周期并生成至少一组绩效计划
- Post-Release Smoke:
  - 白名单内部组织下，员工与主管各完成一条最小评分链路，审计日志可见

## Test Cases
| ID | Title | Level | Preconditions | Steps | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-PERF-001 | HR creates cycle and publishes valid template | Integration | HR 账号、组织范围有效 | 创建周期，配置模板指标及权重，提交发布 | 周期为 draft，模板校验通过并可用于生成计划 | P0 |
| TC-PERF-002 | System generates plans for target employees | Integration | 周期和模板已就绪，组织数据完整 | 触发计划生成 | 为目标员工生成绩效计划并绑定直属主管 | P0 |
| TC-PERF-003 | Employee submits self-review in correct state | E2E | 绩效计划处于 self_review_open | 员工填写并提交自评 | 自评成功，状态推进到 manager_review_pending | P0 |
| TC-PERF-004 | Manager cannot score before employee submission | Integration | 绩效计划仍在 draft 或 self_review_open | 主管直接提交评分 | 请求被拒绝并返回非法状态错误 | P1 |
| TC-PERF-005 | Unauthorized user cannot read another employee plan | Integration | 非所属主管/非 HR 账号 | 查询他人绩效计划 | 返回无权限结果，不泄露敏感详情 | P0 |
| TC-PERF-006 | Plan generation fails clearly when manager mapping is missing | Integration | 组织数据缺少直属主管 | 触发批量生成 | 失败结果可解释，未写入半成品计划 | P1 |

## Edge / Failure Testing
- Boundary Case:
  - 模板指标权重合计为 99%、100%、101% 时的发布行为
- Invalid Input:
  - 空模板项、非法评分范围、无效员工 ID、超长评语
- Timeout / Retry:
  - organization-service 查询超时或瞬时失败时，绩效计划生成结果是否可恢复
- Idempotency / Concurrency:
  - 同一绩效计划被重复提交自评或主管评分时，仅允许一次成功状态推进

## Security Checks
- AuthZ Check: yes - HR、员工、主管三类角色的访问边界必须明确验证
- Sensitive Data Check: yes - 绩效评语、评分与状态流转记录不得通过越权接口泄露
- Input Validation Check: yes - 模板配置、评语长度、评分区间与员工范围都需校验
- Dependency Scan Needed: no

## Test Data / Setup
- Test Account: hr_admin_01 / employee_01 / manager_01 / unauthorized_user_01
- Seed Data: 一个部门、两名员工、一名直属主管、一个有效模板、一个无效模板、若干组织异常数据样本
- Feature Flag: `perf_module_enabled`
- Mock / Stub: 可对 organization-service 提供缺失主管、员工离职、查询超时等 stub 场景

## Release Gate
### Must Pass
- [ ] P0 cases pass
- [ ] Critical path pass
- [ ] No blocker defect
- [ ] No high-risk security issue
- [ ] Rollback plan exists
- [ ] Monitoring items defined

### Block Release If
- 角色权限存在越权读取或越权写入漏洞
- 状态流转存在绕过、重复提交成功或半成品计划落库问题

## Open Defects
| Bug ID | Severity | Summary | Status | Blocks Release |
|---|---|---|---|---|
| none | none | none | none | No |

## Recommendation
- Result: Conditional Pass
- Notes:
  - 允许进入 release prep，但仅限 internal pilot / whitelist 组织范围
  - 若组织主数据稳定性或权限校验仍有未决问题，应阻断更大范围试跑

## Need Human Decision
- 首轮 internal pilot 是否仅选择单个部门作为白名单范围？
- 批量生成绩效计划若耗时偏高，是否允许先以手动触发和小规模组织试跑？

## Next Handoff
- To: Release/SRE Agent
- Goal: 准备发布与灰度策略
- Must Read:
  - Risk Focus
  - Release Gate
  - Open Defects
  - Recommendation
