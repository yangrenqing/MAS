# Quality Gates

## Purpose
定义这套 AI R&D starter 的最小质量门禁，保证从需求到发布的过程具备一致、可复用、可审计的检查标准。

## Scope
适用于以下阶段：
- PRD 输出
- 技术方案输出
- 开发完成
- QA / Security
- Release Preparation
- Staging / Canary / Observe

## Global Rules
1. 所有需求必须有明确验收标准。
2. 所有高风险改动必须有额外 review 和测试说明。
3. 所有发布必须有回滚方案。
4. 所有 agent 输出必须包含 `Need Human Decision` 和 `Next Handoff`。
5. 生产相关动作必须保留人工确认点。
6. 不能以自动化为理由绕过质量门禁。
7. Requirement / PRD / design / test / release 之间的追踪关系必须可恢复，不应只靠口头上下文。
8. 当变更影响行为、范围或 release 风险时，应先或同步更新上游规格，再推进实现与发布准备。

## Gate 1: Requirement Quality
进入技术方案前必须满足：
- [ ] PRD 已填写完整
- [ ] 验收标准明确
- [ ] Scope / Out of Scope 清晰
- [ ] 关键依赖与约束已列出
- [ ] 未决事项已进入 `Need Human Decision`

## Gate 2: Design Quality
进入开发前必须满足：
- [ ] 技术方案覆盖影响模块
- [ ] 数据 / API / 配置变化清晰
- [ ] 风险与回滚点已说明
- [ ] 高风险路径已显式标记
- [ ] 最小实现路径明确

## Gate 3: Development Quality
进入 QA / Release 评估前必须满足：
- [ ] 仅实现需求范围内内容
- [ ] 自测结论可读
- [ ] 测试补充已说明
- [ ] 变更影响摘要已提供
- [ ] 未解决问题已显式列出

## Gate 4: QA / Security Quality
进入发布准备前必须满足：
- [ ] P0 用例通过
- [ ] 核心链路通过
- [ ] 无 blocker 缺陷
- [ ] 无高危安全问题
- [ ] 监控项已定义
- [ ] 回滚方案存在

## Gate 5: Release Preparation Quality
进入 staging / canary / production 决策前必须满足：
- [ ] 发布检查单完整
- [ ] 风险等级已标记
- [ ] Rollout Strategy 已定义
- [ ] Metrics To Watch 已定义
- [ ] Alert Thresholds 已定义
- [ ] Rollback Plan 可执行
- [ ] 发布负责人 / 值班负责人已知晓

## Gate 6: Staging Quality
进入 canary 前必须满足：
- [ ] staging checklist 完整
- [ ] 关键路径验证通过
- [ ] 回归焦点已复核
- [ ] 高风险项已检查
- [ ] Defects / Gaps 已记录
- [ ] 有明确 Recommendation

## Gate 7: Canary Quality
进入继续放量 / 全量前必须满足：
- [ ] canary checklist 完整
- [ ] 监控与阈值可观察
- [ ] 观察窗口内无持续异常
- [ ] rollback trigger 未触发
- [ ] 若触发异常，决策日志完整
- [ ] 是否继续放量由人类最终确认

## Artifacts to use
- `docs/templates/prd.ai.md`
- `docs/templates/design.ai.md`
- `docs/templates/test-plan.ai.md`
- `docs/templates/release-checklist.ai.md`
- `docs/templates/staging-checklist.ai.md`
- `docs/templates/canary-checklist.ai.md`
- `docs/templates/change-summary.ai.md`
- `docs/templates/postmortem.ai.md`
- `docs/templates/pilot-run-notes.ai.md`
- `ops/runbooks/rollback.md`
- `ops/runbooks/incident.md`

## Failure Handling
如果任一 gate 未通过：
- 不进入下一阶段
- 在当前文档中明确阻塞原因
- 将未决项写入 `Need Human Decision`
- 在 `Next Handoff` 中明确谁负责解除阻塞

## Audit Expectation
每个阶段至少应保留：
- 输入来源
- 输出文档
- 风险与已知缺口
- 人工决策点
- 下一步交接对象
- 上游 / 下游关联工件（例如 requirement、PRD、design、test plan、release artifacts）
- 若发生规格变更，对应的 change-control 记录或同步说明
