# Feishu or Slack Integration

## Goal
把飞书或 Slack 作为通知、协同和人工确认入口，而不是唯一事实来源。真正的需求、代码和发布文档仍应保存在 Jira / GitHub / GitLab / 仓库文档中。

## Recommended Scope
适合承接这些场景：
- 需求进入提醒
- Agent handoff 通知
- blocker / risk 升级
- QA / release 窗口提醒
- canary 观察日志播报
- incident 协同频道
- 人工确认请求

## Recommended Channel Design
- `#rd-intake` / 飞书需求群：新需求进入
- `#rd-delivery`：PRD、设计、开发、QA 状态播报
- `#release-ops`：发布准备、staging、canary、rollback 通知
- `#incident-war-room`：事故期间临时指挥
- `#postmortem`：复盘结论同步

## What To Send vs What To Link
### Send directly in chat
- 1~5 行摘要
- 当前阶段
- 风险等级
- 是否需要人工决策
- 下一步责任人

### Link out
- PRD / 设计 / 测试 / 发布检查单全文
- PR / MR
- Jira issue
- dashboard / alert / runbook

原则：聊天工具里只放摘要和动作，不把长文档复制多份。

## Agent Input / Output Landing
### Intake / PRD / Architect
- 当需求进入、PRD完成、设计完成时推送摘要：
  - Requirement ID
  - 风险等级
  - 当前阻塞
  - 文档链接

### Planning / Dev Lead / Developer
- 当任务拆解完成、PR 打开、代码 ready for QA 时推送：
  - 负责人与 ETA 说明可选
  - linked PR / MR
  - 自测状态
  - 是否存在 blocker

### QA / Security
- 当测试结束或发现 blocker 时推送：
  - pass/fail/partial
  - blocker 摘要
  - 是否建议进入 release prep

### Release / SRE
- 发布前、canary 中、观察结束时推送：
  - rollout scope
  - metrics to watch
  - alert thresholds
  - current recommendation
  - Need Human Decision

### Knowledge / Ops
- 事故结束后推送：
  - root cause 摘要
  - action items
  - 文档链接

## Automation Boundary
### Allowed automation
- 自动发送阶段变更通知
- 自动发送 blocker / risk 升级提醒
- 自动提醒补全缺失字段
- 自动播报 canary 观察窗口中的关键指标摘要
- 自动发送“需要人工确认”的 structured message

### Must keep manual confirmation
- 是否上线生产
- 是否继续放量
- 是否执行 rollback
- 对外公告 / 客户通知
- Incident commander 的最终决策

## Message Template Suggestion
```md
[Stage Update]
Requirement: REQ-123
Stage: Release Preparation
Risk: high
Summary: 支付回调链路已完成 QA，等待发布负责人确认 canary 窗口。
Need Human Decision: 是否在今晚 20% 流量范围内开启 canary
Next Owner: Release Owner
Links:
- Release Checklist: <link>
- Dashboard: <link>
- Rollback Runbook: <link>
```

## Minimal Example
1. 新需求进入后，机器人在 `#rd-intake` 发摘要和 Jira/GitHub 链接。
2. PRD 完成后，在 `#rd-delivery` 发 handoff 消息给 Architect / Tech Lead。
3. PR ready for QA 时，在 `#rd-delivery` 通知 QA/Security。
4. 发布前，在 `#release-ops` 发 release checklist、dashboard、rollback runbook 链接。
5. canary 期间定时推送关键指标摘要，但是否继续放量由人类确认。
6. incident 期间在专门频道维护时间线和责任人。

## Suggested First Integration
先做最小集成：
1. 一个统一的阶段变更消息模板
2. 一个 `Need Human Decision` 消息模板
3. 一个 release/canary 通知模板
4. 一个 incident war-room 使用约定
