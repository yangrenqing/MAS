# Sample Adopted-Repo CI Validation

## Goal
给第一次采用 starter 的团队一个“如何验证 placeholder CI 替换是否真的收敛”的真实项目风格样例。

## Meta
- Validation ID: `CI-VALIDATION-REAL-2026-001`
- Project / Repo: `customer-portal-web`
- Runtime: `Node.js + TypeScript`
- Package Manager: `pnpm`
- Primary Platform: `GitHub`
- Requirement ID: `REQ-AUTH-2026-014`
- Date: `2026-03-23`

## Summary
本示例演示在一个已采用 starter 的真实项目风格仓库中，如何把 placeholder CI 安全替换为 repo-local 质量命令，并验证 README、PR 流程、test plan、release prep 与人工审批点仍然保持一致。结果为 `Needs Small Fixes`：主路径已经可复用，但 security-style checks 的接入时机和 repo-specific 高风险路径补充仍需要团队在真实 adoption 中确认。

## Confirmed Facts
- 该仓库已经有稳定的本地命令入口：`pnpm lint`、`pnpm typecheck`、`pnpm test -- password-reset`。
- 当前 adoption 只打算把 PR 质量门禁替换为 repo-local checks，不引入 staging / production deploy。
- `auth/` 已被视为高风险目录，但项目特有的 `identity/` 和 `session/` 路径尚未补入默认规则。

## Assumptions
- 这是一个真实项目风格验证样例，用于验证 adopted-repo guidance，不代表真实生产变更已执行。
- 该团队当前还没有稳定、低噪音的 dependency/security scan 命令，因此 first cut 暂不将其放入默认 PR pipeline。

## Adoption Baseline
- Existing placeholder workflow:
  - 使用 starter 自带的 local-quality placeholder，只包含 TODO 和 guardrail 提示。
- Adoption target:
  - 将 placeholder 替换为 repo 已经稳定使用的 lint / typecheck / focused test 命令。
- Guardrails kept:
  - merge、release、canary、rollback 仍保留人工审批。
  - 不把 migration apply、deploy、traffic promotion 塞进默认 CI。

## Inputs Collected First
| Input | Chosen value | Why it matters |
|---|---|---|
| Runtime | Node.js + TypeScript | 决定安装与命令形态 |
| Package manager | `pnpm` | 避免 README、CI、PR 模板出现多套命令叫法 |
| Stable local lint command | `pnpm lint` | 直接复用开发者已有主入口 |
| Stable local typecheck command | `pnpm typecheck` | 作为 PR 前基础编译/类型门禁 |
| Stable local test command | `pnpm test -- password-reset` | 先接最小回归集，而不是一次塞全量 e2e |
| Manual / nightly-only checks | `pnpm test:e2e`, dependency audit | 噪音、耗时或依赖外部条件，不适合 first cut |
| High-risk paths needing extra review | `auth/`, `identity/`, `session/` | adoption 后应补齐 repo-specific 风险目录 |

## Selected Minimum Command Set
| Check type | Command selected | Why this was chosen |
|---|---|---|
| Lint | `pnpm lint` | 已在本地稳定使用，输出可直接定位问题 |
| Typecheck | `pnpm typecheck` | 与 TypeScript 项目日常检查一致 |
| Tests | `pnpm test -- password-reset` | 聚焦本次 auth bugfix 的最小关键回归 |
| Security-style checks | Not in first PR gate | 团队尚未统一低噪音主命令，先不强行接入 |

## Commands Explicitly Kept Out
| Command / step | Why it stays out of default CI |
|---|---|
| `pnpm test:e2e` | 耗时更长，首轮 adoption 先不把全量端到端测试塞进默认 PR pipeline |
| `pnpm run migrate:apply` | 会触发真实 schema/data 变更，不属于 starter 默认质量门禁 |
| `pnpm run deploy:staging` | 属于环境变更动作，不应作为默认 local-quality workflow 的一部分 |
| `scripts/canary/promote.sh` | 流量放量必须保留人工决定 |
| `scripts/rollback-auth.sh` | rollback 只能作为 runbook 和人工操作参考，不能被默认 CI 自动触发 |

## Safe Cutover Sequence Applied
1. 保留原有 workflow 结构与 local-quality job 名称，只替换命令内容。
2. 先接入 `pnpm lint`、`pnpm typecheck`、`pnpm test -- password-reset`。
3. 把 PR 自检文案改成与 CI 相同的命令名。
4. 把 test plan 和 release checklist 中的 gate 描述改成引用同一组命令。
5. 验证失败输出是否能让 reviewer 一眼看到是哪个命令失败。
6. 明确保留 release / canary / rollback 的人工审批节点，不因 CI 通过而自动放行。

## Validation Results
| Check | Result | Notes |
|---|---|---|
| README、PR 自检、test plan 对命令叫法一致 | pass | 不再同时出现 placeholder 与真实命令 |
| PR 失败时能看懂是哪一个命令挂了 | pass | `lint`、`typecheck`、focused test 输出分离 |
| 默认 pipeline 不暗示 staging / production deploy | pass | workflow 只保留 repo-local quality checks |
| 高风险 auth 变更仍依赖人工 review | pass | CI 通过不等于自动允许 release |
| release checklist 仍以文档和人工批准点为主 | pass | 未把 deploy/rollback 按钮塞入 starter 默认流程 |
| security-style checks 是否应立即接入默认 PR gate | partial | 团队需要先统一主命令与噪音阈值 |

## Friction Points
| Area | What slowed us down | Impact | Severity |
|---|---|---|---|
| test scope | `pnpm test` 覆盖范围过大，首轮 adoption 需要先收窄到 focused regression | 否则 PR pipeline 太慢，团队容易绕过 | medium |
| security checks | 团队还没有一个一致认可的 dependency / static security 主命令 | 暂时无法把 security-style check 也稳定纳入默认 gate | medium |
| risk-path tuning | starter 默认规则没覆盖 `identity/`、`session/` | adopted repo 仍需再做一轮 repo-specific 调整 | medium |

## Artifacts Referenced
- `docs/adopted-repo-guide.md`
- `docs/starter-adoption-checklist.md`
- `examples/sample-real-project-pilot-run-notes.md`
- `examples/sample-external-system-mapping.md`

## Recommendation
- Result: Needs Small Fixes
- Notes:
  - 这套 adopted-repo CI guidance 已足够支撑第一次把 placeholder 替换为真实 repo-local 质量命令。
  - 下一步应在真实 adopted repo 中继续确认 security-style checks 的默认接入策略，以及 repo-specific 高风险路径是否补齐。

## Need Human Decision
- 该 adopted repo 是否要把 dependency / static security check 放入默认 PR pipeline，还是先保留为 nightly / manual gate？
- `identity/` 和 `session/` 相关改动是否需要额外 reviewer，而不仅是 release 前复核？

## Next Handoff
- To: Repo Maintainer / Knowledge-Ops Agent
- Goal: 把这份验证样例中的有效做法回填到 adopted repo 的 CI、PR 模板、test plan 和风险路径配置
- Must Read:
  - Selected Minimum Command Set
  - Commands Explicitly Kept Out
  - Validation Results
  - Friction Points
