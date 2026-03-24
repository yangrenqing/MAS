# Requirement to Release SOP

## Goal
定义从接收需求到发布上线的标准流程，供人类与 AI agents 统一执行。

## Stage 1: Intake
### Input
- 用户需求
- 产品需求
- 客户反馈
- 事故单

### Owner
- Intake Agent
- 产品负责人

### Output
- 标准需求卡
- 缺失信息清单
- 风险标记

## Stage 2: PRD
### Owner
- PRD Agent
- 产品负责人

### Output
- PRD
- 用户故事
- 验收标准
- 边界条件

## Stage 3: Technical Design
### Owner
- Architect Agent
- 技术负责人

### Output
- 技术方案
- 模块影响分析
- 数据/接口变化
- 回滚点

## Stage 4: Planning
### Owner
- Planning Agent
- Dev Lead Agent

### Output
- 任务树
- 依赖图
- Definition of Done
- 测试要求
- 发布要求

## Stage 5: Development
### Owner
- Developer Agent
- Dev Lead Agent

### Output
- 代码改动
- 自测结论
- 测试补充
- PR 说明

### Rules
- 不扩展需求
- 不做无关重构
- 高风险模块保留人工 review

## Stage 6: QA / Security
### Owner
- QA/Security Agent

### Output
- 测试计划
- 测试结果
- 安全检查结果
- 发布建议

### Gate
- P0 用例通过
- 核心链路通过
- 无高危安全问题
- 有回滚方案

## Stage 7: Release Preparation
### Owner
- Release/SRE Agent
- 发布负责人

### Output
- 发布检查单
- 灰度策略
- 监控项
- 回滚预案

## Stage 8: Release / Observe
### Owner
- 发布负责人
- 值班负责人

### Output
- 发布执行记录
- 观察窗口记录
- 是否继续放量/回滚结论

## Stage 9: Postmortem
### Owner
- Knowledge/Ops Agent
- 技术负责人

### Output
- 复盘文档
- 根因分析
- 规则更新建议
- 模板更新建议

## Global Rules
1. 所有需求必须有验收标准
2. 所有发布必须有回滚方案
3. 生产发布必须保留人工决策点
4. 所有 agent 输出必须包含 Next Handoff
5. 高风险路径必须额外 review 和测试
