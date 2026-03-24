# Session Handoff

## What this repo is
This repo is an AI R&D team starter focused on reusable prompts, SOPs, runbooks, templates, quality gates, integration guides, examples, and Claude Code safety scaffolding.
Real product implementations should happen in adopted repos, with only reusable adoption findings fed back into this starter.

## What was completed before this handoff
- Agent prompts were created under `.claude/agents/`.
- AI templates were created under `docs/templates/`.
- One internal starter pilot was completed for `Existing User Magic Link Login`, producing:
  - `examples/pilot-existing-user-magic-link-requirement.md`
  - `examples/pilot-existing-user-magic-link-prd.md`
  - `examples/pilot-existing-user-magic-link-design.md`
  - `examples/pilot-existing-user-magic-link-test-plan.md`
  - `examples/pilot-existing-user-magic-link-release-checklist.md`
  - `examples/pilot-existing-user-magic-link-run-notes.md`
- A Java microservice / internal business system style example chain was added for a Moka-style performance module:
  - `examples/performance-module-requirement-intake.md`
  - `examples/performance-module-prd.md`
  - `examples/performance-module-design.md`
  - `examples/performance-module-test-plan.md`
  - `examples/performance-module-release-checklist.md`
  - `examples/performance-module-run-notes.md`
- The performance-module run notes concluded the starter is `Reusable As-Is` for this Java microservice / internal business system scenario.
- Additional quality-stage templates were added:
  - `docs/templates/staging-checklist.ai.md`
  - `docs/templates/canary-checklist.ai.md`
  - `docs/templates/change-summary.ai.md`
  - `docs/templates/pilot-run-notes.ai.md`
- The main SOP was created at `docs/sop/requirement-to-release.md`.
- `docs/quality-gates.md` was added to define stage-by-stage minimum gates.
- Incident and rollback runbooks were created under `ops/runbooks/`.
- `.claude/settings.json` was updated with practical hook scaffolding:
  - post-edit reminder for self-check / tests
  - pre-Bash release/production keyword review
  - pre-edit high-risk path review context
- `README.md` was updated to reflect the current skeleton.
- `docs/project-status.md` was added to capture current repository state.
- `docs/starter-adoption-checklist.md` was added for adapting this starter to new repos.
- `docs/adopted-repo-guide.md` was added to explain artifact placement, owner mapping, and repo-specific risk-path tuning in adopted repos.
- `docs/afternoon-pilot-guide.md` was added to help run one first pilot in a single afternoon.
- Auto-memory files were persisted for stable cross-session context.
- Integration docs were added under `docs/integrations/`.
- End-to-end sample artifacts were added under `examples/`.
- Additional example artifacts were added:
  - `examples/sample-bugfix-requirement.md`
  - `examples/sample-pilot-run-notes.md`
  - `examples/sample-real-project-pilot-run-notes.md`
  - `examples/sample-real-project-requirement-intake.md`
  - `examples/sample-requirement-intake-validation.md`
  - `examples/sample-adopted-repo-ci-validation.md`
  - `examples/sample-real-adoption-validation.md`
  - `examples/sample-real-adoption-reference-validation.md`
  - `examples/sample-integration-backlink-validation.md`
  - `examples/sample-external-system-mapping.md`
  - `examples/sample-incident.md`
  - `examples/sample-postmortem.md`
  - `examples/sample-backend-migration-requirement.md`
  - `examples/sample-backend-migration-prd.md`
  - `examples/sample-backend-migration-design.md`
  - `examples/sample-backend-migration-test-plan.md`
  - `examples/sample-backend-migration-release-checklist.md`
- `docs/templates/requirement-intake.ai.md` was refined to capture:
  - status source of truth
  - primary tracker ID
  - related references / backlinks for adopted repos
  - explicit `Must Read` handoff focus for the PRD stage
- `docs/integrations/github.md`, `docs/integrations/gitlab.md`, and `docs/integrations/jira.md` were refined to align on:
  - status source of truth
  - primary tracker ID
  - cross-system backlink expectations for adopted repos
- `examples/sample-integration-backlink-validation.md` was added to validate that the current integration/backlink guidance is already sufficient as a first adopted-repo reference; current conclusion is `Reusable As-Is`.
- GitHub issue / PR templates and a non-deploy CI skeleton were added under `.github/`.
- GitLab issue / MR templates were added under `.gitlab/`.
- Optional local workflow example skeletons were added under `examples/`:
  - `examples/sample-github-actions-local-quality.yml`
  - `examples/sample-gitlab-ci-local-quality.yml`
- `docs/adopted-repo-guide.md` and `docs/starter-adoption-checklist.md` were refined with more concrete adopted-repo guidance covering placeholder-CI replacement, minimum reviewer policy, and safe security-style-check rollout.
- `docs/sdd-v2.0.0-positioning.md` was added to make the repo's SDD boundary explicit: this starter is SDD-capable and SDD-oriented, but its final maturity must be proven in adopted repos.
- GitHub / GitLab PR/MR templates were upgraded to request traceability links plus spec-change-control notes.
- `docs/templates/change-summary.ai.md`, `docs/templates/release-checklist.ai.md`, and `docs/quality-gates.md` were refined to make traceability and behavior-change sync expectations more visible.

## Best next step
Continue from the highest-value remaining starter work:
1. Validate the new V2.0.0 traceability / spec-change-control expectations in one real adopted repo before making them stricter defaults.
2. Use the `performance-module-*` example chain as the reference when a team needs a Java microservice / internal business system style sample.
3. Validate and tune the reviewer-policy and security-style-check guidance after more adopted-repo feedback appears.
4. Keep `docs/adopted-repo-guide.md` aligned with future real adoption feedback.
5. Refine GitHub / GitLab templates or integration/backlink guidance again only after concrete real-adoption gaps appear.

## Important constraints
- Keep the repo generic for any project.
- Do not add production deployment automation.
- Preserve human decision points for release, rollback, and high-risk paths.
- Follow the existing structured template style.
- Treat `.claude/settings.json` as an incrementally extended baseline.

## Important files to read first next time
1. `README.md`
2. `docs/project-status.md`
3. `docs/session-handoff.md`
4. `docs/sdd-v2.0.0-positioning.md`
5. `docs/adopted-repo-guide.md`
6. `examples/performance-module-requirement-intake.md`
7. `examples/performance-module-run-notes.md`
8. `examples/sample-real-project-requirement-intake.md`
9. `examples/sample-requirement-intake-validation.md`
10. `examples/sample-real-project-pilot-run-notes.md`
11. `examples/sample-real-adoption-validation.md`
12. `examples/sample-real-adoption-reference-validation.md`
13. `examples/sample-integration-backlink-validation.md`
14. `examples/sample-external-system-mapping.md`
15. `docs/integrations/github.md`
16. `docs/starter-adoption-checklist.md`
17. `.claude/settings.json`
18. `docs/sop/requirement-to-release.md`

## Open work items
- GitHub and GitLab templates now include traceability and spec-change-control prompts, but may need another pass after one real adopted repo tries them end-to-end.
- Integration/backlink guidance now has a dedicated validation example at `examples/sample-integration-backlink-validation.md`; only revisit if later real adoption feedback shows a concrete gap.
- Validate and tune the reviewer-policy and security-style-check guidance after more adopted-repo feedback appears.
- Decide which V2.0.0 traceability fields should remain template prompts versus becoming stricter review / CI expectations.
- Avoid adding more sample chains unless they introduce a genuinely new workflow shape beyond the existing feature, bugfix, migration, pilot, and Java microservice/internal-system coverage.

## Notes for the next session
- This worktree is the dedicated `release/v2.0.0` line at `~/ai-rd-team-v2.0.0`; keep V2.0.0 positioning, release-facing wording, and traceability/spec-change-control refinements here only.
- Preserve worktree isolation: do not let generic starter work drift into this V2.0.0 worktree if it belongs on `~/ai-rd-team` / `develop`, and do not copy V2.0.0-only positioning back into `develop` by accident.
- Existing hook validation by `jq` was previously blocked because `jq` is not installed in the environment.
- Hook behavior was later validated with Python-based checks, and `.claude/settings.json` is parseable.
- Prioritize reusable docs and safe quality automation before adding more workflow complexity.
