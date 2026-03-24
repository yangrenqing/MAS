# Change Summary

## Meta
- Change Summary ID: {{change_summary_id}}
- Requirement ID: {{requirement_id}}
- Release ID: {{release_id_or_none}}
- Owner: {{owner}}
- Date: {{date}}

## Summary
{{one_paragraph_summary}}

## Confirmed Facts
- {{fact_1}}
- {{fact_2}}
- {{fact_3}}

## Assumptions
- {{assumption_1_or_none}}
- {{assumption_2_or_none}}

## Business Change
- User-facing change:
  - {{business_change_1}}
- Non-user-facing change:
  - {{business_change_2_or_none}}

## Technical Change
- Module / Service:
  - {{module_1}}
  - {{module_2_or_none}}
- Data / API / Config:
  - {{change_1}}
  - {{change_2}}

## Risk Summary
- Risk Level: {{low|medium|high}}
- Primary Risk:
  - {{risk_1}}
- Secondary Risk:
  - {{risk_2_or_none}}

## Validation Summary
- Tests run:
  - {{test_1}}
  - {{test_2}}
- Not yet validated:
  - {{gap_1_or_none}}

## Rollback Summary
- Rollback available: {{yes|no}}
- Rollback reference:
  - {{rollback_ref_or_none}}
- Special caution:
  - {{rollback_caution_or_none}}

## Communication Notes
- For release owner:
  - {{release_note_1}}
- For oncall:
  - {{oncall_note_1_or_none}}
- For business stakeholders:
  - {{biz_note_1_or_none}}

## Need Human Decision
- {{decision_1_or_none}}
- {{decision_2_or_none}}

## Next Handoff
- To: QA/Security Agent / Release/SRE Agent
- Goal: 复用本摘要做测试聚焦与发布准备
- Must Read:
  - Technical Change
  - Risk Summary
  - Validation Summary
  - Rollback Summary
