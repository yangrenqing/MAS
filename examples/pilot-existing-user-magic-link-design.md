# Technical Design

## Meta
- Design ID: DES-PILOT-2026-001
- Requirement ID: REQ-PILOT-2026-001
- Title: Existing User Magic Link Login
- Author: Architect Agent
- Reviewers: Tech Lead, Security Reviewer
- Target Release: 2026-04 Wave 1

## Summary
在现有认证体系上增加 magic link 登录分支：用户请求一次性登录链接，系统生成短期有效、单次使用的令牌并通过邮件发送，点击后完成登录。首发通过 feature flag 控制，并以 canary 方式逐步放量。

## Confirmed Inputs
- PRD Version: pilot-existing-user-magic-link-prd.md v1
- Related Modules:
  - web-login-ui
  - auth-service
  - notification-service
- Constraints:
  - 不暴露账户存在性
  - 首发必须支持 feature flag 和 canary

## Assumptions
- 事务邮件服务支持新增 magic link 模板。
- 现有 session 创建逻辑可复用，不需要重写登录后会话流程。

## Technical Goal
- 在不破坏现有密码登录的前提下增加可灰度的 magic link 登录能力。
- 保证令牌单次使用、可过期、可审计，并具备基础风控能力。

## Non-Goals
- 重构整个认证系统。
- 支持移动端深链或第三方身份提供商。

## Current State
- Current Flow: Web 用户当前仅可通过邮箱 + 密码登录，忘记密码后需要走重置流程。
- Current Limitation:
  - 忘记密码用户恢复登录成本较高。
  - 当前登录入口无法服务“只想快速回来继续操作”的回访用户。

## Proposed Approach
### Overview
在登录页增加 magic link 入口；请求接口在校验邮箱后生成随机一次性令牌并写入短期存储，邮件服务发送带签名参数的登录链接；验证接口校验令牌、状态、有效期、redirect target 和风控信息，成功后复用现有 session 创建逻辑完成登录，并将令牌标记为已使用。

### Affected Modules
- web-login-ui
- auth-service
- notification-service

### Step-by-Step Design
1. Web 登录页新增 magic link 请求入口，并在提交后展示统一成功提示，不暴露账号是否存在。
2. auth-service 提供 `POST /auth/magic-link/request` 与 `POST /auth/magic-link/verify` 两个接口，前者生成并存储一次性令牌，后者完成验证与登录。
3. notification-service 增加 magic link 邮件模板；发布时通过 feature flag 控制入口展示，并用 canary 观察成功率、错误率和滥用情况。

## API / Contract Changes
### New
- Name: POST /auth/magic-link/request
- Caller: web-login-ui
- Input: email, redirect_target
- Output: accepted=true
- Errors: rate_limited, temporarily_unavailable

### Modified
- Name: POST /auth/session
- Change: 无接口变更，复用现有会话创建逻辑作为 magic link 验证成功后的内部调用。
- Compatibility: backward compatible

### Unchanged but Relevant
- existing email-password login API
- password reset request API

## Data Changes
### Schema / Model
- Add: Redis 中新增短期 magic_link token record，包含 user_id、expires_at、used=false、redirect_target、request_metadata。
- Modify: none
- Remove: none

### Migration
- Needed: no
- Plan: none
- Reversible: yes

## Security / Permission Impact
- Auth Impact: 新增无需密码的登录路径，必须确保令牌随机性、短时有效和单次使用。
- Permission Impact: 登录后权限模型不变，沿用现有 session / role 判定。
- Sensitive Data Impact: 邮件内容和日志中不得输出完整 token，仅保留审计所需最小信息。

## Reliability / Performance Impact
- Latency Risk: 发送邮件接口依赖外部邮件服务，可能影响响应时间。
- Throughput Risk: 高频请求可能放大邮件服务和 rate limiter 压力。
- Dependency Risk: 邮件服务异常会直接影响功能可用性。

## Compatibility
- Backward Compatibility: yes
- Forward Compatibility: partial
- Feature Flag Needed: yes
- Rollout Guard: `auth_magic_link_login` feature flag + canary entry percentage

## Rollback Plan
- Rollback Trigger:
  - magic link verify error rate 持续高于阈值
  - 登录成功率明显低于密码登录基线
- Rollback Steps:
  1. 关闭 `auth_magic_link_login` feature flag。
  2. 停止继续放量并保留密码登录为默认入口。
  3. 观察错误率恢复后再清理未使用的短期令牌数据。
- Non-Reversible Risk: none

## Alternatives Considered
### Option A
- Description: 使用完全无状态的签名 token，验证时不落库存储。
- Pros:
  - 无需额外短期存储。
- Cons:
  - 更难实现单次使用与主动失效。
- Rejected Because: 首发更重视单次使用与回滚控制，状态化方案更稳妥。

### Option B
- Description: 使用关系型数据库表存储 magic link token。
- Pros:
  - 易于审计与查询。
- Cons:
  - 需要新增表与迁移，增加首发范围。
- Rejected Because: 当前需求更适合使用短期存储快速落地，避免引入 migration 风险。

## Tasking Guidance
- Backend Tasks:
  - 新增 request / verify 接口、令牌存储与 rate limit。
- Frontend Tasks:
  - 登录页入口、请求表单、成功/失败提示页。
- QA Tasks:
  - 覆盖过期、重复使用、重放、跳转目标校验等用例。
- Release Tasks:
  - 配置 feature flag、监控登录成功率与 verify 错误率。

## Risks
- redirect target 校验不严可能引入开放重定向风险。
- 令牌重复使用或审计不足会放大认证风险。
- 邮件延迟可能降低用户体验并影响发布评估。

## Need Human Decision
- redirect target 是否仅允许站内白名单路径？
- magic link 首发是否限制为已验证邮箱且近 90 天活跃用户？

## Next Handoff
- To: Planning Agent
- Goal: 拆解成可并行执行任务
- Must Read:
  - Affected Modules
  - API / Contract Changes
  - Data Changes
  - Rollback Plan
