# Starter Adoption Checklist

## Goal
用最小成本把这套 starter 适配到一个新的项目仓库，同时保留人工审批点和安全边界。

## Before Copying
- [ ] 明确目标项目主平台：GitHub / GitLab / 其他
- [ ] 明确目标项目技术栈和包管理器
- [ ] 明确目标项目当前 CI 能力
- [ ] 明确哪些路径属于高风险模块
- [ ] 明确发布 / 回滚 / 值班负责人

## Files to Bring In
- [ ] `.claude/agents/*.md`
- [ ] `.claude/settings.json`
- [ ] `docs/templates/*.ai.md`
- [ ] `docs/sop/requirement-to-release.md`
- [ ] `docs/quality-gates.md`
- [ ] `ops/runbooks/incident.md`
- [ ] `ops/runbooks/rollback.md`
- [ ] 目标平台协作模板：`.github/**` 或 `.gitlab/**`

## Adaptation Checklist
### 1. Project context
- [ ] 在 `README.md` 写清项目定位和仓库入口
- [ ] 新建或更新 `docs/project-status.md`
- [ ] 新建或更新 `docs/session-handoff.md`
- [ ] 明确推荐阅读顺序

### 2. Agents and templates
- [ ] 保留 `Need Human Decision` 和 `Next Handoff`
- [ ] 根据项目术语微调 agent prompt
- [ ] 根据项目类型补充必填字段
- [ ] 删除与当前项目无关的样例或占位说明

### 3. Settings and hooks
- [ ] 保留默认安全权限
- [ ] 保留生产 / 发布相关命令的人工确认
- [ ] 按项目实际高风险路径更新 pre-edit review 范围
- [ ] 确认 hooks 只做提醒或 ask，不自动做生产动作
- [ ] 验证 `.claude/settings.json` 可被正常读取

### 4. CI and validation
- [ ] 先确认项目运行时、包管理器和现有命令入口（如 `pnpm` / `npm` / `uv` / `go test` / `make`）
- [ ] 将 CI placeholder 替换为真实 lint 命令
- [ ] 将 CI placeholder 替换为真实 typecheck 命令（如适用）
- [ ] 将 CI placeholder 替换为真实 test 命令
- [ ] 如需要，补充依赖或静态安全检查命令
- [ ] 确认 README、PR/MR 模板、test plan 对这组命令的叫法一致
- [ ] 确认 CI 不包含 staging / production deploy、自动 rollback、自动放量

### 5. Platform workflow
- [ ] 选择 GitHub 或 GitLab 模板并按实际流程微调
- [ ] 建好基础 labels / tags / approval 规则
- [ ] 保护主分支或受保护分支
- [ ] 明确哪些目录 / 文件类型属于额外 reviewer 范围
- [ ] 高风险改动要求额外 review，且不以 CI 通过替代
- [ ] 合并和发布保留人工审批

### 6. Security-style checks
- [ ] 先确认团队是否已有稳定、低噪音的 dependency / static security 主命令
- [ ] 若没有统一主命令，先保留为 nightly / manual / release-prep gate
- [ ] 若要进入默认 PR gate，确认失败输出可读且处理路径明确
- [ ] 不把 deploy、migration apply、rollback、真实外部系统写操作包装成 security-style checks
- [ ] README、PR/MR 模板、test plan、release checklist 对 security-style checks 的位置和叫法一致

### 7. Release guardrails
- [ ] 确认 rollback runbook 可执行
- [ ] 确认 release checklist 适配当前项目
- [ ] 确认 staging / canary 观察项适配当前项目
- [ ] 明确 metrics、alerts、rollback trigger
- [ ] 明确最终放量由人类确认

## First Pilot Recommendation
- [ ] 选择一个中等复杂度需求试跑，不要选最复杂的需求
- [ ] 至少走通：需求 → PRD → 设计 → 测试计划 → 发布准备
- [ ] 记录 friction points 和缺口
- [ ] 试跑后回填到 `docs/project-status.md` 和 `docs/session-handoff.md`
- [ ] 只在试跑后再细化 hooks、模板、CI

## Validation Expectation
- [ ] 新仓库中的 README、project-status、session-handoff 保持一致
- [ ] 所有新增模板仍保持通用，不绑定无关技术栈
- [ ] 没有任何文件暗示自动生产发布
- [ ] 关键人类决策点仍然存在
