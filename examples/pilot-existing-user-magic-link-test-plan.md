# Test Plan

## Meta
- Test Plan ID: TP-PILOT-2026-001
- Requirement ID: REQ-PILOT-2026-001
- Design ID: DES-PILOT-2026-001
- Owner: QA/Security Agent
- Target Release: 2026-04 Wave 1
- Environment: staging + canary

## Summary
本测试计划聚焦 magic link 登录功能的正确性、安全性和发布可观测性，重点验证单次使用、过期控制、不暴露账户存在性、redirect 安全约束，以及 feature flag + canary 发布路径。

## Confirmed Inputs
- Acceptance Criteria:
  - 已验证邮箱的现有用户可请求并使用 magic link 登录。
  - magic link 单次有效且过期后不可用。
- Affected Modules:
  - web-login-ui
  - auth-service
  - notification-service
- Known Risks:
  - 认证链路安全风险。
  - 邮件依赖导致的可用性风险。

## Assumptions
- staging 环境可访问测试邮箱。
- feature flag 可单独控制 magic link 入口展示。

## Test Objectives
- 验证 magic link 主流程成功率和正确跳转行为。
- 验证过期、重放、伪造、频控和 redirect 非法场景。
- 验证发布前后的关键监控指标和回滚条件。

## In Scope
- request / verify 接口。
- 登录页新入口与成功/失败提示。
- feature flag、审计日志、监控项。

## Out of Scope
- 原生 App 适配。
- 企业 SSO / OAuth 流程。

## Risk Focus
- High-Risk Path:
  - auth-service token verify path
  - redirect target validation path
- High-Risk Dependency:
  - transaction email delivery service
- Security Focus:
  - token single-use、expiration、replay prevention、account enumeration protection

## Test Strategy
### Unit
- Target:
  - token generation、expiration check、single-use state transition、redirect whitelist validator
- Pass Condition:
  - 安全相关核心函数分支覆盖到成功、过期、重复使用、非法跳转目标场景

### Integration
- Target:
  - request API + token store + email send mock
- Pass Condition:
  - request 成功写入短期存储，verify 成功后令牌失效，重复 verify 被拒绝

### E2E / Critical Path
- Target:
  - 用户从登录页请求 magic link 到完成登录并跳转回目标页
- Pass Condition:
  - 已验证测试用户可完成登录；未验证用户或非法 token 无法完成登录

### Regression
- Must Recheck:
  - 原密码登录流程
  - 忘记密码流程

### Smoke
- Pre-Release Smoke:
  - feature flag 打开后登录页正确展示入口，request/verify 基础链路可用
- Post-Release Smoke:
  - canary 用户可正常请求并完成登录，仪表盘指标更新正常

## Test Cases
| ID | Title | Level | Preconditions | Steps | Expected Result | Priority |
|---|---|---|---|---|---|---|
| TC-001 | Existing verified user logs in with magic link | E2E | 已验证测试账号、flag 打开 | 请求链接，打开邮件，点击链接 | 登录成功并跳转目标页 | P0 |
| TC-002 | Expired magic link is rejected | Integration | 有已过期 token | 调用 verify 接口 | 登录失败并提示重新请求 | P0 |
| TC-003 | Used magic link cannot be reused | Integration | token 已成功使用一次 | 再次访问同一链接 | 登录失败，审计中记录重复使用 | P1 |
| TC-004 | Invalid redirect target falls back safely | Integration | redirect target 非白名单 | 请求链接并验证 | 登录成功但仅跳转到安全默认页 | P1 |
| TC-005 | Unknown email gets generic response | E2E | 不存在邮箱 | 发起请求 | 页面提示一致，不暴露账户存在性 | P0 |

## Edge / Failure Testing
- Boundary Case:
  - token 在过期边界前后访问的行为一致且可解释
- Invalid Input:
  - 篡改 token、非法 redirect target、空 email 请求
- Timeout / Retry:
  - 邮件服务超时时 request 接口返回统一可恢复结果
- Idempotency / Concurrency:
  - 同一链接被两次快速点击时仅一次成功

## Security Checks
- AuthZ Check: yes - 登录后权限模型沿用现有 session 角色，不应出现权限提升
- Sensitive Data Check: yes - 邮件和日志中不输出完整 token
- Input Validation Check: yes - email 与 redirect target 均需校验
- Dependency Scan Needed: no

## Test Data / Setup
- Test Account: verified_user@example.test / unverified_user@example.test
- Seed Data: 已存在账号、可访问测试邮箱、预置 redirect 白名单
- Feature Flag: `auth_magic_link_login`
- Mock / Stub: staging 中允许 mock email callback 或测试 inbox

## Release Gate
### Must Pass
- [ ] P0 cases pass
- [ ] Critical path pass
- [ ] No blocker defect
- [ ] No high-risk security issue
- [ ] Rollback plan exists
- [ ] Monitoring items defined

### Block Release If
- verify 错误率高于阈值或重复使用校验失效
- redirect target 校验存在绕过或账户枚举风险

## Open Defects
| Bug ID | Severity | Summary | Status | Blocks Release |
|---|---|---|---|---|
| none | none | none | none | No |

## Recommendation
- Result: Conditional Pass
- Notes:
  - 允许进入 release prep，但必须以 feature flag + canary 方式上线
  - 上线前需再次确认 redirect target 白名单策略和告警阈值

## Need Human Decision
- canary 期间是否只对内部员工和 5% 外部流量开放？
- 发现邮件延迟升高但登录成功率正常时，是否继续放量？

## Next Handoff
- To: Release/SRE Agent
- Goal: 准备发布与灰度策略
- Must Read:
  - Risk Focus
  - Release Gate
  - Open Defects
  - Recommendation
