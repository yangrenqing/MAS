# Incident Runbook

## Meta
- Incident ID:
- Severity:
- Commander:
- Oncall:
- Started At:
- Current Status:

## Trigger Conditions
- 大面积报错
- 核心业务中断
- 性能显著退化
- 安全事件
- 数据错误或数据丢失风险

## Immediate Actions
1. 确认影响范围
2. 确认是否需要暂停发布或回滚
3. 指定 incident commander
4. 拉起相关责任人
5. 建立统一信息同步渠道

## First 15 Minutes Checklist
- [ ] 是否正在持续扩散
- [ ] 是否需要立即回滚
- [ ] 是否影响付费/订单/权限/登录
- [ ] 是否需要对外通知
- [ ] 是否保留现场证据

## Roles
- Commander:
- Communications:
- Investigation:
- Mitigation:
- Recorder:

## Investigation Notes
- Suspected Cause:
- Scope:
- Affected Services:
- Affected Users:
- Known Good Version:

## Mitigation Options
- Option 1: 回滚
- Option 2: 降级
- Option 3: 关闭 feature flag
- Option 4: 流量切换

## Decision Log
| Time | Decision | Owner | Reason |
|---|---|---|---|
|  |  |  |  |

## Recovery Criteria
- 错误率恢复正常
- 核心链路恢复
- 关键监控稳定
- 风险已受控

## Closure Checklist
- [ ] 影响已解除
- [ ] 用户侧验证完成
- [ ] 事故公告完成
- [ ] 复盘会议已安排
- [ ] 后续行动项已建单
