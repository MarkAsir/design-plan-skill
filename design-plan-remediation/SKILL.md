---
name: design-plan-remediation
description: Revise authorized plain or structured technical designs, OpenSpec artifacts, and generic or Superpowers implementation plans after evidence-backed review. Use when current-gate blockers or required findings must be rechecked and repaired without imposing a framework-specific shape, changing frozen decisions, expanding scope, implementing future code or tests, or treating watch and later-stage work as document defects.
---

# Design and Plan Remediation

## Core contract

Repair only authorized planning artifacts after independently confirming each
finding. Produce candidate closure; never grant `CLOSED` or READY.

1. Read applicable organization, project, methodology, repository, and artifact-format rules.
2. Confirm modification authority; audit-only means no edit.
3. Freeze gate, risk profile, snapshot, editable paths, charter, Non-Goals, decisions, accepted risks, and must-not-change behavior.
4. Inspect required upstream, downstream, code, rule, and runtime evidence read-only.
5. Do not modify implementation, tests, external systems, or frozen upstream artifacts unless separately authorized through the owning workflow.

Read [references/finding-admission.md](references/finding-admission.md) before
editing. For a plan or plan-only request, also read
[references/implementation-plan-profiles.md](references/implementation-plan-profiles.md).
For design artifacts that are not OpenSpec, also read
[references/generic-design-profile.md](references/generic-design-profile.md).

## Establish intake

Require:

```text
target gate and LEAN/AUTO/ASSURANCE profile
current snapshot or PlanAuditKey
active findings and minimum closure contracts
editable artifact allowlist
read-only evidence boundary
charter, scope, Non-Goals, completion criteria
frozen decisions, accepted risks, unresolved decisions
```

Do not load historical repair narratives by default. Use current raw artifacts,
the compact active ledger, and the finding evidence. If different gates or
product choices materially change the repair, return `DECISION REQUIRED`.

## Recheck and admit findings

Admit a finding only when all are true:

```text
still exists
+ BLOCKER or REQUIRED at the current gate
+ inside frozen scope and not a Non-Goal
+ every write path is authorized
+ no unresolved decision is being made
+ not WATCH, CONCERN, NEXT-STAGE, or OUT-OF-SCOPE work
+ smallest correct repair does not add an independently deliverable subsystem
```

An auditor's fix suggestion constrains the invariant, not the implementation.
Reject stale, disproved, over-scoped, or already-repaired advice. A plan-only
finding that invalidates frozen design returns
`UPSTREAM DESIGN REOPEN REQUIRED` with no plan mutation.

## Form root-cause batches

Cluster admitted findings by root cause and decision chain:

```text
charter/scope -> requirement/scenario -> decision/trade-off
              -> task/order -> plan/interface/command
              -> verification responsibility -> rollout/recovery -> risk
```

Edit the authoritative owner first, then propagate only through affected
references. Preserve stable identifiers. Remove superseded semantics. Inspect
sibling services, languages, entry points, and consumers only when they share
the same invariant.

Treat proposal, contract, decision, work, and acceptance as semantic ownership
roles rather than required files. Preserve a generic document's existing
structure when it expresses those roles unambiguously.

## Keep the repair bounded

Use the smallest change that restores the approved invariant.

Before adding a new actor, controller, state, storage, protocol, lock, fallback,
retry path, deployment action, or recovery channel, establish owner, authority,
activation order, valid/rejected transitions, fail-closed behavior, and
verification. If it changes the capability boundary, Non-Goals, evidence cost,
or can ship independently, return `DECISION REQUIRED` or propose a separate
change.

Do not:

- solve an untriggered WATCH;
- convert a later-stage note into current document scope;
- add detailed test cases to OpenSpec;
- implement future source or tests while repairing a plan;
- create recovery-of-recovery trees beyond the approved risk slice;
- rewrite frozen design through plan prose.

Do not repair toward maximum specificity. If a competent executor can choose a
detail without changing approved behavior, safety, compatibility, ordering,
ownership, or the stable oracle, keep that detail in the implementation stage.

## Apply surgical changes

For each batch:

1. edit the authoritative contract or artifact owner;
2. propagate the complete affected decision chain;
3. update only affected tasks, plan steps, verification responsibility, rollout, recovery, risks, and mappings;
4. preserve unrelated wording, files, and project style;
5. ensure each phase uses only current or earlier outputs and ends in a compatible, verifiable state;
6. re-read the affected chain after a material decision change.

Split a plan task when it has independently committable behavioral outputs,
different owners/environments, compatibility phases, or independent
failure/release boundaries. Merge fragments that lack an independent stable
result, oracle, and safe commit/revert boundary. Do not split by file, function,
or line count alone.

Keep verification stage-owned:

- OpenSpec: testable behavior, key acceptance scenarios, critical journey, verification owner.
- Plan: test layer, path/owner, prerequisites, fixture category, environment, and stable oracle.
- Development/integration/release: detailed cases, fixtures, execution, and runtime evidence.

## Verify the repair

Run only gate-appropriate checks:

1. **Static**: structure, traceability, baseline/delta consistency, enumeration, removed semantics.
2. **Safe rehearsal**: current paths, tools, shell, permissions, interfaces, and command feasibility; do not implement the plan.
3. **Reverse**: every edit traces to an admitted finding; every affected contract has work and verification; no new work lacks an approved contract.

Inspect the exact diff for unauthorized paths, unrelated changes, scope drift,
stale wording, inconsistent siblings, new mechanisms, and mapping/count errors.
Label evidence `STATIC`, `PLANNED`, `EXECUTED`, or `UNVERIFIED`; a future command
written in a plan remains `PLANNED`.

## Hand off candidate closure

Return one:

- `CANDIDATE CLOSED`: admitted repairs are ready for independent audit.
- `OPEN`: repair or current-gate evidence remains incomplete.
- `DECISION REQUIRED`: authority, scope, risk acceptance, or product choice is required.
- `REJECTED`: the finding is disproved.
- `UPSTREAM DESIGN REOPEN REQUIRED`: a plan-only finding invalidates frozen design.

Report the input/output snapshot, admission result per finding, root-cause
batches, edited paths, preserved decisions, new mechanisms, executed versus
planned evidence, routed residuals, and exact re-audit boundary.

Read [references/templates.md](references/templates.md) only when a formal
intake, admission ledger, repair map, or handoff is required.
