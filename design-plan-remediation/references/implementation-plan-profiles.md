# Implementation Plan Remediation Profile

Use when the authorized artifact is an implementation plan.

## Plan-only boundary

Treat the approved design as frozen and read-only. Require the current upstream
snapshot, plan generation, applicable rules, and PlanAuditKey. The plan is the
only editable artifact unless the user explicitly reopens design.

Stop with no mutation when:

- the plan generation or upstream snapshot changed;
- the plan is absent, superseded, archived, or outside the selected path set;
- a finding requires changing design, code, tests, rules, or external state;
- the plan exposes an upstream contradiction or missing decision.

Route creation/regeneration to the repository plan-writing workflow. An
upstream defect returns `UPSTREAM DESIGN REOPEN REQUIRED`.

## Plan-owned repairs

Repair only confirmed plan-gate defects such as:

- output-before-input ordering;
- wrong or incomplete paths, interfaces, owners, or prerequisites;
- a task too large to yield one stable behavioral result, or too fragmented to verify independently;
- missing current-gate risk sequencing, compatibility phase, rollback/fail-closed responsibility;
- missing requirement-to-task or task-to-requirement mapping;
- infeasible cwd, tool, command, prerequisite, expected result, or oracle;
- test responsibility not routed to unit, integration, E2E, implementation verification, or release;
- unresolved placeholder that transfers a frozen decision to the executor.

Do not invent behavior to fill a design gap.

## Granularity repair

A task has one primary behavioral objective, coherent change chain, stable
result, and verification oracle.

Split for independently committable/revertible outputs, different
owners/environments, expand/switch/contract phases, stable intermediate states,
or independent failure/release boundaries. Merge fragments without an
independent observable result, oracle, or safe commit/revert boundary.

Never split merely by file, function, or line count. If a split creates a broken
intermediate state, merge it or introduce an already-approved compatible phase.

## Test and Superpowers adaptation

For OpenSpec-derived plans, preserve frozen behavioral contracts. Map each
approved contract to test layer, owner/path, prerequisites, fixture category,
environment, and stable oracle. Identify the critical E2E journey without
writing the complete test suite.

Apply the Superpowers adaptation only after authoritative selection by the
audit profile or applicable project rules; headings and checkboxes alone are
not sufficient. For Superpowers plans, preserve required headers, execution
handoff, file map, interfaces, task order, commands, and commit boundaries. Use TDD red/green steps
for behavioral code when required by project workflow. Documentation and
mechanical configuration use the smallest relevant check.

Do not expand plans with full future source or exhaustive case matrices unless
the governing format expressly requires exact code and ambiguity would change
behavior. Concise references to authoritative contracts are allowed; vague
decisions and unnamed checks are not.

Default repair detail is: behavioral objective, stable location or anchor,
dependencies, approved constraints, smallest relevant check, and oracle. Leave
local algorithms, helper bodies, fixture content, and ordinary error branches
to implementation when they cannot alter a frozen contract. Require exact
commands only when tool/order semantics are themselves a safety,
compatibility, destructive-action, or production-activation control.

At PLAN READY, validate present facts and command feasibility without
implementing. Future results remain `PLANNED`.

## Generic plan adaptation

For a generic plan, preserve its existing headings, tables, checklists, and
terminology. Repair the normalized behavioral tasks and traceability without
adding Superpowers headers, code-block cadence, TDD wording, or commit
boundaries unless project rules require them. A frozen user- or
workflow-approved plain design is a valid upstream baseline. Keep any generated
trace IDs in the temporary ledger unless durable IDs are separately authorized.
This fallback minimum does not override stricter organization, methodology,
project, or artifact-format rules, including required positive/negative
verification and phase or commit boundaries.

## Lifecycle

A changed upstream decision, regenerated plan, selected path-set change, or material
rule change creates a new PlanAuditKey. Older findings become history; run a new
audit before editing the new generation.
