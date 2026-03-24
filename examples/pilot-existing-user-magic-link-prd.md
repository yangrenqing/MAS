# PRD

## Meta
- Requirement ID: REQ-PILOT-2026-001
- Title: Existing User Magic Link Login
- Type: feature
- Priority: P1
- Requested By: Growth Team
- Product Owner: Alice Chen
- Tech Owner: Bob Wang
- Target Release: 2026-04 Wave 1
- Due Date: 2026-04-15

## Summary
为现有注册用户增加邮箱 magic link 登录能力，作为密码登录的补充入口，降低忘记密码造成的登录失败与支持工单，同时保持发布过程可灰度、可监控、可回滚。

## Confirmed Facts
- 当前 Web 端已经支持邮箱密码登录。
- 系统已具备发送事务邮件的能力。
- 支持团队持续收到密码重置相关工单。

## Assumptions
- 首发仅面向已验证邮箱的现有用户。
- 首次上线通过 feature flag 控制，并先进行小流量 canary。

## Business Context
- Problem: 回访用户忘记密码后登录失败率高，影响回流与转化。
- Why Now: 团队当前季度目标之一是降低登录摩擦并减少支持成本。
- If Not Done: 密码重置工单和用户流失会继续维持高位。

## Goals
- Business Goal:
  - 降低密码重置相关支持工单。
  - 提升回访用户登录成功率。
- User Goal:
  - 用户无需记住密码即可安全登录。
  - 用户点击邮件链接后能快速回到原目标页面。

## Non-Goals
- 替换现有密码登录流程。
- 支持短信验证码或第三方 OAuth 登录。

## Target Users
- Primary User: 已注册且邮箱已验证的回访 Web 用户
- Secondary User: 支持团队和运营团队

## User Stories
1. As an existing user, I want to receive a one-time login link by email, so that I can sign in without resetting my password.
2. As a returning user, I want to land on my original destination after login, so that I can continue the action I intended to take.

## Scope
### In Scope
- 登录页提供“Email me a magic link”入口。
- 用户输入邮箱后可请求一次性登录链接。
- 用户点击链接后完成登录并跳转回原目标页面。
- 对 magic link 请求和验证增加基础风控、监控与审计。

### Out of Scope
- 新用户注册与邮箱验证流程。
- 原生 App 登录支持。
- 企业级 SSO 集成。

## Main Flow
1. 用户在登录页选择 magic link 登录。
2. 用户输入邮箱并提交请求。
3. 系统校验用户条件并发送一次性登录链接。
4. 用户点击链接后完成登录并跳转至原目标页或默认首页。

## Edge Cases
- 邮箱未注册或未验证时，不暴露账户存在性。
- 链接已过期、已使用或被篡改时，返回统一失败页并提示重新请求。
- 用户在短时间内频繁请求链接时，触发 rate limit。
- redirect target 非法时，回退到安全默认页而不是透传跳转。

## Acceptance Criteria
- [ ] 已验证邮箱的现有用户可在 Web 登录页请求 magic link。
- [ ] magic link 单次有效，使用后立即失效。
- [ ] magic link 过期、重复使用或非法访问时不会登录成功。
- [ ] 功能可通过 feature flag 控制并支持灰度发布。

## Failure Conditions
- 未授权用户可通过链接直接登录。
- 链接可重复使用、在过期后仍可生效，或允许非法 redirect 跳转。

## Dependencies
- Upstream Dependency: 事务邮件服务正常可用。
- External System: Web 风控 / rate limit 组件。
- Team Dependency: 安全团队确认有效期、redirect 白名单与审计要求。

## Constraints
- Time Constraint: 需在 2026-04 Wave 1 前完成首发。
- Compliance Constraint: 登录邮件内容不可包含敏感个人信息。
- Security Constraint: 不得泄露账户是否存在，且需要单次令牌约束与 redirect 校验。
- Performance Constraint: 发送登录邮件的接口 P95 不应显著高于现有密码重置接口。

## Risks
- 邮件延迟可能影响用户感知和成功率。
- 认证链路改动若设计不当会带来账户接管风险。

## Unknowns
- 有效期最终定为 10 分钟还是 15 分钟仍待确认。
- 首次 canary 流量范围由 5% 还是 10% 起步仍待确认。

## Need Human Decision
- magic link 有效期最终采用 10 分钟还是 15 分钟？
- 首次发布的 canary 起始流量范围是多少？

## Next Handoff
- To: Architect Agent
- Goal: 将 PRD 转换为最小可执行技术方案
- Must Read:
  - Acceptance Criteria
  - Edge Cases
  - Risks
  - Dependencies
