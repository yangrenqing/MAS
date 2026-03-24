# Sample Postmortem

## Meta
- Incident ID: INC-2026-001
- Requirement ID: REQ-2026-002
- Owner: Knowledge/Ops Agent
- Severity: SEV-1
- Date: 2026-03-24

## Summary
本次事故发生在 Web 密码重置 verify 失败路径。部分用户点击过期或已使用的 reset link 时，auth-service 将可预期的业务失败错误错误地上抛为 500，导致用户看到服务异常页而不是可恢复提示，进而影响登录恢复成功率并触发支持工单增长。团队通过快速降级恢复了失败路径展示，并确认需要补充错误分类、回归测试和监控拆分。

## Confirmed Facts
- 事故集中在 password reset verify 失败路径，而不是整个登录系统不可用。
- 过期 token 和重复使用 token 都可能触发 500 页面。
- 应用层降级后，用户可重新请求 reset link，5xx 指标恢复正常。

## Assumptions
- 最近一次 auth-service 相关改动改变了失败路径异常映射逻辑。
- 如果当时有更细的业务错误监控，问题会更早被识别为“失败路径处理缺陷”而不是泛化的服务异常。

## Impact
- Affected Users: 命中过期或已使用 reset link 的 Web 用户
- Affected Systems: auth-service, web-login-ui, support workflow
- Business Impact: 登录恢复链路受阻，支持工单和用户挫败感上升
- Start Time: 2026-03-23 09:12 local time
- End Time: 2026-03-23 10:03 local time

## Timeline
| Time | Event | Owner |
|---|---|---|
| 09:12 | 错误率告警触发，support 同步用户看到 500 页面 | Oncall |
| 09:18 | incident commander 建立统一 channel 并暂停相关发布 | Carol Liu |
| 09:24 | 团队确认问题集中在 expired / used token verify 路径 | Bob Wang |
| 09:31 | 临时降级上线，统一失败路径返回可恢复提示 | David Lin |
| 10:03 | 指标恢复并确认用户可重新请求 reset link | Oncall |

## Root Cause
- Primary Cause: 业务可预期的 token invalid / expired / used 异常未被正确映射到统一失败结果，最终落成 500 响应
- Contributing Factors:
  - 失败路径回归测试覆盖不足，重点放在成功重置主链路
  - 监控没有区分业务失败与服务异常，导致问题定位信息不够直接

## Detection
- How Detected: verify API 5xx 告警 + 支持团队反馈用户截图
- Why Not Detected Earlier: 预发布验证没有覆盖“过期链接”“重复使用链接”这类失败路径，且仪表盘缺少细分错误类型

## Response
- Mitigation: 先做应用层降级，统一将失效 token 返回可恢复失败页，并引导用户重新请求密码重置链接
- Rollback Used: no
- Recovery Validation: 观察 5xx 恢复、支持团队复测、用户重新请求 reset 成功

## What Went Well
- Oncall 与支持团队较快拼接出“用户截图 + 指标异常”的完整信号。
- 团队优先选择更快恢复用户路径的降级措施，而不是盲目扩大改动范围。

## What Went Wrong
- 认证失败路径测试不完整，缺少过期和重复使用 token 的明确回归用例。
- 监控和错误分类过粗，业务失败与真正 500 混在一起，增加定位成本。

## Preventive Actions
- Action 1: 为 expired / used / tampered token 增加单元、集成和 E2E 回归用例
- Action 2: 在 auth-service 中统一业务失败异常映射，避免再落成 500
- Rule Update Needed: 认证类 bugfix 在 release prep 前必须明确列出失败路径测试结果
- Template Update Needed: 后续可补一份 bugfix test plan 或 incident-to-hotfix 示例
- Monitoring Update Needed: 将 password reset verify 的业务失败类型与 5xx 分开监控

## Need Human Decision
- 该问题的热修复是否需要走独立上线窗口，还是并入下一次受控发布？
- 是否将 auth / reset / verify 类路径纳入更严格的高风险 edit 提醒范围？

## Next Handoff
- To: Knowledge/Ops Agent / Tech Owner
- Goal: 更新规则、模板和治理项
- Must Read:
  - Root Cause
  - Preventive Actions
  - Detection
