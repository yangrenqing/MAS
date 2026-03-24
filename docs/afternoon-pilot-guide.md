# Afternoon Pilot Guide

## Goal
用一个下午把这套 starter 从“看过文档”推进到“真实试跑过一次”，并在结束时留下可回填的结论，方便继续迭代模板、hooks 和质量门禁。

## What this guide is for
适合第一次试这套框架时使用：
- 不要求你先把所有 agent 都配到位
- 不要求接通真实 GitHub / GitLab / Jira 自动化
- 不要求自动发版
- 重点是走通一次 **需求 → PRD → 设计 → 测试计划 → 发布准备 → 试跑记录**

## Best Requirement to Pick
优先选一个 **中等复杂度** 的需求，满足这些条件：
- 有明确用户价值
- 会改 1-3 个模块，而不是全局重构
- 需要测试和发布准备，但不需要真实生产变更
- 有一些风险点，但可以通过 feature flag / rollback 思路表达

### Good examples
- 新增一个登录方式
- 一个已有功能的中等复杂度 bugfix
- 一个受控的数据结构迁移方案

### Avoid for first pilot
- 超大范围重构
- 依赖很多外部团队的需求
- 必须真实发布到生产才能验证的需求
- 涉及不可逆数据删除的需求

## Recommended Starter Path
如果你今天下午想最快试起来，建议优先用这三类参考：
1. 功能类：`examples/sample-requirement.md` → `examples/sample-prd.md` → `examples/sample-design.md`
2. bugfix 类：`examples/sample-bugfix-requirement.md`
3. 高风险迁移类：`examples/sample-backend-migration-requirement.md`

## Minimal Reading Order Before You Start
1. `README.md`
2. `docs/project-status.md`
3. `docs/sop/requirement-to-release.md`
4. `docs/quality-gates.md`
5. 一个与你今天需求最接近的 `examples/*`

## One-Afternoon Pilot Flow

### Step 1: Create or normalize the requirement
目标：把原始想法收敛成一张标准需求卡。

模板：
- `docs/templates/requirement-intake.ai.md`

可直接参考：
- `examples/sample-requirement.md`
- `examples/sample-bugfix-requirement.md`
- `examples/sample-backend-migration-requirement.md`

最低要求：
- 背景和目标明确
- 风险点被写出来
- 明确 `Need Human Decision`
- 能交接给 PRD 阶段

建议产出：
- 一个新的 requirement 文档，或在真实 issue 中整理好等价内容

### Step 2: Produce a PRD
目标：把需求变成可验收的交付定义。

模板：
- `docs/templates/prd.ai.md`

最低要求：
- 验收标准可判断
- 范围 / 非范围明确
- 风险和依赖明确
- `Next Handoff` 指向设计阶段

### Step 3: Produce a technical design
目标：确认实现路径、影响模块、数据/接口变化和回滚点。

模板：
- `docs/templates/design.ai.md`

最低要求：
- 写清受影响模块
- 写清关键技术决策
- 写清 rollback 思路
- 高风险路径保留人工 review

### Step 4: Produce a test plan
目标：确认测试覆盖和发布阻断条件。

模板：
- `docs/templates/test-plan.ai.md`

最低要求：
- 至少有 P0 / critical path
- 明确 regression focus
- 明确 release gate
- 明确哪些问题会 block release

### Step 5: Produce release preparation artifacts
目标：在不做真实 deploy 的前提下，验证这套框架能否走到发布准备。

模板：
- `docs/templates/release-checklist.ai.md`
- `docs/templates/staging-checklist.ai.md`
- `docs/templates/canary-checklist.ai.md`
- `docs/templates/change-summary.ai.md`

最低要求：
- 说明 rollout 思路
- 说明 metrics / alerts / rollback trigger
- 说明人类批准点仍然存在
- 不要加入自动生产发布动作

### Step 6: Record the pilot itself
目标：把今天下午真实试跑中的顺畅点、摩擦点、缺口回填下来。

模板：
- `docs/templates/pilot-run-notes.ai.md`

参考示例：
- `examples/sample-pilot-run-notes.md`

最低要求：
- 记录哪里顺畅
- 记录哪里卡住
- 记录缺少哪些模板 / 集成 / 自动化
- 给出 `Reusable As-Is` / `Needs Small Fixes` / `Needs Material Changes`

## Suggested Agent Sequence
如果你要按 starter 的角色来试，建议最小只用这几个：
1. `intake`
2. `prd`
3. `architect`
4. `developer` 或直接手工补实现思路
5. `qa-security`
6. `release-sre`
7. `knowledge-ops`

如果今天时间有限，可以跳过真正的开发实现，只做：
- requirement
- PRD
- design
- test plan
- release checklist
- pilot run notes

这样也足够验证这套框架是否“能跑起来”。

## Fastest Valid Pilot
如果你只有 1-2 小时，走这个最小链路：
1. 选一个需求
2. 写 requirement
3. 写 PRD
4. 写 design
5. 写 test plan
6. 写 release checklist
7. 写 pilot run notes

只要能走完这 7 步，你就已经能判断：
- 模板是否顺手
- handoff 是否清晰
- 哪些 gate 太重或太空
- 哪些文档还需要补

## What to Watch During the Pilot
### Good signals
- 你几乎不用解释每一步该交给谁
- 下游文档能直接消费上游输出
- 风险、回滚、人工决策点都能自然表达
- 不需要为了“自动化”而绕过治理

### Bad signals
- 多个模板字段重复但含义不清
- 角色边界混乱
- 设计 / 测试 / 发布阶段的 handoff 断掉
- 质量门禁写了，但无法用来判断 go / no-go
- 真实仓库 adoption 时不知道怎么替换 placeholder CI

## Recommended End-of-Day Output
今天下午结束时，至少保留这些内容：
- 一份 requirement
- 一份 PRD
- 一份 design
- 一份 test plan
- 一份 release checklist
- 一份 pilot run notes

如果时间允许，再补：
- 一份 change summary
- 一份 staging checklist
- 一份 canary checklist

## After the Pilot
试跑结束后，把结论优先回填到：
1. `README.md`
2. `docs/project-status.md`
3. `docs/session-handoff.md`
4. 对应模板文件
5. `.claude/settings.json`（仅在确有必要时做增量调整）

## Guardrails
- 不要把 starter 试跑变成真实生产操作
- 不要删除人工批准点
- 不要为了快而跳过 rollback 思考
- 不要把 placeholder workflow 当成生产可用流水线
- 若需求过大，先缩小范围再试

## Quick Recommendation
如果你下午马上要试：
- 首选一个 feature 或 bugfix 场景
- 先不用追求真实代码实现
- 先验证文档链路和 handoff 是否顺
- 最后一定写 `pilot-run-notes`

这样最容易在半天内得到高质量反馈。
