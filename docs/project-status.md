# Project Status

## Repository Role
- This repository is the starter kit for an AI-native R&D team.
- It is intended to be reused across projects as a template repo / pilot baseline.
- It currently focuses on process scaffolding, agent prompts, templates, quality gates, integration guidance, and safe operational guardrails rather than product-specific code.

## Current Completion

### Already in place
- Claude Code custom agents:
  - `.claude/agents/intake.md`
  - `.claude/agents/prd.md`
  - `.claude/agents/architect.md`
  - `.claude/agents/planning.md`
  - `.claude/agents/dev-lead.md`
  - `.claude/agents/developer.md`
  - `.claude/agents/qa-security.md`
  - `.claude/agents/release-sre.md`
  - `.claude/agents/knowledge-ops.md`
- Claude Code project settings with safe defaults:
  - `.claude/settings.json`
- AI-fillable templates:
  - `docs/templates/requirement-intake.ai.md`
  - `docs/templates/prd.ai.md`
  - `docs/templates/design.ai.md`
  - `docs/templates/test-plan.ai.md`
  - `docs/templates/release-checklist.ai.md`
  - `docs/templates/staging-checklist.ai.md`
  - `docs/templates/canary-checklist.ai.md`
  - `docs/templates/change-summary.ai.md`
  - `docs/templates/pilot-run-notes.ai.md`
  - `docs/templates/postmortem.ai.md`
- Process docs:
  - `docs/sop/requirement-to-release.md`
  - `docs/quality-gates.md`
- Operational runbooks:
  - `ops/runbooks/incident.md`
  - `ops/runbooks/rollback.md`
- Continuity docs:
  - `README.md`
  - `docs/project-status.md`
  - `docs/session-handoff.md`
  - `docs/starter-adoption-checklist.md`
  - `docs/adopted-repo-guide.md`
  - `docs/afternoon-pilot-guide.md`
- Integration guides:
  - `docs/integrations/github.md`
  - `docs/integrations/gitlab.md`
  - `docs/integrations/jira.md`
  - `docs/integrations/feishu-or-slack.md`
  - GitHub / GitLab / Jira guides now include source-of-truth, primary tracker, and backlink baseline guidance for adopted repos
- Collaboration templates:
  - `.github/ISSUE_TEMPLATE/feature-request.yml`
  - `.github/ISSUE_TEMPLATE/bug-report.yml`
  - `.github/pull_request_template.md`
  - `.gitlab/issue_templates/Feature.md`
  - `.gitlab/issue_templates/Bug.md`
  - `.gitlab/merge_request_templates/Default.md`
- Optional local workflow examples:
  - `examples/sample-github-actions-local-quality.yml`
  - `examples/sample-gitlab-ci-local-quality.yml`
  - `docs/adopted-repo-guide.md` and `docs/starter-adoption-checklist.md` now include more concrete adopted-repo guidance for placeholder-CI replacement, minimum reviewer policy, and safe security-style-check rollout
- End-to-end examples:
  - `examples/sample-requirement.md`
  - `examples/sample-bugfix-requirement.md`
  - `examples/sample-backend-migration-requirement.md`
  - `examples/sample-real-project-requirement-intake.md`
  - `examples/sample-prd.md`
  - `examples/sample-backend-migration-prd.md`
  - `examples/sample-design.md`
  - `examples/sample-backend-migration-design.md`
  - `examples/sample-test-plan.md`
  - `examples/sample-backend-migration-test-plan.md`
  - `examples/sample-release-checklist.md`
  - `examples/sample-backend-migration-release-checklist.md`
  - `examples/sample-pilot-run-notes.md`
  - `examples/sample-real-project-pilot-run-notes.md`
  - `examples/sample-requirement-intake-validation.md`
  - `examples/sample-adopted-repo-ci-validation.md`
  - `examples/sample-real-adoption-validation.md`
  - `examples/sample-real-adoption-reference-validation.md`
  - `examples/sample-integration-backlink-validation.md`
  - `examples/sample-external-system-mapping.md`
  - `examples/sample-incident.md`
  - `examples/sample-postmortem.md`
- Recorded internal pilot chain:
  - `examples/pilot-existing-user-magic-link-requirement.md`
  - `examples/pilot-existing-user-magic-link-prd.md`
  - `examples/pilot-existing-user-magic-link-design.md`
  - `examples/pilot-existing-user-magic-link-test-plan.md`
  - `examples/pilot-existing-user-magic-link-release-checklist.md`
  - `examples/pilot-existing-user-magic-link-run-notes.md`
- Java microservice / internal business system example chain:
  - `examples/performance-module-requirement-intake.md`
  - `examples/performance-module-prd.md`
  - `examples/performance-module-design.md`
  - `examples/performance-module-test-plan.md`
  - `examples/performance-module-release-checklist.md`
  - `examples/performance-module-run-notes.md`

### Current config posture
- Safe-by-default permissions are enabled in `.claude/settings.json`.
- Production/release-like Bash commands are flagged for human confirmation.
- High-risk edit paths such as auth/payments/billing/orders/db/migrations/sql are flagged for stronger review context.
- Post-edit reminders ask for self-check, impact summary, test status, and follow-up risk notes.
- The repository is not currently a git repository.

## What is still missing

### High-priority missing pieces
- Validate and tune the new reviewer-policy and security-style-check guidance after more adopted-repo feedback appears

### Recently refined
- `docs/adopted-repo-guide.md` now includes a concrete minimum reviewer-policy baseline for adopted repos, including small-team fallback and explicit statements that CI, hooks, and AI-generated artifacts do not replace human review.
- `docs/adopted-repo-guide.md` now includes a safer rollout model for security-style checks, clarifying when they belong in manual, nightly, or default PR gates.
- `docs/starter-adoption-checklist.md` now includes explicit checklist items for reviewer scope, extra review expectations, and safe security-style-check rollout.


### Medium-priority missing pieces
- Another refinement pass after teams try the updated integration/backlink guidance in a real adoption
- Keep adopted-repo guidance and platform templates aligned with future real adoption feedback

## Recommended next actions
1. Keep this repo technology-agnostic and process-first.
2. Add CI only for lint / typecheck / test / security-style checks, never for direct production deploy.
3. Validate the new reviewer-policy and security-style-check guidance against future adopted-repo feedback before expanding it further.
4. Keep adopted-repo guidance and platform templates aligned with future real-adoption findings.

## Constraints and guardrails
- Do not add automation that can deploy or change production directly.
- Keep human approval points for release, rollback, and high-risk changes.
- Reuse the existing AI template structure:
  - Confirmed Facts
  - Assumptions
  - Need Human Decision
  - Next Handoff
- Extend existing settings incrementally rather than replacing the current permission model.

## Resume priority

### P0
- Validate and tune the new reviewer-policy and security-style-check guidance after real adoption feedback

### P1
- Another refinement pass after teams try the updated integration/backlink guidance in a real adoption
- Template and integration refinements based on pilot findings
- Keep adopted-repo guidance aligned with future pilot findings

### P2
- More examples under `examples/` only if they add a genuinely new workflow shape beyond the existing feature, bugfix, migration, pilot, and Java microservice/internal-system references

## Validation expectations
- README and project-status stay aligned.
- Newly added docs remain generic and reusable across projects.
- No new file should imply direct production execution.
- Future sessions should be able to resume by reading:
  1. `README.md`
  2. `docs/project-status.md`
  3. `docs/session-handoff.md`
  4. auto-memory files
