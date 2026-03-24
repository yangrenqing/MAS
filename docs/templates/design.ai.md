# Technical Design

## Meta
- Design ID: {{design_id}}
- Requirement ID: {{requirement_id}}
- Title: {{title}}
- Author: {{author}}
- Reviewers: {{reviewers}}
- Target Release: {{target_release}}

## Summary
{{one_paragraph_design_summary}}

## Confirmed Inputs
- PRD Version: {{prd_version}}
- Related Modules:
  - {{module_1}}
  - {{module_2}}
- Constraints:
  - {{constraint_1}}
  - {{constraint_2}}

## Assumptions
- {{assumption_1}}
- {{assumption_2}}

## Technical Goal
- {{technical_goal_1}}
- {{technical_goal_2}}

## Non-Goals
- {{non_goal_1}}
- {{non_goal_2}}

## Current State
- Current Flow: {{current_flow_summary}}
- Current Limitation:
  - {{limitation_1}}
  - {{limitation_2}}

## Proposed Approach
### Overview
{{proposed_approach_summary}}

### Affected Modules
- {{affected_module_1}}
- {{affected_module_2}}
- {{affected_module_3}}

### Step-by-Step Design
1. {{design_step_1}}
2. {{design_step_2}}
3. {{design_step_3}}

## API / Contract Changes
### New
- Name: {{api_name_or_none}}
- Caller: {{caller}}
- Input: {{input}}
- Output: {{output}}
- Errors: {{errors}}

### Modified
- Name: {{api_name_or_none}}
- Change: {{change}}
- Compatibility: {{compatibility}}

### Unchanged but Relevant
- {{relevant_api_1}}

## Data Changes
### Schema / Model
- Add: {{field_or_table_1_or_none}}
- Modify: {{field_or_table_2_or_none}}
- Remove: {{field_or_table_3_or_none}}

### Migration
- Needed: {{yes|no}}
- Plan: {{migration_plan_or_none}}
- Reversible: {{yes|no}}

## Security / Permission Impact
- Auth Impact: {{auth_impact_or_none}}
- Permission Impact: {{permission_impact_or_none}}
- Sensitive Data Impact: {{sensitive_data_impact_or_none}}

## Reliability / Performance Impact
- Latency Risk: {{latency_risk_or_none}}
- Throughput Risk: {{throughput_risk_or_none}}
- Dependency Risk: {{dependency_risk_or_none}}

## Compatibility
- Backward Compatibility: {{yes|no|partial}}
- Forward Compatibility: {{yes|no|partial}}
- Feature Flag Needed: {{yes|no}}
- Rollout Guard: {{guard_or_none}}

## Rollback Plan
- Rollback Trigger:
  - {{trigger_1}}
  - {{trigger_2}}
- Rollback Steps:
  1. {{rollback_step_1}}
  2. {{rollback_step_2}}
  3. {{rollback_step_3}}
- Non-Reversible Risk: {{non_reversible_risk_or_none}}

## Alternatives Considered
### Option A
- Description: {{option_a}}
- Pros:
  - {{pro_1}}
- Cons:
  - {{con_1}}
- Rejected Because: {{reason}}

### Option B
- Description: {{option_b_or_none}}
- Pros:
  - {{pro_1_or_none}}
- Cons:
  - {{con_1_or_none}}
- Rejected Because: {{reason_or_none}}

## Tasking Guidance
- Backend Tasks:
  - {{be_task_1}}
- Frontend Tasks:
  - {{fe_task_1}}
- QA Tasks:
  - {{qa_task_1}}
- Release Tasks:
  - {{release_task_1}}

## Risks
- {{risk_1}}
- {{risk_2}}
- {{risk_3}}

## Need Human Decision
- {{decision_1_or_none}}
- {{decision_2_or_none}}

## Next Handoff
- To: Planning Agent
- Goal: 拆解成可并行执行任务
- Must Read:
  - Affected Modules
  - API / Contract Changes
  - Data Changes
  - Rollback Plan
