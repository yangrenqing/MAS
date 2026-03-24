# Sample Incident Runbook Record

## Meta
- Incident ID: INC-2026-001
- Severity: SEV-1
- Commander: Carol Liu
- Oncall: David Lin
- Started At: 2026-03-23 09:12 local time
- Current Status: mitigated

## Trigger Conditions
- Password reset verify API 5xx error rate exceeded 12% for 10 minutes
- Support tickets reported that users clicking expired reset links saw a 500 error page
- Login recovery success rate dropped below the agreed threshold

## Immediate Actions
1. 确认问题集中在 password reset verify 失败路径，而不是整个 auth-service 全面故障。
2. 暂停与 auth 相关的非必要发布操作。
3. 指定 incident commander、investigation owner 和 communications owner。
4. 在统一 incident channel 中同步时间线和决策。
5. 保留错误日志、告警截图和最近变更记录。

## First 15 Minutes Checklist
- [x] 是否正在持续扩散
- [x] 是否需要立即回滚
- [x] 是否影响付费/订单/权限/登录
- [x] 是否需要对外通知
- [x] 是否保留现场证据

## Roles
- Commander: Carol Liu
- Communications: Emma Zhao
- Investigation: Bob Wang
- Mitigation: David Lin
- Recorder: Mia Chen

## Investigation Notes
- Suspected Cause: reset token verify 失败路径未正确处理 expired / used token，异常被上抛为 500
- Scope: Web password reset flow only
- Affected Services: auth-service, web-login-ui
- Affected Users: 过去 45 分钟内命中过期或重复使用 reset link 的用户
- Known Good Version: auth-service build 2026.03.20.2

## Mitigation Options
- Option 1: 回滚 auth-service 到上一稳定版本
- Option 2: 临时降级为统一失败页并禁用有问题的 verify 分支
- Option 3: 关闭相关 feature flag / 开关（若存在）
- Option 4: 对外提示用户重新请求密码重置链接

## Decision Log
| Time | Decision | Owner | Reason |
|---|---|---|---|
| 09:18 | 暂停 auth 相关发布与变更合入 | Carol Liu | 避免故障面扩大 |
| 09:24 | 先采取应用层降级，统一返回可恢复失败结果 | David Lin | 比完整回滚更快恢复用户路径 |
| 09:31 | 对支持团队发布内部说明，指导用户重新请求 reset link | Emma Zhao | 减少重复工单与误导性沟通 |
| 09:47 | 保留热修复方案，但要求人工 review 后再上线 | Carol Liu | 认证链路属于高风险模块 |

## Recovery Criteria
- password reset verify 5xx error rate 恢复到基线范围
- 用户点击失效 reset link 时展示统一失败结果，而不是 500 页面
- 支持团队验证用户可通过重新请求链接完成密码重置
- 关键监控 30 分钟内保持稳定

## Closure Checklist
- [x] 影响已解除
- [x] 用户侧验证完成
- [x] 事故公告完成
- [x] 复盘会议已安排
- [x] 后续行动项已建单
