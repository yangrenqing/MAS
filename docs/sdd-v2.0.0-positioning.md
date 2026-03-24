# AI R&D Team Starter — SDD Positioning V2.0.0

## Purpose

This document defines the **V2.0.0 positioning** of this repository from an SDD perspective.

Important boundary:
- this repository is **not** a product/business implementation repo
- it is a **starter / operating system for an AI-native R&D team**
- therefore, the main question is not whether this repo itself is already a fully spec-driven product
- the main question is whether this repo already provides enough structure to help an adopted repo become more spec-driven

---

## V2.0.0 conclusion

Current classification:

> **AI-native R&D operating system + Mini-SDD skeleton + SDD-ready starter**

It is already well beyond prompt-only AI collaboration, because it has:
- structured requirement intake
- PRD / design / test-plan / release templates
- quality gates
- rollback and incident runbooks
- handoff and continuity documents
- integration/backlink guidance
- internal pilot chains and example chains

But it is still not the same as a fully closed-loop SDD system, because the following are not yet fully enforced across real adopted repos:
- end-to-end traceability from requirement to release evidence
- formal spec change control
- workflow-level enforcement through PR / MR / CI / hooks
- proof from repeated real adopted-repo execution

So the correct V2.0.0 reading is:

> this repo is **SDD-capable and SDD-oriented**, but not yet a proven fully closed-loop SDD implementation standard.

---

## What is already strong in this starter

### 1. Structured upstream specification assets already exist
The starter already provides reusable artifacts for:
- requirement intake
- PRD
- technical design
- test plan
- release checklist
- staging / canary / change summary / pilot notes
- postmortem

This means the repo already treats specifications as first-class inputs rather than after-the-fact decoration.

### 2. Continuity is already treated as a first-class concern
The starter includes:
- `docs/project-status.md`
- `docs/session-handoff.md`
- `docs/starter-adoption-checklist.md`
- `docs/adopted-repo-guide.md`

This is important because real SDD is not only about writing specs; it is also about making work resumable, reviewable, and auditable across sessions, agents, and humans.

### 3. Quality and operational guardrails are not blank
The repo already includes:
- stage-by-stage quality gates
- rollback runbook
- incident runbook
- release preparation templates
- safety-oriented Claude settings

That means this repo is already aiming at **controlled delivery**, not just content generation.

### 4. External tracking and backlink thinking already exists
The intake template and integration docs already include:
- source of truth
- primary tracker ID
- related references / backlinks

This is a real SDD-positive signal because it reduces fragmentation between requirement docs, issues, PRs, and release artifacts.

### 5. It already has pilot evidence, not only theory
This repo already contains:
- internal pilot chains
- feature / bugfix / migration / pilot examples
- a Java microservice / internal business system example chain

So it is no longer just a concept starter. It already has reusable execution shape.

---

## What still prevents it from being called “fully landed SDD”

### 1. Traceability is still partial, not enforced as a closed loop
The biggest remaining gap is not “lack of templates”.
The biggest gap is that the repo still needs stronger default expectations for a recoverable chain like:

`Requirement Intake -> PRD -> Design -> Tasking -> PR/MR -> Test Evidence -> Release Evidence`

Until this chain becomes easier to prove and harder to skip, the starter is still better described as **SDD-ready** rather than fully SDD-closed-loop.

### 2. Spec change control is not yet strong enough
A real SDD workflow needs a stable answer to questions like:
- what changed in the intended behavior?
- where was that change recorded?
- which upstream spec was updated first?
- what test / release evidence corresponds to that change?

Without explicit change control, teams can still drift into “code-first, docs-later” behavior.

### 3. Enforcement is still lighter than the ideal target state
Today many of the rules are well documented, but in many places they are still closer to:
- recommended structure
- expected behavior
- starter guidance

than to:
- mandatory PR / MR fields
- CI-visible traceability checks
- hook-enforced artifact discipline

So V2.0.0 should be read as **SDD-ready and partially operationalized**, not yet **SDD-enforced**.

### 4. Real adopted-repo proof is still the final test
A starter only becomes a proven lightweight SDD system after repeated real adoption shows that it actually:
- reduces requirement drift
- lowers rework
- improves multi-agent handoff quality
- improves release clarity
- keeps rollback / audit / context recovery manageable

That proof must come from adopted repos, not only from the starter repo itself.

---

## What V2.0.0 upgrades in this repository

This V2.0.0 positioning introduces a more explicit SDD framing:

### A. Positioning is now explicit
This repo is now described more clearly as:
- starter first
- adopted-repo oriented
- SDD-capable, but not pretending to be a finished heavy SDD system

### B. Traceability expectations are made more visible
PR / MR and release-related artifacts should make it easier to recover links to:
- requirement
- PRD
- design
- test plan
- change summary
- release checklist

### C. Change-control expectations are made more visible
Behavior-changing work should make it clearer whether:
- upstream spec changed
- related artifacts were updated
- unresolved decisions still need human approval

### D. The next maturity step is now clearer
The next target is not “add more templates”.
The next target is:
- stronger traceability
- stronger change control
- better workflow enforcement
- real adopted-repo validation

---

## Recommended next maturity path after V2.0.0

### Priority 1 — make traceability recoverable by default
At minimum, adopted repos should make it easy to recover:
- Requirement ID
- PRD link
- Design link
- Test-plan link
- Change summary / release checklist link
- primary tracker / issue / PR / MR references

### Priority 2 — make spec change control explicit
For changes that alter behavior, scope, or release risk, adopted repos should record:
- what spec changed
- why it changed
- which downstream artifacts must be revisited
- whether human approval is still needed

### Priority 3 — shift from “recommended” to “enforced” where practical
Reasonable next enforcement points include:
- PR / MR template fields
- review checklist expectations
- CI-visible artifact references
- hooks that remind authors when upstream artifacts are missing

### Priority 4 — validate in real adopted repos
The real graduation test is not more prose in this starter.
It is whether a real repo can use this chain and get measurable benefits in:
- alignment
- handoff quality
- test clarity
- release clarity
- rollback readiness

---

## Simple one-line verdict

**V2.0.0 means this repo should be understood as a serious Mini-SDD starter with explicit traceability and change-control direction, but its final maturity still depends on real adopted-repo validation and stronger enforcement.**
