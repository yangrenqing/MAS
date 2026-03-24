# Rollback Runbook

## Meta
- Service:
- Owner:
- Oncall:
- Environment:
- Last Updated:

## Purpose
用于在发布或运行异常时快速、安全地回滚变更。

## Use When
- 错误率超过阈值
- 核心业务指标异常下跌
- 严重性能退化
- 数据一致性风险出现
- 高优先级告警持续触发

## Preconditions
- 已确认问题与本次发布相关
- 已通知值班负责人
- 已暂停继续放量
- 已准备回滚后验证步骤

## Rollback Decision
- Trigger Summary:
- Decision Maker:
- Risk Level:
- User Impact:

## Rollback Steps
1. 停止灰度 / 停止继续发布
2. 切回上一个稳定版本
3. 回退配置或 feature flag
4. 如涉及迁移，执行数据补偿或只读保护
5. 验证服务恢复

## Validation Checklist
- [ ] 错误率恢复
- [ ] 核心接口恢复
- [ ] 核心业务指标恢复
- [ ] 关键日志无持续异常
- [ ] 告警恢复到正常水平

## Data Considerations
- 是否涉及不可逆迁移：
- 补偿方案：
- 数据核对方式：

## Communication
- 通知对象：
- 对外说明模板：
- 业务侧同步人：

## Aftercare
- 记录故障时间线
- 保留相关日志与指标截图
- 创建事故复盘任务
- 更新发布检查单和质量规则
