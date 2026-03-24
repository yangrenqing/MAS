# Requirement Intake Card

> Use this template to normalize a raw issue, chat request, or incident follow-up into a structured requirement before PRD handoff. Write `TBD` when information is missing. Put inferred content in `Assumptions`. Keep `Need Human Decision` and `Next Handoff` filled. For first-time adopted repos, decide one `Status Source of Truth` before cross-system syncing, and include the planned artifact path in `Related References` when known.

## Meta
- Requirement ID: {{REQ-YYYY-NNN_or_TBD}}
- Title: {{short_requirement_title}}
- Type: {{feature|improvement|bugfix|migration|tech_debt|incident|ops}}
- Priority: {{P0|P1|P2|P3}}
- Source: {{customer_request|support_ticket|product_request|incident_followup|ops_need|TBD}}
- Requested By: {{team_or_person}}
- Intake Owner: {{agent_or_person}}

## Summary
{{one_paragraph_summary}}

## Tracking and References
- Status Source of Truth: {{jira|github_issue|gitlab_issue|repo_doc|TBD}}
- Primary Tracker ID: {{AUTH-123|repo#123|TBD}}
- Related References:
  - {{reference_1_or_TBD}}
  - {{reference_2_or_TBD}}
  - {{planned_artifact_path_or_TBD}}

## Confirmed Facts
- {{fact_1}}
- {{fact_2}}
- {{fact_3}}

## Assumptions
- {{assumption_1_or_TBD}}
- {{assumption_2_or_TBD}}

## Business Context
- Problem: {{problem_statement}}
- Why Now: {{why_now}}
- Expected Value: {{expected_value}}

## Initial Scope
### In Scope
- {{in_scope_1}}
- {{in_scope_2}}
- {{in_scope_3}}

### Out of Scope
- {{out_scope_1}}
- {{out_scope_2}}
- {{out_scope_3_or_TBD}}

## Risks Seen At Intake
- {{risk_1}}
- {{risk_2}}
- {{risk_3_or_TBD}}

## Missing Information
- {{missing_info_1_or_none}}
- {{missing_info_2_or_none}}
- {{missing_info_3_or_none}}

## Need Human Decision
- {{decision_1_or_none}}
- {{decision_2_or_none}}

## Next Handoff
- To: PRD Agent
- Goal: 将该需求转换为可验收的 PRD
- Must Read:
  - Tracking and References
  - Initial Scope
  - Risks Seen At Intake
  - Missing Information
