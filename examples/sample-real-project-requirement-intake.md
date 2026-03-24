# Sample Real-Project Requirement Intake Card

## Meta
- Requirement ID: REQ-AUTH-2026-014
- Title: Expired Password Reset Link Shows 500 Error
- Type: bugfix
- Priority: P1
- Source: support_ticket
- Requested By: Support Team
- Intake Owner: Intake Agent

## Summary
将“过期或已使用的密码重置链接偶发返回 500 错误”的原始支持反馈和监控信号，归一化为一个可继续流转到 PRD 的标准需求卡。目标是让失败路径可恢复、可解释，并在真实项目协作系统中保持稳定编号与回链。

## Tracking and References
- Status Source of Truth: jira
- Primary Tracker ID: AUTH-142
- Related References:
  - GitHub Issue: customer-portal-web#128
  - Change Ticket: CHG-221
  - Planned Artifact Path: docs/ai/requirements/REQ-AUTH-2026-014.md

## Confirmed Facts
- 当前产品已支持邮箱密码重置流程。
- 支持团队已收到真实用户反馈：点击旧链接后看到错误页，无法判断下一步操作。
- 错误监控显示密码重置 verify 接口在异常峰值时存在 5xx。

## Assumptions
- 本次 intake 面向一个已采用 starter 的真实项目风格仓库，因此保留 Jira、GitHub Issue 和 change ticket 的回链信息。
- 首轮交付以修复失败路径和补齐验证为主，不重构整个密码重置体验。

## Business Context
- Problem: 用户在密码重置失败时看到 500 页面，会放大登录受阻和支持压力。
- Why Now: 问题已影响真实用户恢复登录，且属于认证相关核心链路。
- Expected Value: 降低 reset 流程异常率、减少支持工单，并让失败路径可恢复。

## Initial Scope
### In Scope
- 修复 reset token verify 失败路径的异常处理。
- 为过期、重复使用、非法 token 提供统一失败结果和重试引导。
- 补充该链路的回归测试与错误监控检查点。

### Out of Scope
- 重构整个密码重置流程。
- 修改密码复杂度策略。
- 新增 magic link 或其他登录方式。

## Risks Seen At Intake
- 认证与密码重置链路属于高风险模块，必须保留人工 review。
- 若失败处理不一致，可能泄露 token 状态或账户存在性。
- 回归不足可能影响正常的密码重置成功路径。

## Missing Information
- 当前 500 错误是否仅由过期 token 触发，还是也包含重复使用与非法 token，仍需确认。
- 失败页文案是否沿用现有“重新发送链接”提示仍需产品确认。
- 是否需要作为独立 hotfix 提前发版仍需发布负责人确认。

## Need Human Decision
- 该修复是否需要脱离常规发版窗口，以 hotfix 方式优先上线？
- 失败页是否统一展示“重新请求密码重置链接”的明确 CTA？

## Next Handoff
- To: PRD Agent
- Goal: 将该 bugfix 需求转换为可验证的修复范围、验收标准与外部系统回链规则
- Must Read:
  - Tracking and References
  - Initial Scope
  - Risks Seen At Intake
  - Missing Information
