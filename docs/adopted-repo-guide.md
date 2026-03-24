# Adopted Repo Guide

## Goal
给第一次把这套 starter 落到真实项目仓库的团队一份“怎么放、谁来负责、哪些地方要保守调整”的最小指南。

这份文档补充 `docs/starter-adoption-checklist.md`：
- checklist 负责检查“有没有做”
- 本文负责说明“在真实仓库里通常怎么放、怎么映射、怎么调”

## Recommended Placement in an Adopted Repo

### Minimal directory layout
如果目标项目仓库允许增加 AI 文档目录，优先使用一组稳定路径：

```txt
docs/
  ai/
    intake/
    requirements/
    prd/
    design/
    test-plans/
    release/
    pilot-runs/
    postmortems/
```

推荐映射：
- requirement intake：`docs/ai/intake/`
- normalized requirement：`docs/ai/requirements/`
- PRD：`docs/ai/prd/`
- design：`docs/ai/design/`
- test plan：`docs/ai/test-plans/`
- release checklist / staging / canary / change summary：`docs/ai/release/`
- pilot run notes：`docs/ai/pilot-runs/`
- postmortem：`docs/ai/postmortems/`

### If the repo prefers fewer directories
如果目标仓库不想引入太多目录，可以收敛为：

```txt
docs/
  ai/
    requirements/
    delivery/
    operations/
```

最重要的是：
- 同一类文档放在固定位置
- requirement ID 在路径或文件名中稳定出现
- README 或项目内导航文档能说明这些目录怎么读

### What should stay at repo root vs project-internal docs
保留在仓库根或既有标准位置：
- `.claude/agents/`
- `.claude/settings.json`
- `.github/**` 或 `.gitlab/**`
- 项目主 `README.md`

放进项目内部文档目录：
- requirement / PRD / design / test plan / release prep / pilot notes
- project-specific external mapping
- rollout notes / incident drill / postmortem

## Recommended Naming Convention

优先保持“稳定 ID + 简短语义名”：
- `REQ-AUTH-2026-014-password-reset.md`
- `PRD-AUTH-2026-014-password-reset.md`
- `DESIGN-AUTH-2026-014-password-reset.md`
- `TEST-AUTH-2026-014-password-reset.md`
- `REL-AUTH-2026-014-password-reset.md`
- `PILOT-AUTH-2026-014.md`

如果团队已经有 Jira / GitHub / GitLab 编号体系，不要强行替换；保持：
- 一个稳定 Requirement ID
- 外部系统编号都回链到这个 Requirement ID

可参考：`examples/sample-external-system-mapping.md`

## Minimum Owner Mapping

第一次 adoption 不需要把每个角色都拆成独立人类岗位，但下面这些责任必须有人兜底：

| Responsibility | Minimum human owner | AI assist expectation |
|---|---|---|
| Requirement intake | 产品 / PM / 需求 owner | 归一化需求、补齐缺口、标出 unknowns |
| PRD acceptance | 产品 owner | 整理 scope / non-scope / acceptance |
| Technical design | Tech lead / senior engineer | 汇总影响模块、风险、rollback 思路 |
| Implementation coordination | Dev lead 或项目负责人 | 拆任务、追踪依赖、组织评审 |
| QA / security review | QA owner 或资深开发 | 生成测试计划、指出 release blockers |
| Release approval | 发布负责人 / 值班负责人 | 汇总 checklist、变更摘要、观察项 |
| Rollback decision | On-call / incident commander | AI 只能提供建议，不能代替最终判断 |

### Small-team mapping
如果团队很小，可以合并角色，但不要合并掉责任：
- PM + Tech Lead 可以是同一个人
- Dev lead + QA owner 可以暂时由同一个 senior engineer 兼任
- Release approver 和 rollback decider 仍建议是明确的人类，不要默认由开发者自己放行

### What must remain human
这些动作必须保留人类确认：
- 是否进入 canary
- 是否扩大流量
- 是否执行 rollback
- auth / payments / orders / billing / data migration 等高风险改动是否通过
- 生产环境或真实外部系统的最终变更

## Minimum Reviewer Policy for an Adopted Repo

starter 不替 adopted repo 直接写死 approver 名单，但第一次 adoption 最少应把“什么改动需要额外 reviewer、额外 reviewer 应来自哪里、什么不能靠 CI 通过自动替代”说清楚。

### Good enough first reviewer-policy baseline
| Change shape | Minimum extra review expectation | Why this is a safe first rule |
|---|---|---|
| 普通功能、小范围 UI、低风险文案 | 按团队默认 PR / MR review 规则执行 | 不把所有改动都抬到高风险流程 |
| auth / payments / billing / orders / admin / internal-api / runtime-config | 除作者外，至少再有一名了解该域风险的人类 reviewer | 这些目录出错后影响登录、计费、权限或生产行为 |
| migration / backfill / schema / ledger / rollback-sensitive config | 除作者外，至少再有一名能判断数据与回滚影响的人类 reviewer | 重点不是代码风格，而是可恢复性和不可逆后果 |
| release checklist / canary / rollback 相关改动 | 发布负责人、on-call 或等价 owner 明确知晓 | 避免“代码过了 CI 就默认可发” |

### What the adopted repo should fix explicitly
第一次 adoption 至少把下面几项写清楚：
- 哪些目录或文件类型命中“额外 review”范围
- 额外 reviewer 应来自哪个角色或责任面：如 tech lead、domain owner、senior engineer、release owner、on-call
- 高风险改动是否允许作者自己合并；如果团队很小，也应保留“至少第二个人类”复核
- reviewer 需要明确看什么：影响范围、测试说明、rollback plan、Need Human Decision、外部系统影响

### Small-team fallback
如果团队规模很小，可以不追求复杂审批矩阵，但至少保留：
- 作者之外的第二个人类 reviewer
- 对 auth / billing / migration / runtime-config 等高风险改动，由更熟悉该域的人优先 review
- release approver 与 rollback decider 不默认等同于作者本人

### What reviewer policy must not depend on
下面这些信号都不能替代额外 reviewer：
- CI 通过
- hooks 给出提醒
- AI 生成了 test plan / release checklist
- PR 模板里作者自己勾选了“已自测”

## Repo-Specific High-Risk Path Tuning

### Keep the default posture
默认的安全思路不要变：
- `Write|Edit` 的高风险路径只做额外提醒 / review context
- `Bash` 命中 release / production / deploy 相关关键词时转为人工确认
- hooks 默认不做自动 deploy、自动 rollback、自动改生产

### How to tune path rules
不要只保留 starter 里的通用词，应该按项目真实目录补充。常见做法：
- 保留基础路径：`payments`、`auth`、`billing`、`orders`、`db/migrations`
- 叠加项目专有路径：
  - 身份与会话：`identity`、`session`、`login`、`permissions`
  - 数据与迁移：`schema`、`backfill`、`etl`、`ledger`
  - 配置与基础设施：`infra`、`runtime-config`、`terraform`
  - 高权限后台：`admin`、`ops`、`internal-api`

### Good path-tuning rules
优先按“真实改动风险”来调，而不是按团队感觉来调：
- 涉及认证、支付、订单、账单、权限、迁移、生产配置的目录进高风险范围
- 普通 UI 文案、样式、小型页面布局通常不放入高风险范围
- 如果某模块一旦出错就会影响登录、计费、核心交易或不可逆数据变更，应纳入高风险范围

### Example tuning process
1. 从最近 3-5 次高风险事故 / 线上变更里找共同目录
2. 对照真实代码目录，把默认规则补齐
3. 先只加最明显的高风险路径，不要一次加太宽
4. 首个 pilot 后再根据误报 / 漏报做第二轮调整

## How to Replace Placeholder CI Safely

采用仓库后，CI 最先替换的应该是本地质量命令，而不是部署动作：
- lint
- typecheck
- unit / integration test
- dependency / static security checks

不要加入：
- staging deploy
- production deploy
- 自动 rollback
- 自动放量

一个安全的 adoption 顺序是：
1. 先把 placeholder workflow 改成真实 lint / typecheck / test
2. 再把 test-plan 与 release checklist 中的检查项对齐这些命令
3. 最后才考虑是否补充更多只读验证

### Minimum inputs to collect first
在真正改 `.github/workflows/ci.yml` 或 `.gitlab-ci.yml` 之前，先把这几项收清楚：
- 项目运行时：Node / Python / Go / Java / 其他
- 包管理器或命令入口：`pnpm` / `npm` / `yarn` / `uv` / `poetry` / `go test` / `make`
- 本地开发者已经稳定在用的质量命令
- 哪些检查是 PR / MR 必跑，哪些只适合手动或夜间跑
- 哪些命令依赖外部系统、真实云环境或敏感凭证，避免误塞进默认 CI

### Good replacement rule
优先把“开发者本地已经稳定使用”的命令搬进 CI，而不是为了看起来完整临时造一套新命令：
- 有现成 `package.json` / `Makefile` / `justfile` / `tox` / `nox` / `Taskfile`，优先复用
- 没有稳定命令时，先补项目内命令入口，再接入 CI
- 同一个检查只保留一个主入口，避免 README、PR 模板、test plan 和 CI 各写各的

### Recommended minimum command set
第一次 adoption 只要求一套最小闭环：

| Check type | What to wire first | Example command shapes |
|---|---|---|
| Lint | 代码风格 / 静态规则 | `pnpm lint`, `npm run lint`, `uv run ruff check .`, `golangci-lint run` |
| Typecheck | 类型或编译前检查 | `pnpm typecheck`, `npm run typecheck`, `mypy .`, `tsc --noEmit` |
| Tests | 最小回归集 | `pnpm test`, `npm test`, `pytest`, `go test ./...` |
| Security-style checks | 依赖或静态扫描 | `npm audit --production`, `pip-audit`, `semgrep --config auto` |

这些例子只是命令形状，不是要求每个 adopted repo 全部都上。

### What to keep out of the default pipeline
下面这些内容即使项目里存在，也不应作为 starter 默认 CI 骨架的一部分：
- 真正发版命令
- 数据 backfill / schema apply / migration apply
- 会改写外部系统状态的 smoke script
- 需要生产凭证的验证步骤
- 自动决定 canary 放量或 rollback 的逻辑

### Safe cutover sequence for an adopted repo
1. 先保留现有 placeholder 文件结构不变，只替换命令内容
2. 先接 `lint`、`typecheck`、`test` 三类本地命令
3. 命令稳定后，再补 dependency / static security checks
4. 把 PR / MR 模板里的 self-check 文案改成与 CI 相同的命令名
5. 把 test plan / release checklist 中的 gate 描述改成引用同一组命令
6. 只有在团队已经习惯这些质量门禁后，才考虑增加更多只读验证

### Fast validation after replacement
替换后至少确认这几件事：
- README、adoption checklist、test plan 对 CI 的叫法一致
- PR / MR 上能看懂到底是哪一个命令失败
- 没有任何步骤暗示 staging / production deploy
- 高风险变更仍然依赖人工 review，而不是被 CI 通过自动放行
- release checklist 中引用的是文档和人工批准点，不是 deploy 按钮

### Common mistakes
- 直接把本地临时 debug 命令塞进 CI
- 同时保留 placeholder 文案和真实命令，导致团队不知道哪个才是 gate
- 把 e2e、load test、DB migration 检查一次性全塞进默认 PR pipeline
- 在 adopted repo 还没明确 owner 和 rollback plan 前，就把 release job 也一起接进来
- CI 已经换成真实命令，但 PR 模板、test plan、release checklist 仍写 placeholder

### Good enough first version
对于第一次 adoption，只要做到下面这些，就已经足够：
- PR / MR 时自动跑 repo-local lint / typecheck / test
- 如有必要，再补一个 dependency 或静态安全检查
- 失败信息能直接指导开发者修复
- 所有 release、canary、rollback 仍保留人工节点
- 文档里能明确指出这些命令是当前质量门禁的最小来源

## How to Introduce Security-Style Checks Safely

第一次 adoption 的重点是把 security-style checks 接得足够稳，而不是接得足够多。

### Good enough first decision order
1. 先确认团队是否已经有一个稳定、低噪音、开发者本地也会运行的主命令。
2. 如果还没有，就先把 security-style checks 保留为 nightly / manual / release-prep gate，不要硬塞进默认 PR pipeline。
3. 只有当命令输出稳定、误报可控、失败后有明确处理路径时，再考虑放进默认 PR gate。

### Recommended rollout posture
| Maturity | Where to place the check first | Suitable examples | Why this is safer |
|---|---|---|---|
| 还没有统一主命令 | manual / pilot / release-prep checklist | 临时 dependency audit、一次性静态扫描 | 先验证噪音和可执行性 |
| 有主命令但噪音偏高 | nightly / scheduled validation | 较慢或误报较多的依赖扫描 | 避免团队因为噪音绕过默认 gate |
| 命令稳定且失败语义清晰 | default PR / MR gate | 低噪音 dependency / static checks | 能真正成为日常质量门禁 |

### What counts as a good first security-style check
优先选择下面这类检查，而不是一上来追求“大而全”：
- 开发者本地已经稳定运行
- 不依赖生产凭证或真实外部环境
- 失败输出能告诉开发者下一步怎么修
- 不会默认触发 deploy、migration、rollback、traffic promotion

### What to keep out
下面这些不应被包装成 starter 默认 security-style checks：
- 需要真实云权限或生产凭证的扫描
- 会修改依赖、基础设施或外部系统状态的命令
- 需要人工判断结果、却被误当成自动 gate 的脚本
- 噪音过高、团队尚未统一处理方式的扫描集合

### Minimum documentation to keep aligned
一旦 adopted repo 决定了 security-style checks 的放置位置，至少同步这几处：
- README / adoption notes 中当前主命令叫什么
- PR / MR 模板中作者需要执行哪些本地检查
- test plan / release checklist 中哪些属于默认 gate，哪些属于 nightly 或 manual
- reviewer policy 中哪些高风险改动即使通过扫描也仍需额外人工 review

## Recommended First Real Adoption Flow

在 adopted repo 中，第一次不要追求“全自动”。

推荐顺序：
1. 建好 `README.md`、`docs/project-status.md`、`docs/session-handoff.md`
2. 放入 `.claude/agents/`、`.claude/settings.json`、核心模板
3. 选一个中等复杂度需求做 pilot
4. 跑通 intake → PRD → design → test plan → release prep → pilot notes
5. 把 friction points 回填到项目内文档和 hooks 调整建议

可参考：
- `docs/afternoon-pilot-guide.md`
- `examples/sample-real-project-pilot-run-notes.md`

## Adoption Smells

如果出现这些信号，说明 adoption 还没收敛好：
- requirement / PRD / design 分散在多个随意目录里
- 没有人明确负责 release approval 或 rollback decision
- 高风险路径规则仍停留在 starter 默认值，但与真实仓库结构不匹配
- CI 还在跑 placeholder，test plan 却写成了真实 gate
- 为了快而把 deploy / release / rollback 自动化塞进 hooks

## Minimum Validation Before Calling Adoption "Ready"
- 项目内文档目录已经固定
- requirement ID 规则已经固定
- 至少一条 pilot 文档链路已经走通
- release approver / rollback owner / on-call 已明确
- repo-specific high-risk paths 已做首轮调整
- CI placeholder 已替换为真实质量命令，且不含生产动作

## Next References
- Checklist：`docs/starter-adoption-checklist.md`
- First pilot：`docs/afternoon-pilot-guide.md`
- External IDs：`examples/sample-external-system-mapping.md`
- Real-project-style pilot example：`examples/sample-real-project-pilot-run-notes.md`
- CI replacement validation example：`examples/sample-adopted-repo-ci-validation.md`
