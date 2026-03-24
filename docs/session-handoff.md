# Session Handoff

## What this repo is
This repo is an AI R&D team starter focused on reusable prompts, SOPs, runbooks, templates, quality gates, integration guides, examples, and Claude Code safety scaffolding.

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

## Best next step
Continue from the highest-value remaining starter work:
1. Use the new `performance-module-*` example chain as the reference when a team needs a Java microservice / internal business system style sample.
2. Validate and tune the reviewer-policy and security-style-check guidance after more adopted-repo feedback appears.
3. Keep `docs/adopted-repo-guide.md` aligned with future real adoption feedback.
4. Refine GitHub / GitLab templates or integration/backlink guidance again only after concrete real-adoption gaps appear.

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
4. `docs/adopted-repo-guide.md`
5. `examples/performance-module-requirement-intake.md`
6. `examples/performance-module-run-notes.md`
7. `examples/sample-real-project-requirement-intake.md`
8. `examples/sample-requirement-intake-validation.md`
9. `examples/sample-real-project-pilot-run-notes.md`
10. `examples/sample-real-adoption-validation.md`
11. `examples/sample-real-adoption-reference-validation.md`
12. `examples/sample-integration-backlink-validation.md`
13. `examples/sample-external-system-mapping.md`
14. `docs/integrations/github.md`
15. `docs/starter-adoption-checklist.md`
16. `.claude/settings.json`
17. `docs/sop/requirement-to-release.md`

## Open work items
- GitHub and GitLab templates now exist, but may need another pass after the new mapping example is tried in a real adoption.
- Integration/backlink guidance now has a dedicated validation example at `examples/sample-integration-backlink-validation.md`; only revisit if later real adoption feedback shows a concrete gap.
- Validate and tune the reviewer-policy and security-style-check guidance after more adopted-repo feedback appears.
- If later real adoption feedback shows a concrete gap, decide whether reviewer expectations should also be surfaced more directly in GitHub / GitLab templates.
- Avoid adding more sample chains unless they introduce a genuinely new workflow shape beyond the existing feature, bugfix, migration, pilot, and Java microservice/internal-system coverage.

## Notes for the next session
- This repo is now tracked in git on `develop`, and the V2.0.0 line is isolated in a separate worktree at `~/ai-rd-team-v2.0.0` on `release/v2.0.0`; keep reusable starter work on `develop` and keep V2.0.0-only positioning / release refinements inside the dedicated worktree.
- Preserve worktree isolation: do not let `develop` absorb V2.0.0-specific positioning/material by accident, and do not mutate the V2.0.0 worktree with generic starter changes that belong back on `develop` first.
- Existing hook validation by `jq` was previously blocked because `jq` is not installed in the environment.
- JSON in `.claude/settings.json` was successfully read back, so the settings file is currently parseable.
- Prioritize reusable docs and safe quality automation before adding more workflow complexity.
