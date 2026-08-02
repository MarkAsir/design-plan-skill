# Modes and Gates

Choose one primary document mode.

## DESIGN-ONLY

Use for an approved design set without an implementation plan. The set may be
one plain design document, several role-oriented documents, OpenSpec artifacts,
or an equivalent format.

- Default gate: `DESIGN READY`.
- Design artifacts are editable only when remediation is authorized.
- Code, tests, rules, runtime, and later plans are read-only evidence.
- A missing plan is `NEXT STAGE NOT STARTED`, never a repeated wait or blocker.
- Review premise/value, scope/boundary, architecture/contracts, and delivery/operations.
- Require testable behavior, key acceptance scenarios, critical journey, and verification ownership; do not require detailed test cases.

Select one adapter after the mode:

- **OpenSpec**: preserve proposal/spec/design/tasks ownership, stable IDs, delta
  semantics, and repository validation commands.
- **Generic design**: normalize available sections into charter, behavior
  contracts, decisions, work ownership, and acceptance responsibilities. One
  file may own several roles. Do not require OpenSpec filenames, directories,
  IDs, validators, or document splitting.

On readiness, include a compact attestation in the final response or immediate
runtime handoff with artifact/rule snapshots, charter, frozen decisions,
accepted concerns, routed items, auditor identity, and timestamp. Do not
persist it after completion unless the user explicitly requests an audit artifact.

## PLAN-ONLY

Use for an existing implementation plan derived from frozen approved design,
regardless of its authoring framework.

- Default gate: `PLAN READY`.
- Only the explicitly selected current plan path set is editable.
- Upstream design, code, tests, rules, and runtime facts are read-only.
- Apply dependency, granularity, risk, coverage, and feasibility lenses.
- If no plan exists, return `PLAN NOT READY: GENERATION REQUIRED` and route once to plan writing; do not wait in a loop.
- A regenerated plan or changed upstream decision starts a new PlanAuditKey.
- A plan defect is repaired in the plan; an upstream defect returns `UPSTREAM DESIGN REOPEN REQUIRED`.

Select the Superpowers adapter only when the user, path, or document format
identifies it. Otherwise use the generic plan profile and do not require
Superpowers headers, code-block cadence, TDD phrasing, or commit boundaries
unless project rules independently require them. Creation/regeneration remains
owned by the applicable plan-writing workflow.

## COMBINED

Use only when the user explicitly authorizes both design and plan artifacts in
one invocation. Treat it as two ordered sub-runs, never as simultaneous mutation:

1. complete the DESIGN-ONLY sub-run and freeze its DESIGN READY snapshot;
2. if design changed, stop plan repair, route once to the applicable plan-writing
   workflow, and start a new PLAN-ONLY generation only after a regenerated plan exists;
3. if design was already ready and unchanged, bind the existing plan to that
   snapshot with a new PlanAuditKey and continue PLAN-ONLY;
4. grant PLAN READY only after both gates hold on their respective bound snapshots.

The same invocation may continue across regeneration only when that separate
plan-writing action is authorized. Otherwise return the routing result and
stop. Never patch a plan whose upstream design snapshot changed.

Do not combine unrelated plans merely to reduce review cycles.

## Later gates are routing targets

`IMPLEMENTATION READY` is not automatically triggered by document completion.
Route to the repository implementation-verification workflow only when:

- the user explicitly requests implementation verification;
- the implementation-verification skill/command is invoked; or
- production changes and tests exist and the user says implementation is complete.

For OpenSpec repositories, this normally routes to `openspec-verify-change`.

`RELEASE READY` requires an explicit release/readiness request after
implementation evidence and a release candidate or target environment exist.
Route to QA, canary, ship, or release workflows. Never auto-run it after
IMPLEMENTATION READY.

LEAN/AUTO/ASSURANCE changes review depth, not these gate boundaries.
