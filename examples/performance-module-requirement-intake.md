# Performance Module Requirement Intake Card

## Meta
- Requirement ID: REQ-PERF-2026-001
- Title: Moka-Style Performance Management Module
- Type: feature
- Priority: P1
- Source: product_request
- Requested By: HR Systems Team
- Intake Owner: Intake Agent

## Summary
将“搭建一个用于学习与演练的 Moka 风格绩效模块项目”的原始想法，归一化为可继续流转到 PRD 的标准需求卡。目标是在一个采用 starter 的真实项目风格场景里，围绕 JDK 17、Spring Boot、Spring Cloud、Nacos 与 MySQL，完成绩效周期、模板、绩效计划、自评与主管评分的首轮闭环设计，并验证这套 starter 是否能稳定支撑企业内部业务系统需求链路。

## Tracking and References
- Status Source of Truth: jira
- Primary Tracker ID: PERF-101
- Related References:
  - GitHub Issue: moka-perf-demo#12
  - Architecture Epic: PERF-ARCH-07
  - Planned Artifact Path: docs/ai/requirements/REQ-PERF-2026-001.md

## Confirmed Facts
- 目标练手项目技术栈已明确为 JDK 17、Spring Boot、Spring Cloud、Nacos 与 MySQL。
- 目标业务域为 Moka 风格绩效管理模块，而非通用 CMS 或电商场景。
- 首轮试跑希望覆盖一条从需求到发布准备的完整文档链路，而不是直接实现完整生产系统。

## Assumptions
- 首轮交付会采用 `performance-service`、`organization-service` 与 `gateway` 的基础服务拆分，用于承载绩效域与组织域边界。
- 第一阶段只覆盖内部绩效流程最小闭环，不接入薪酬、晋升、OKR、360 评估或真实审批流引擎。

## Business Context
- Problem: 团队需要一个足够真实但范围可控的业务项目，用来练习 starter 在 adopted repo 场景下的多角色协作与 handoff。
- Why Now: 当前 starter 已完成一轮内部 pilot，需要借助更接近真实企业内部系统的样例继续验证可复用性。
- Expected Value: 形成一套更贴近 Java 微服务团队的参考需求链路，同时暴露模板、集成与质量门禁在真实业务背景下的缺口。

## Initial Scope
### In Scope
- 绩效周期管理，包括创建、发布、关闭周期。
- 绩效模板管理，包括指标项、权重与评分方式配置。
- 绩效计划生成与执行，包括员工自评、主管评分与基础状态流转。

### Out of Scope
- 薪酬计算、调薪联动与奖金发放。
- 多轮校准会、跨部门评审与 360 评估。
- 原生移动端、企业微信或钉钉集成。

## Risks Seen At Intake
- 绩效数据包含敏感的人事信息，角色与数据访问边界必须保留人工 review。
- 周期、模板与绩效单状态流转较多，若规则定义不清，容易出现错评、漏评或重复提交。
- 组织架构与汇报关系若来自外部主数据，边界和同步策略不清会影响首轮设计稳定性。

## Missing Information
- 首轮评分模型是五分制、百分制还是自定义等级制，仍需确认。
- 主管评分链路是仅一级主管审批，还是需要支持二级主管复核，仍需确认。
- 绩效计划是按周期批量生成，还是允许员工按条件补建，仍需确认。

## Need Human Decision
- 首轮版本是否只支持“员工自评 + 一级主管评分”的最小闭环？
- 首轮样例是否把组织与员工主数据视为只读上游依赖，而不在本项目内实现完整主数据管理？

## Next Handoff
- To: PRD Agent
- Goal: 将该需求转换为可验收的 PRD
- Must Read:
  - Tracking and References
  - Initial Scope
  - Risks Seen At Intake
  - Missing Information
