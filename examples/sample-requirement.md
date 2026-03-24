# Sample Requirement Card

## Meta
- Requirement ID: REQ-2026-001
- Title: Existing User Magic Link Login
- Type: feature
- Priority: P1
- Source: Customer support + product request
- Requested By: Growth Team
- Intake Owner: Intake Agent

## Summary
为现有注册用户增加邮箱 magic link 登录方式，减少忘记密码导致的登录失败和支持工单，同时不替代当前密码登录。

## Confirmed Facts
- 当前产品已支持邮箱密码登录。
- 邮件发送基础设施已经存在，可用于发送登录链接邮件。
- 支持团队反馈密码重置相关工单持续偏高。

## Business Context
- Problem: 一部分回访用户因忘记密码无法快速登录，导致转化流失和支持成本上升。
- Why Now: 当前季度目标要求降低登录摩擦并提高回访用户成功登录率。
- Expected Value: 降低密码重置请求、提升登录成功率、缩短用户恢复会话时间。

## Initial Scope
### In Scope
- Web 端为现有用户提供“邮箱验证码/魔法链接登录”入口。
- 发送一次性登录链接到用户邮箱。
- 用户点击链接后完成登录并跳转回原目标页面。
- 链接具备有效期、单次使用限制和基础频率限制。

### Out of Scope
- 新用户注册流程改造。
- 手机短信登录。
- 原密码登录流程下线。
- 企业 SSO / OAuth 接入。

## Risks Seen At Intake
- 认证链路属于高风险模块，必须保留人工 review。
- 邮件送达与延迟会影响登录成功率。
- 若链接可重复使用，会带来安全风险。

## Missing Information
- magic link 有效期需要由产品和安全共同确认。
- 是否只对已验证邮箱用户开放仍需确认。
- 是否首发仅对部分流量放量仍需确认。

## Need Human Decision
- 首发是否仅对已验证邮箱用户开放？
- 首次发布是否走 feature flag + canary？

## Next Handoff
- To: PRD Agent
- Goal: 将该需求转换为可验收的 PRD
- Must Read:
  - Initial Scope
  - Risks Seen At Intake
  - Missing Information
