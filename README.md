# AI R&D Team Starter

这是一套可直接落地的 **AI 原生研发团队模板**，适合在 Claude Code 中搭建多 agent 协作流程。

## 目录结构

```txt
.claude/
  agents/
    intake.md
    prd.md
    architect.md
    planning.md
    dev-lead.md
    developer.md
    qa-security.md
    release-sre.md
    knowledge-ops.md
  settings.json

.github/
  ISSUE_TEMPLATE/
    feature-request.yml
    bug-report.yml
  workflows/
    ci.yml
  pull_request_template.md

.gitlab/
  issue_templates/
    Feature.md
    Bug.md
  merge_request_templates/
    Default.md

docs/
  project-status.md
  session-handoff.md
  adopted-repo-guide.md
  starter-adoption-checklist.md
  afternoon-pilot-guide.md
  quality-gates.md
  integrations/
    github.md
    gitlab.md
    jira.md
    feishu-or-slack.md
  sop/
    requirement-to-release.md
  templates/
    requirement-intake.ai.md
    prd.ai.md
    design.ai.md
    test-plan.ai.md
    release-checklist.ai.md
    staging-checklist.ai.md
    canary-checklist.ai.md
    change-summary.ai.md
    pilot-run-notes.ai.md
    postmortem.ai.md

examples/
  sample-requirement.md
  sample-bugfix-requirement.md
  sample-backend-migration-requirement.md
  sample-real-project-requirement-intake.md
  sample-prd.md
  sample-backend-migration-prd.md
  sample-design.md
  sample-backend-migration-design.md
  sample-test-plan.md
  sample-backend-migration-test-plan.md
  sample-release-checklist.md
  sample-backend-migration-release-checklist.md
  sample-pilot-run-notes.md
  sample-real-project-pilot-run-notes.md
  sample-requirement-intake-validation.md
  sample-adopted-repo-ci-validation.md
  sample-real-adoption-validation.md
  sample-real-adoption-reference-validation.md
  sample-integration-backlink-validation.md
  sample-external-system-mapping.md
  sample-incident.md
  sample-postmortem.md
  pilot-existing-user-magic-link-requirement.md
  pilot-existing-user-magic-link-prd.md
  pilot-existing-user-magic-link-design.md
  pilot-existing-user-magic-link-test-plan.md
  pilot-existing-user-magic-link-release-checklist.md
  pilot-existing-user-magic-link-run-notes.md
  sample-github-actions-local-quality.yml
  sample-gitlab-ci-local-quality.yml
  performance-module-requirement-intake.md
  performance-module-prd.md
  performance-module-design.md
  performance-module-test-plan.md
  performance-module-release-checklist.md
  performance-module-run-notes.md

ops/
  runbooks/
    incident.md
    rollback.md
```

## 这套模板包含什么

### 1. Agent 模板
位于 `.claude/agents/`。

用途：
- `intake`：接需求，整理需求卡
- `prd`：输出 PRD 与验收标准
- `architect`：输出技术方案
- `planning`：拆任务与依赖
- `dev-lead`：统筹开发分工
- `developer`：实现代码与自测
- `qa-security`：测试与安全验证
- `release-sre`：发布、灰度、回滚、观测
- `knowledge-ops`：复盘与知识沉淀

### 2. AI 文档模板
位于 `docs/templates/`。

用途：
- `requirement-intake.ai.md`：需求 intake / 归一化模板，支持 source-of-truth、主 tracker 与外部回链字段
- `prd.ai.md`：需求定义模板
- `design.ai.md`：技术方案模板
- `test-plan.ai.md`：测试计划模板
- `release-checklist.ai.md`：发布检查单模板
- `staging-checklist.ai.md`：staging 验证模板
- `canary-checklist.ai.md`：canary 观察模板
- `change-summary.ai.md`：变更摘要模板
- `postmortem.ai.md`：事故复盘模板
- `pilot-run-notes.ai.md`：首轮试跑记录模板

这些模板是为 **AI agent 自动填写**设计的，特点是：
- 标题固定
- 字段原子化
- 区分 confirmed / assumptions / unknowns
- 强制 handoff 给下游角色
- 保留 `Need Human Decision` 与 `Next Handoff`

### 3. SOP、质量门禁与 Runbook
位于 `docs/`、`docs/sop/` 和 `ops/runbooks/`。

用途：
- `docs/sop/requirement-to-release.md`：从需求到发布的标准流程
- `docs/quality-gates.md`：各阶段最小质量门禁
- `ops/runbooks/incident.md`：事故响应 runbook
- `ops/runbooks/rollback.md`：回滚 runbook

### 4. 持续上下文文档
位于 `docs/`。

用途：
- `docs/project-status.md`：记录当前仓库完成度、缺口与优先级
- `docs/session-handoff.md`：记录下次进入时的推荐阅读顺序和剩余工作
- `docs/starter-adoption-checklist.md`：把这套 starter 迁移到新仓库时的最小接入清单
- `docs/adopted-repo-guide.md`：说明 adopted repo 中的文档落位、owner 映射、高风险路径调优、reviewer-policy baseline 与 security-style-check rollout
- `docs/afternoon-pilot-guide.md`：指导首次在半天内试跑一遍 starter

### 5. 外部系统集成说明
位于 `docs/integrations/`。

用途：
- `github.md`：GitHub issue / PR / release 接法
- `gitlab.md`：GitLab issue / MR / environment 接法
- `jira.md`：Jira workflow / field / ticket 接法
- `feishu-or-slack.md`：通知、升级、人工确认消息接法

### 6. 可选本地 workflow 示例
位于 `examples/`。

用途：
- `sample-github-actions-local-quality.yml`：GitHub Actions 的本地质量检查示例
- `sample-gitlab-ci-local-quality.yml`：GitLab CI 的本地质量检查示例
- 仅提供 adoption 后的占位骨架，不包含 staging / production deploy
- 用于帮助新仓库把 placeholder CI 收敛到真实 lint / typecheck / test / security-style checks
- 具体替换顺序、常见误区与最小验证方法见 `docs/adopted-repo-guide.md`

### 7. 端到端样例
位于 `examples/`。

用途：
- 用一个真实感较强的需求样例把模板串起来
- 帮助下次直接试跑整套流程
- 当前样例：`Existing User Magic Link Login`
- 附加样例：`Expired Password Reset Link Shows 500 Error` bugfix 需求卡
- 真实项目风格 intake 示例：`sample-real-project-requirement-intake.md`
- 迁移样例：`Migrate Legacy Order Billing JSON to Structured Columns`
- Java 微服务 / 内部业务系统风格样例：`performance-module-{requirement-intake,prd,design,test-plan,release-checklist,run-notes}.md`
- 内部 pilot 链路：`pilot-existing-user-magic-link-{requirement,prd,design,test-plan,release-checklist,run-notes}.md`
- 试跑记录示例：`sample-pilot-run-notes.md`
- 真实项目风格 pilot 示例：`sample-real-project-pilot-run-notes.md`
- adopted repo CI 替换验证示例：`sample-adopted-repo-ci-validation.md`
- 真实项目风格整体 adoption 验证示例：`sample-real-adoption-validation.md`
- adoption readiness 参考样例充分性验证：`sample-real-adoption-reference-validation.md`
- requirement-intake 模板验证示例：`sample-requirement-intake-validation.md`
- integration / backlink guidance 验证示例：`sample-integration-backlink-validation.md`
- 外部系统编号映射示例：`sample-external-system-mapping.md`
- 事故处理示例：`sample-incident.md` 与 `sample-postmortem.md`

## 如何使用

## 方式一：把这里当成模板仓库
1. 在这里维护 agent prompt 和模板。
2. 新项目启动时，把 `.claude/agents/` 和 `docs/templates/` 复制到目标项目。
3. 根据项目技术栈和组织方式微调。

## 方式二：直接在这个目录继续扩展
1. 在本目录下继续补充：
   - `docs/templates/*.md`
   - `docs/integrations/*.md`
   - `examples/*`
   - `ops/runbooks/*.md`
   - `.github/workflows/*.yml` 或 `.gitlab-ci.yml`
2. 把这里作为 AI 研发流程的母版项目。

## 建议工作流

推荐按下面顺序使用：

1. `intake` 接收原始需求
2. `prd` 输出 PRD
3. `architect` 输出技术方案
4. `planning` 拆任务
5. `dev-lead` 分发开发任务
6. `developer` 实现代码
7. `qa-security` 做验证
8. `release-sre` 生成发布方案
9. `knowledge-ops` 做复盘沉淀

如果要快速试跑，可以直接参考：
1. `examples/sample-requirement.md`
2. `examples/sample-prd.md`
3. `examples/sample-design.md`
4. `examples/sample-test-plan.md`
5. `examples/sample-release-checklist.md`

如果要参考高风险迁移类场景，可以直接看：
1. `examples/sample-backend-migration-requirement.md`
2. `examples/sample-backend-migration-prd.md`
3. `examples/sample-backend-migration-design.md`
4. `examples/sample-backend-migration-test-plan.md`
5. `examples/sample-backend-migration-release-checklist.md`

如果要参考 Java 微服务 / 内部业务系统风格样例，可以直接看：
1. `examples/performance-module-requirement-intake.md`
2. `examples/performance-module-prd.md`
3. `examples/performance-module-design.md`
4. `examples/performance-module-test-plan.md`
5. `examples/performance-module-release-checklist.md`
6. `examples/performance-module-run-notes.md`

## settings.json 说明

项目级 Claude Code 配置位于：
- `.claude/settings.json`

当前配置包含：
- 安全默认权限
- 禁用 bypass-permissions 模式
- 基础 hooks 脚手架
- 项目级环境变量示例

### 当前权限策略
- 默认允许：`Read` / `Grep` / `Glob` / `Agent`
- 高风险命令保持询问或禁止
- 发布、强制推送、硬重置等动作不会被默认放开

### 当前 hooks 脚手架
- `PostToolUse` on `Write|Edit`：写完文件后提醒补充自测、影响范围、测试更新说明
- `PreToolUse` on `Bash`：命中 deploy/release/production/canary/rollback 等关键词时，自动要求人工确认
- `PreToolUse` on `Edit`：命中 `auth/payments/billing/orders/db/migrations/sql` 等高风险路径时，注入额外 review 与回滚提醒

### 关于 agents 目录
Claude Code 会自动识别 `.claude/agents/` 下的自定义 agents，**不需要额外配置 agent directory 字段**。

## 已包含的项目骨架

当前目录已经包含这几类基础文件：

1. agent prompts：`.claude/agents/*.md`
2. Claude Code 项目配置：`.claude/settings.json`
3. GitHub / GitLab 协作模板：`.github/**`、`.gitlab/**`
4. 核心模板：`docs/templates/*.ai.md`
5. 质量门禁与流程文档：`docs/quality-gates.md`、`docs/sop/requirement-to-release.md`
6. 运行手册：`ops/runbooks/*.md`
7. 持续上下文文档：`docs/project-status.md`、`docs/session-handoff.md`
8. 外部系统集成说明：`docs/integrations/*.md`
9. 新仓库接入清单：`docs/starter-adoption-checklist.md`
10. adopted repo 落地指南：`docs/adopted-repo-guide.md`
11. 端到端样例：`examples/*`
12. 可选本地 workflow 示例：`examples/sample-github-actions-local-quality.yml`、`examples/sample-gitlab-ci-local-quality.yml`

## 推荐阅读顺序

下次进入这个仓库时，建议先读：
1. `README.md`
2. `docs/project-status.md`
3. `docs/session-handoff.md`
4. `docs/adopted-repo-guide.md`
5. `examples/sample-real-project-requirement-intake.md`
6. `examples/sample-requirement-intake-validation.md`
7. `examples/sample-real-project-pilot-run-notes.md`
8. `examples/sample-real-adoption-validation.md`
9. `examples/sample-real-adoption-reference-validation.md`
10. `examples/sample-integration-backlink-validation.md`
11. `examples/sample-external-system-mapping.md`
12. `docs/integrations/github.md`
13. `docs/starter-adoption-checklist.md`
14. `.claude/settings.json`
15. `docs/sop/requirement-to-release.md`

## Prompt / 模板使用规则

建议在所有 agent prompt 中统一加入这几条：

1. 必须严格按模板输出
2. 无信息时写 `TBD`
3. 推测内容写入 `Assumptions`
4. 需要人确认的事项写入 `Need Human Decision`
5. 输出必须能直接交给下游 agent 使用

## 适合的落地方式

如果你是从零开始搭团队，建议先启用这 5 个核心 agent：

- `intake`
- `architect`
- `planning`
- `developer`
- `qa-security`

等流程稳定后，再启用：
- `dev-lead`
- `release-sre`
- `knowledge-ops`

## 注意事项

- 不要一开始就追求全自动发版
- 关键模块必须保留人工 review
- 生产发布必须保留人工决策点
- 所有 agent 输出都应包含 `Next Handoff`
- 不能以自动化为理由绕过质量门禁

## 你可以继续做什么

当前这套 starter 已经完成一轮内部 pilot，下一步通常是：

1. 先读 `docs/project-status.md` 和 `docs/session-handoff.md`，确认当前最高优先级
2. 在真实 adopted repo 中验证 reviewer-policy 与 security-style-check guidance 是否足够
3. 只在真实 adoption 暴露具体缺口时，再继续细化 `docs/adopted-repo-guide.md`、GitHub / GitLab 模板或 integration/backlink guidance
4. 视技术栈把 placeholder CI 替换成真实 lint / typecheck / test / security-style checks，但不要加入 staging / production deploy
5. 把后续真实 adoption 反馈回填到 `docs/project-status.md`、`docs/session-handoff.md` 和相关 examples
